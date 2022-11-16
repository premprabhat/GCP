---
title: "Deploy Arm based VMs using GUI"
type: docs
weight: 2
hide_summary: true
description: >
    Learn how to deploy Arm based VMs using GUI..
---

# How to deploy Arm based VMs via GUI
Log in to your GCP account and in the Google Cloud console, go to the [VM instances page](https://console.cloud.google.com/compute/instances?_ga=2.159262650.1220602700.1668410849-523068185.1662463135).

![h3](https://user-images.githubusercontent.com/67620689/202090364-2946214c-2347-4538-b2b0-3a36f45caee0.PNG)
   
## Select your project
    
![image](https://user-images.githubusercontent.com/67620689/202095985-103deaa4-610d-45ea-a84c-65af2bbfec41.PNG)

## Click Create instance
    
![h3](https://user-images.githubusercontent.com/67620689/202090934-aa0aa2da-e0f7-4aea-b8db-bc4988b781b2.PNG)

## Specify a Name for your VM. For more information, see [Resource naming convention](https://cloud.google.com/compute/docs/naming-resources#resource-name-format).

![image4](https://user-images.githubusercontent.com/67620689/202098830-532b5dc8-f6b5-4cff-931c-ec41edd08516.PNG)

## Choose a Zone for this VM that supports Tau T2A
This series is available only in select regions and zones. More information on regions and zones at which it is available can be found [here](https://cloud.google.com/compute/docs/regions-zones#available).
   
![image1](https://user-images.githubusercontent.com/67620689/202097168-6208b6ae-3627-47b3-a397-7783769e6727.PNG)

## Select General-purpose from the Machine family options.
Select T2A from the Series drop-down menu and a T2A Machine type from the drop-down menu.
   
![image](https://user-images.githubusercontent.com/67620689/202092025-53aef76d-ee09-415a-a84b-fb21d6e329f3.PNG)

## In the Boot disk section, click Change, and then do the following:
On the Public images tab, choose the following:
 * The default Debian-11-Arm64 image or any other supported Arm OS. Here we have chosen ubuntu 22.10 version.
 * Boot disk type
 * Boot disk size

Then click on select.

![image2](https://user-images.githubusercontent.com/67620689/202097666-b618266c-3dde-49b8-adb5-887380066648.PNG)

## To create and start the VM, click Create.
   
![image3](https://user-images.githubusercontent.com/67620689/202098038-7bfb0b6c-af18-4d5c-92a8-ca90a57bc25b.PNG)
   
## Configure Storage
By default, EC2 instances come with 8GiB of storage. You may want to increase this for scenarios that require log files and backups. In our case, we selected 25GiB of storage. Selecting a volume type of General Purpose SSD (gp2) is a good starting point. You can explore the different storage options after you understand the nature of your workload better.  
   
![image](https://user-images.githubusercontent.com/87687468/189878035-87d9721f-c58e-4ce7-800b-093d4d3e59ce.png)
   
Set Delete On Termination to yes. This will ensure that the volume is deleted and will not incur charges on you account once the instance is terminated.
   
## Review and Launch
Check the details in the summary box and press launch.

![image](https://user-images.githubusercontent.com/87687468/189878839-3ef022f7-2be7-458a-b0ce-20cf5ee0bcaa.png)

## SSH into the launched instance
We can login into created instance using SSH. To do that, we require access to the key pair (Pem file) which we have downloaded. By default, pem file permission is set to `rw-r--r--`. This is not allowed for SSH Pem files. We have to change the permission for this Pem file to 400, so that only the current user can read this Pem file.

```
$ chmod 400 demoserver.pem
```
   
Now, only the owner of the Pem file can read it. In fact, these are permissions that a Pem file expects. Select the Instance by checking the box of the instance you want to connect to and GO TO » connect.
   
![image](https://user-images.githubusercontent.com/87687468/192154311-55889d4e-6dd2-4bc3-81a9-95cca7356e0a.png)
   
Here in SSH Client, you will see instructions that explain how to SSH into the instance.
   
![image](https://user-images.githubusercontent.com/87687468/190095052-41851f3d-61db-486f-9c00-2f504587bdcc.png)
   
If in your terminal, you are in the same directory where the Pem file is present. you will not need the full path for the pem file. Use following command to ssh an instance:
   
```
ssh -i "demoserver.pem" ubuntu@<Public IP/DNS address>
```
[<-- Return to Learning Path](/content/en/cloud/azure/#sections)
