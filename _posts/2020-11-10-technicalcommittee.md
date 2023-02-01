---
title: "Comité technique - Novembre 2020"
date: November 10, 2020
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
[Presentation of the technical achievements and ongoing work](https://docs.google.com/presentation/d/1MNTJpUoJANI6G0u6dyW7SY0Gjzccq3qCia9eYqRqqd8/edit#slide=id.p) 

## Priority list for the consortium at Inria

From the discussion during the technical committee, the scikit-learn Consortium at Inria defined the following list of priorities for the coming year:

- Continue effort helping with project maintenance to keep the target to release twice a year (+ bugfix releases).
- Continue developments of the model inspection tools (and related features):
    - Help, review, and document the support for feature names in pipelines: [pull request](https://github.com/scikit-learn/scikit-learn/pull/18444) (in progress)
    - Extend PDP tools to support categorical variables: [issue](https://github.com/scikit-learn/scikit-learn/issues/14969, https://github.com/scikit-learn/scikit-learn/pull/18298) (in progress)
    - Document in User Guide and with examples to give intuitions, insights, caveats pros and cons of the different methods: comparison of model agnostic tools such as SHAP, permutation importances. [pull request](https://github.com/scikit-learn/scikit-learn/pull/18139) (in progress)
    - Cross-reference more bleeding-edge inspection tools from external libraries such as SHAP, ELI5, InterpretML, from the documentation and examples in scikit-learn.
    - Help finalize and review the missing features for the new implementation of Gradient Boosting Trees, i.e. interaction constraints, categorical data (the related pull request has been merged but usability can be improved, see discussion in [issue](https://github.com/scikit-learn/scikit-learn/issues/18894), and sparse data (not started yet).
    - Continue improving generic tools for handling categorical variables. In particular finalize the implementation of Impact Coding in [pull request](ttps://github.com/scikit-learn/scikit-learn/pull/17323) and better document the pros and cons of each approach in examples.
- Continue work on making scikit-learn estimators more scalable w.r.t. number of CPUs and less prone to over-subscription issues when using native threadpools (OpenMP / BLAS).
    - MiniBatchKMeans is in progress [pull request](https://github.com/scikit-learn/scikit-learn/pull/17622)
    - LogisticRegression is a good candidate by making the computation of the loss and gradient parallel over the different samples.
    - MiniBatchNMF is almost ready, for increased scalability. [pull request](https://github.com/scikit-learn/scikit-learn/pull/16948)
- Implementation of Balanced Random Forest and Balanced Bagging Classifier: [pull request](https://github.com/scikit-learn/scikit-learn/pull/13227). This is in progress, currently solving lower level issues with scorer. Still some internal refactoring is needed: https://github.com/scikit-learn/scikit-learn/pull/18589. To move forward we need to compare to a classifier that can change its decision threshold: https://github.com/scikit-learn/scikit-learn/pull/16525. Once done, it should be reconsidered if we need additional tools such as a generic resample API: SLEP005.
- Improve support for quantification of uncertainties in predictions and calibration measures
    - Expected Calibration Error for classification: https://github.com/scikit-learn/scikit-learn/pull/11096
    - QuantileRegression for Linear Models: https://github.com/scikit-learn/scikit-learn/pull/9978
    - QuantileRegression for HGBRT
    - Calibration reporting tool and documentation for quantile regression models (quantile loss in sklearn.metrics, average quantile coverage)
    - Isotonic Calibration should not impact rank-based metrics: https://github.com/scikit-learn/scikit-learn/issues/16321
- Quantification of fairness issues and potential de-biasing
    - Example to raise awareness on the issues of fairness, possibly adapted from https://github.com/TwsThomas/fairness/blob/master/fairness%20adult_sgd.ipynb
    - Work on sample props as a prerequisite to go further.
- Improve documentation with extra-examples: time series feature engineering and cross-validation based model evaluation.
Review literature for confidence interval for predictions: bootstrapping and conformal predictions (in collaboration with Léo Dreyfus-Schmidt at Dataiku). See https://arxiv.org/abs/1604.04173 and https://cdsamii.github.io/cds-demos/conformal/conformal-tutorial.html for instance.
- Callback and logging (interruption) monitor and checkpoint fitting loop. One application would be to better integrate with (internal and external) hyper-parameter search strategies that can leverage checkpointing to make model selection more resource efficient. Related PR: https://github.com/scikit-learn/scikit-learn/pull/16925
- Consider custom loss functions, in particular for HGBRT.
- Consider whether survival analysis should be tackled in scikit-learn:
    - An example to educate on the biases introduced by censoring and point to proper tools, such as the lifeline or the scikit-survival projects.
    - Open a discussion on whether techniques such as survival forests should be included in scikit-learn or if they should be considered out of the scope of the project.
- Continue effort on benchmark and compliance tests:
    - Benchmark against ONNX models and use results to optimize ONNX or sklearn
    - http://www.xavierdupre.fr/app/_benchmarks/helpsphinx/index.html
    - Assist RAPIDS devs if they are interested in sharing the benchmark suite to keep an up-to-date comparison point.
    - Make easier to compare the relative performance of the same method implemented by different libraries with possibly slight API variations (e.g. vanilla sklearn vs daal4py vs cuML).
- Evaluate interoperability with other types of arrays that are compatible with the numpy API for some preprocessing methods once the n_features_in_ PR is merged (by customizing the _validate_input method. Such arrays would potentially include implementations from CuPy, ClPy, dpctl (or other interfaces to SYCL), JAX, dask any other library that provides computation and on-device memory allocation via a numpy compatible interface. Possibly by leveraging the newly introduced _array_function__ protocol (NEP 18) or array modules (NEP 37, see also NEP 35 which might be replaced by https://data-apis.org):
    - https://github.com/scikit-learn/scikit-learn/pull/16112
    - https://github.com/scikit-learn/scikit-learn/pull/17676
    - https://github.com/scikit-learn/scikit-learn/pull/16574
- Evaluate interoperability with other types of array (e.g. dask arrays and CuPy arrays) for pipeline / column transformer, cross-validation, and parameter search. Depending on the outcome of the discussion w.r.t. features names in Pipeline and XArray.
- Finally, scikit-learn developers have observed several times that the default solver we use for very commonly used linear models could benefit from several important improvements:
- Deprecating inconsistently used “normalize” option and instead point the users to the preprocessing module (StandardScaler, MinMaxScaler…)
https://github.com/scikit-learn/scikit-learn/pull/17743 (under progress)
- Investigate the possibility to automatically use a data-derived preconditioner for the solver in particular for smooth solvers in LogisticRegression / PoissonRegression, etc:
https://github.com/scikit-learn/scikit-learn/pull/15583
- Re-evaluate the choice of the default value of max_iter if necessary.

One the community side:

- Organise more regular technical sprints (possibly by inviting past sprint contributors to try to foster a long term relationship and hopefully recruit new maintainers).
    - Better preparation for issues
    - Plan with greater advance
- Renew the organization of beginners’ workshops for prospective contributors.
- Conduct a new edition of its 2013 survey among all scikit-learn users.


 