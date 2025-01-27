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

If you go back to Windows and enter the network location we connected to, you should find the downloaded archive, HDSentinel, and the html report that was just printed.

<img src="./image/3-1 Access HDS report from Windows.png" width=720 height=450>

If you run the HTML report directly, you'll see the report as shown below.

<img src="./image/3-2 html report.png" width=720 height=450>

Now let's monitor the drives on your NAS with HDSentinel installed on Windows.

<img src="./image/3-3 Access HDS report from HDS (1).png" width=720 height=450>

Click the Browse button and specify the html file.

<img src="./image/3-4 Access HDS report from HDS (2).png" width=720 height=450>

After a successful load, you can see how many physical drives are viewed and where the report is located.

<img src="./image/3-5 Access HDS report from HDS (3).png" width=720 height=450>

Click the OK button and watch HDSentinel read and display the report. You should see the drives on your TrueNAS added to the list of drives next to it.


# 4. Scheduling report generation

To return to TrueNAS and schedule the HDSentinel, we'll reopen the shell and gain privileges with ```sudo su```.

Type in ```vim /etc/crontab``` and you should see something like the screen below.

<img src="./image/4-1 Set crontab (int).png" width=720 height=450>

In the image above, maneuver the arrows so that the cursor is in front of 17 as indicated by the arrow, press ```i``` once, press ```ENTER```, and move up so that we have a blank line.

<img src="./image/4-2 Set crontab (fin).png" width=720 height=450>

Enter ```*/10 *  *  *  * root    /mnt/Public/HDSentinel/HDSentinel -r /mnt/Public/HDSentinel/hdsreport.html -html``` as shown in the image above. This will cause HDSentinel to run every 10 minutes and output an html report.


And let's save this edit by pressing ```esc``` and typing ```:wq```.

<img src="./image/4-3 Complete.png" width=720 height=450>

If we wait a bit, we will see the html report modified in Windows and HDSentinel will reflect this change.

Now we can easily monitor the drives belonging to our TrueNAS with HDSentinel from Windows!

# 5. References

[How to: monitor Network Attached Storage (NAS) status](https://www.hdsentinel.com/how_to_monitor_network_attached_storage_nas_status.php)

