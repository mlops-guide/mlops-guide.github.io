# Continuous Delivery with CML, Github Actions and Watson ML

To deploy or update the deployment without the need to manually do so by the Watson ML UI or by running a script, we use GItHub Actions to run a workflow everytime we make a new release and then deploy the model that way. The complete workflow can be found [here](https://github.com/MLOPsStudyGroup/dvc-gitactions/blob/master/.github/workflows/deploy_on_release.yaml).

The workflow downloads the data from dvc the uses credentials to run the following bash script:

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
