---
title: "Deploy Arm based VMs using GUI"
type: docs
weight: 2
hide_summary: true
description: >
    Learn how to deploy Arm based VMs using GUI.
---

# How to deploy Arm based VMs via GUI
Log in to your google account and in the Google Cloud console, go to the [VM instances page](https://console.cloud.google.com/compute/instances?_ga=2.159262650.1220602700.1668410849-523068185.1662463135).

![h3](https://user-images.githubusercontent.com/67620689/202090364-2946214c-2347-4538-b2b0-3a36f45caee0.PNG)
   
## Select your project
    
![image](https://user-images.githubusercontent.com/67620689/202095985-103deaa4-610d-45ea-a84c-65af2bbfec41.PNG)

## Click on Create instance
    
![h3](https://user-images.githubusercontent.com/67620689/202090934-aa0aa2da-e0f7-4aea-b8db-bc4988b781b2.PNG)

## Specify a Name for your VM. For more information, see [Resource naming convention](https://cloud.google.com/compute/docs/naming-resources#resource-name-format).

![image4](https://user-images.githubusercontent.com/67620689/202098830-532b5dc8-f6b5-4cff-931c-ec41edd08516.PNG)

## Choose a Zone for this VM that supports Tau T2A
This series is available only in select regions and zones. More information on regions and zones at which it is available can be found [here](https://cloud.google.com/compute/docs/regions-zones#available).
   
![image1](https://user-images.githubusercontent.com/67620689/202097168-6208b6ae-3627-47b3-a397-7783769e6727.PNG)

## Select General-purpose from the Machine family options.
Select T2A from the Series drop-down menu and a T2A Machine type from the drop-down menu.
   
![image](https://user-images.githubusercontent.com/67620689/203740482-d820ced1-5eeb-4c07-99a3-18a7a7511966.PNG)

## In the Boot disk section, click Change, and then do the following:
On the Public images tab, choose the following:
 * The default Debian-11-Arm64 image or any other supported Arm OS. Here we have chosen ubuntu 20.04 version.
 * Boot disk type
 * Boot disk size

Then click on select.

![c3](https://user-images.githubusercontent.com/67620689/203740594-e118f88a-52da-41f2-b5bd-4a3d35990536.PNG)

## To create and start the VM, click Create.
   
![image3](https://user-images.githubusercontent.com/67620689/202098038-7bfb0b6c-af18-4d5c-92a8-ca90a57bc25b.PNG)

## Generate key-pair(public key, private key) using ssh keygen
Generate the key pair using the following command:

```
ssh-keygen -t rsa -b 2048
```
![image](https://user-images.githubusercontent.com/67620689/203761628-d7f4ade8-a132-4af9-b012-778c82b6d94d.PNG)

## Add the public key into your VM.
In VM instance page select your Project. In Metadata click on SSH keys and then add the public key.
![image](https://user-images.githubusercontent.com/67620689/203762984-abfcecf4-87d6-4a06-b546-ad955dee4bc6.PNG)
   
## SSH into the launched instance
If in the terminal, you are in the same directory where the private file is present. you will not need the full path for the private file. Use following command to ssh an instance:
   
```
ssh -i "private_key" ubuntu@<Public IP/DNS address>
```
![image](https://user-images.githubusercontent.com/67620689/203761659-9be08da4-f537-4971-b9aa-51f7c1eddcb4.PNG)

[<-- Return to Learning Path](/content/en/cloud/gcp/#sections)
