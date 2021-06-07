# Project Workflow

In this section, we present a workflow example on how to create a new experiment in the project. This is also a step-by-step guide to the video presented on the home page.

!!! Note
        If you don't have a configured project already, go check out the [**Implementation Guide**](../Structure/project_structure/) first.


## Setup your environment

### Cloning your repository and installing dependencies

Let's start by cloning the project repository you will be working on. We need to make sure that we have installed all the dependencies necessary.

!!! tip
    It's a good practice to create a virtual environment for each project you work. You can do that using [venv](https://docs.python.org/3/library/venv.html) or [conda](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

<asciinema-player rows=18 theme="monokai" cols=200 src="../ASCII_cinema/workflow/part1.cast"></asciinema-player>
```
git clone https://github.com/mlops-guide/dvc-gitactions.git
cd dvc-gitactions
pip3 install -r requirements.txt
pre-commit install
```

### Updating the data and checking the project specifications

Your environment is ready! So now let's:

- Download the data
- Check the project pipelines
- Reproduce the main experiment(Optional)
- See current metrics

<asciinema-player rows=18 theme="monokai" cols=200 src="../ASCII_cinema/workflow/part2.cast"></asciinema-player>

```
dvc pull
dvc dag
dvc repro
dvc metrics show
```
!!! info
    If you are confused with this DVC's commands, check out at the Implementation Guide the [Versioning section](../Versionamento/index.md).

## Working on a New Update

Now that you are familiar with the project let's try a new experiment.

### Edit the necessary files

First, you should edit those files affected for your experiment. 

Here we are changing our model from a Logistic Regression classifier to a Random Forest at ```model.py```.


=== "model.py"
```python
pipe = Pipeline(
        [
            ("scaler", StandardScaler()),
-            ("LR", LogisticRegression(random_state=0, max_iter=num_estimators)),
+             (
+                 "RFC",
+                 RandomForestClassifier(
+                     criterion="gini",
+                     max_depth=10,
+                     max_features="auto",
+                     n_estimators=num_estimators,
+                 ),
+             ),
        ]
    )
```


!!! info
    If you forgot how this project is organized and what each file is responsible for, go check out [Tools and Project Structure](../Structure/project_structure.md).
    


### Create new branch and Reproduce pipelines

Second, let's create a new branch at our repository to version this new experiment. After that, we can reproduce the experiment and see how the new metrics compare to the current model metrics.

<asciinema-player rows=18 theme="monokai" cols=200 src="../ASCII_cinema/workflow/part3.cast"></asciinema-player>
```
git checkout -b RandomForestClassifier
dvc repro
dvc metrics diff
```

!!! note
    Observe that DVC avoided running the preprocess stage since our model change affected only the train and evaluate stages. To see more about DVC pipelines go check out [Working with pipelines](../Versionamento/pipelines_dvc.md).


Even though our experiment didn't improve our current model metrics, we will consider it good for production to demonstrate the rest of the workflow cycle.


### Test and Commit

Our experiment is ready, now let's:

- Format our code with black
- Upload to our branch in our Github repository


<asciinema-player rows=18 theme="monokai" cols=200 src="../ASCII_cinema/workflow/part4.cast"></asciinema-player>
```
black .
git add .
dvc push
git commit -m "Random Forest Experiment"
git push origin RandomForestClassifier
```

!!! info
    The tests and format checking which ran after the commit command were executed by [pre-commit](https://github.com/pre-commit/pre-commit)


## Experiment Deployment

### Pull request and automated report

After uploading to our branch, we can now create a Pull Request at the Github Website.

<video width="100%"  controls>
    <source src="../assets/workflow/create_pr.mp4" type="video/mp4">
</video>




!!! info
    If forgot or want to know more about how this automated report was generated, go check out the [Continuous Integration with CML and Github Actions](../CICD/cml_testing.md)

### Release and Watson Deployment

Supposing our experiment was merged to the main branch, we can consider it ready for deployment. To do so, let's release a new version of the project using Github Website.

<video width="100%"  controls>
    <source src="../assets/workflow/release_watson.mp4" type="video/mp4">
</video>

After releasing the new version, CML and Github Actions will trigger a script responsible for deploying our model to Watson ML.


<video width="100%"  controls>
    <source src="../assets/workflow/model_pred.mp4" type="video/mp4">
</video>

!!! info
    If forgot or want to know more how this deployment happens, go check out the [Continous Delivery with CML, Github Actions and Watson ML](../CICD/cml_deploy.md)

## Monitoring

Ending our workflow cycle, we can use IBM OpenScale tool to monitor the model in production.

<video width="100%"  controls>
    <source src="../assets/workflow/openscale.mp4" type="video/mp4">
</video>

There we can create monitors for Drift, Accuracy and Fairness. We can also explain the model's predictions, understanding which feature had more weight in the decision and also see what changes would need to be made for the outcome to change.

!!! info
    If forgot or want to know more how to monitor your model in production, go check out the [Monitoring with IBM OpenScale](../Openscale/index.md)




