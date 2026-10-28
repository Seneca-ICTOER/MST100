---
id: lab6
title: Lab 6 - Introduction to Windows Powershell
sidebar_position: 6
description: Using Powershell Commands.
---

# Lab 6: Introduction to Windows Powershell

## Overview

This week's lab will cover the following:

- Learning basic Powershell commandlets
- Starting and stopping services using Powershell
- Aliases
- Powershell profiles

## Lab 6 Notes

Windows PowerShell is an updated, more powerful version of the command line. It is essentially a combination of a Windows Command Line and a scripting environment rolled into one. It can help with repetitive tasks, processes acting on many files at once, automating and scheduling tasks, and configuring Windows components and services.

While PowerShell is a command line interface, it is more powerful and flexible than the traditional Windows Command Line. It supports complex decision making,
connecting to a wide variety of data sources, and can even be used when building graphical user interfaces.

PowerShell is now an essential skill for IT and server administrators. Windows IT professionals who only use a GUI interface for server and network management will still need to know how to use PowerShell as it is by far the most efficient method to accomplish many tasks. It is constantly being developed and there are hundreds of commands.

PowerShell uses commands called CMDLETS (pronounced command-lets). They are English-like commands and use a VERB-NOUN format.
It is easy for Linux and Windows Command Line users to get a quick start with PowerShell because of built-in aliases and syntax that is easy to pick up on the fly.

## Objectives

By the end of this lab, you will be able to:

- Learn how to use Powershell CMDLETS and syntax
- Use a variety of Powershell CMDLETS to:
  - start and stop services
  - display system information
  - set and use aliases
- Create and maintain a Powershell profile

## Investigation 0: Reconnecting to the Internet

In lab 4, we changed Server1's IP address and network settings so that it could no longer connect to the Internet. We are going to need the Internet for this lab so we are going to temporarily revert back to the old settings. We will change back to a static IP at the end of lab 7.

1. Using what you learned in lab 4, change Server1's network settings back to "Obtain an IP address automatically" and "Obtain DNS server address automatically".
2. Reboot Server1.
3. When Server1 is back up, confirm you can connect to the Internet by opening your web browser and going to a website.

## Investigation 1: Customizing a Shortcut to Point to a Home Directory

In this exercise we will configure the Powershell Console to open in a directory called “MyScripts.” We will be doing everything on Server1.

1. To open Powershell, type Powershell from the Run command, or click the Powershell icon on the task bar.
2. First, let's create our new directory. Enter the following command:

```bash
New-Item C:\MyScripts -type directory
```

You should see a message appear confirming the creation of the new directory.

3. Now create 3 files in that directory:

```bash
New-Item C:\MyScripts\file1.txt -type file
New-Item C:\MyScripts\file2.txt -type file
New-Item C:\MyScripts\file3.txt -type file
```

Again, you should see confirmation of your commands successfully running.

4. Now let's create a special shortcut on our desktop that we can use to open Powershell in a specific directory. Navigate to your desktop.
5. Click the blue Windows icon at the bottom of the screen. You should see a "Windows Powershell" icon. If you don't, type in "Powershell" and it will appear.
6. Right-click the "Powershell" icon and click on "Open file location". A window will appear that contains two "Powershell" icons.
7. Right-click the icon **that doesn't have the (x86)** in its name and click "Properties". A window with a few fields will appear. Look for the "Target:" field.
8. Copy the contents of the "Target:" field.
9. Close the last two windows you opened so that you can see your desktop. Right click anywhere on the desktop that is empty and click on "New" > "Shortcut". The "Create Shortcut" window will appear.
10. Paste what you copied from the "Target:" field in step 8 into the window and click "Next".
11. Rename the shortcut "MyPowershell" and click "Finish". Your new shortcut should now appear on your desktop.
12. Right-click your new shortcut and click on "Properties".
13. Replace the "Start in" field with the path to your "MyScripts" directory: C:\MyScripts
14. Click "Apply" and then "OK".
15. Now launch Powershell using your newly created shortcut. You should find that when it launches, the command line shows that you are located in your C:\MyScripts directory.
16. You can further customize your Powershell experience. Notice the small downward facing arrow at the top of the Powershell window (right next to the +). Click that arrow and then click "Settings". There are several ways you can customize the way your Powershell terminal looks and behaves here. Feel free to change anything you like before moving forward.

**Screenshot 1: Take a screenshot showing that your Powershell terminal opens to the C:\MyScripts directory**

## Investigation 2: Using Powershell CMDLETs

PowerShell commands have been standardized using a “verb-noun” naming convention known as a cmdlet. This standard simplifies the learning curve and provides a description of what the CMDLET does.

Let's try a few basics.

1. To see a list of CMDLETs available in PowerShell enter the following cmdlet:

```bash
get-command
```

Unfortunately, this command will output a lot of information so we would have to scroll all the way back up to see it all.

2. Let's try that again but using "more" to make the output more manageable:

```bash
get-command | more
```

That will allow us to scroll through the output more cleanly. (You can hit "q" while in the list to get back to the prompt)

3. Next, let's list all the commands that use a specific verb, such as "Get". Enter the following:

```bash
get-command -Verb Get
```

Again, you can pipe to "more" to make the output more manageable if you like.

You can use the previous two examples to find any CMDLET that exists on your system.

### Part 1: Getting Help

The "Help" CMDLET noun is very useful in retrieving information about particular CMDLETs.

1. To begin, enter the following:

```bash
get-help
```

Scroll through the output. Notice the part where it mentions downloading and installing help files.

Our system may not be fully updated with all available CMDLETs and associated help files. Let's take care of that now.

1. Close the Powershell window
2. Right-click your Powershell shortcut on the desktop and click "Run as administrator"
3. Now enter the following into Powershell:

```bash
Update-Help
```

Powershell will likely have a lot to update so be patient and let it do its thing. You may get an error at the end of the update mentioning "UI culture(s)". If that is the only error you get, you can safely ignore it.

Now that our Powershell help files are up to date let's take a look at a few.

1. In Powershell, enter:

```bash
Get-Help Get-Childitem
```

The output will display syntax usage, a description, and related information for the command. The "Get-Help" CMDLET is a useful tool when you are getting started with a CMDLET you haven't used yet.

2. You can also use this CMDLET with aliases. Try:

```bash
Get-Help dir
```

Notice that the output of this command shows you the same thing as when we ran "Get-Help" with "Get-Childitem". That is because "dir" is an alias of "Get-Childitem". We will come back to aliases later in this lab.

3. Next, try:

```bash
Get-Help Get-Childitem -Full
```

This outputs a MUCH more detailed help page for the CMDLET. If you look around in it you will also find that it lists the built-in aliases for the CMDLET in the "NOTES" section (in this case "ls", "dir" and "gci"). This is useful to remember for longer CMDLETS.

4. You can also get some examples of the CMDLET being used with:

```bash
Get-Help Get-Childitem -Examples
```

These are included in the output when using the "-Full" option as well but this option gives you less to scroll through.

5. Let's try one more:

```bash
Get-Help Get-Childitem -Online
```

Your default web browser should open and bring you to the official Microsoft page for that CMDLET.

6. In the next section you will be using CMDLETs to manage services on Server1. Use "Get-Help" to learn about the the CMDLETs you will be using:

```bash
Get-Help Get-Service
Get-Help Start-Service
Get-Help Stop-Service
```

### Part 2: Using CMDLETs to Start and Stop Services

There are several CMDLETs associates with the "Service" noun:

- Get-Service
- New-Service
- Restart-Service
- Set-Service
- Start-Service
- Stop-Service
- Suspend-Service

1. We are going to use a few of these with a simple service but first let's take a look at the services available on our computer with:

```bash
Get-Service
```

You will see a list of all the services available on Server1 and their status.

2. We can get service information for a specific service with the "-name" option:

```bash
Get-Service -name Spooler
```

3. Notice that the Spooler service is currently running. Let's stop it:

```bash
Stop-Service -name Spooler
```

4. Now, check its status again:

```bash
Get-Service -name Spooler
```

You should see the status has changed from "Running" to "Stopped".

**Screenshot 2: Take a screenshot showing that the Spooler service is stopped**

5. Before we move on, start it back up again:

```bash
Start-Service -name Spooler
```

**Screenshot 3: Take a screenshot showing that the Spooler service is Running**

## Investigation 3: Aliases

Aliases are strings that reference CMDLETs. Powershell has several built-in aliases (including many Linux commands) to make it easier for people to get started with Powershell, even if they are used to different operating system.

### Part 1: Finding Built-In Aliases

You have already seen a few of these earlier in the lab (the aliases for Get-Childitem). There are a couple CMDLETs that are useful when looking for existing aliases.

1. In Powershell, enter:

```bash
Get-Alias
```

The output of this CMDLET will show you a list of aliases on the left and what CMDLET they reference on the right.

2. You can also check specific CMDLETs for what aliases they have. Try:

```bash
Get-Alias -Definition Get-Childtem
```

You should get a listing of all the aliases associated with the "Get-Childitem" CMDLET. Try running each alias and see what the output looks like. They should all be the same.

### Part 2: User-Defined Aliases

While the built-in aliases are useful, the real power in aliases comes from the ability to create our own. You can add however many aliases for however many CMDLETs you'd like to better suit your environment or administration style.

However, user-defined aliases are not permanent. They exists as long as the Powershell session is active and are removed when the session is closed. Before we get around that problem, let's test it out to see what happens.

1. Run the following in Powershell:

```bash
Set-Alias gs Get-Service
```

2. Now run the alias you just set:

```bash
gs
```

You should get the output of the "Get-Service" CMDLET.

4. Now try:

```bash
gs -name Spooler
```

You should get the status of the Spooler service.

**Screenshot 4: Take a screenshot showing the Spooler status output coming from the "gs" alias**

5. Now, close Powershell, open it again, and try running your "gs" alias again. What happened? You should be met with an error.

6. Add the alias again:

```bash
Set-Alias gs Get-Service
```

7. Now run:

```bash
Get-Alias -Name gs
```

This will show you what CMDLET the "gs" alias is mapped to.

**Screenshot 5: Take a screenshot showing the "gs" alias mapped to the "Get-Service" CMDLET.**

There are a few reasons why aliases are not permanent. In particular, we don't want old aliases hanging around when we start scripting as they can cause problems if we don't know they are there or if we forget what aliases have been set.

There is a way to make them permanent if we want to, however...

### Part 3: Creating a Profile

You can create a PowerShell profile to customize your environment and to add session-specific elements to every PowerShell session that you start.

A PowerShell profile is a script that runs when PowerShell starts. You can use the profile as a logon script to customize the environment. You can add commands, aliases, functions, variables, snap-ins, modules, and PowerShell drives.

We are going to stick to a simple example here but Powershell Profiles are extremely useful to administrators because you can tailor Powershell sessions to your specific local and network environment.

"$PROFILE" is a special system variable that references a file representing the users specific Powershell startup script.

1. In Powershell run:

```bash
$PROFILE
```

You should get a file path pointing to your current profile.

2. Try to edit this file with:

```bash
Notepad $Profile
```

You should be met with an error. Click "OK" and then close the Notepad window. You received an error because no profile actually exists yet. Let's create one.

3. Enter the following command:

```bash
New-Item -Itemtype File -Path $Profile -Force
```

You should receive output showing that the file has been created.

4. Now let's try opening it again:

```bash
Notepad $Profile
```

An empty file should open up.

5. Let's add our "gs" alias to it. Enter the following into the file:

```bash
Set-Alias gs Get-Service
```

Save the file and close it.

6. Close Powershell and open it again.
7. Try running:

```bash
gs -name Spooler
```

Your alias should be working.

8. Run:

```bash
type $profile
```

This will give you the contents of your profile file.

**Screenshot 6: Take a screenshot of the output of "type $profile" which should have your alias entry.**

## Lab 6 Sign Off

You should have 6 screenshots that you have been taking throughout this lab.

Put all 6 of these screenshots into a text document and upload it to the Lab 4 Submission page in Blackboard.
