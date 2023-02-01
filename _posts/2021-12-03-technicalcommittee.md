---
title: "Comité technique - Décembre 2021"
date: December 3, 2021
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

From the discussion during the technical committee of December 3rd 2021, the scikit-learn Consortium defined the following list of priorities for 2022:

- **High priority**: Continue effort helping with project maintenance to keep the target to release twice a year (+ bugfix releases).
    - Bugfix release 1.0.2 end of December 2021 (or early January 2022) with macOS/arm64 support for the first time
    1.1 planned the first quarter of 2022
    - Refactor common tests to avoid missing estimators unintentionally and split the hard constraints from the scikit-learn internal conventions
- **High priority** Improve the feature names story:
    - Finalize get_features_names_out for all transformers https://github.com/scikit-learn/scikit-learn/issues/21308
    - Better handle feature names handling between meta- and base-estimators https://github.com/scikit-learn/scikit-learn/issues/21599#issuecomment-976480476
    - Use feature names to specify categorical variables in HistGradientBoostingClassifier/Regressor: related to https://github.com/scikit-learn/scikit-learn/issues/18894
    - Feature names propagation embedded in the datastructure returned by the transformers in pipelines (Pandas-in / pandas-out): https://github.com/scikit-learn/enhancement_proposals/pull/48

- Continue developments of the model inspection tools (and related features):

- **High priority** Documentwith examples in  in User Guide to give intuitions, insights, caveats pros and cons of the different methods: comparison of model agnostic tools such as SHAP, permutation importances. https://github.com/scikit-learn/scikit-learn/pull/18139 (in progress)
- Review http://proceedings.mlr.press/v119/kumar20e.html
- Extend PDP tools to support categorical variables: https://github.com/scikit-learn/scikit-learn/issues/14969, https://github.com/scikit-learn/scikit-learn/pull/18298, https://github.com/scikit-learn/scikit-learn/pull/19438 (in progress)
- Cross-reference more bleeding-edge inspection tools from external libraries such as SHAP, ELI5, InterpretML, from the documentation and examples in scikit-learn.
- [Idea to be explored] Expose the computation of Mean Decrease in Impurity and decision effects in trees on test samples to be able to efficiently compute both local and global explanation for trees (alternative to TreeSHAP and SAGE): see Local MDI (https://arxiv.org/abs/2111.02218) and Sabaas’ method (https://github.com/andosa/treeinterpreter)
- [Idea to be explored] Relate to permutation importance and test set MDI to SAGE for global explanations in an example or in the documentation to highlight pros / cons / pitfalls

- Finalize the implementation of Impact Coding in https://github.com/scikit-learn/scikit-learn/pull/17323 and better document the pros and cons of each approach in examples.
- Improve documentation with extra examples and topic-based discussions:

- Operationalization of models / MLOps
    - Other packages to use
    - Good practices (we are an entry point of the community)
        - Version control
        - Automate (CI / CD)
        - Long blocks of code should be in importable Python modules with tests, not in notebooks
    - [Idea to be explored] Declarative construction of pipelines (pros / cons)
    - Add side boxes in our doc / examples on the production logic (it may differ from exploration mode)
- Annotation regarding models’ hyperparameter: https://github.com/scikit-learn/scikit-learn/pull/17929

- Improving performance and scalability

- Comparative benchmark to identify how scikit-learn’s most popular models perform compared to other libraries (scikit-learn-intelex, XGBoost, lightgbm, ONNXRuntime, etc.)
    - https://github.com/mbatoul/sklearn_benchmarks (in progress)
- Continue work on making scikit-learn estimators more scalable w.r.t. number of CPUs, in particular with Cython optimization for the pairwise distances together with argmin/argpartition pattern used for KNearestNeighbors and many scikit-learn estimators
    - ArgKMin PR under review: https://github.com/scikit-learn/scikit-learn/pull/21462
    - Follow-up PRs for radius neighbors

- DOC and tools: safer recommendation for the right metrics for a given y_test.
- Improve support for quantification of uncertainties in predictions and calibration measures

- **High priority**: Expected Calibration Error for classification: https://github.com/scikit-learn/scikit-learn/pull/11096
- **High priority**: QuantileRegression for HistGradientBoostingRegressor (in progress) https://github.com/scikit-learn/scikit-learn/pull/21800
- Improve QuantileRegression for Linear Models (sparse support, alternative penalty and related solver):
    - Sparse input data (in progress) https://github.com/scikit-learn/scikit-learn/pull/21086
- Calibration reporting tool and documentation for quantile regression models (quantile loss in sklearn.metrics, average quantile coverage)
- Isotonic Calibration should not impact rank-based metrics: https://github.com/scikit-learn/scikit-learn/issues/16321
    - New lead, centered isotonic regression: https://github.com/scikit-learn/scikit-learn/pull/21454
- Start to brainstorm on how to represent a distribution on predictions

- Compliance tests for ONNX exports from https://github.com/onnx/sklearn-onnx

- Improve the default solver in linear models:

- Investigate the possibility to automatically use a data-derived preconditioner for the solver in particular for smooth solvers in LogisticRegression / PoissonRegression, etc:
https://github.com/scikit-learn/scikit-learn/pull/15583
- Re-evaluate the choice of the default value of max_iter if necessary.

- Document the limitations of quantifying statistical association vs causation by introducing the concept of interventional distributions vs observational distributions
    https://github.com/scikit-learn/scikit-learn/pull/20451
- Monotonicity constraints for Decision Trees (still in progress) https://github.com/scikit-learn/scikit-learn/pull/13649
- Dealing with Imbalanced Classification problem
    - Document the pitfalls of different evaluation metrics and link to imbalanced-learn
    - Implementation of Balanced Random Forest and Balanced Bagging Classifier: https://github.com/scikit-learn/scikit-learn/pull/13227. This is in progress, currently solving lower level issues with the scorer API. Still some internal refactoring is needed: https://github.com/scikit-learn/scikit-learn/pull/18589. To move forward we need to compare with a classifier that can change its decision threshold: https://github.com/scikit-learn/scikit-learn/pull/16525. Once done, it should be reconsidered if we need additional tools such as a generic resample API: SLEP005.

- Quantification of fairness issues and potential mitigation
    - Document fairness assessment metrics

Example to raise awareness on the issues of fairness, possibly adapted from https://github.com/TwsThomas/fairness/blob/master/fairness%20adult_sgd.ipynb
Work on sample props (SLEP 0006) as a prerequisite to go further (under review + prototype implementation).

Interaction constraints for Histogram Gradient-Boosting: https://github.com/scikit-learn/scikit-learn/issues/19148 (discussed in core dev meetings, agreed to be in-scope and under review)
Developer API: Making it easier for 3rd party developers by separating out non-user facing API that is not private (tested + backward compatibility)
Tutorial / guide on various strategies to assess certainty in predictions: impact of the choice of the loss function (e.g. mse, pinball, poisson), stability to resampling (e.g. using the Bagging meta-estimators), Gaussian process regression with covariance estimation and points to other external resources (e.g. conformal predictions, explicit bayesian posterior modeling…)
Programmatically defining good value / starting points for hyperparameter grids:
    https://github.com/scikit-learn/scikit-learn/issues/5004
    https://github.com/scikit-learn/scikit-learn/pull/5564
    https://github.com/scikit-learn/scikit-learn/pull/18821
    https://github.com/scikit-learn/scikit-learn/pull/16898
    https://github.com/scikit-learn/scikit-learn/pull/16287
Programmatic way to specify hyperparameter search without param name string mangling with `__`
    Discussed in core devs meeting
    SLEP required https://github.com/scikit-learn/scikit-learn/pull/21784
    Related to `__sk_clone__` proposal: https://github.com/scikit-learn/scikit-learn/issues/21838
Callback and logging (interruption) monitor and checkpoint fitting loop. One application would be to better integrate with (internal and external) hyper-parameter search strategies that can leverage checkpointing to make model selection more resource efficient.
    This is can be useful for teaching and general UX (progress bars that work on parallel sub-tasks even in notebook)
    This can be important for MLOps (monitoring, snapshotting model for inspection, etc.)
    It can help us gain agility during development (write better tests for the impact of convergence criteria, easier convergence debugging)
    Related PR: https://github.com/scikit-learn/scikit-learn/pull/16925
    New prototype in
Consider allowing users to pass custom loss functions, in particular for Histogram Gradient-Boosting (maybe without guarantees on backward compat).
    A common loss module is a first step: https://github.com/scikit-learn/scikit-learn/pull/20811 (under review)
More flexible support for alternative input data container types:

Evaluate interoperability with other types of arrays that are compatible with the numpy API for some preprocessing methods once the n_features_in_ PR is merged (by customizing the _validate_input method. Such arrays would potentially include implementations from ClPy, dpctl (or other interfaces to SYCL), JAX, dask any other library that provides computation and on-device memory allocation via a numpy compatible interface. Possibly by leveraging the newly introduced __array_function__ protocol (NEP 18) or array modules (NEP 37, see also NEP 35 which might be replaced by https://data-apis.org):
    https://github.com/scikit-learn/scikit-learn/pull/16112
    https://github.com/scikit-learn/scikit-learn/pull/17676
    https://github.com/scikit-learn/scikit-learn/pull/16574

 

Longer term: Big picture tasks which require more thinking

MLOps: Model auditing and data auditing

Improve UX via HTML repr for model with diagnostics with recorded fit-time warnings.
Model auditing tools that output HTML reprs.
Talk to various people to understand their needs and practices
Recommendation for statistical tests for distribution drift
Tool for Model cards generation / documentation / template

Consider whether survival analysis and training models on censored data should be tackled in scikit-learn:
    Organize a workshop to move understanding of the current ecosystem.

Document that this problem exists in the documentation
Maybe: an example to educate on the biases introduced by censoring and point to proper tools, such as the lifelines and scikit-survival projects.
Open a discussion on whether techniques such as survival forests / adapted loss functions for gradient boosting (need for sample props?) should be included in scikit-learn (see https://data.princeton.edu/wws509/notes/c7.pdf) or if they should be considered out of the scope of the project.
Survival analysis tools need to go beyond point wise predictions and this might  be more generally useful in scikit-learn, possibly uncertainty quantification in predictions.

Time series forecasting
    Organize a workshop to better understand the API and technical choices on different Python libraries to know which one we should illustrate and point to in examples and doc.
    Maybe basic time-series windowed feature engineering and cross-validation based model evaluation

 

Community: On the community side:

Continue regular technical sprints and topic focused workshops (possibly by inviting past sprint contributors to try to foster a long term relationship and hopefully recruit new maintainers).
    Better preparation for issues
    Plan with greater advance
    Fewer people in the sprints (to be able to provide better mentoring)
Make the consortium meetings more transparent:
    Invite Adrin and other advisory board people to the meetings
    Make the weekly tasking more visible
Renew the organization of beginners’ workshops for prospective contributors, probably before a sprint.
Organize a workshop on uncertainty quantification and calibration and possibly followed by 2 days of sprint
Organize a workshop on our software-engineering practices
Conduct a new edition of its 2013 survey among all scikit-learn users.
