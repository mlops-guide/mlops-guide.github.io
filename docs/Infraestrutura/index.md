# Setting Up the IBM Enviroment with Terraform

## Introduction

Make a script to automate the infrastructure deployment is a very good practice as it enables a fast deployment in a disaster scenario, as well as an alternative plan to change providers as needed.

For this example, we will use [Terraform](https://www.terraform.io/) to deploy the needed IBM resources and a Python script to configure the deployment space inside the IBM Watson environment.

## Requirements

We need to [install the terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli) and the ibm module.

### IBM module

  To install IBM's module paste the following code into your terraform script then run ```terraform init```.
  
```
  terraform {
    required_providers {
      ibm = {
        source = "IBM-Cloud/ibm"
        version = "1.24.0"
      }
    }
  }

  provider "ibm" {
  #  Configuration options
  }
```

## Terraform

Terraform allow an easy and fast way to write a infrastructure code, as it is clean and simple syntax to use and read. It saves the current state and any changes made on the script will only make the changes needed, eg. change an instance name won't reset the instance itself or make any changes on other resources.

### Terraform script

```
#### MODULE

  terraform {
    required_providers {
      ibm = {
        source  = "IBM-Cloud/ibm"
        version = "~> 1.12.0"
      }
    }
  }


  provider "ibm" {}

  #### RESOURCE GROUP

  data "ibm_resource_group" "group" {
    name = "GROUP_NAME"
  }

  #### Watson machine learning resource
  resource "ibm_resource_instance" "wml" {
    name              = "WML_NAME"
    service           = "pm-20"
    plan              = "lite"
    location          = "us-south"
    resource_group_id = data.ibm_resource_group.group.id
    tags              = ["TEST", "TERRAFORM"]

  }

  #### IBM Cloud Object Storage

  resource "ibm_resource_instance" "cos" {
    name              = "COS_NAME"
    service           = "cloud-object-storage"
    plan              = "standard"
    location          = "global"
    resource_group_id = data.ibm_resource_group.group.id
    tags              = ["TERRAFORM", "TEST"]

  }

```

This script will create:

- Work group
- Watson machine learning resource
- IBM COS instance

### Authentication

  Before you continue, you will need to create an **API Key** and assign it to an environment variable called **IBMCLOUD_API_KEY**

### Terraform plan and apply
If we run the command ```terraform plan``` the following output shows all the changes that will be made, in this case, create all the resources.
```
Terraform will perform the following actions:

  # ibm_resource_instance.cos will be created
  + resource "ibm_resource_instance" "cos" {
      + crn                     = (known after apply)
      + dashboard_url           = (known after apply)
      + extensions              = (known after apply)
      + guid                    = (known after apply)
      + id                      = (known after apply)
      + location                = "global"
      + name                    = "TESTE_COS"
      + plan                    = "standard"
      + resource_controller_url = (known after apply)
      + resource_crn            = (known after apply)
      + resource_group_id       = "3f5502dbe92e42579cfdfec471f3ebd5"
      + resource_group_name     = (known after apply)
      + resource_name           = (known after apply)
      + resource_status         = (known after apply)
      + service                 = "cloud-object-storage"
      + status                  = (known after apply)
      + tags                    = [
          + "TERRAFORM",
          + "TEST",
        ]
    }

  # ibm_resource_instance.wml will be created
  + resource "ibm_resource_instance" "wml" {
      + crn                     = (known after apply)
      + dashboard_url           = (known after apply)
      + extensions              = (known after apply)
      + guid                    = (known after apply)
      + id                      = (known after apply)
      + location                = "us-south"
      + name                    = "TESTE_TERRAFORM"
      + plan                    = "lite"
      + resource_controller_url = (known after apply)
      + resource_crn            = (known after apply)
      + resource_group_id       = "3f5502dbe92e42579cfdfec471f3ebd5"
      + resource_group_name     = (known after apply)
      + resource_name           = (known after apply)
      + resource_status         = (known after apply)
      + service                 = "pm-20"
      + status                  = (known after apply)
      + tags                    = [
          + "TERRAFORM",
          + "TESTE",
        ]
    }

Plan: 2 to add, 0 to change, 0 to destroy.

```

If everything correct then run **```terraform apply```** to create the infrastructure.

## Script Python 

The current IBM's terraform module is in development and some features are still missing. So we develop a python script to integrate to IBM's DataPak to enable some important configurations, the project and the deployment space.

As both the terraform and python script will work together, the last will have the same proprities as terraform, it will save the state and only make changes as needed. Both will be executeed by a shell script.  

We are going to use the following libs.

```
import os
import json
from ibm_watson_machine_learning import APIClient

class InitializeInfra:
    def __init__(self):

        self.terraform_output = ".terraform/output.json"

        self.state_name = "infra_state.json"
        self.state_path = ".terraform"

        self.resources_destroyed = False  

        self.client = None

        self.cos_crn = None
        self.wml_crn = None
        self.wml_name = None
        self.logs = None

        self.space_id = None
        self.set_credentials()
        self.run()
```