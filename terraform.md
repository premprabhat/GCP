---
title: "Deploy Arm based VMs with Terraform"
type: docs
weight: 3
hide_summary: true
description: >
   Learn how to Deploy Arm based VMs with Terraform.
---

# How to deploy Arm based VMs with Terraform
This guide will show how to deploy a Arm based VM using Terraform.

## Pre-requisites
* An [installation of Terraform](https://www.terraform.io/cli/install/apt)
* An [installation of Google Cloud CLI](https://cloud.google.com/sdk/docs/install-sdk#deb)

## Creating a service account
In the Google Cloud console, go to the [Create service account](https://console.cloud.google.com/projectselector/iam-admin/serviceaccounts/create?_ga=2.68028177.1220602700.1668410849-523068185.1662463135) page.

Select a project. 

Enter a service account name to display and description in the Google Cloud console.

If you do not want to set access controls now, click Done to finish creating the service account. To set access controls now, click Create and continue.


The installation of Terraform on your Desktop/Laptop needs to communicate with Azure. Thus, Terraform needs to be authenticated.

For authentication, we need to run `az login` which provides code to run in browser.

![image](https://user-images.githubusercontent.com/42368140/196459799-6278da9d-e91c-4dc1-b8c3-c327dfa0394b.png)

Run in browser as below:
![image](https://user-images.githubusercontent.com/42368140/196459871-9a3e1c1e-0582-4d55-838a-03e397d68ed7.png)

You will see details in command line as below after logging in browser
![image](https://user-images.githubusercontent.com/42368140/197953418-ddb9cd41-72b9-4a97-88f1-1f490644f36b.PNG)

## Generate key-pair(public key, private key) using ssh keygen

### Generate the public key and private key
Before using Terraform, we need to first generate the key-pair(public key, private key) using ssh-keygen. Then we are going to associate both public and private keys with Arm VMs.

Generate the key pair using the following command:
```
  ssh-keygen -t rsa -C <username>
``` 

By default, the above command will generate the public as well as private key at location **~/.ssh**. But we can override the end destination with a custom path(Eg: **/home/ubuntu/azure/** followed by key name **azure_key**).

**Output when a key pair is generated:**

![image](https://user-images.githubusercontent.com/42368140/196460197-587b96b5-f108-432b-85d6-9cf9976d26a1.PNG)

**Note:** We have to use public key **azure_key.pub** inside the Terraform file to provision/start the Arm VMs and private key **azure_key** to connect to VM.

## Terraform infrastructure
Start by creating an empty `main.tf` file.

### Create required resources

Add resources required to create a VM in `main.tf`.

Add below code in `main.tf` file:

```
  resource "google_service_account" "default" {
  project = "snappy-byway-368307"
  account_id   = "service_account_id"
  display_name = "odidev"
  }

  resource "google_compute_instance" "default" {
    project = "snappy-byway-368307"
    name         = "instance-arm"
    machine_type = "t2a-standard-1"
    zone         = "us-central1-a"

    boot_disk {
      initialize_params {
      image = "ubuntu-os-cloud/ubuntu-2004-lts-arm64"
      }
    }
    network_interface {
      network = "default"
	   nic_type {
        GVNIC
      }
    }
  }
```

## Terraform commands

### Initialize Terraform

```
  terraform init
```

![image](https://user-images.githubusercontent.com/42368140/196460749-f9d7ea1e-fc69-4ba6-887c-da488053ef91.PNG)

### Create a Terraform execution plan

Run `terraform plan` to create an execution plan.
```
  terraform plan -out main.tfplan
```
**Key points:**

* The **terraform plan** command is optional. We can directly run **terraform apply** command. But it is always better to check the resources about to be created.
* The terraform plan command creates an execution plan, but doesn't execute it. Instead, it determines what actions are necessary to create the configuration specified in your configuration files. This pattern allows you to verify whether the execution plan matches your expectations before making any changes to actual resources.
* The optional -out parameter allows you to specify an output file for the plan. Using the -out parameter ensures that the plan you reviewed is exactly what is applied.

### Apply a Terraform execution plan

Run `terraform apply` to apply the execution plan to your cloud infrastructure. Below command creates all required infrastructure.
```
  terraform apply main.tfplan
```
![image](https://user-images.githubusercontent.com/42368140/196460956-609770ff-c263-4dd6-b8ad-03c740ec42cf.PNG)

### Verify created resource
In the Google Cloud console, go to the [VM instances page](https://console.cloud.google.com/compute/instances?_ga=2.159262650.1220602700.1668410849-523068185.1662463135). The VM we created through terraform must be displayed in the screen.

![image](https://user-images.githubusercontent.com/42368140/196461182-bde106db-1def-4270-be53-df97b87be21b.PNG)

### Use private key to SSH into Azure VM
Connect to VM using the private key(/home/ubuntu/azure/azure_key) created through `ssh-keygen`.

Go to VM instance page. In Metadata click on SSH keys and then add the data of file name.

![image](https://user-images.githubusercontent.com/42368140/196461435-bf928a89-4c3f-453b-8d20-91c384e6552f.PNG)

Run following command to connect to VM through SSH:

```
  ssh -i "/home/ubuntu/azure/azure_key" azureuser@<Public IP>
```

![image](https://user-images.githubusercontent.com/42368140/196461586-4e2a93ba-2379-4d7c-b737-b0918eaa54da.PNG)

### Clean up resources

Run `terraform destroy` to delete all resources created.

```
  terraform destroy
```
![image](https://user-images.githubusercontent.com/42368140/196463306-1e559148-4b9a-414c-b862-06c6aa33557e.PNG)

[<-- Return to Learning Path](/content/en/cloud/azure/#sections)
