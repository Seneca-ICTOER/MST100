---
id: lab9
title: Lab 9 - Introduction to Active Directory
sidebar_position: 9
description: Active Directory
---

# Lab 9: Introduction to Active Directory

## Overview

This week's lab will cover the following:

- Workgroups
- Adding and Modifying Users
- Creating and Modifying Groups
- Shares and Permissions

## Lab 8 Notes

You will be running all 3 VMs at once in this lab. You will likely notice some stuttering and slow down. Please be patient and do not force any VMs off without checking with your teacher.

## Objectives

By the end of this lab, you will be able to:

-

## Investigation 0: Removing DNS and Creating a Public Share

Before we can begin with Active Directory, we need to remove the DNS role that we installed in Lab 4. This is because the installation of Active Directory will need to install its own instance of DNS to work alongside it. We will also be creating a directory in which our domain users can store their shares.

1. First, double check that the IP address of your Server1 is still 10.10.xxx.10. If it is not, change it now.
2. Open the Server Manager, click on "Manage" in the top right and use the "Remove Roles and Features" wizard to remove the DNS role.
3. Once it is removed, reboot Server1.
4. Log back in and confirm that he DNS role has been removed.
5. Using the File Explorer, navigate to your C: drive and create a new directory called "Public".
6. Share this directory (like you did in Lab 8) and give everyone "Full Control" using "Advanced Sharing".

## Investigation 1: Installing Active Directory

### Part 1: Beginning Installation

1. Open Server Manager and click on "Add Roles and Features".
2. Click the checkbox for "Active Directory Domain Services" in the "Select server roles" window and add all required features.
3. Continue through the installation, noting the details of what ADDS is and click "Install".
4. When the installation has completed, close the installation wizard.

### Part 2: Promote Your Server to a Domain Controller

Installing the Active Directory Domain Services role does not immediately make your server a Domain Controller. More configuration is required.

1. On the Server Manager, top right corner, you will see a Warning Label (yellow triangle with an exclamation point). Click this icon an select: Promote this server to a domain controller.
2. Click the radio button to A"dd a new forest", enter your domain name "SenecaID.local" (from the Prelab Chart), and click "Next".
3. In Domain Controller Options window, for the "Forest functional" and "Domain functional level", select Windows Server 2025. Leave the "Domain Name System (DNS) server" and "Global Catalog" options checked. Assign a password of P@ssw0rd, confirm the password, and click "Next".
4. Click "Next" in the "DNS Options window.
5. In the Additional Options window, take the defaults and click "Next".
6. In the Paths window, take the defaults and click "Next".
7. Review your selections and click "Next".
8. Review the Prerequisites Check. Don’t worry about the warnings. They will not affect our setup. However, if there are any red errors, you will need to go back, troubleshoot and confirm all the prerequisites are completed. Click Install. The installation will complete, and your server will restart. It takes a bit of time to complete this procedure.

## Investigation 2: Managing Active Directory

Congratulations! You have now created a new Active Directory Domain. Notice the login now shows as the DOMAIN\Administrator. You are no longer logging in as a local Administrator, you have leveled up to a Domain Administrator.

The Local Users and Groups tool that you used in the previous lab cannot be used to set up users in your domain because they are local to the server where they were created. We must create Domain Users and Groups, using the Active Directory Users and Computers tool. The process is very similar, but you will be creating these users and groups in the AD DS database that is used by all users and computers in the domain, not just on the local server.

### Part 1: Creating a New Organizational Unit

1. From the Tools menu, select the tool called, Active Directory Users and Computers.
2. When the tool opens, you should see the name of your domain in the left pane. Click your domain name to view the contents of the domain in the right pane of the tool. You will see several containers, including a Users
3. Double-click the Users container to view the contents. (Note: You will see your users from Lab 8 here. While they are listed as users, they cannot be used to log into the domain. We _could_ change their accounts to do so but it is much cleaner to make new accounts. As such, we will be leaving them alone and creating new ones for this lab.)
4. Right-click your Domain name in the left pane. From the pop-up menu, select "New" and then "Organizational Unit".

We are going to create a new OU for our Head Office and create users, groups and
computers within that OU. This would allow us to delegate administration of our Head
Office to a particular group of Administrators at that office.

5. Type in Head Office for the name of the OU, and click OK. The Head Office OU should appear in your list.

### Part 2: Creating Computer Accounts

Computer accounts can be created as the computers join the domain or an administrator can create the accounts for the computers ahead of time, which is what we are going to do now.

1. Right-click the "Head Office" OU and select "New" and then "Computer". Enter the name "HO-FileServer1" in the computer name field.
2. Create 3 more computer accounts called: "HO-ACCT1", "HO-MGMT2", "HO-SALES3"

### Part 3: Creating Domain Users

1. Right-click the "Head Office" OU, select "New" and then "User". The New Object – User dialog box will appear.
2. Create a new user by filling in the fields with the following information:

- First name: Samantha
- Last name: Moreno
- User logon name: smoreno

Click "Next"

- Password: P@ssw0rd
- Confirm Password: P@ssw0rd
- Uncheck "User must change password at next logon
- Check "Password never expires"

Click "Next"
Click "Finish"

3. When you return to the "Head Office" OU, double-click on "Samantha Moreno" and click the "Profile" tab.
4. Click the radio button for "Connect", set the letter to "H" and enter the following in the "To:" field:

```bash
\\S1-XXXXXX\Public\%username%
```

(Replace "S1-XXXXXX" with your server name, ex. S1-HHEIM)

4. Create the following 3 users in Head Office with the same password settings as Samantha:

- Ray Barone, rbarone
- Kevin Smith, ksmith
- Chris Reeves, creeves

### Part 4: Creating Domain Local Groups

A domain local security group is used when Active Directory is deployed. This type of group is typically used to manage resources in a domain and to give global groups from the same and other domains access to those resources. A Domain Local Group can contain user accounts, global groups and universal groups, but it is a good practice to only add groups to a local group.

1. In Active Directory Users and Computers, right-click the "Head Office" OU and select "New" and then "Group". Enter "Executives" as the Group Name, the select "Domain Local group" for the Group Scope, and leave the type as "Security".
2. Create 2 more Domain Local Groups this way named "SalesReps" and "Designers".

### Part 5: Creating Domain Global Groups

Creating Domain Global Groups is the same as Domain Local Groups. Typically, Global Groups contain user accounts, then the Global Groups are added to the Local Groups to gain access to resources who list the Local Group in the ACL (Access Control List).

1. In Active Directory Users and Computers, right-click the "Head Office" OU and select "New" and then "Group". Enter "HO-Executives" as the Group Name, the select "Global group" for the Group Scope, and leave the type as "Security".
2. Create 2 more Domain Global Groups named "HO-SalesReps" and "HO-Designers".

**Check-in:** At this point you should have 3 Global Groups and 3 Domain Local Groups created in the Head Office OU.

### Part 6: Modifying Group Membership

We will add users to a Global Group and then place the Global Group into a Domain Local Group.

1. Click the "Head Office" OU in the left pane so the contents of Head Office are displayed in the right pane.
2. Double-click your "HO-SalesReps" Global Group, and the groups Properties dialog will appear.
3. Click the "Members" tab, then click "Add" to add users to the Global Group. Click
   the Advanced button and then select Object Types. Ensure that only Users has a checkmark beside it. Then click Find Now. All the user accounts should appear in the list. These user accounts will be a combination of Local Users in the Users folder and Domain Users which we have created in the Head Office OU.
4. Select any 3 users from the "Head Office OU" (do NOT select any users from Lab 8), then click "OK". Click "OK" and the users should be added to your Global Group.
5. Now click the "Member" Of tab, and select "Add" to Add this Global Group to a Domain Local Group. Click Advanced, then Object Types. Make sure only Groups is selected. Then Find Now to see a list that contains only groups.
6. Find your Domain Local Group called "SalesReps". Select it and then click "OK",
   then "OK", then OK again on the "HO-SalesReps" Properties dialog box.
7. To verify that you have added your Global Group to the Domain Local Group double-click your SalesReps Local Group and click the Members Tab to see the Global Group listed.

## Investigation 3: Adding a Windows Client to your Domain

The last thing we will do is add our Windows Client VM to the Active Directory Domain we have set up on Server1. This is a very small but accurate example as to how things are typically done. In fact, when you log into the Seneca lab computers this is exactly what you are doing.

1. Begin by powering on and logging into your Client1 VM. Be sure to leave Server1 powered on while you do these next steps.
2. Double check that our Client1 VM is still set to use the static address you assigned in lab 4.
3. Click the blue Windows icon at the bottom of the screen and type "Workgroup". Click on "Change workgroup name."
4. Click on the "Change" button in the "System Properties" window.
5. Click the "Domain" radio button and enter your domain. (username.local)
6. You will be asked to provide a username and password. Use "administrator" and "P@ssword", the credentials you set up for your domain admin account. You should see a welcome message pop up.
7. Reboot Client1
8. When it comes to the log in screen, click on "Other User" in the bottom left corner and then click on "How do I sign in to another domain?"

Notice the instructions that pop up. When logging into a domain, we need to provide the domain and the username, at least initially. Let's log in using the smoreno user we created.

9. For the username use: **domainame\smoreno** (replace "domainname" with your actual domain name) and use "P@ssw0rd" as your password. Windows will perform a first time login script and will eventually land into the regular Windows desktop.
10. Open the File Explorer and navigate to "This PC". Notice that there is a network share drive, the "H:" drive.

This is the shared folder that we set up for this user in Active Directory. If we had another client VM, we could log in with the same user from that machine as well and access that same share folder. We can also access the "FF7" directory that was created in lab 8.

11. At the top of the File Explorer window, enter "\\Server1Name\" (for example "S1-hheim") and press "Enter".

You should see several directories including the "FF7" directory and the directories inside it. They have been shared and so anyone on the Domain can access them. This user cannot do anything in these folders because they have no permissions to do so but they can see what's there.

Let's try logging in as another user.

12. Click the blue Windows button, click on "Samantha Moreno", and then on "Switch user".
13. This time log in as "ksmith". This time you don't need to provide the domain as this machine is already connected to it. Once again, Windows will go through its first time login script and you will land on the Windows desktop.
14. Check "This PC" again. You should see the network share again, this time under "ksmith".

**Screenshot 7: Take a screenshot showing the output of the above commands**

## Lab 9 Sign Off

You should have 7 screenshots that you have been taking throughout this lab.

Put all 7 of these screenshots into a text document and upload it to the Lab 8 Submission page in Blackboard.
