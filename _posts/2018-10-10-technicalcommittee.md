---
title: "Comité technique - Octobre 2018"
date: Octobre 10, 2018
categories:
  - Comités techniques
tags:
  - Comité technique
postauthors:
  - name: François Goupil
    image: francois_goupil.jpeg 
layout: splash
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

## Priority list for the consortium at Inria, year 2018–2019


From the points discussed at the meeting, the Technical Committee is proposing the following list of priorities for the actions of the consortium, to be used by the management team for allocating consortium resources:

 

- **Faster release cycle & dedicate resources for maintenance.**
- **Benchmark and compliance tests** (Intel & Nvidia): scikit-learn-benchmarks → collaboration on PR from Intel to accelerate the set of benchmarks to implement.
- Historical benchmark data of the scikit-learn code base will be used to detect performance regression or highlight improvements on the scikit-learn master branch. An action point across different actors is to collaboratively assemble and centralize benchmarks.
- The asv (air speed velocity) test suite will also be re-used to collect the metrics for several implementations (e.g. leveraging the alternative implementations in daal4py for intel or cuML for Nvidia), possibly on alternative hardware. The built-in reporting tool of asv does not appear to make it possible to contrast 2 runs of the same benchmark suite with different implementations / hardware but the data is stored in JSON so it should be easy enough to come with our reporting code for this task (e.g. bar plots with matplotlib).

*Remark: We will impose that to be able to push to the benchmark dashboard, the code should first pass or not the compatibility test (just check_estimator?)*

- **Tools to compare validity of model between scikit-learn versions** (retraining with the same parameters on the same dataset should –in general– yield the same fitted attributes and predictions on a validation set)

→ useful for to check that two alternative implementations of the same model can behave as drop-in replacements. This will also be useful to help automate model lifecycle and productization: handling library upgrade safely (numpy, scipy, BLAS, scikit-learn) and check correctness of exported models (e.g. ONNX runtimes).

- **Interpretability**:
    - Improving the documentation:  tutorial on common pitfalls and good practices.
    - Make it explicit that interpretability can have different meaning in different contexts for different purpose (ease of debugging and checking bias in training set, transparency of the decision function, gaining knowledge on the underlying data generating process (scientific inference, business insights), explainability of individual decisions for end-users that are impacted by the model decisions…)
    - Generic meta-estimator for model agnostic aggregate feature importance: remove one feature at a time and do all this in parallel -> permutation tests.
    - Partial dependence plots for more (all?) models.
    - Document available 3rd party tools for things that are outside of the scope of scikit-learn (open area for research): shap, LIME, yellowbrick, ELI5.
    - Add an “invertible” option to hashing vectorizer, so that it is less black box
- **Confidence interval for predictions**(in support) of the algorithms:
        For a model-agnostic solution: Bootstrap 632+ [Efron ‎1997] + cross_val_predict.

- In specific models it can be implemented most efficiently. Right now, the only API is return_std for some model, which may not be well suited.

- ***New GBT model**:
    - Binning-based fast training algorithm

- Richer losses (Poisson, Gamma, GLM+GBRT).
- Quantile regression
- Assuming that the new code is simpler that our existing implementation, it would make it possible for advanced users to build business specific regularization logic into the code of the trees (but no guarantee of stability of the internal API).

- **Quantile regression** for linear and GBRT models (check reference on implicit quantile networks)
- **Better missing data handling**.
    - Special bin for missing data in KBinDiscretizer

- A specific bin in decision trees
- More generally, attention to handling missing data here and there in scikit-learn

- **Better categorical encoding**:
    - Target encoding/Impact encoding.

- Stateless one-hot encoder using hashing.

- **Callback and logging** (interruption) monitor progress.
- General algorithm **computational optimization**:
    - Better Cython (using prange and direct BLAS call to scipy).
- Continuous improvement of parallel single machine (e.g. better oversubscription handling when composing thread runtimes).
- Distributed computing: ongoing effort with dask / dask-ml developers to improve the ability to run scikit-learn efficiently on shared and distributed computing resource and progressively explore how to best deal with high  data volumes scenarii (out-of-core / cluster partitioned data)

- **Sample_props**.
- **Feature names**: get_feature_name as added sugar to coefficient and feature importance.
- **Fostering community**: in parallel with the effort listed above, fostering technical communication across the community, including developers in partners team is crucial. For this, the foundation should dedicate resources to organize technical sprints, open to any contributor with enough technical expertise and time to contribute meaningfully. A monthly open sprint in Paris will be considered, with presence of at least one of the foundation engineers. Additionally, we will give **open technical training** (or tutorials) to develop a technically competent community. We will distinguish three kind of sprints:

    - **Technical contribution sprints**, where the goal is to contribute to the scikit-learn codebase. Participation to such a sprint will be limited to people with experience in contribution to Python data tools
    - **Training sprints**,
    - **Usecase sprints**

 