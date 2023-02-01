---
title: "Comité technique - Juillet 2019"
date: July 4, 2019
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

## Priority list for the consortium at Inria, year 2018–2019

From the points discussed at the meeting, the Technical Committee is proposing the following list of priorities for the actions of the consortium, to be used by the management team for allocating consortium resources:


- Continue effort to help with project maintenance to keep the target to release twice a year.

- Development of the “inspect” module:
    - Help finalize the pull requests for the newly introduced methods for model inspections.
    [issue](https://github.com/scikit-learn/scikit-learn/issues/14969)
    - Better interoperability with pandas dataframe and scikit-learn pipeline, in particular with the feature names.
    [pull request](https://github.com/scikit-learn/scikit-learn/pull/14028)
    - Document with User Guide and examples to give intuitions, insights, caveats of the different methods.
    - Cross-reference more bleeding-edge inspection tools from external libraries such as SHAP and ELI5 from the documentation and examples in scikit-learn. Possibly add an example in scikit-learn to compare SHAP values to scikit-learn permutation importances.
    - Integrate guidelines on interpretation and uncertainty of coefficients of linear models in scikit-learn as a scikit-learn example or tutorial in the main repo.
- Implementation of Poisson, Gamma, Tweedie regression losses for linear models and gradient boosting trees.
[pull request](https://github.com/scikit-learn/scikit-learn/pull/14300)
- Improvement of machine learning pipeline with feature names.
- Help finalize the missing features for the new implementation of Gradient Boosting Trees (i.e., native support for missing values, categorical data, and sparse data).
- Continue effort on benchmark and compliance tests:
    - Integration of ONNX models.
    - Make it easier to reuse the scikit-learn benchmark suite to compare with alternative implementation of the models from DAAL and RAPIDS.
- Develop a new resampler meta-estimator for imbalanced classification problems, by working with the community on SLEP005.
- Propagation of feature names (e.g. within scikit-learn pipeline), by working with the community on SLEP008.
- Improve documentation with extra-examples: time series applications, model inspection, quantification of predictive uncertainty and uncertainties on linear model coefficients, “anti-pattern” examples.
[issue](https://github.com/scikit-learn/scikit-learn/issues/14081)
- Evaluate interoperability with other type of array (e.g. dask arrays and CuPy arrays) for some preprocessing methods, pipeline / column transformer, cross-validation and parameter search. Possibly by leveraging the newly introduced _array_function__ protocol.

We recall the list of priorities of the previous year which would be considered for this year as well. Note that some of these points are ongoing work:

- Tools to compare validity of model between scikit-learn versions.
- Confidence interval for predictions: look at the bootstrapping, and review literature on conformal predictions (in collaboration with Léo Dreyfus-Schmidt at Dataiku who will work on benchmarking the literature). See [paper](https://arxiv.org/abs/1604.04173) and [tutorial](https://cdsamii.github.io/cds-demos/conformal/conformal-tutorial.html) for instance.
- Quantile regression for linear and GBRT models.

- Better missing data handling.
- Better categorical encoding.
- Callback and logging (interruption) monitor progress.
- Organise monthly technical sprint.



 