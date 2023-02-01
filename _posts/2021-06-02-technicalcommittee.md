---
title: "Comité technique - Juin 2021"
date: June 2, 2021
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

# Agenda of the day

*9.10 am – 10 am* 
[Presentation of the technical achievements and ongoing work by O. Grisel](https://docs.google.com/presentation/d/1U4yE72-RmagrNAxsIvgfpQVvOu8T3PoJM9c1UnLVz98/edit)

*10 am – 12 pm*	
Feedback and exposition of each partner of the consortium

*1 pm – 2 pm*	
Collaborative drafting session of the updated roadmap

*2 pm – 5 pm*	
Afternoon discussions on Discord

# Attendees

**Scikit-learn @ Fondation Inria**	

Alexandre Gramfort (Inria, advisory committee of the consortium)

Olivier Grisel (consortium engineer)

Guillaume Lemaître (consortium engineer)

Jérémie Du Boisberranger (consortium engineer)

Chiara Marmo (consortium COO)

Loic Esteve (Inria engineer)

Mathis Batoul (Inria intern)

Julien Jerphanion (Inria engineer)

Gaël Varoquaux (Consortium Director)

**Consortium partners**	

*AXA:*
Thibault Laugel
Valeriy Ischenko
Xavier Renard

*Dataiku:*
Leo Dreyfus Schmidt
Samuel Ronsin

*Fujitsu:*
Norbert Preining

*Microsoft:*
Xavier Dupré

*BNP-Paribas-Cardiff:*
Sébastian Conort

**Scikit-learn community**

Adrin Jalali (community member of the advisory board, at Zalando)

Joel Nothman (community member of the advisory board, at university of Sydney)

# Priority list for the consortium at Inria, year 2021–2022

From the discussion during the technical committee, the scikit-learn Consortium at Inria defined the following list of priorities for the coming year:

- **High priority**: Continue effort helping with project maintenance to keep the target to release twice a year (+ bugfix releases).
    Toward scikit-learn 1.0
- Continue developments of the model inspection tools (and related features):
    - **High priority**: Help, review, and document the support for feature names in pipelines: https://github.com/scikit-learn/scikit-learn/pull/18444 (in progress)
    - **High priority**: Finalize the plotting helpers API. The new API allows:
        - Plotting by providing a fitted estimator, the data, and target. It requires computing the predicted target.
        - Plotting by providing the true and predicted targets. It avoids to unnecessary recompute the predicted target.
    - Extend PDP tools to support categorical variables: https://github.com/scikit-learn/scikit-learn/issues/14969, https://github.com/scikit-learn/scikit-learn/pull/18298 (in progress)
    - Document in User Guide and with examples to give intuitions, insights, caveats pros and cons of the different methods: comparison of model agnostic tools such as SHAP, permutation importances. https://github.com/scikit-learn/scikit-learn/pull/18139 (in progress)
    - Cross-reference more bleeding-edge inspection tools from external libraries such as SHAP, ELI5, InterpretML, from the documentation and examples in scikit-learn.
- **High priority**: Finalize the implementation of Impact Coding in https://github.com/scikit-learn/scikit-learn/pull/17323 and better document the pros and cons of each approach in examples.
- Improve documentation with extra examples and topic-based discussions:
    - Operationalization of models / MLOps:
        - Other packages to use
        - Good practices (we are an entry point of the community)
            - Version control
            - Automate (CI / CD)
            - Long blocks of code should be in Python scripts, not in notebooks
        - Declarative construction of pipelines
        - Add side boxes in our doc / examples on the production logic (it may differ from exploration mode)
    - Time series feature engineering and cross-validation based model evaluation
    - Annotation regarding models’ hyperparameter: https://github.com/scikit-learn/scikit-learn/pull/17929
- Improving performance and scalability
    - Comparative benchmark to identify how scikit-learn’s most popular models perform compared to other libraries (scikit-learn-intelex, XGBoost, lightgbm, ONNXRuntime, etc.): https://github.com/mbatoul/sklearn_benchmarks
    - Continue work on making scikit-learn estimators more scalable w.r.t. number of CPUs, in particular with Cython optimization for the pairwise distances together with argmin/argpartition pattern used for KNearestNeighbors and many scikit-learn estimators
    - Experiments are being made in a separate repository: https://github.com/scikit-learn-inria-fondation/pdist_aggregation
- Improve support for quantification of uncertainties in predictions and calibration measures
    - **High priority**: Expected Calibration Error for classification: https://github.com/scikit-learn/scikit-learn/pull/11096
    - **High priority**: QuantileRegression for HistGradientBoostingRegressor
    - Improve QuantileRegression for Linear Models (sparse support, alternative penalty and related solver): https://github.com/scikit-learn/scikit-learn/pull/9978
    - Calibration reporting tool and documentation for quantile regression models (quantile loss in sklearn.metrics, average quantile coverage)
    - Isotonic Calibration should not impact rank-based metrics: https://github.com/scikit-learn/scikit-learn/issues/16321
- Compliance tests for ONNX exports from https://github.com/onnx/sklearn-onnx
- Improve the default solver in linear models:
    - **High priority** Deprecating and fixing bugs related to inconsistently-used “normalize” option and instead point the users to the preprocessing module (StandardScaler, MinMaxScaler, etc.): https://github.com/scikit-learn/scikit-learn/pull/17743 (under progress)
    - Investigate the possibility to automatically use a data-derived preconditioner for the solver in particular for smooth solvers in LogisticRegression / PoissonRegression, etc:
    https://github.com/scikit-learn/scikit-learn/pull/15583
    - Re-evaluate the choice of the default value of max_iter if necessary.
- Monotonicity constraints for Decision Trees https://github.com/scikit-learn/scikit-learn/pull/13649
- Implementation of Balanced Random Forest and Balanced Bagging Classifier: https://github.com/scikit-learn/scikit-learn/pull/13227. This is in progress, currently solving lower level issues with the scorer API. Still some internal refactoring is needed: https://github.com/scikit-learn/scikit-learn/pull/18589. To move forward we need to compare with a classifier that can change its decision threshold: https://github.com/scikit-learn/scikit-learn/pull/16525. Once done, it should be reconsidered if we need additional tools such as a generic resample API: SLEP005.
- Quantification of fairness issues and potential mitigation
    - Document fairness assessment metrics
    - Example to raise awareness on the issues of fairness, possibly adapted from https://github.com/TwsThomas/fairness/blob/master/fairness%20adult_sgd.ipynb
    - Work on sample props (SLEP 0006) as a prerequisite to go further.
- Interaction constraints for Histogram Gradient-Boosting: https://github.com/scikit-learn/scikit-learn/issues/19148
- Tutorial / guide on various strategies to assess certainty in predictions: impact of the choice of the loss function (e.g. mse, pinball, poisson), stability to resampling (e.g. using the Bagging meta-estimators), Gaussian process regression with covariance estimation and points to other external resources (e.g. conformal predictions, explicit bayesian posterior modeling…)
- Programmatically defining good value / starting points for hyperparameter grids:
    - https://github.com/scikit-learn/scikit-learn/issues/5004
    - https://github.com/scikit-learn/scikit-learn/pull/5564
    - https://github.com/scikit-learn/scikit-learn/pull/18821
    - https://github.com/scikit-learn/scikit-learn/pull/16898
    - https://github.com/scikit-learn/scikit-learn/pull/16287
- Callback and logging (interruption) monitor and checkpoint fitting loop. One application would be to better integrate with (internal and external) hyper-parameter search strategies that can leverage checkpointing to make model selection more resource efficient.
    - This can be important for MLOps (monitoring, snapshotting model for inspection, etc.)
    - It can help us gain agility during development (write better tests for the impact of convergence criteria, easier convergence debugging)
    - Related PR: https://github.com/scikit-learn/scikit-learn/pull/16925
- Consider allowing users to pass custom loss functions, in particular for Histogram Gradient-Boosting (maybe without guarantees on backward compat).
    - A common loss module is a first step: https://github.com/scikit-learn/scikit-learn/pull/19088
- Consider whether survival analysis should be tackled in scikit-learn:
    - An example to educate on the biases introduced by censoring and point to proper tools, such as the lifeline or the scikit-survival projects.
    - Open a discussion on whether techniques such as survival forests should be included in scikit-learn or if they should be considered out of the scope of the project.
- More flexible support for alternative input data container types:
    - Evaluate interoperability with other types of arrays that are compatible with the numpy API for some preprocessing methods once the n_features_in_ PR is merged (by customizing the _validate_input method. Such arrays would potentially include implementations from ClPy, dpctl (or other interfaces to SYCL), JAX, dask any other library that provides computation and on-device memory allocation via a numpy compatible interface. - Possibly by leveraging the newly introduced __array_function__ protocol (NEP 18) or array modules (NEP 37, see also NEP 35 which might be replaced by https://data-apis.org):
        - https://github.com/scikit-learn/scikit-learn/pull/16112
        - https://github.com/scikit-learn/scikit-learn/pull/17676
        - https://github.com/scikit-learn/scikit-learn/pull/16574

On the community side:

- Organise more regular technical sprints (possibly by inviting past sprint contributors to try to foster a long term relationship and hopefully recruit new maintainers).
- Better preparation for issues
- Plan with greater advance
- Renew the organization of beginners’ workshops for prospective contributors, probably before a sprint.
- Organize a workshop on uncertainty quantification and calibration and possibly followed by 2 days of sprint.
- Conduct a new edition of its 2013 survey among all scikit-learn users.Quantification of fairness issues and potential de-biasing

# Detailed minutes of the meeting

**Exposure of partners’ comments and priorities**

*Fujitsu:*
- Feedback on the Fujitsu Sprint.
- Beginner introduction to scikit-learn development in Japanese
- Resources from the MOOC and scikit-learn doc translated to Japanese
- 25 participants
- 11 PRs / 3 merged
- 9 gave feedback
- Planning recurrent sprint twice a year
- Planning to enlarge the audience to Korea and China. Potential interest of other partners in the same time zone (Valeriy Ischenko from AXA, is located in Singapore)
- Also targeting returning people
- Want to contribute to the Open Source ecosystem and make Japanese companies more Open Source aware. Language is a barrier to wider adoption.
- Possible todo: identify a “quick start” to help onboarding, which could then be translated to a local language
- No specific technical priority since the priority is to get Fujitsu use and contribute to OSS

*AXA:*
- AXA is interested in the general improvements as well as in the recent MOOC
- Focus on interpretability in particular when dealing with regulators
- What user expects and trying to figure out guidelines on usage and limitations of such methods
- Problem with education on naive usage of interpretability methods
- Further improvements on documentation and MOOC chapter on interpretability could help
- End of June: exercise to implement interpretability tools for a credit scoring use case
- Interested in following general scikit-learn developments (features, performance, documentation)
- Data Scientists at AXA are asked to study the interpretability of their models. They mostly are relying on SHAPE and LIME as those projects are well known and accessible.
- Also fairness:
- Research at AXA on fairness assessment, mentioned problem with lack of access on protected attributes in Europe (but laws is evolving)
- Integration with e.g. http://fairlearn.org would benefit from progress on SLEP006
- Proposal (from Adrin) to add interpretability tools in scikit-learn-extra,
- Adrin also mention the new European proposal https://digital-strategy.ec.europa.eu/en/library/proposal-regulation-laying-down-harmonised-rules-artificial-intelligence
- AXA is also available in helping in sprint for Far and Middle East

*BNP-Paribas-Cardiff:*

- Interested in following the development synergies among consortium / community members
- Interested in the MOOC
- Focus on management of operational risk (related to documentation and data scientist education)
- Better crafting pipelines : building / versioning / diff of the pipelines
- Question: yaml to configure pipelines, with a goal of standardizing (only a subset of features)
    - Easier to diff / read, automate documentation of pipelines
    - Streamline the pipeline auditing process using a web interface for instance
- Suggestion:
    - function to persist back and forth only a subset of pipelines
        - Both reading and writing is important
        - Toml or yaml?
    - Section in docs to operationalize / good practice to build a pipeline
- People write hairy code to build such of this pipeline
- Declarative pipelines could help integration with CI/CD
    - Something as simple as listing the classes and their arguments
    - Similar to https://github.com/mbatoul/sklearn_benchmarks/blob/master/config.yml
- Need is compensated via custom configuration based pipeline deployments
- TODO: Could be answered by better documentation on the deployment / operations and good practices
- MLFlow was mentioned as a candidate for such use cases

- Development related to feature names / data frame / interpretability is important
- Interested in improvements of the documentation for MLOps good practices
- Interested in uncertainty estimation (QuantileRegression) and calibration
- Todo: organise workshop on uncertainty and calibration

*Microsoft:*

- Interested on performance benchmarking effort in particular when comparing to ONNXRuntime
- ONNX dev update:
- CPU optims
- WASM deployment target
- ARM deployment target
- Debate on feature vs size of ONNXRuntime project (and runtime binaries?)
- Ability to make it custom models
- Extensions for text
- Support for sparse features becoming more important for customers
- Make it possible to export FunctionTransformer with custom python code using the numpy API and a decorator
- Known issues with sklearn to ONNX conversions:
- Float vs double in decision trees can cause rare but problematic discrepancies in the decision function
- Should sklearn try to use floats as much as possible in tree-based models?
- TODO? Better cleaner homogeneous support
- Mention ONNX in documentation about operationalizing pipelines
- Seek feedback from users.

*Dataiku:*

- Focus on MLOps problems (version machine learning experiments, compare models, pre-deployment model update checks)
- Detecting distribution drift (research topic) + correcting drift
- Model inspection and model failure detection: machine learning diagnostics (leakage, overfitting, parameter choice, compare to dummy classifier performance, class imbalance vs choice of metrics)
- Stress test center: test the model with perturbations / missing values, change of feature scales: detect and notify or assert robustness
- Model document generator: summarize modeling choices
- Uncertainty estimation / calibration
- Education issue : calibrated classifier does not always work, you have to check
- Conformal Predictions / quantile regression coverage (still new / research)
AutoML effort:
- Recent blog post: “Distributed Hyperparameter Search: How It’s Done in Dataiku”
- Happy with inclusion of Successive Halving
- Benchmarks with multiple datasets / hyper params: interested in joining efforts / contributing (potential TODO)
- Interests in programmatically defining good starting points for HP grids (potential TODO)
    - Ran experiments to rank which HP should be tuned first
    - Non stupid behaviors
    - Dataiku changed defaults HPs based on the results of their experiments
- Moving away from single Python version deployments
- Some customers won’t upgrade to Python 3 and are stuck to Python 2
- Interested in feature engineering:
    - Spline features in particular
- Interested in categorical features in scikit-learn-contrib / impact coding
- Similar issues for single vs double precision issue when deploying exported trees (no action wanted)
- TODO: Interested in moving forward the PR on monotonicity constraints for decision trees
