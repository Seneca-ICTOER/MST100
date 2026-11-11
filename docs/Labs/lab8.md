---
id: lab8
title: Lab 8 - Local Users and Groups
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

You will be asked to take screenshots along the way in this lab. It is easier to do so rather than having to go back at the end of the lab to take them. The criteria for some screenshots may not easily be replicated once you are at the end of the lab so be sure to take them when told to do so.

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

- User Name: CStrife
- Password: P@ssw0rd
- Password hint: the same password we use for everything

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

### Part 5: Creating More Users and Groups

1. Using whichever method you prefer, create the following groups (you can add whatever description you'd like):

- Ranged
- Melee
- Magic

2. Using whichever method you prefer, create the following users (use "P@ssw0rd" for all their passwords, remember to uncheck "User must change password at next logon" and check "Password never expires" for all of them):

- Username: BWallace
- Full Name: Barret Wallace
- Description: Environmentalist

- Username: VValentine
- Full Name: Vincent Valentine
- Description: Emo Vampire

- Username: YKisaragi
- Full Name: Yuffie Kisaragi
- Description: Ninja

3. Configure your groups and users as follows:

- Group: Avalanche
- Members: AGainsborough, CStrife, TLockheart, BWallace

- Group: Shinra
- Members: CStrife, VValentine,

- Group: Ranged
- Members: BWallace, VValentine, YKisaragi

- Group: Melee
- Members: CStrife, TLockheart, CHighwind

- Group: Magic
- Members: AGainsborough

**Screenshot 5: Take a screenshot showing which users are members of the "Avalanche", "Shinra", "Ranged", "Melee", and "Magic" groups.**

## Investigation 3: Create Shares and Set Permissions

### Part 1: Creating Share Permissions

We are now going to apply Share and Permission settings to a set of folders for the users and groups we have created.

1. Create a directory on your C: drive on Server1 called "FF7". Inside of that directory create 3 directories called "Magic", "Melee", and "Ranged".
2. Before we can actually apply any settings to this directory, we have to turn off a built in default setting that will hinder our ability to set specific shares and permissions ourselves. In the File Explorer, look near the top of the window and you should see three dots. Click those three dots and then select "Options".
3. Click on the "View" tab and scroll down until you see "Use Sharing Wizard". Uncheck this box, click "Apply", and then click "OK".
4. In the File Explorer, navigate back to your C: drive.
5. Right-click your "FF7" directory and select "Properties". Click on the "Sharing" tab and then on "Advanced Sharing".
6. Check the "Share this Folder" box and then click "Permissions".
7. Check the box that allows "Full Control" for everyone, then click "Apply" and then "OK".
8. Back in the "Advanced Sharing" window, click "Apply" and then "OK".
9. Now we will set the permissions for the "Magic" directory. Navigate to the Permissions window for this directory like you did with the first one (Right click on directory >> Properties >> Sharing >> Advanced Sharing >> Permissions)
10. Click "Add", type in "Magic", click "Check Names" (you should see S1-USERNAME\Magic appear), then click "OK".
11. Highlight the "Magic" group and change the permissions to "Full Control".
12. Add the "Shinra" group and give them "Full Control" as well.
13. Add the "Ranged" group and give them only "Read" access.

What we have just done is set the "Share" permissions. This is one side of permissions that are applied to users and groups. These can technically be used to access directories and resources across the network. Next we will set local permissions. Both are necessary to allow for our users and groups to actually access what we want them to. Remember the principle of "Least Privilege". The most restrictive permissions will always take effect. So we will now set the same local permissions as we did with the share permissions.

14. Go into the "Properties" for the "Magic" directory and this time click on "Security".
15. Click "Edit" and add the "Magic" group and give it "Full Control". Do the same for the "Shinra" group.
16. Add the "Ranged" group and leave the default permissions it is assigned.
17. Repeat steps 9-16 for the "Melee" and "Ranged" directories giving them the following permissions (remember to do this for both Share and Local permissions):

- Directory: Melee
- Permissions:
- Shinra - Full Control
- Melee - Full Control
- Avalanche - Read

- Directory: Ranged
- Permissions:
- Shinra - Full Control
- Ranged - Full Control
- Melee - Read

### Part 2: Testing our Share and Local Permissions

Now that we have set up our shares and permissions, let's test them out to see exactly what we have done.

1. Sign out of your current user by clicking the blue Windows icon at the bottom of the screen, clicking on "Administrator" and then clicking "Sign Out".
2. Sign in as "Cloud Strife".
3. Open Notepad, write a few words, and then save the file as "sharefile1.txt" in the "Melee" directory. Repeat this step two more times to create "sharefile2.txt" in the "Magic" directory and "sharefile3.txt" in the "Ranged" directory.
4. Sign out again and log back in as "Aerith Gainsborough".
5. Using the File Explorer, navigate to the "Magic" directory, open "sharefile2.txt", make a change to the file, and then save it.
6. Now try to do the same with the "sharefile1.txt" in the "Melee" directory. What happens when you try to modify that file?

The "AGainsborough" user is part of the "Magic" and "Avalanche" groups so they have full permissions to create and modify anything in the "Magic" directory. But they only have "Read" permissions for the "Melee" directory so they can look but not touch anything in that directory.

Lets try one more user.

7. Sign out of the AGainsborough account and log in as "Barret Wallace".
8. Once logged in, try to modify the "sharefile1.txt" in the "Melee" directory and the "sharefile2.txt" in the "Magic" directory.

Again, you should find that the difference in permissions here means that this user can modify one of those files but the other file they cannot.

9. Sign out of this user account and log back in as "Administrator".
10. Open PowerShell and run the following 3 commands:

```bash
Get-SmbShareAccess -Name "Magic"
Get-SmbShareAccess -Name "Melee"
Get-SmbShareAccess -Name "Ranged"
```

**Screenshot 6: Take a screenshot showing the output of the above commands**

11. Run the following commands in PowerShell:

```bash
Get-ACL "C:\FF7\Magic").Access | Select-Object IdentityReference, FileSystemRights
Get-ACL "C:\FF7\Melee").Access | Select-Object IdentityReference, FileSystemRights
Get-ACL "C:\FF7\Ranged").Access | Select-Object IdentityReference, FileSystemRights
```

**Screenshot 7: Take a screenshot showing the output of the above commands**

## Lab 8 Sign Off

You should have 7 screenshots that you have been taking throughout this lab.

Put all 7 of these screenshots into a text document and upload it to the Lab 8 Submission page in Blackboard.
