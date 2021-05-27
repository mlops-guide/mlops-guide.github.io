# Managing the deployment space

The current IBM's terraform module is in development and some features are still missing. So we will use a Python script to manage the IBM Watson's deployment space, including create, delete and get the *space_id*, which will be very important as it is used in other scripts.

### Authentication

The following code is used to authenticate to the provider, note that it uses the same environment variable as terraform.
```
import os
import sys
from pprint import pprint
import json
from ibm_watson_machine_learning import APIClient

TERRAFORM_OUTPUT = '.terraform/terraform.tfstate'

def authentication():

    if os.getenv("IBMCLOUD_API_KEY"):

        wml_credentials = {
            "url": "https://us-south.ml.cloud.ibm.com",
            "apikey": os.environ.get("IBMCLOUD_API_KEY"),
        }
        client = APIClient(wml_credentials)  # Connect to IBM cloud

        return client

    raise Exception("API_KEY environment variable not defined")
```

### Terraform output

To create a deployment space, we need to get some metadata from the resources created by the terraform script. Here we use the output defined on terraform.

```
def terraform_output(terraform_path=TERRAFORM_OUTPUT):

    output = dict(json.load(open(terraform_path)))['outputs']
    
    cos_crn = output["cos_crn"]["value"]
    wml_crn = output["wml_crn"]["value"]["crn"]
    wml_name = output["wml_crn"]["value"]["resource_name"]
    
    state = {
        "cos_crn" : cos_crn,
        "wml_name": wml_name,
        "wml_crn" : wml_crn
    }
    return state
```

### Creating a space

Now with the metadata in hands, we can finally create a deployment space.

```
def create_deployment_space(
    client, cos_crn, wml_name, wml_crn, space_name="default", description=""
):

    ## Project info
    metadata = {
        client.spaces.ConfigurationMetaNames.NAME: space_name,  
        client.spaces.ConfigurationMetaNames.DESCRIPTION: description,
        client.spaces.ConfigurationMetaNames.STORAGE: {
            "type": "bmcos_object_storage",
            "resource_crn": cos_crn,
        },

        ## Project compute instance (WML)
        client.spaces.ConfigurationMetaNames.COMPUTE: { 
            "name": wml_name,
            "crn": wml_crn,
        },
    }

    space_details = client.spaces.store(meta_props=metadata)  # Create a space
    return space_details
```

### Get the space_id

With the space created, now we have the *space_id*, the following function is used to retrieve it. This info will be used on other scripts that will be shown on next pages.

```
def update_deployment_space(client, new_name, space_id):

    metadata = {client.spaces.ConfigurationMetaNames.NAME: new_name}

    space_details = client.spaces.update(space_id, changes=metadata)
    return space_details
```


!!! note

    To see the complete script [click here](https://github.com/mlops-guide/dvc-gitactions/blob/master/.infra/datapak_manage.py)