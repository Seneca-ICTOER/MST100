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

In lab 4, we changed Server1's IP address and network settings so that it could no longer connect to the Internet. We are going to need the Internet for this lab so we are going to temporarily revert back to the old settings. We will change back to a static IP at the end of this lab.

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
get-service
```

You will see a list of all the services available on Server1 and their status.



