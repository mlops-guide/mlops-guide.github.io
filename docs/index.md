# MLOps Guide

!!! warning
    **This guide is not completed yet**. Be careful before using it as some parts might be missing or still unfinished.

This site is intended to be a MLOps Guide to help projects and companies to build more reliable MLOps environment. This guide should contemplate the theory behind MLOps and an implementation that should fit for most use cases.

## What is MLOps?

MLOps is a methodology of operation that aims to facilitate the process of bringing an experimental Machine Learning model into production and maintaining it efficiently. MLOps focus on bringing the methodology of DevOps used in the software industry to the Machine Learning model lifecycle. In that way we can define some of the main features of a MLOPs project:

* Data and Model Versioning
* Feature Management and Storing
* Automation of Pipelines and Processes
* CI/CD for Machine Learning
* Continuos Monitoring of Models

## What is Contemplated on This Guide?

* Introduction to MLOps Concepts
* Tutorial for Building a MLOps Environment

## MLOps Environment
This video shows how an example of workflow with a complete MLOps project. This exact project can be found [here](https://github.com/MLOPsStudyGroup/dvc-gitactions) and is an example end-to-end made for this guide.
<script id="asciicast-410111" src="https://asciinema.org/a/410111.js" async></script>


## Contents

- __MLOps Theory:__ A brief introduction to the main Principles of MLOps: [Data and Model Versioning](../MLOps/Data/), [Feature Management and Storing](../MLOps/PipelineAutomation/), [Automation of Pipelines and Processes](../MLOps/FeatureStore/),[ CI/CD for Machine Learning](../MLOps/CICDML/) and [Continuos Monitoring of Models](../MLOps/Monitoring/). As well as Common Tools used to address each of those points.

- __Implementatios Guide__
     - __Introduction__
        - [__Tools and Project Structure:__](../Structure/project_structure/) Introduction to the project structure and tools that will be used.
        - [__Starting a New Project with Cookiecutter:__](../Structure/starting/) Introduction to  Cookiecutter and how to create a new project based on our template.
    - __Enviroment__
        - [__Setting Up the IBM Enviroment with Terraform:__](../infraestrutura/Terraform/) Using Terraform to set up the IBM Enviroment via code.
        - [__Managing the deployment space:__](../infraestrutura/Python/) How to manage IBM's tools with Terraform.

    - __Versioning__
        - [__What is DVC?:__](../Versionamento/) Introduction to DVC, installation and how to set-up remote storage.
        - [__Data Versioning:__](../Versionamento/basic_dvc/) Working with DVC to version data and models.
        - [__Working with Pipelines:__](../Versionamento/pipelines_dvc/) Creating and reproducing pipelines for training and evaluating models.

    - __Deployment__
        - [__Deployment with Watson Machine Learning:__](../Deployment) Using Watson ML API to deploy models as an online web service.

    - __CI/CD for Machine Learning__
        - [__Continuous Integration with CML and Github Actions:__](../CICD/cml_testing/) Introduction to CML and GitHub Actions and how to create automatic testing and reportng workflows.
        - [__Continous Delivery with CML, Github Actions and Watson ML:__](../CICD/cml_deploy/) Using CML and GitHub Actions create automatic deployments for every new release.

    - __Monitoring__
        - [__Monitoring with IBM Openscale:__](../Openscale) Setting up Openscale enviroment, creating monitors, evaluating the model and explaining predictions.

    - __Complete Cycle__
        - [__Complete Cycle:__](../Openscale): Demonstrating how to use the complete pipeline in a development cycle.



### Architecture
The following diagram shows the complete MLOps flow used on the tutorial. SInce the guide is modular, a team can chose to swap tools at any point due to project preferences and use cases.
<img src="./assets/DiagramMLOPs.png" alt="drawing" />


## Next

MLOps Theory

[📚 Learn More About MLOps Theory](/MLOps/Data/){ .md-button .md-button--primary }
!!! tip
    It is recommended that you learn about the theory before implementing MLOps into your project

Implementation Guide

[📃 Follow the Tutorial to Start a Project](/Structure/project_structure/){ .md-button .md-button--primary }

