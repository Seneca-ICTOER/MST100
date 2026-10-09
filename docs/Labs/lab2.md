---
id: lab2
title: Lab 2 - Installing Virtual Machines
sidebar_position: 2
description: Installing 3 virtual machines that will be used for the rest of the course.
---

# Lab 2: Installing Windows Server 2025 and Windows 11 Virtual Machines

## Overview

This week's lab will cover the following:

- Accessing VMWare Workstation Pro
- Installing the following 3 Virtual Machines:
  - Server1: Windows Server 2025 Datacenter (with GUI)
  - Server2: Windows Server 2025 Datacenter Core (without GUI)
  - Client1: Windows 11 Education   
- Performing post-installation tasks on all 3 virtual machines

## Lab 2 Notes

The primary method to do the labs in MST100 is to use virtual machines running on VMware inside the Seneca PC Windows image. There will be a few times in this course where you will need to have two VMs turned on at once but for the most part it is best to make sure that you have only one turned on at a time so as to ensure the stability of your system. Even so, you may experience VM crashes or system lock ups from time to time as MS Windows and VMware are quite resource intensive.

## Objectives

By the end of this lab, you will be able to:

- use VMWare to install virtual machines
- install a GUI version of Windows Server 2025
- install a GUI-less version of Windows Server 2025
- install an Education version of Windows 11
- recognize the differences in these installations and the time/resources needed for each

## Investigation 1: Installing your VMs

In this investigation, you will install all 3 virutual machines using VMWare Workstation Pro on a Seneca lab computer.

1. Begin by logging into a Seneca lab computer and then plugging in your external hard drive (which should have your ISO files and Keys text file on it from Lab 1)
2. Check to make sure your external hard drive is recognized by the computer by opening the File Explorer and navigating to your external hard drive.
3. Double check that the directories you created in lab 1 are there.
4. Open VMWare Workstation Pro from the desktop.


### Part 1-1: Installing Windows Server 2025 Datacenter (GUI)

In this part, you will install Windows Server 2025 Datacenter with a GUI.

1. Click "Create a New Virtual Machine" on the Home tab in VMWare Workstation Pro.
2. Select "Typical" and click "Next".
3. Select "Installer disc image file (iso)" and then click "Browse".
4. Navigate to the ISOs directory on your external hard drive and open the Windows Server 2025 ISO. 
5. You should notice a message below the path to the ISO that says "Windows Server 2025 detected. This operating system will use Easy Install". Click "Next"
6. Enter your Windows Server 2025 product key (which should be in your "Keys" file on your external hard drive).
7. For "Version of Windows to Install" leave the default (should be "Windows Server 2025 Datacenter").
8. For "Full name", enter "Administrator".
9. For "Password", enter "P@ssw0rd".
10. Do **NOT** check "Log on automatically". Click "Next".
11. For "Virtual Machine name", enter "Server1".
12. For "Location", click "Browse" and navigate to and select the directory called "Server1" in your external hard drive and click "OK". Confirm the path to your "Server1" directory has been selected (this is important, if you create your VM on the lab computer and not your external hard drive, it will be lost when you turn off the computer). Click "Next". 
13. Leave the disk size at 60 GB and select "Store virtual disk as a single file". Click "Next"
14. Click "Customize Hardware".
15. Change "Memory" to 4096 MB.
16. Change "Processors" to 1 Processor and 4 cores per processor. Click "Close". Click "Finish".
17. The virtual machine will launch and the installation will begin. You will be asked to put your product key in once again. Do so and click "Next".
    - *to "paste" into the VM, click inside the Product Key box so that the cursor is seen inside it, then click "Edit" in VMWare and select "Paste".
18. From here, most of the installation will be automatic. The VM will restart several times during the installation.
19. Eventually you will land on the Windows Server 2025 desktop and VMWare Tools will auto-install. When it is finished, it will ask you to restart the system. Click "Yes" and your VM will reboot. 

### Part 1-2: Windows Server 2025 GUI Post-Installation Tasks

Now that our Server1 is installed, there are a few things we need to do to prepare it for our labs. We are going to ensure our Time Zone is correct and we are going to name our system appropriately. Then we are going to make sure our system is completely updated. FInally, we will confirm internet connectivity and download a better web browser.

1. Log into your Server1. To use Ctrl+Alt+Del in your VM, click "VM" in VMware and select "Send Ctrl+Alt+Del".
2. You should be met with the Server Manager Dashboard. If not, click on the Server Manager icon at the bottom of the screen.
3. A pop up will appear asking you to "Try Windows Admin Center/ Azure Arc". Click "Don't show me this message again" and close the pop-up.
4. Click "Local Server" on the left side of the screen. 
5. In the "Properties" window, look for "Time Zone" and make sure it is set to "UTC -05:00 Eastern Time". If it is something else, change it to our time zone.
6. Next, click on the semi-randomized name next to "Computer Name". Then click "Change".
7. Change the computer name to "S1-SenecaID" (for example, my Seneca ID is hheim so my Server1 computer name would be "S1-hheim").
8. Leave the default Workgroup selected and click "OK". A message will pop up saying the computer name will not change until the system is restarted. Click "OK".
9. Close the Computer Name window and another message will pop up asking if you want to restart the computer now. Click "Restart Later".
10. Next, find "Windows Update" in the "Properties" window and click the blue text beside it. Click the blue "Check for updates" button.
11. Windows Server will now download and install all necessary updates.
12. While they are downloading, scroll down and click on "Advanced Options".
13. Turn the "Receive updates to other Microsoft products" toggle from **Off** to **On**.
14. Click "Windows Update" at the top of the screen to get back to the main Updates window.
15. You will likely need to wait a little while for the updates to finish downloading and installing. Eventually, all the updates listed will say "Pending restart". Once you see this, click the blue "Restart now" button.
16. Server1 will reboot and install updates. This will likely take a while and it will likely reboot a few times but just let it do its thing. Do not turn off the VM while it is updating. If your VM freezes during the updates, check with you teacher before moving forward.
17. When you are met with the login screen, log in and return to the Updates window.
18. Click the blue "Check for updates" button. If any remaining updates are available they will install. Repeat this cycle until you are met with a "You're up to date" message.
19. Next, we are going to confirm internet connectivity and download a different web browser that MS Edge.
20. Open MS Edge (the icon will be at the bottom of your screen).
21. Go through the first-run questions (just say no to everything).
22. Go to the Mozilla Firefox website: www.firefox.com
23. Download Firefox and install it.
24. Once it is installed, open Firefox and go to google.com to make sure everything is working.
25. Shut down Server1.

**Note:** During installations it is a good idea to only have one VM (the one you are installing) powered on at a time.

### Part 2-1: Installing Windows Server 2025 Core (No GUI)

Now we are going to install our second server, Server2. This installation will be very similar to the first, although the server will look quite different when we are done. 

1. Click "Create a New Virtual Machine" on the Home tab in VMWare Workstation Pro.
2. Select "Typical" and click "Next".
3. Select "Installer disc image file (iso)" and then click "Browse".
4. Navigate to the ISOs directory on your external hard drive and open the Windows Server 2025 ISO. 
5. You should notice a message below the path to the ISO that says "Windows Server 2025 detected. This operating system will use Easy Install". Click "Next"
6. Enter your Windows Server 2025 product key (which should be in your "Keys" file on your external hard drive).
7. For "Version of Windows to Install" change it to "Windows Server 2025 Datacenter Core". **VERY IMPORTANT!!**
8. For "Full name", enter "Administrator".
9. For "Password", enter "P@ssw0rd".
10. Do **NOT** check "Log on automatically". Click "Next".
11. For "Virtual Machine name", enter "Server2".
12. For "Location", click "Browse" and navigate to and select the directory called "Server2" in your external hard drive and click "OK". Confirm the path to your "Server2" directory has been selected (this is important, if you create your VM on the lab computer and not your external hard drive, it will be lost when you turn off the computer). Click "Next". 
13. Leave the disk size at 60 GB and select "Store virtual disk as a single file". Click "Next"
14. Click "Customize Hardware".
15. Change "Memory" to 4096 MB.
16. Change "Processors" to 1 Processor and 4 cores per processor. Click "Close". Click "Finish".
17. The virtual machine will launch and the installation will begin. You will be asked to put your product key in once again. Do so and click "Next".
    - *to "paste" into the VM, click inside the Product Key box so that the cursor is seen inside it, then click "Edit" in VMWare and select "Paste".
18. From here, most of the installation will be automatic. The VM will restart several times during the installation.
19. Eventually you will land on the Windows Server 2025 Core config screen and VMWare Tools will auto-install. When it is finished, it will ask you to restart the system. Click "Yes" and your VM will reboot. 

### Part 2-2: Windows Server 2025 Core Post-Installation Tasks

Now that Server2 is installed we are going to perform the same post-installation tasks that we did on Server1. However, because this is the "Core" version of Windows Server 2025 there is no GUI and so our configurations will be done a little differently.

1. Begin by logging into Server2 (again, click "VM" in VMWare and then select "Send Ctrl+Alt+Del").
2. Put in your password and you will be brought to the SConfig screen. SConfig is a module that allows us to make basic system configurations without the need for commands or a GUI.
3. Find the option for "Date and Time", enter the corresponding number, and press "Enter". The Date and Time window should appear.
4. Check to ensure that UTC -05:00 Eastern Time is selected. If not change it to that time zone. Press "OK".
5. Next we will change the computer name. Find the option for "Computer Name", enter the corresponding number, and press "Enter".
6. Type in your computer name as "S2-SenecaID" (for example, my Seneca ID is hheim so my Server2 computer name would be "S2-hheim").
7. The system will ask you to restart. Select "(Y)es".
8. Once the system reboots and you have logged back in to the SConfig screen, find the option for "Update Settings", enter the corresponding number, and press "Enter".
9. On the "Update Settings" screen, enter "5" (Opt-in to Microsoft Update) and press "Enter". Press "Y" and press "Enter" on the next screen to confirm. Press "Enter" to continue.
10. Back on the SConfig screen, find the option for "Install Updates", enter the corresponding number, and press "Enter".
11. Enter "1" (All quality updates) and press "Enter".
12. Windows Server will search for applicable updates and eventually it will ask you which updates to install. Enter "A" (All Updates) and press "Enter".
13. Windows Server will not download and install its updates. This may take a while.
14. Once the installations are complete, the system will ask you to restart. Enter "Y" (yes) and press "Enter".
15. Server2 will restart. It may continue updating and restart again.
16. When Server2 comes back up, log in and go back into the "Install Updates" option. Once again, Enter "1" and press "Enter".
17. If there are any remaining updates, enter "Y" and press "Enter". Then, reboot if asked to.
18. Repeat these steps until searching for all quality updates results in the "There are no applicable updates" message. Press "Enter" to return to the SConfig screen.
19. Shut down Server2.

### Part 3-1: Installing Windows 11 Education

In this part we are going to install our Client1 system which will be running Windows 11 Education. Unfortunately, Windows 11 does not have an Easy Install feature in VMWare so this installation will be a little more involved.

1. Click "Create a New Virtual Machine" on the Home tab in VMWare Workstation Pro.
2. Select "Typical" and click "Next".
3. Select "Installer disc image file (iso)" and then click "Browse".
4. Navigate to the ISOs directory on your external hard drive and open the Windows 11 ISO. 
5. You should notice a message below the path to the ISO that says "Windows 11 x64 detected. Click "Next"
6. For "Virtual Machine name", enter "Client1".
7. For "Location", click "Browse" and navigate to and select the directory called "Client1" in your external hard drive and click "OK". Confirm the path to your "Client1" directory has been selected (this is important, if you create your VM on the lab computer and not your external hard drive, it will be lost when you turn off the computer). Click "Next".
8. Windows 11 uses TPM hardware to encrypt certain important files. VMware Worksation adds the TPM module automatically as long as you select the right options:
  - Choose Encryption Type: Only the files needed to support a TPM are encrypted.
  - Password: Your normal VM password.
9. Click "Next".
10. Leave the disk size at 64 GB and select "Store virtual disk as a single file". Click "Next"
11. Click "Customize Hardware".
12. Change "Processors" to 1 Processor and 4 cores per processor. Click "Close". Click "Finish".
13. When the VM starts, pay close attention to the black screen. As soon as you see the "Press any key..." message, click with your mouse into the VM and press the space bar. Failure to do so will result in the VM not booting to its installer and you will have to begin again.
14. On the "language setting" screen, use the default selections and click "Next".
15. On the "keyboard setting" screen, use the default selections and click "Next".
16. On the "setup option" screen, select "Install Windows 11", check the "I agree everything..." box and click "Next".
17. On the next screen, enter your Product Key for Windows 11 (this will be a different key than the Windows Server key) and click "Next".
18. Accept the license terms on the next screen.
19. On the "location to install" screen, select the only available option and click "Next".
20. On the "Ready to install" screen, click "Install".
21. Windows 11 will now install. It will reboot and take some time before you arrive at the next input screen.
22. When you regain control you will be on the "country or region screen". Select "Canada" and click "Yes".
23. On the "keyboard layout" screen, leave the default ("US") and click "Yes".
24. Skip a second keyboard layout.
25. Windows will now check for updates and install them as necessary.
26. The next screen you see will be the "Sign in" screen. Click "Sign-in options" and then click "Domain join instead".
27. For your name, use your SenecaID (ex. hheim) and click "Next".
28. For your password, use "P@ssw0rd" and click "Next".
29. Next, fill out 3 security questions, clicking "Next" along the way.
30. Select "No", "No", "Required Only", "No", "No" on the next 5 screens.
31. Windows will then do some more updates. This may take some time and the system may reboot.
32. When it is done with updating, you will land on the Windows 11 desktop.


### Part 3-2: Post-installation Tasks for Windows 11

Now that our Client1 is installed, there are a few things we need to do to prepare it for our labs similar to what we did on Server1 and Server2. 

We will begin with the basics - time zone and computer name.

1. Right-click on the time in the bottom-right corner of the Windows 11 VM and left-click "Adjust date and time".
2. Make sure the time zone is set to UTC -05:00 Eastern Time.
3. Click the Windows icon at the bottom of the screen (4 blue squares), type "Computer Name", and then click on "View your PC name".
4. Click "Rename this PC" and give it the name "C1-SenecaID" (for example, my Seneca ID is hheim so my Client1 computer name would be "C1-hheim").
5. You will be asked if you want to restart the computer. Restart it.
6. When the computer comes back up, log in and click on the Windows icon at the bottom of the screen (4 blue squares). Type "updates" and click "Check for Updates".
7. Click the blue "Check for Updates" button in the Windows Update window.
8. While they are downloading , scroll down and click on "Advanced Options".
10. Turn the "Receive updates to other Microsoft products" toggle from **Off** to **On**.
11. Click "Windows Update" at the top of the screen to get back to the main Updates window.
12. Like on Server1, get your Client1 fully updated. This may require some time and reboots but keep updating until you are met with the "You're up to date" message.

Now that your system is fully up to date, there is one more thing we have to do. Recall that with Server1 and Server2, something called VMWare tools was automatically installed when the initial installation was complete. This will not happen on Windows 11. We will have to install it manually.

13. You should notice a beige bar at the bottom of your Client1 VM with a "Install Tools" button. Click that button. (If you don't see the beige bar or the button, click on "VM" in VMWare and then click on "Install VMWare Tools").
14. Wait a few seconds and you should see a pop up in the bottom right corner of your screen that says "DVD Drive (D:) VMWare Tools". Click this pop up.
15. A new window will pop up with a "Run setup.exe" button. Click this button. Click "Yes" when the VMware installation launcher window pops up.
16. The VMware tools installer will start. Click "Next". Select "Typical" and click "Next". Click "Install". Click "Finish" when the installation is complete and then restart Client1.
17. Log back into Windows and install Firefox the same way you did with Server1.
18. Shut down Client1.

Congratulations! You now have three fully functioning virtual machines that you will be using for the rest of the course.

## Lab 2 Sign Off

Take the following 3 screenshots:
- Log into Server1. Go into the Server Manager on Server1, click on "Local Server", and take a screenshot of the entire "Properties" window.
- Log into Server2. Take a screenshot of the "Welcome to Windows Server 2025 Datacenter" screen.
- Log into Client1. Go into the Windows Update window and take a screenshot that shows the "You're up to date" message.

Put all 3 of these screenshots into a single text document and upload it to the Lab 2 Submission page in Blackboard.
