---
title: "Comité technique - Février 2020"
date: February 03, 2020
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

## Slides (O. Grisel)
[Presentation of the technical achievements and ongoing work](https://docs.google.com/presentation/d/1ImPMyg91vJsG0KKXMDn9USBKSZFrMlzeqvn6enXmJ_A/edit#slide=id.p) 

## Priority list for the consortium at Inria, year 2018–2019

From the discussion during the technical committee, the scikit-learn Consortium at Inria defined the following list of priorities for the coming year:


- Continue effort helping with project maintenance to keep the target to release twice a year (+ bugfix releases).
- Development of the model inspection tools (and related):
    - Help, review, and document the support for feature names in pipelines:  https://github.com/scikit-learn/scikit-learn/pull/14315
    - Extend PDP tools to support categorical variables and add error bars: https://github.com/scikit-learn/scikit-learn/issues/14969
    - Integrate guidelines on interpretation and uncertainty of coefficients of linear models in scikit-learn as a scikit-learn example or tutorial in the main repo: https://github.com/scikit-learn/scikit-learn/pull/15706
    - Document with User Guide and examples to give intuitions, insights, caveats of the different methods: comparison of model agnostic tools such as SHAP, permutation importances… (pros and cons)
    - Cross-reference more bleeding-edge inspection tools from external libraries such as SHAP, ELI5, InterpretML, from the documentation and examples in scikit-learn. Possibly add an example in scikit-learn to compare SHAP values to scikit-learn permutation importances and highlight pro-and-cons.
    - Add support for HGBRT to SHAP: https://github.com/slundberg/shap/issues/1028
    - Add ICE plots: https://github.com/scikit-learn/scikit-learn/pull/16164
- Finish review of the implementation of Poisson, Gamma, Tweedie regression losses for linear models and gradient boosting trees: https://github.com/scikit-learn/scikit-learn/pull/14300
- Help finalize and review the missing features for the new implementation of Gradient Boosting Trees (i.e. sample weights, monotonic constraints, interaction constraints, categorical data, and sparse data).
- Continue work on making scikit-learn estimators more scalable w.r.t. number of CPUs and less prone to over-subscription issues when using native threadpools (OpenMP / BLAS)
- Continue effort on benchmark and compliance tests:
    - Benchmark against ONNX models and use results to optimize ONNX or sklearn
    - http://www.xavierdupre.fr/app/_benchmarks/helpsphinx/index.html
    - Find a way to keep the benchmark running: share the benchmark execution results as JSON + HTML report on a common public repo. Use github pages to publish an aggregate view of the results: https://github.com/jeremiedbb/scikit-learn_benchmarks
    - Assist RAPIDS devs if they are interested in sharing the benchmark suite to keep an up-to-date comparison point.
- Implementation of Balanced Random Forest and Balanced Bagging Classifier: https://github.com/scikit-learn/scikit-learn/pull/13227. Once done, reconsider if we need additional tools such as a generic resample API: SLEP005
- Improve support for quantification of uncertainties in predictions and calibration measures
    - New calibration error for classification: https://github.com/scikit-learn/scikit-learn/pull/11096
    - QuantileRegression for Linear Models: https://github.com/scikit-learn/scikit-learn/pull/9978
    - QuantileRegression for HGBRT
    - QuantileRegression for MLP (optional)
    - Calibration reporting tool and documentation for quantile regression models
    - Isotonic Calibration should not impact rank-based metrics: https://github.com/scikit-learn/scikit-learn/issues/16321
- Evaluate interoperability with other types of array (e.g. dask arrays and CuPy arrays) for some preprocessing methods once the n_features_in_ PR is merged (by customizing the _validate_input method. Possibly by leveraging the newly introduced __array_function__ protocol (NEP 18) or array modules (NEP 37): https://github.com/scikit-learn/scikit-learn/pull/16112
- Evaluate interoperability with other types of array (e.g. dask arrays and CuPy arrays)  for pipeline / column transformer, cross-validation, and parameter search. Depending on the outcome of the discussion w.r.t. features names in Pipeline and XArray.
- OpenMP GPU offloading: evaluation of the experimental Cython branch
- Example on quantification of fairness issues and potential de-biasing
- Extend the “anti-pattern” / pitfalls examples in the scikit-learn documentation: https://github.com/scikit-learn/scikit-learn/issues/14081
- Improve documentation with extra-examples: time series feature engineering and cross-validation based model evaluation.
- Extend support of categorical encoding e.g. (impact coding) and better document the pros and cons of each approach in examples.
- Review literature for confidence interval for predictions: bootstrapping and conformal predictions (in collaboration with Léo Dreyfus-Schmidt at Dataiku). See https://arxiv.org/abs/1604.04173 and https://cdsamii.github.io/cds-demos/conformal/conformal-tutorial.html for instance.
- Callback and logging (interruption) monitor and checkpoint fitting loop. One application would be to better integrate with (internal and external) hyper-parameter search strategies that can leverage checkpointing to make model selection more resource efficient.

One the community side:

- Organise more regular technical sprints (possibly by inviting past sprint contributors to try to foster a long term relationship and hopefully recruit new maintainers).
- Renew the organization of  beginners’ workshops for prospective contributors.
- Conduct a new edition of its 2013 survey among all scikit-learn users.


