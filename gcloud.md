---
title: "Deploy Arm based VMs with gcloud"
type: docs
weight: 3
hide_summary: true
description: >
   Learn how to Deploy Arm based VMs with gcloud.
---

# How to deploy Arm based VMs with gcloud
This guide will show how to deploy a Arm based VM using gcloud.

## Pre-requisites
* An [installation of Google Cloud CLI](https://cloud.google.com/sdk/docs/install-sdk#deb)

## Deploy Arm based VM with gcloud command
```
  gcloud compute instances create VM_NAME --project=PROJECT_NAME --zone=ZONE --machine-type=MACHINE_TYPE --image-project=IMAGE_PROJECT [--image=IMAGE | --imagefamily=IMAGE_FAMILY] --network-interface=nic-type=GVNIC
```

## Generate key-pair(public key, private key) using ssh keygen

Generate the key pair using the following command:
```
  ssh-keygen -t  rsa -C <username>
``` 

## Add the public key in GUI
In VM instance page select your Project. In Metadata click on SSH keys and then add the data of file name.

If in your terminal, you are in the same directory where the Pem file is present. you will not need the full path for the pem file. Use following command to ssh an instance:
```
  ssh -i "demoserver.pem" ubuntu@<Public IP/DNS address>
```

[<-- Return to Learning Path](/content/en/cloud/azure/#sections)
