# Continuous Delivery with CML, Github Actions and Watson ML

To deploy or update the deployment without the need to manually do so by the Watson ML UI or by running a script, we use GItHub Actions to run a workflow everytime we make a new release and then deploy the model that way. 

## Making a Release

## Git Actions
The workflow downloads the data from dvc the uses credentials, to understand all the previous steps, refer to the [last](https://mlopsstudygroup.github.io/mlops-guide/CICD/cml_testing/#adding-train-and-evaluate-workflow) section where we explained step by step [what is CML](https://mlopsstudygroup.github.io/mlops-guide/CICD/cml_testing/#what-is-cml) and [how to implement workflows](https://mlopsstudygroup.github.io/mlops-guide/CICD/cml_testing/#testing-with-github-actions). The complete workflow can be found [here](https://github.com/MLOPsStudyGroup/dvc-gitactions/blob/master/.github/workflows/deploy_on_release.yaml).


```yaml 
run: |
    # Install requirements
    pip install -r requirements.txt
    # Pull data & run-cache from S3 and reproduce pipeline
    dvc pull --run-cache
    dvc repro
    # Decrypt credentials file
    gpg --quiet --batch --yes --decrypt --passphrase="$CRED_SECRET" --output credentials.yaml credentials.yaml.gpg
    # Check if there is a deployment already, if positive update it, otherwise deploys it for the first time
    ./src/scripts/Scripts/git_release_pipeline.sh 

```

***Installing requirements***
```
pip install -r requirements.txt
```

***Pull the versioned data and reproduce the full pipeline of training and evaluation***
```
dvc pull --run-cache
dvc repro
```

***Decrypting Credentials File***
```
    gpg --quiet --batch --yes --decrypt --passphrase="$CRED_SECRET" --output credentials.yaml credentials.yaml.gpg
```

***Run a script to Deploy or Update the Deployment***
```
./src/scripts/Scripts/git_release_pipeline.sh 
```
This is the following bash script:

    #!/bin/sh

    if  ! python3 ./src/scripts/Pipelines/git_release_pipeline.py ./
    then 
        echo "      Model already has been deployed, updating it"
        python3 ./src/scripts/Pipelines/model_update_pipeline.py ./models/model.joblib ./ ./credentials.yaml
        python3 ./src/scripts/Pipelines/model_update_deployment_pipeline.py ./ ./credentials.yaml
    else    
        echo "      Deploying model for the first time" 
        python3 ./src/scripts/Pipelines/model_deploy_pipeline.py ./models/model.joblib ./ ./credentials.yaml
    fi

First we check if there is a model UID in the ```metadata.yaml```, if positive, we consider the model has already been deployed, and the we run scripts to update the [model](https://github.com/MLOPsStudyGroup/dvc-gitactions/blob/master/src/scripts/Pipelines/model_update_pipeline.py) and the [deployment](https://github.com/MLOPsStudyGroup/dvc-gitactions/blob/master/src/scripts/Pipelines/model_update_deployment_pipeline.py), to find more details on how we use scripts to deploy the model on Watson Machine Learning you can go to [this](https://mlopsstudygroup.github.io/mlops-guide/Deployment/) page from our guide.