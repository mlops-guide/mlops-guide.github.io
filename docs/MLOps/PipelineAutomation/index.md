# Automation

## Why automate Machine Learning?

The automation of machine learning pipelines is highly correlated with the maturity of the project. There are several steps between developing a model and deploying it, and a good part of this process relies on experimentation.

Executing this workflow with less manual intervention as possible should result in:

 - Fast deployments
 - More reliable process
 - Easier problem discovering

## What can be automated?

In the table below, it's possible to find machine learning stages and what tasks may be automated:

ML Stages | Tasks
:---: | :---:
Data Engineering  | Data acquiring, validation, and processing
Model Development | Model training, evaluation, and testing
Continuous Integration | Build and testing
Continuous Delivery | Deployment new implementation of a model as a service
Monitoring | Setting alerts based on pre-defined metrics

## Levels of automation

Defining the level of automation has a crucial impact on the business behind the project. For example: data scientists spend a  good amount of time searching for good features, and this time can cost too much in resources which can impact directly the business return of investment(ROI).

[MLOps.org](https://ml-ops.org/content/mlops-principles#automation) describes a three-level of automation in machine learning projects:

1. **Manual Process:** Full experimentation pipeline executed manually using Rapid Application Development(RAD) tools, like Jupyter Notebooks. Deployments are also executed manually.
2. **Machine Learning automation:** Automation of the experimentation pipeline which includes data and model validation.
3. **CI/CD pipelines:** Automatically build, test and deploy of ML models and ML training pipeline components, providing a fast and reliable deployment.