# 0. Important Notes

This guide is based on TrueNAS SCALE 24.04.11 and Windows 10 22H2 running on ESXi 8. 

An update will be provided later when the hardware structure is changed to a bare metal setup.

I apologize for any awkwardness with the translator.


# 1. Create Dataset and Connect to Windows

I'm going to create a new Dataset under the Pool called Public to install and run HDSentinel and store reports.

<img src="./image/1-1 Add Dataset Menu.png" width=720 height=450>

I'll name it HDSentinel and specify the Dataset Preset as SMB.

<img src="./image/1-2 Add Dataset Naming.png" width=720 height=450>

This should have enabled the SMB share. Let's create a user for access to this Dataset.

<img src="./image/1-3 Add User.png" width=720 height=450>

So let's head over to Windows and connect to what we just set up.

<img src="./image/1-4 Windows Setting.png" width=720 height=450>

In place of storage.lab in the image below, enter the IP assigned to your TrueNAS.

<img src="./image/1-5 Windows Connect to TrueNAS.png" width=720 height=450>

Log in with the user we created in the previous step and you'll have access to the network location.

<img src="./image/1-6 Complete Add.png" width=720 height=450>


# 2. Install HDSentinel
