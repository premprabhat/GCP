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

## Prerequisites
* An [installation of Terraform](https://www.terraform.io/cli/install/apt)
* A Google cloud account
* An [installation of Google Cloud CLI](https://cloud.google.com/sdk/docs/install-sdk#deb)

## Acquire user credentials
```
gcloud auth application-default login
```
Open the URL in the browser and copy the authentication code.

![image](https://user-images.githubusercontent.com/67620689/204243048-f413184a-6478-480c-825d-241c5607353e.PNG)

Now paste the authentication code as below:
![image](https://user-images.githubusercontent.com/67620689/204242841-58e30570-1f88-4755-b3d2-32d7052a9b5d.PNG)

## Generate key-pair(public key, private key) using ssh keygen
Before using Terraform, we need to first generate the key-pair(public key, private key) using ssh-keygen. Then we are going to associate both public and private keys with Arm VMs.

Generate the key pair using the following command:
```
  ssh-keygen -t rsa -b 2048
``` 

By default, the above command will generate the public as well as private key at location **~/.ssh**. But we can override the end destination with a custom path(Eg: **/home/ubuntu/gcp/** followed by key name **gcp_key**).

**Output when a key pair is generated:**

![image](https://user-images.githubusercontent.com/67620689/204243311-cb5bb41b-e0ec-489c-987d-f54522486797.PNG)

## Terraform infrastructure
Add resources required to create a VM in `main.tf`.

Add below code in `main.tf` file:

```
provider "google" {
  project = "project_id"
  region = "us-central1"
  zone = "us-central1-a"
}

resource "google_compute_project_metadata_item" "ssh-keys" {
  key   = "ssh-keys"
  value = file("public_key_location")
}

resource "google_compute_instance" "vm_instance" {
  name         = "vm_name"
  machine_type = "t2a-standard-1"

  boot_disk {
    initialize_params {
      image = "ubuntu-os-cloud/ubuntu-2004-lts-arm64"
    }
  }

  network_interface {
    network = "default"
    access_config {
      // Ephemeral public IP
    }
  }
}
```

## Terraform commands

### Initialize Terraform
Run `terraform init` to initialize the Terraform deployment. This command downloads all the modules required to manage your resources.

```
  terraform init
```

![image](https://user-images.githubusercontent.com/67620689/204243851-df54e9a3-8c9f-402d-9265-25cb035d14b1.PNG)

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
![image](https://user-images.githubusercontent.com/67620689/204243999-583c2187-b9d1-4b91-9349-fe48b8089d45.PNG)

### Verify created resource
In the Google Cloud console, go to the [VM instances page](https://console.cloud.google.com/compute/instances?_ga=2.159262650.1220602700.1668410849-523068185.1662463135). The VM we created through terraform must be displayed in the screen.

![image](https://user-images.githubusercontent.com/67620689/204244210-00741212-de05-49f9-b4eb-e4943b809c70.PNG)

Run following command to connect to VM through SSH:

```
  ssh -i "/home/ubuntu/azure/gcp_key" ubuntu@<Public IP>
```
![image](https://user-images.githubusercontent.com/67620689/204244402-b0eeb224-de92-45fe-90a7-e10d2408da99.PNG)

### Clean up resources

Run `terraform destroy` to delete all resources created.

```
  terraform destroy
```
![image](https://user-images.githubusercontent.com/42368140/196463306-1e559148-4b9a-414c-b862-06c6aa33557e.PNG)

[<-- Return to Learning Path](/content/en/cloud/azure/#sections)
