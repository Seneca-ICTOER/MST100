---
id: lab7
title: Lab 7 - Windows Powershell Continued
sidebar_position: 7
description: Installing features, importing modules, functions, and scripting with Powershell.
---

# Lab 7: Windows Powershell Continued

## Overview

This week's lab will cover the following:



## Lab 7 Notes



## Objectives

By the end of this lab, you will be able to:
- Install Featured
- Import Modules
- Create Functions
- Use ISE to create a script
- Run a script

## Investigation 1: Importing Modules

We can use Powershell to add features just like we do with the Server Manager. In fact, you already did this back in lab 3 on Server2 (when you added and removed Print Services).

### Part 1: Adding a New Feature

Let's try adding Hyper-V to Server1.

1. In Powershell enter the following:
```bash
Add-WindowsFeature Hyper-V-Tools
```
2. Once that finished installing, enter the following:
```bash
Add-WindowsFeature Hyper-V-Powershell
```
You should get "Success" confirmation messages for both installations. 

**Screenshot 1: Take a screenshot showing the two Hyper-V features were added successfully.**

Now that we have our new features installed, we can move on to its modules.

### Part 2: Importing a Module for our New Feature

A module is a package that contains PowerShell commands, such as CMDLETs, providers, functions, workflows, variables, and aliases.

People who write commands can use modules to organize their commands and share them with others.

After a module is imported, you can use the module members in your session.

Some modules come pre-imported (installed). Some modules are on the server but need to be installed before you are able to use the CMDLETs and function in them.

1. To find the modules that have already been imported into your session, at the PowerShell prompt, type:
```bash
Get-Module
```
2. You can also get a listing of modules that are installed but not yet imported into your session:
```bash
Get-Module -ListAvailable
```
3. To import a module we use the "Import-Module" CMDLET. Let's add the Hyper-V module:
```bash
Import-Module Hyper-V
```
If the CMDLET works, you should get no output.

4. Now let's see what that module added:
```bash
Get-Command -Module Hyper-V
```
You should be met with a list of all the CMDLETs that the Hyper-V module added to Powershell.

**Screenshot 2: Take a screenshot showing the output of "Get-Command -Module Hyper-V".**

5. Remember you can always use the "Get-Help" CMDLET for help with new CMDLETS but you will probably have to update your help files:
```bash
Update-Help
```
You can ignore any errors. Then:
```bash
Get-Help New-VM
```

6. Now generally there is no point in removing modules since they are lost once you close the session but you can technically do so manually with:
```bash
Remove-Module Hyper-V
```

## Investigation 2: Creating a Function

A function is a list of Powershell statements that has a name that you assign. When you run a function, you type the function name. The statements in the list run as if you had typed them at the command prompt. It is like a mini-script or a complex CMDLET or alias with parameters and options.

We will create two simple functions.

1. Begin by closing all Powershell windows if you have any open. 
2. Next, right-click the MyPowershell shortcut on your desktop and click "Run as administrator".
3. Enter the following:
```bash
Function Start-PSAdmin {Start-Process PowerShell -Verb RunAs}
```
4. Now try running your new function:
```bash
Start-PSAdmin
```
A new PowerShell window should appear.

Now let's try something with a bit of utility. We are going to add a multi-line function called "Get-SmallFiles". This function has a user-defined variable called "$size". This function displays all the files that are smaller than the user-defined variable and excludes directories.

5.  At the PS prompt, enter:
```bash
function Get-SmallFiles {
  Param($Size)
  Get-ChildItem | Where-Object {
    $_.Length -lt $Size -and !$_.PSIsContainer
  }
}
```
*Note - as you enter this into PowerShell, the "\{" and "\}" special characters will allow you to use multiple lines for the function. You also do not need to worry about spacing or tabs. PowerShell will take care of that for you as well.*

6. Before we run this function, let's take a step to help show that it works. We already have 3 empty files in our "MyScripts" directory. Let's add another file that has something in it. Run:
```bash
Copy-Item -Path "C:\Windows\iis.log" -Destination "C:\MyScripts\iiscopy.txt"
```

7. Now, make sure you are in your "MyScripts" directory and get a listing of its contents (You should know a few ways to do this now!) and note what the output is.

8. Finally, run your function for files smaller than 50 bits:
```bash
Get-SmallFiles -Size 50
```
Notice the difference in output. Your function is only returning the empty files because they are smaller than the specified size. 

## Investigation 3: Updating Profile with Modules and Functions

The problem with the modules and functions we've added and used in this lab is that they disappear when we close our PowerShell session. However, just like an Alias, we can put these into our Profile so that they are available whenever we open a new session.

1. Run:
```bash
Notepad $profile
```
2. Inside the file, add the module import and functions you have done so far in this lab:
```bash
Import-Module Hyper-V

Function Start-PSAdmin {Start-Process PowerShell -Verb RunAs}

function Get-SmallFiles {
  Param($Size)
  Get-ChildItem | Where-Object {
    $_.Length -lt $Size -and !$_.PSIsContainer
  }
}
```
*Note - Unlike when you first created the "Get-SmallFiles" function, the tab spacing will not be done automatically for you in the "$Profile" file. For clarity, the first line has 0 tabs, the second and third line have 1 tab, the fourth line has 2 tabs, the fifth line has 1 tab, and the last line has 0 tabs.*







