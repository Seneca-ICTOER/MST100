---
id: lab8
title: Lab 8
sidebar_position: 8
description: Local Users and Groups
---

# Lab 8: Local Users and Groups

## Overview

This week's lab will cover the following:

- Workgroups
- Adding and Modifying Users
- Creating and Modifying Groups
- Shares and Permissions

## Lab 8 Notes

## Objectives

By the end of this lab, you will be able to:

-

## Investigation 1: Creating and Joining a Workgroup

By default, if you do not make any changes to Server after installing, your server is in a
workgroup called "WORKGROUP".

Let's change that to be more specific to our use case.

1. Logon to your server. Open Server Manager, click Local Server, select the blue WORKGROUP.
2. The System Properties dialog box should appear. Click the Change button. The Computer Name/Domain Changes screen appears allowing you to change your workgroup. In the Workgroup field, type SENECA, and then click OK. You should receive a Welcome to the SENECA Workgroup message. Click OK.
3. Restart your computer.
4. Once your computer has restarted, use the same procedure to verify you are in the SENECA workgroup.

**Screenshot 1: Take a screenshot showing Server1 is now part of the SENECA workgroup.**

## Investigation 2: Users and Groups

### Part 1: Using the User Accounts Option in the Control Panel to Create Users

The most basic way to add and modify users on MS Server is with the "User Accounts" option in the Windows Control Panel.

1. Open the Control Panel (use Windows key and start typing Control Panel. It should appear in the list on the right) and select "User Accounts". Select "Manage another account".

_What accounts are currently on Server1? How did they get there? What are their properties?_

2. Click "Add a user account".

You will see the blue "Add a user" window appear. Add a user with the following information:

User Name: CStrife
Password: P@ssw0rd
Password hint: the same password we use for everything

3. Click "Next". Notice that the user is being added as a "Local Account". Click "Finish".
4. Back on the "Choose the user you would like to change" screen, select the new CStrife user account.
5. Click "Change the account type". Note that you can only make a Standard User (default) or Administrator.

6. Create another user but make this user an administrator. Give this user account your name (use your Seneca username, whatever is before the @ sign in your Seneca email) and use "P@ssw0rd" for it's password.

**Screenshot 2: Take a screenshot showing the users listed in the User Accounts window (there should be 4 now - 2 admins and 2 regular local users).**

7. Close the User Accounts tool.

### Part 2: Create and Manage User Accounts Using Local Users and Groups Tool

We can also use the Server Manager to deal with users. We are also able to create and modify groups here.

1. From Server Manager, click "Tools" > "Computer Management". Then expand "Local Users and Groups".
2. Select "Users". _What accounts exist on your server? What are the users you have created? Which ones are there by default?_
3. Right-click on "CStrife", and select "Properties". The CStrife Properties dialog box appears. Flip through all the tabs related to the CStrife account. Note which groups this account belongs to on the "Member Of" tab.
4. Change the Full Name field to say "Cloud Strife" and enter a description “Soldier 1st Class” for the CStrife account. Click "OK".
5. Open the account with your name. Make the same changes to the account with your name and description “Local Administrator”. Using the "Member Of" tab, see which groups this account is a member of.
6. Notice the changes you made appear in the list of users pane in the tool.
7. Right-click on Users and select New User. Enter the following information in the New User dialog box:

- Username: TLockheart
- Full Name: Tifa Lockheart
- Description: Bar Manager
- Password and Confirm Password: P@ssw0rd
- Uncheck the "User must change password at next logon" box.
- Check the "Password never expires" box.
- Click Create.

8. Create another user account with the following information:

- Username: SHighwind
- Full Name: Sid Highwind
- Description: Pilot
- Password and Confirm Password: P@ssw0rd
- Uncheck the "User must change password at next logon" box.
- Check the "Password never expires" box.
- Click Create.
- Click Close.

9. Right-click "SHighwind" a and select "Rename". Change the name to "CHighwind". Go back into the properties of this user account and change the full name to: "Cid Highwind".

10. Create a new user with whatever information you'd like.
11. Delete the extra user account that you just created in step 10. Notice the warning you receive when doing so. _What does that mean? Why does MS Server do this?_
    **Screenshot 3: Take a screenshot showing the users listed with their Full Name and Descriptions in the Users window in Server Manager.**

### Part 3: Local Groups

Next we will work with some local groups in the Server Manager.

1. Click "Groups", under "Local Users and Groups".
2. Right-click "Groups" and select "New Group". Create a group named "Avalanche", with a description of "Environmentalist Group". Click Create.
3. Create another group called "Shinra" with a description of "Energy Company".
4. Create another group called "Jenova" with a description of "Reunion".
5. Double-click on the "Avalanche" group. Click "Add" to add members to the group.
6. Enter "TLockheart" in the "Enter the object names to select". Click "Check Names". Click "OK". You will see TLockheart has been added to the members list. Click "OK".
7. Go back to the list of users and double-click "CStrife". Click the "Member Of" tab and click the "Add" button.
8. Click "Advanced" and then on the Select Groups dialog box, click "Find Now". In the Search results, select "Avalanche" and "Shinra" (use CTRL + Click to select both at the same time) and then click OK.

Check that "Avalanche" and "Shinra" groups are listed in the "Enter the object names to select" box and click "OK".

In CStrife's Properties, see the groups that this user is now a member of, then click OK.

6. Go back to the list of Groups and right-click on the "Avalanche" group and select "Properties". Notice that TLockheart and CStrife are now members of the "Avalanche" group.
7. Right-click the "Jenova" group and select Rename. Rename it to "Reunion".
8. Delete the "Reunion" group. Notice the warning message that appears when you delete a group. _What does that mean? Why does MS Server do this?_

**Screenshot 4: Take a screenshot showing which users are members of the "Avalanche" group.**

### Part 4: Creating Users and Groups on the Command Line

We can also create users and groups on the command line.

1. Open an Administrative Command Prompt and enter the following to create a new user:

```bash
net user AGainsborough P@ssw0rd /add /comment:”Healer” /expires:never /fullname: "Aerith Gainsborough” /passwordchg:no
```

2. Now enter the following to add Aerith to the "Avalanche" group:

```bash
net localgroup “Avalanche” AGainsborough /add
```

3. Go back and check the list of users in the Server Manager. You may have to refresh the list but when you do you will see your new user there. You can check their properties to see that they have also been successfully added to the "Avalanche" group.
