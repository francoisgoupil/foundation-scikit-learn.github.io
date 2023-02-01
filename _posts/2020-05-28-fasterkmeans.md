---
title: "Implementing a faster KMeans in scikit-learn 0.23"
category: Technique
tags: 
- technique
- insights

layout: splash
postauthors:
  - name: Jérémie du Boisberranger
    image: jeremie_duboisberranger.jpg 
header:
  overlay_image: /assets/images/cover image sklearn.png
  overlay_filter: linear-gradient(rgba(41,171,226,0.9), rgba(247,147,30,0.9))
sidebar:
  nav: "docs"
lang: fr

---
<div>
  <img src="/assets/images/posts_images/{{ page.featured-image }}" alt="">
  {% include postauthor.html %}
</div>

The [0.23](https://scikit-learn.org/stable/whats_new/v0.23.html#version-0-23-0) version of scikit-learn was released a few days ago, bringing new features, bug fixes and optimizations. In this post we will focus on the rework of KMeans, a long going work started almost two years ago. Better scalability on machines with many cores was the main objective of this journey. It forced us to touch core challenges of low-level parallelism.

## KMeans clustering

Before describing the optimization details, let’s remind the principles of the algorithm. The goal is to group data points into clusters, based on the distance from their cluster center. We start with a set of data points and a set of centers. First, the distances between all points and all centers are computed and for each point the closest center is identified: during this step a label is attached to each cluster. Then, the center of the cluster is updated to be the barycenter of its assigned data points.

A benchmark comparison with[ daal4py](https://intelpython.github.io/daal4py/), the python wrappers for [Intel’s DAAL](https://software.intel.com/content/www/us/en/develop/tools/data-analytics-acceleration-library.html) library, showed that a significant speed-up could be hoped both in sequential and in parallel runs (the discussion, initiated by [François Fayard](https://github.com/FrancoisFayard), started [here](https://github.com/scikit-learn/scikit-learn/issues/10744)). Furthermore, a preliminary profiling showed that the computation of the distances is the critical part of the algorithm but finding the labels and updating the centers is also not negligible and would quickly become the bottleneck once the first part is well optimized.

## CPU Cache optimization

The previous implementation exposed a parameter, called *precompute_distances*, aimed to switch between memory and speed optimization. Favoring speed means that all distances are computed at once and stored in a distance matrix of shape (*n_samples*, *n_clusters*). Then labels are computed on this distance matrix. It’s fast, especially because it can be done using a [BLAS](http://www.netlib.org/blas/) (Basic Linear Algebra Subprogram) library which is optimized for the different families of CPU. The drawback is that it creates a potentially large temporary array which can take a lot of memory. On the other hand, favoring memory efficiency means that distances are computed one at a time and labels are updated on the fly. There is no temporary array but it’s slower, because distance computation cannot be vectorized.

Besides causing memory issues, **a large temporary array does not provide optimal speed** either. Indeed moving data from the RAM into the CPU and vice versa is quite slow. If we need a variable several time for our computations but we have to fetch it from the RAM each time, we are wasting a lot of time. This is what happens in the k-means algorithm: back and forth from point to center positions to update labels and distances. Ideally we want the data to stay as close to the CPU as possible, meaning in the CPU cache, while it’s needed for the computations.

The solution we chose is to **compute the distances for only a chunk of data at a time**, creating a temporary array of shape (*chunk_size*, *n_clusters*).

<img loading="lazy" class="wp-image-1650 aligncenter" src="/assets/images/posts_images/kmeans/chunk_size-1.svg" alt="" width="560" height="362">

Choosing the right chunk size is crucial. A CPU can do the same operation on several variables at once in a single instruction (this is a SIMD CPU, for Single Instruction Multiple Data). If the temporary array is too small we don’t fully exploit the vectorization capabilities of the CPU. If the temporary array is too large it does not fit in the CPU cache. We can clearly see that in the figure beside. We chose a chunk size of 256 (2⁸) samples. It guarantees that the temporary array will fit in the CPU cache which is typically a few MB, while keeping a good vectorization capability.
    
Overall, this new implementation is faster than both previous implementations and has a very small memory footprint (only a few MB). Also, this allowed us to simplify the API by deprecating the precompute_distances parameter. Benchmarks on single core are shown in the figure below. Timing measurements are on the left and the corresponding speed-ups on the right.

<img class="aligncenter wp-image-1666" src="/assets/images/posts_images/kmeans/speedup-3.svg" alt="" width="1080">

## Parallelism, using data-level OpenMP

The new implementation also changed the parallelism scheme. Previously, a first level of parallelism, handled by the [joblib](https://joblib.readthedocs.io/en/latest) library, was implemented at the outer most level. The *n_jobs parameter* was used to control the number of processes to run the *n_init* complete runs of k-means (despite its name, *n_init* is actually about complete runs, not just the initialization). That meant that we couldn’t use the full capacity of a machine with more than *n_init* cores (the default is 10 and it is usually not useful to take a bigger value). Another level of parallelism came from the BLAS library used in the computation of the distances. However the other steps of the iteration loop are sequential which prevent good scalability.

<img loading="lazy" class="alignright wp-image-1679" src="/assets/images/posts_images/kmeans/v022-1.svg" alt="" width="453" height="372">

In version 0.23, we decided to move the outer parallelism to the data level. For one chunk of data, we can compute all distances between the points and the clusters, find the labels, and even compute a partial update of the centers. Here, the parallelism is implemented using the [OpenMP library in Cython](https://cython.readthedocs.io/en/latest/src/userguide/parallelism.html). Putting the parallelism at this level gives us a much better scalability and we can now fully benefit from all the cores of the CPU, even if the user decide to use *n_init*=1.

<img class="size-medium wp-image-1680 alignright" src="/assets/images/posts_images/kmeans/v023-1.svg" alt="" width="560">

The figures below show the time to fit a KMeans instance with *n_init*=1 (on a large dataset on the left and on a small dataset on the right) for various number of available cores. Green and blue curves concern scikit-learn 0.22. There is barely no scalability on a large dataset (time is reduced by a factor of 2 between using 1 or 16 cores) and no scalability at all on a small dataset. Red and orange curves concern scikit-learn 0.23. Scalability is much better and near perfect on large datasets if we ignore the initialization (orange curve). We discuss the scalability issues of the initialization in the last section.

<img class="aligncenter wp-image-1663" src="/assets/images/posts_images/kmeans/scalability-3.svg" alt="" width="1080">

In this new implementation, the parallelism at the data level is able to fully exploit all the available cores of the CPU, which means that the parallelism from the distances computation can lead to a situation of thread **oversubscription**, i.e. more threads than available cores are trying to run simultaneously. We had to find a way to disable this second level of parallelism coming from the BLAS library. This was the main challenge of this rework of KMeans. This challenge lead to the development of a new python library, [threadpoolctl](https://github.com/joblib/threadpoolctl), **to dynamically control, at the python level, the number of threads used by native C libraries** like OpenMP and several BLAS libraries. Threadpoolctl is now a dependency of scikit-learn, and we hope that it will be used more in the wider Python ecosystem.

## Further optimizations

Latest benchmarks still show that DAAL is faster than the 0.23 scikit-learn implementation, by a factor of up to two. Improving the performances will require optimizations, essentially regarding vectorization, that we cannot apply at the Cython level.

However there’s still room for improvement regarding the initialization of the centers (*k-means++*). It still has a poor scalability and since it takes a significant proportion of a run of KMeans, the whole estimator does not scale in an optimal way, as shown in the figure above. Although we think that a rework of *k-means++* might be possible: a simpler solution might be to run the initialization on a subset of the data (a discussion has been started [here](https://github.com/scikit-learn/scikit-learn/issues/11493)). We hope this would make the initialization take a negligible proportion of the whole run of KMeans, even if this does not solve the scalability issue.
