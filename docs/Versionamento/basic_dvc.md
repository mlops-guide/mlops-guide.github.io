# Data Versioning

So now that your repository is initialized and connected with your remote storage, we can start to version your data.

## dvc add

Suppose you have a dataset.csv file inside your data folder and you want to add this file under the data version control of your project. The first step is to put this file under DVC local control and DVC cache by running:

```
$ dvc add data/dataset.csv
```
### COLOCAR IMAGEM DO PROCESSO

``` dvc add ``` works the same way ```git add``` command. Your dataset is now under DVC local control and DVC cache(which is by default local but can be configured to be shared). But now, you need to put your data under Git version control with ```git add```. Note that DVC created two files named ```dataset.csv.dvc``` and ```.gitignore``` in your data folder. These files are responsible for *codifying* your data:

- dataset.csv.dvc: This file points where your actual data is and every time that your data change, this file changes too.
- .gitignore: This file won't allow git to upload your data file to your repository. DVC creates automatically so you won't need to worry about it.

## dvc checkout

To see how DVC has your data under control, you can remove your ```dataset.csv``` and try running ```dvc checkout data/dataset.csv.dvc```.
### COLOCAR ASCII CINEMA AQUI

## dvc push

So now DVC and Git have your data under control. To finally allocated this data in your remote storage you need to run:

```
$ dvc push data/dataset.csv
```

After that, your data is already in your remote storage and you just need to let Git know that by:

```
$ git commit -m 'data push'
```

It's recommended that you *tag* your commits every time you change your data before pushing it so that is easier to track it:

```
$ git tag default_dataset
$ git push
```

## dvc pull

Finally, every time you or anybody needs to work with that dataset, you can checkout that tag on git:

```
$ git checkout deafult_dataset
```

And to pull the dataset from your remote storage, just run:

```
dvc pull
```

___

[*NEXT:* Working with Pipelines](./pipelines_dvc.md)