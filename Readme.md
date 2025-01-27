# 0. Important Notes

This guide is based on ```TrueNAS SCALE 24.04.11``` and ```Windows 10 22H2``` running on ```ESXi 8.0 Update 3```. 

An update will be provided later when the hardware structure is changed to a bare metal setup.

I apologize for any awkwardness with the translator.


# 1. Create Dataset and Connect to Windows

I'm going to create a new Dataset under the Pool called ```Public``` to install and run HDSentinel and store reports.

<img src="./image/1-1 Add Dataset Menu.png" width=720 height=450>

I'll name it ```HDSentinel``` and specify the Dataset Preset as SMB.

<img src="./image/1-2 Add Dataset Naming.png" width=720 height=450>

This should have enabled the SMB share. Let's create a user for access to this Dataset at ```Credentials/Local Users/Add```.

<img src="./image/1-3 Add User.png" width=720 height=450>

So let's head over to Windows and connect to what we just set up.

<img src="./image/1-4 Windows Setting.png" width=720 height=450>

In place of ```storage.lab``` in the image below, enter the IP assigned to your TrueNAS.

<img src="./image/1-5 Windows Connect to TrueNAS.png" width=720 height=450>

Log in with the user we created in the previous step and you'll have access to the network location.

<img src="./image/1-6 Complete Add.png" width=720 height=450>


# 2. Install HDSentinel

I'll be installing and using HDSentinel from ```System Settings/Shell``` in the WebUI, not SSH, so I don't need a tool like PuTTY.

You can refer to the image below for things like output.

<img src="./image/2-1 Install HDS.png" width=720 height=450>

1. Gain permissions with ```sudo su```. Enter the password for the admin account you created when you installed TrueNAS.
2. Let's get ready to go to the dataset we created for HDSentinel with the command ```cd ../..```.
3. Navigate to the dataset we created with the command ```cd mnt/Public/HDSentinel```, modifying the path below ```mnt/``` to suit your needs.
4. Download the Linux version of HDSentinel to your location with the command ```wget https://www.hdsentinel.com/hdslin/hdsentinel-020c-x64.zip```. Based on the requirements of TrueNAS, this should be sufficient, but in the unlikely event that an ARM version is released in the future, please refer to <https://www.hdsentinel.com/hard_disk_sentinel_linux.php> and copy the link to the appropriate file and insert it after ```wget```.
5. You can verify that the download is complete with the ```ls``` command.
6. We don't have an image for reference, but regardless, ```unzip hdsentinel-020c-x64.zip```.
7. If you type ls one more time, you should see another file named ```HDSentinel```, which is different from the existing zip file, and this is the executable. Make it executable with ```chmod 755 HDSentinel```.

<img src="./image/2-2 Execute HDS.png" width=720 height=450>

You can now run HDSentinel. Run HDSentinel via ```./HDSentinel``` and it will tell you the status of your drives as shown in the image above.

Now that you've verified that HDSentienl is working, check to see if it's outputting an html report as well. ```./HDSentinel -r ./hdsreport.html -html```



If the HTML file is successfully generated, you should see a slightly different output than just now. Please refer to the image below.

<img src="./image/2-3 Execute HDS with html report.png" width=720 height=450>

# 3. Read reports in Windows
