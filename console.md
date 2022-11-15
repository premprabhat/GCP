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

![image](https://user-images.githubusercontent.com/87687468/189866780-e67c8a99-e5f2-445f-938c-a672cd926c4a.png)
   
## Select your project and click Continue.
Name the instance.
    
![image](https://user-images.githubusercontent.com/87687468/192811901-40232129-2405-4a33-803c-1a9e40934b44.png)

## Click Create instance.
    
![image](https://user-images.githubusercontent.com/87687468/192811901-40232129-2405-4a33-803c-1a9e40934b44.png)

## Specify a Name for your VM. For more information, see [Resource naming convention](https://cloud.google.com/compute/docs/naming-resources#resource-name-format).
   
![image](https://user-images.githubusercontent.com/87687468/192594550-95c51ac9-d1cd-4f0d-98f2-a1fce1a78b2d.png)

## Choose a Zone for this VM that supports Tau T2A
This series is available only in select regions and zones. More information on regions and zones at which it is available can be found [here](https://cloud.google.com/compute/docs/regions-zones#available).
   
![image](https://user-images.githubusercontent.com/87687468/192596029-21b7dcc2-917c-41d0-bda2-3763584f7f00.png)
 
## Select General-purpose from the Machine family options.
Select T2A from the Series drop-down menu and a T2A Machine type from the drop-down menu.
   
![image](https://user-images.githubusercontent.com/87687468/189890580-0b647d1e-baad-4597-95ad-7fcad81e9324.png)

## In the Boot disk section, click Change, and then do the following:
On the Public images tab, choose the following:
 * The default Debian-11-Arm64 image or any other supported Arm OS. Here we have chosen ubuntu 22.10 version.
 * Boot disk type
 * Boot disk size

Then click on select.

## To create and start the VM, click Create.
   
![image](https://user-images.githubusercontent.com/87687468/189876379-1d9118c8-a9a6-4e6d-892a-e37443d37546.png)
   
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
