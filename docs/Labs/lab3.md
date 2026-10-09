---
id: lab3
title: Lab 3 - Configuring Windows Server
sidebar_position: 3
description: Configuring basic settings in Windows Server and Windows 11
---

# Lab 3: Configuring Windows Server

## Overview

This week's lab will cover the following:

- Adding and removing roles in Windows Server
- Learning about and editing the registry


## Lab 3 Notes

You will be asked to take screenshots along the way in this lab. It is easier to do so rather than having to go back at the end of the lab to take them. Also, one screenshot cannot be taken once the lab has been completed so please take care to take your screenshots when instructed to.

## Objectives

By the end of this lab, you will be able to:
- Add roles in Windows Server
- Remove roles in Windows Server
- Export Registry entries for backup
- Add registry entries
- modify registry entries

## Investigation 1: Installing and Removing Roles with a GUI

### Part 1: Installing Roles in Server Manager

In this investigation, you will install 3 new roles into Server1 and then remove 1.

1. Begin by logging into Server1 and open Server Manager.
2. Click on "Add roles and features".
3. The "Add Roles and Features" wizard window will appear. Click "Next".
4. Select "Role based or feature-based installation" and click "Next".
5. Select your server (it should be the only one there) and click "Next".
6. The next screen will have a listing of all he available roles that can be installed onto your Windows Server. Check the following 3 roles (*NOTE: As you select each role, it will prompt you to add necessary features for that role. Accept what it gives you to install and click "Add Features"*):
  - Print and Document Services
  - Web Server (IIS)
  - DNS (When selecting DNS you will get a Validation Results warning. Do not worry about this. We will be fixing this issue in lab 4. Just click "Continue" when it appears)
7. With all 3 roles selected, click "Next".
8. Click "Next" on the next few windows, accepting the default options each one gives you until you arrive on the "Confirm installation selections" window.
9. Check the "Restart the destination server automatically if required" box and then click "Install".
10. Your roles will now be installed onto Windows Server. Wait for the installations to all complete and then click "Close".

You should now see the new roles you installed have been added as tiles in your Server Manager listed under "Roles and Server Groups". They will also appear in the top-left area of Server Manager listed under "Dashboard", "Local Server", and "All Servers". 

You will also notice that these tiles are all coloured red. This is because they have not been finalized because we have not rebooted.

Reboot your Server1.

When the server and Server Manager come back up, wait a moment and you will see your new Roles appear as tiles once again but now with green accents to show they are ready to go. We will leave them for now but we will come back to DNS and IIS in lab 4.

**Screenshot 1: Take a screenshot showing the role tiles in Server Manager (DNS, IIS, Print Services)**

### Part 2: Removing Roles in Server Manager

Now that we have roles installed let's look at how we can remove them.

1. In the top-right area of Server Manager, click on "Manage" >> "Remove Roles and Features".
2. Click "Next" in the "Remove Roles and Features" wizard window.
3. Select your server (it should be the only one there) and click "Next".
4. Uncheck "Print and Document Services", click "Remove Features" in the window that appears, and then click "Next".
5. Click "Next" again and then on the "Confirm removal selections" screen heck the "Restart the destination server automatically if required" box and then click "Remove".
6. Windows will remove the Print Services role. Click "Close" when it is finished.
7. Shut down Server1.

**Screenshot 2: Take a screenshot showing the role tiles in Server Manager (DNS and IIS)**

Now you know how to add and remove roles in Windows Server. Pretty easy, right? Well that's because we have the GUI to use on Server1. Next we will do the same thing on Server 2 with no GUI...

## Investigation 2: Installing and Removing Roles on Server Core (with no GUI)

### Part 1: Installing Roles in Powershell

Installing and removing roles on Server Core is not difficult but it requires the installation of the Server Manager module and the use of Powershell. 
We will be discussing Powershell in more detail in lab 6 and 7 but for now just keep in mind that it is required for most tasks in Server Core.

1. Power on and log into Server2. You should be met with the "Welcome to Windows Server 2025 Datacenter" screen with 15 options.
2. Type "15" and press "Enter". This will bring you to the command line. Windows Server 2025 will default to a Powershell command line. You can tell because you will see the letters "PS" next to your current directory path.
3. To install Server Manager, enter the following command:
```bash
import-module servermanager
```
You should see no output if it runs successfully.

4. Now, let's see what roles and features are already installed and which ones are available to install. Run the following command:
```bash
get-windowsfeature
```
This will generate a large listing of all available windows roles and features. An "X" beside a role or feature indicates that it is already installed.

Let's add a new one - the "Print Services" role.

5. Enter the following command:
```bash
add-windowsfeature print-services -restart
```
Server Core will install the Print Services role and when it has completed you will be met with output specifying the success of the installation (Exit Code: Success).

You can also confirm that the role was succussfully installed by running the "get-windowsfeature" command again. Notice that there is not an "X" next to "Print and Document Services" and "Print Server".

**Screenshot 3: Take a screenshot here to show you successfully installed the Print Services role on Server2.**

Now we will remove that same role.

### Part 2: Removing Roles in Powershell

1. Reboot your Server2 with the following command:
```bash
shutdown /r /t 0
```
2. When it is finished rebooting, log in and return to the Powershell command line.

3. Now, enter the following command:
```bash
remove-windowsfeature print-services -restart
```
Windows should reboot once the role has been removed. If not, reboot manually.

4. Log back in, return to the Powershell command line and enter the "get-windowsfeature" command once again. Note that "X"s next to "Print and Document Services" and "Print Server" have been removed. 

**Screenshot 4: Take a screenshot here to show you successfully removed the Print Services role on Server2.**

5. Shut down your Server2 with the following command:
```bash
shutdown /s /t 0
```

## Investigation 3: The Windows Registry

The Windows Registry is a critically important part of any Microsoft Windows operating system. It is essentially a database that stores all computer, system, application and user settings. More importantly, it is referenced by Windows for just about every operation that can occur on the OS. It cannot be overstated just how important the Registry is and how important it is to not mess around in it.

In this next section we are going to mess around in the Registry.

### Part 1: Exporting Registry Keys

Before we go in and start changing things, we are going to learn how to backup up parts of the registry.
We are not going to change anything critical in the Registry but (as is the case with any configuration on any system) it is always a good idea to backup your default settings. That way, if you break something, you can always revert to the backup. 

Entries in the registry are called "Keys". They vary widely in structure and function but they can all be backed up the same way. We are going to be modifying the key that controls the legal notice when logging into Windows so we are going to back up that key along with the entire location in which it sits.

1. Power on Server1 and log in.
2. Click the blue Windows button at the bottom of the screen and type "run" and hit "Enter".
3. The "Run" dialogue box will appear. Enter "regedit" and click "OK". The Registry Editor window will appear. *Note: Take extra care when navigating the Registry. There are many directories that appear as duplicates down different paths of the registry directory tree and you want to make sure you are in the right place before making any changes. Changes made in the wrong place can end in disastrous results.*
4. Navigate to the following path in the Registry Editor: HKEY_LOCAL_MACHINE\ SOFTWARE\ Microsoft\ Windows\ CurrentVersion\ policies\ system
5. If you are in the right place you will see the "legalnoticecaption" and "legalnoticetext" keys. Click "File" in the top-left of the Registry Editor window and then click "Export".
6. Name your backup "RegistryLegalNotice.reg" and save it to the C: drive.
7. Using the File Explorer, navigate to your C: drive.

**Screenshot 5: Take a screenshot showing your RegistryLegalNotice.reg file in the C: drive directory.**

### Part 2: Modifying Registry Keys

Now that we have a backup, we can proceed to modify the legal keys.

1. Back in the Registry Editor, double click on the legalnoticecaption key. Enter the value as "This is a Secure PC" and click "OK".
2. Double click on the legalnoticetext key. Enter the value as "Only authorized users can log into this computer and use this network. Non-authorized users will be recorded and prosecuted." and click "OK".
3. Click "View" at the top of the editor and then "Refresh".
4. Close the Registry Editor and reboot Server1.

Once it reboots and you go to log in, notice that you will now see your legal messages.

**Screenshot 6: Take a screenshot showing these messages before you log in.**

### Part 3: Adding Registry Keys

Keys can be modified but we can also add keys for new functionality or if keys are missing.

1. Log back into Server1 and open the Registry Editor again.
2. Navigate to the following path (BE CAREFUL and double check where you are!): HKEY_LOCAL_MACHINE\ SOFTWARE\ Microsoft\ Windows NT\ CurrentVersion
3. If you are in the right place you will see the several keys with "Build" in the name at the top of the list.
4. Right-click in the white space where the keys are stored and click "New">>"String Value".
5. Name it "RegisteredOwner",press "Enter", then double click it and enter your name for its value.
6. Again, right-click in the white space where the keys are stored and click "New">>"String Value".
7. Name it "RegisteredOrganization",press "Enter", then double click it and enter "Seneca Polytechnic" for its value.
8. Click "View" at the top of the editor and then "Refresh".
9. Hit Winkey+R on your keyboard (a nice shortcut to bring up the "Run" dialogue box). Enter "winver.exe" and click "OK". The "About Windows" window should appear and your Name and Organization will be listed at the bottom.

**Screenshot 7: Take a screenshot showing your name and organization in the "About Windows" window.**

Let's add another key. This time we will add something that is actually useful on our system.

10. Navigate to the following path in the Registry Editor (CAUTION! THIS IS A DIFFERENT PATH THAN THE LAST ONE!): HKEY_LOCAL_MACHINE\ SOFTWARE\ Policies\ Microsoft\ Windows NT
11. There should be only one key here (the Default key). Right-click in the white space and click "New">>"Key". You will see a new path entry on the left side of the screen appear inside "Windows NT".
12. Rename it to "Reliability"
13. Now, highlight "Reliability", right-click in the white space to the right and click "New">>"DWORD".
14. Rename it to "ShutdownReasonOn".
15. Click "View" at the top of the editor and then "Refresh".
16. Close the Registry Editor and reboot Server1. Notice that when you reboot Server1 you are no longer asked to provide a reason as to why the server is shutting down. But why exactly did that happen?
17. Log back into Server1 and navigate back to your "ShutdownReasonOn" key. Double-click it and notice its "value" data. It is set to "0". You will learn about binary and hex and decimal values in another course but that "0" value means that the key is turned off. So we added a key that specifies to turn off the functionality that asks the user why they are shutting down the server. If we were to change that value to "1" we would turn that key on and the server would once again begin asking the user for reasons when shutting down.

**Screenshot 8: Take a screenshot showing your ShutdownReasonOn key and its value data.**

## Lab 3 Sign Off

You should have 8 screenshots that you have been taking throughout this lab.

Put all 8 of these screenshots into a text document and upload it to the Lab 3 Submission page in Blackboard.
