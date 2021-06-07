# Setting Up the IBM Environment with Terraform

## Introduction

Infrastructure as a code(IaC) is a process of managing and provisioning mechanisms for authenticating, planning and implementing servers and data centers in the cloud and private network without the need to manually configure everything through the provider's UI. IaC is a good practice that has been gaining attention since the DevOps culture started to grow and it is one of many processes that MLOps shares with it. For example, it makes easier to fast redeploy the infrastructure in a disaster scenario or an alternative plan to quickly change providers.

For this example, we will use [Terraform](https://www.terraform.io/) to deploy the needed IBM resources and a Python script to manage the deployment space inside the IBM Watson environment.


## What is Terraform?

Terraform is an open-source infrastructure as a code software tool created by HashiCorp. It enables users to define and provision an infrastructure in a high-level configuration language. It saves the current state and any changes made on the script will only make the changes needed, e.g.,  change an instance name won't reset the instance itself or make any changes on other resources.

## Requirements

We need to [install the terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli) and the IBM's module.


## Terraform Script

```
  # IBM's module.
  # It will download the module when run "terraform init"

  terraform {
    required_providers {
      ibm = {
        source  = "IBM-Cloud/ibm"
        version = "~> 1.12.0"
      }
    }
  }


  provider "ibm" {}

  
  # This will create a resource group
  # It separates the resources used inside IBM's cloud

  data "ibm_resource_group" "group" {
    name = "GROUP_NAME"
  }

 
 # This part will deploy a Watson machine learning resource
  
  resource "ibm_resource_instance" "wml" {
    name              = "WML_NAME"
    service           = "pm-20"
    plan              = "lite"
    location          = "us-south"
    resource_group_id = data.ibm_resource_group.group.id
    tags              = ["TEST", "TERRAFORM"]

  }

 
 # This deploys a IBM Cloud Object Storage resource
  
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
- Watson Machine Learning resource
- IBM COS instance

To install IBM's module run ```terraform init```.

## Authentication

  Before you continue, you will need to create an **API Key** and assign it to an environment variable called **IBMCLOUD_API_KEY**.

## Terraform Plan and Apply
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

If everything is correct, then run **```terraform apply```** to create the infrastructure.

## Outputs

After everything is set, terraform create a state file with the resources metadata. This file can be quite overwhelming  to work with and we will need some of this metadata. That's the use for an output file, when we run ```terraform apply```, it detects this file and create a section on the state file with the information needed and it is easier to work with.

The following script is an output file with the metadata that will be used on the next steps.

```
output "cos_crn" {
  value = ibm_resource_instance.cos.crn
}

output "wml_name" {
  value = ibm_resource_instance.wml.name
}

output "wml_crn" {
  value = ibm_resource_instance.wml
}

```

It is very straightforward and easy to read and understand. Note that, it is possible to create this file after everything is deployed and run a ```terraform apply``` again without worrying  about the instances.

