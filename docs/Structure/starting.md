# Starting a New Project with Cookiecutter


## What is Cookiecutter?
Cookiecutter is a CLI tool that can be used to create projects based on templates. It can create folder structures and static files based on user input info on predefined questions. In this guide cookiecutter will create the project structure based on the [MLOps Cookiecutter Template](https://github.com/mlops-guide/mlops-template)

## Installing Cookiecutter
Cookiecutter is officially supported on Linux, MacOS and Windows. More info on installing it can be accessed on their [documentation](https://cookiecutter.readthedocs.io/en/1.7.2/installation.html#install-cookiecutter)


### Python

```bash
pip install --user cookiecutter
```
or

```bash
pip3 install --user cookiecutter
```

### Alternative: Homebrew (MacOS only)
```bash
brew install cookiecutter
```

### Alternative: Debian/Ubuntu
```bash
sudo apt-get install cookiecutter
```

## Creating a New Project
Now that cookiecutter is configured we can use the template to create a structured new project

```bash
cookiecutter https://github.com/mlops-guide/mlops-template.git
```

This should result in the following questions, which will be used to fill the project with info
```bash
author [mlops-guide]:
project_name [Australia Weather Prediction]:
project_slug [australia_weather_prediction]:
project_version [v0.1]:
model_type [scikit-learn_0.23]:
Select open_source_license:
1 - MIT
2 - BSD-3-Clause
3 - No license file
Choose from 1, 2, 3 [1]:
use_github_actions_for_CICD [y]:
use_pytest [y]:
use_black [y]:
```

## Basic Structure
After running cookiecutter, the project tree should be as the following. You can check this by running ```$ tree``` on Linux, using Finder on MacOS or File System on Windows.
```
.
├── LICENSE
├── README.md
├── data
│   └── README.md
├── metadata.yaml
├── models
│   └── README.md
├── notebooks
│   └── README.md
├── requirements.txt
├── results
│   └── README.md
└── src
    ├── scripts
    │   └── README.md
    └── tests
        ├── README.md
        └── test_australia_weather_prediction.py

7 directories, 11 files
```





