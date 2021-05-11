# DVC Setup

## Install

DVC can be installed as a Python Library with pip package manager:

```
$ pip install dvc
```

Depending on which remote storage interface you're using, it's important to install optional dependencies(s3, azure, gdrive, gs, oss, ssh or all). In this  project  we are using S3 interface to connect with IBM Cloud Object Storage.

```
$ pip install "dvc[s3]"
```

It's also possible to install DVC using conda:

```
$ conda install -c conda-forge mamba
$ mamba install -c conda-forge dvc
$ mamba install -c conda-forge dvc-s3
```

## Project Setup

### Initialization

After installing DVC, you can go to your project's folder and initialize both Git and DVC:

```
$ git init
$ dvc init
```

If you run ```git status```, you can check DVC's initial config files that were created.

ascii_cinema do git status

### Remote Storage

Finally, we are going to setup our remote storage to keep our datasets and models. 

With ```dvc remote add``` you can point to your remote storage.
```
$ dvc remote add -d remote-storage s3://bucket_namme/folder/
```

DVC uses by default AWS CLI to authenticate. IBM Cloud Object Storage works with S3 interface and also AWS, the only additional step here is to configure the IBM endpoint and configure the credentials.

```
$ dvc remote modify remote-storage endpointurl \
                    https://s3.us-south.cloud-object-storage.appdomain.cloud
```

To configure the credentials you can use default ```~/.aws/credentials``` file and just add a new profile, or point to a new credential path:


Run:
```
$ dvc remote modify myremote profile myprofile
```
Modify ```~/.aws/credentials```
```
[default]
aws_access_key_id = ************
aws_secret_access_key = ************

[myprofile]
aws_access_key_id = ************
aws_secret_access_key = ************

```

or

```
$ dvc remote modify credentialpath /path/to/creds
```

If you have any problems trying to configure your remote storage, go check [DVC docs for remote command](https://dvc.org/doc/command-reference/remote/)

___

