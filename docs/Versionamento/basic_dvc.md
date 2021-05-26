# Data Versioning

So now that your repository is initialized and connected with your remote storage, we can start to version your data.

!!! warning
    This section will use the help of the [template repository](https://github.com/mlops-guide/dvc-gitactions) to show how to version data with DVC. Feel free to reproduce it with your own data files for your project.

## dvc add

Suppose you have downloaded our [_weatherAUS.csv_](https://www.kaggle.com/jsphyg/weather-dataset-rattle-package) or another file inside your data folder and you want to add this file under the data version control of your project. The first step is to put this file under DVC local control and DVC cache by running:

```bash
$ dvc add data/weatherAUS.csv
```

``` dvc add ``` works the same way ```git add``` command. Your dataset is now under DVC local control and DVC cache(which is by default local but can be configured to be shared). But now, you need to put your data under Git version control with ```git add```. Note that DVC created two files named ```weatherAUS.csv.dvc``` and ```.gitignore``` in your data folder. These files are responsible for *codifying* your data:

- weatherAUS.csv.dvc: This file points where your actual data is and every time that your data change, this file changes too.
- .gitignore: This file won't allow git to upload your data file to your repository. DVC creates automatically so you won't need to worry about it.

These metafiles with ```.dvc``` extension are YAML files that contain some key-value pair information about your data or model. Here it's an example:

```yaml
outs:
  - md5: a304afb96060aad90176268345e10355
    path: weatherAUS.csv
```

The *md5* is a very common hash function that takes a file content and produces a string of thirty-two characters. So if you make just a small change in a data file or model controlled by DVC, the md5 hash will be recalculated and that's how your colleagues will keep track of what's new in your experiment.

If  you are interested in all ```.dvc``` arguments, check out the [official docs](https://dvc.org/doc/user-guide/project-structure/dvc-files).

## dvc checkout

To see how DVC has your data under control, you can remove your ```weatherAUS.csv``` . After that, try running ```dvc checkout data/weatherAUS.csv.dvc``` and you will see that your dataset is back!

## dvc push

So now DVC and Git have your data under control. To finally allocated this data in your remote storage you need to run:

```bash
$ dvc push data/weatherAUS.csv
```

After that, your data is already in your remote storage and you just need to let Git know that by:

```bash
$ git commit -m 'data push'
```

## dvc pull

Finally,  you can checkout to your experiment branch and pull your dataset related to that experiment:

```bash
$ git checkout branch_name
```

And to pull the dataset from your remote storage, just run:

```bash
$ dvc pull
```

___