---
id: pracassignment
title: MST100 Practical Assignment
sidebar_position: 4
description: Practical Assignment for MST100
---

# Practical Lab Assignment for MST100

## Overview

For this assignment, you will be creating a new Client VM and adding it to the domain that you created in Lab 9. You will also be creating a set of users, groups, and share directories for the new users. These will all be connected to Active Directory and the domain you set up in Lab 9.

Unlike the labs, you will not be given detailed, step by step instructions on how to accomplish the tasks on this assignment. However, you have already done everything that you do in this assignment in labs 4 and 9 (just with different parameters). Go back and double check those labs if you get stuck during the assignment.

You will be asked to take several screenshots for each part of this assignment.

- Make sure to take screenshots of exactly what is asked of you.
- Be sure to name your screenshots according to the instructions.
- Take actual screenshots on the PC, do NOT use your phone to take pictures of the screen.
- Use either jpeg or png format for the images.

**Failure to follow the instructions regarding screenshots will result in marks deducted.**

## Part A: Creating a New Virtual Machine

### Part A Tasks:

1. Create a new virtual machine using the Windows 11 ISO file. Use the same virtual hardware setup as you did with your "Client1" VM. Give the VM and the Computer Name the name "A1Client". Your initial username should be your Seneca ID.
2. After the VM has been installed, install VMware Tools.
3. Next, add a local user to "A1Client" named "hheim" with a password of "P@ssw0rd". This will look a little different than doing it on a server. Make sure to do this **without** creating a Microsoft account.

### Part A Screenshots:

1. Click the Windows icon in "A1Client", type in "name", click on "View your PC name". Take a screenshot showing the name of your new VM. Name the screenshot **A1ClientName**
2. Click the Windows icon in "A1Client", type in "run" and hit "Enter". Enter "winver.exe" and hit "Enter". Take a screenshot of the window that appears. Name the screenshot **A1ClientVer**
3. Take a screenshot showing that you created the "hheim" user. Name the screenshot **localuseradd**

## Part B: AD users and Groups

### Part B Tasks:

1. On Server1, create a new OU called **SenecaIDA1** (where "SenecaID" is your actual Seneca ID).
2. In this OU, create two new users with the standard password we have been using and passwords set to never expire:

- Chris Johnson (cjohnson)
- Jason Carman (jcarman)

3. In Server1, create a directory called "A1Share" in the root of your C: drive.
4. Configure the "A1Share" directory to be shared. For permissions, give "Full control" to everyone. (Hint: you did this in Investigation 3 of lab 8).
5. Add Home drives for both users you created in step 2 in the "A1Share" directory you created in step 3 using the V: Drive. (Hint: this is similar to what you did in lab 9 but the path for the home folder will need to reference the directory you just created).
6. Create a new Domain Local Group called **SenecaIDDL** (where "SenecaID" is your actual Seneca ID) inside the OU from step 1.
7. Add the 2 users from step 2 to this new Domain Local group.

### Part B Screenshots:

1. Take a Screenshot of Chris Johnson's profile tab. Name the screenshot **cjohnson**
2. Take a Screenshot of Jason Carman's profile tab. Name the screenshot **jcarman**
3. Take a screenshot of the OU displaying the users and DL group you created. Name the screenshot **A1ou**
4. Take a screenshot of the Domain Local group's "Members" tab, showing the users have been added. Name the screenshot **A1DLgroup**

## Part C: Add A1Client to Domain

### Part C: Tasks

1. Configure A1Client to use the following network configurations:

- IP address: 10.10.xxx.116
- Subnet Mask: 255.255.255.0

- DNS Server: 10.10.xxx.10

2. Add A1Client to the domain you set up in lab 9. (Hint: refer to Investigation 3 in Lab 9)
3. After adding A1Client to the domain, you will be required to reboot. Upon rebooting, log in as Chris Johnson.
4. Once logged in, use File Explorer to navigate to the "cjohnson" share you set up in Part B.
5. Log out of cjohnson and log in as jcarman. Use File Explorer to navigate to the "jcarman" share you set up in Part B.

### Part C: Screenshots:

1. Open a command prompt, run "ipconfig" and take a screenshot showing the output. Name the screenshot **A1tcpip**
2. In the command prompt, run "ping 10.10.xxx.10" and take a screenshot showing the output. Name the screenshot **A1ping**
3. On A1Client, take a screenshot showing that "A1Client" has been added to the domain. Name the screenshot **A1ClientDomain**
4. On Server1, in the Server Manager navigate to "Tools" >> "Active Directory Users and Computers" >> "Computers". Take a screenshot that "A1Client" is now listed. Name the screenshot **A1ClientAD**
5. Logged in as cjohnson, take a screenshot showing you can access the "cjohnson" share. Name the screenshot **cjohnsonshare**
6. Logged in as jcarman, take a screenshot showing you can access the "jcarman" share. Name the screenshot **jcarmanshare**

## Part D: Remote Desktop

### Part D Tasks:

1. Enable RDP on Server1.
2. From A1Client, use Remote Desktop to remote into Server1 as your domain administrator.

### Part D Screenshots:

1. Take a screenshot of the setting you used to enable RDP on Server1. Name the screenshot **RDPsetting**.
2. Take a screenshot showing the RDP session from A1Client into Server1 (make sure your screenshot includes the small, gray connection bar at the top). Name the screenshot **A1RDP**

## Submitting Practical Lab Assignment

1. Create a Word Document and name it "MST100PracLabAssignment".
2. Add your name, student ID, and student number to the top of the document.
3. Cleanly add all the screenshots you took during the assignment to the document in the same order in which they were taken.
4. Save the file as a PDF and upload it to the "Practical Lab Submission" page in Blackboard.
