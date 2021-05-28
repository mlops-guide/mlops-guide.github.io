# Versioning

## Data and Model Versioning

The use of code versioning tools is vital in the software development industry. The possibility of replicating the same code base so that several people can work on the same project simultaneously is a great benefit. In addition, versioning these bases allows them to work in different sections in an organized manner and without compromising the integrity of the code in production.

As much as these tools solve several problems in software development, there are still issues in machine learning projects. Code versioning is still crucial, but when working on new experiments it's important to guarantee the same properties for data and models.

In a machine learning project, data scientists are continuously working on the development of new models. This process relies on trying different combinations of data, parameters, and algorithms. It's extremely positive to create an environment where it's possible to go back and forth on older or new experiments. 

![Versioning Diagram](project-versions.png "Versioning diagram")
[^]: Image from DVC Documentation. https://dvc.org/doc/use-cases/versioning-data-and-model-files

## Reproducibility

When discussing versioning, it's important to understand the term **reproducibility**. While versioning data, models, and code we are able to create a nice environment for data scientists to achieve the ultimate goal that is a good working model, there is a huge gap between this positive experiment to operationalize it. 

To guarantee that the experimentation of the data science team will become a model in the production for the project, it's important to make sure that key factors are documented and reusable. The following factors listed below were extracted from *"Introducing MLOps"* (Treveil and Dataiku Team 57) :

 - **Assumptions:** Data Scientist's decisions and assumptions must be explicit.
 - **Randomness:** Considering that some machine learning experiments contain pseudo-randomness, this needs to be in some kind of control so it can be reproduced. For example, using "seed".
 - **Data:** The same data of the experiment must be available.
 - **Settings:** Repeat and reproduce experiments with the same settings from the original.
 - **Implementation:** Especially with complex models, different implementations can have different results. This is important to keep in mind when debugging.
 - **Environment:** It's crucial to have the same runtime configurations among all data scientists.

## Popular Versioning Tools

Merging data, model, and code versioning isn't an easy task but fortunately, there are several tools being created or constantly updated to feel the need for data and model versioning. These tools can offer ways for transforming data with pipelines to configuring models.

We listed below some of these tools available for usage:

Tool      | License | Developer | Observations |
--------- | :-----: | :-------: | :----------- |
IBM Watson ML | Proprietary | IBM | Focused on model versioning |
DVC       | Open-source | Iterative | Popular lightweight open-source tool focused on data, model and pipeline versioning. Can be easily integrated with CML. |
Pachyderm | Open-source | Pachyderm | Data platform built on Docker and Kubernetes. Focused on Version Control and automating workloads with parallelization. |
MLflow    |  Open-source | Databricks | Popular tool for many parts of the Machine Learning lifecycle, including versioning of processes and experiments. Can be easily integrated with other tools |
Git LFS (Large File System) | Open-source | Atlassian, GitHub, and others |  Git extension that permits large files on Git repositories. Can be used to share large data files and models, using Git versioning. |


















