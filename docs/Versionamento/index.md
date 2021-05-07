# What is DVC?

In the [MLOps Concepts]('MLOps/Data/index.md') section we were able to show how versioning data, models, and pipelines are so important in an ML project. In this section, we will cover DVC and how you can set up this tool to version your data, models and automatize pipelines.

DVC, which goes by Data Version Control, is essentially an experiment management tool for ML projects. DVC software is built upon [Git](https://git-scm.com) and its main goal is to *codify* data, models and pipelines through the command line.

This is all possible because DVC replaces large files (such as datasets and ML models) with small metafiles that point to the original data. By doing that, it's possible to keep these metafiles along with the source code of the project in a repository while large files are kept in at a remote data storage.

### COLOCAR IMAGEM EXPLICATIVA

Since DVC works on top of Git, its syntax and workflow are also similar so that you're able to treat data and model versioning just as you do with code. Although DVC can work stand-alone, it's highly recommended to work alongside Git.

DVC can also manage the project's pipelines to make experiments reproducible for all members. These pipelines are lightweight and are created using dependency graphs. 

It's important to note that DVC is free, open-source and platform agnostic, so it runs with a diverse option of OS, programming languages and ML libraries.


- [DVC Setup](./setup_dvc.md)

- [DVC Basics](./basic_dvc.md)

- [DVC Pipelines](./pipelines_dvc.md) 
