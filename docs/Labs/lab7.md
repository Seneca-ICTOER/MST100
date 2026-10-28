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

**Screenshot 3: Take a screenshot showing what happens when you run "Start-PSAdmin".**

Now let's try something with a bit of utility. We are going to add a multi-line function called "Get-SmallFiles". This function has a user-defined variable called "$size". This function displays all the files that are smaller than the user-defined variable and excludes directories.

5.  At the PS prompt, enter:

```bash
Function Get-SmallFiles {
  Param($Size)
  Get-ChildItem | Where-Object {
    $_.Length -lt $Size -and !$_.PSIsContainer
  }
}
```

_Note - as you enter this into PowerShell, the "\{" and "\}" special characters will allow you to use multiple lines for the function. You also do not need to worry about spacing or tabs. PowerShell will take care of that for you as well._

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

**Screenshot 4: Take a screenshot showing the output of "Get-SmallFiles -Size 50".**

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

_Note - Unlike when you first created the "Get-SmallFiles" function, the tab spacing will not be done automatically for you in the "$Profile" file. For clarity, the first line has 0 tabs, the second and third line have 1 tab, the fourth line has 2 tabs, the fifth line has 1 tab, and the last line has 0 tabs._

**Screenshot 5: Take a screenshot showing the contents of your "$Profile" file.**

3. Save your file and close it. Also, close your PowerShell window.

4. Open a new PowerShell window and test to make sure your module import and two functions are working.

## Investigation 4: Introduction to PowerShell Scripting

Scripting is an integral part of being any kind of administrator. Scripts will make your life easier and will allow you to automate many tasks and process all sorts of system and network based information.

That said, the idea of scripting and how it is used in practice is entirely dependant on who is using it and all kinds of other factors surrounding your local and network environment, security policies, goals of your company, and just so much more.

It is a wide and deep ocean of which PowerShell is only a small part. It is unlikely that you will become a scripting expert of any kind doing solely what is taught to you in this course or any other courses you take.

You will not learn all that much from slides and lectures.

**You need to actually _do_ scripting.**

In this course/lab we are going to just barely scratch the surface of PowerShell scripting. This may seem daunting (and it is) but you will come to better understand scripting and how to effectively write and use scripts the more your write and use scripts.

And remember, being bad at something is just the first step towards being kinda ok at something.

### Part 1: PowerShell ISE

Before we actually get into scripting, you should be introduced to PowerShell ISE (Integrated Scripting Environment). ISE is basically a text editor and PowerShell terminal in one application that has many useful features and tools for scripting and testing of scripts.

Let's open it up.

1. Click the Windows icon and type "PowerShell".
2. Click on "Windows PowerShell ISE".
   The application will open and it will look similar to a standard PowerShell terminal but you will notice many other buttons and tools available.

Notice on the right hand side there is a list of CMDLETs. You can type into the "Name" field at the top to search for specific ones and you can further refine your search by changing the "Modules" dropdown to a specific module (like Hyper-V for example).

You can use the main PowerShell terminal in the middle as normal.

3. Use it to navigate to your "MyScripts" directory with the following:

```bash
cd c:\MyScripts
```

Notice that as you do so, PowerShell ISE will give you an auto-fill option for your directory. This is called "Intellisense" and it will recommend how to finish your commands. You can hit the "Tab" key on your keyboard to accept a suggestion.

4. Now let's open the Script Pane. Click on "View" > "Show Script Pane".

Your PowerShell ISE window will be split in two, with the PowerShell terminal below and the newly visible Script Pane above.

The Script Pane allows us to create scripts but it also has a neat feature in that it allows us to run CMDLETs, scripts, or parts of scripts for testing without actually saving a script file.

5. Enter the following in the Script Pane:

```bash
Get-Date
Get-ChildItem
```

6. Find the green "Run Script" arrow above the Script Pane and click it. Notice what happens.
   The entries for the script you are writing above are processed and their output appears in the PowerShell terminal below. This is super useful when we are creating scripts because it allows us to test chunks of our script without needing an entire one.

7. Close PowerShell ISE. Click "No" when asked it you want to save changes.

### Part 2: Using Variables

In the context of scripting, a variable is a string that contains a value. What that value is will depend on how the variable is created and what is stored in it.

Let's try creating a simple variable, storing a value in it, and then recalling that value.

1. Open PowerShell ISE. The Script Pane should open as well. Open it if it does not.
2. Enter the following into the Script Pane:

```bash
$name= Read-Host "What is your name?"
```

3. Click the green "Run Script" arrow above the Script Pane and take a look at the PowerShell terminal (with the blue background). You should see that you are being asked for your name.
4. Enter your name in the PowerShell terminal and hit "Enter". What happened?
5. Now, in the PowerShell terminal, enter the following:

```bash
echo $name
```

You should see your name appear.
What we have just done is stored a value in a variable called "name", denoted by the ($) in front of it. That special symbol in front of a string tells us that it is a variable. Variables are very useful in scripting because it allows us to apply sets of instructions to all sorts of inputs depending on what we want our script to do.

Let's enhance our script a bit using another variable.

6. First, let's actually save our script so we can access it later. In PowerShell ISE, click "File" and then "Save As". Name your script "MyFirstScript" and save it in your "MyScripts" directory.
7. Now, below the line asking for a name, add the following to your script:

```bash
$age= Read-Host "How old are you?"
```

8. Click the green "Run Script" arrow above the Script Pane. You will get a message stating the script you are about to run will be saved. Click "OK" and take a look at the PowerShell terminal below. Answer the two questions.
9. In the PowerShell terminal, enter the following:

```bash
echo $name
echo $age
```

We have now stored two variables: the "name" variable and the "age" variable. To do this we used the "Read-Host" CMDLET. This allows us to ask the user to input a string of data which is captured and stored in the variables we specified. Now that we have these variable full of data we can actually do something with them. The simplest way to use the data is to write it back out to the screen. To do that we will use the "Write-Host" CMDLET.

10. Back in the Script Pane, enter this next line into your script and then click the green "Run Script" arrow:

```bash
Write-Host "Hello $name. Welcome to $env:COMPUTERNAME"
```

Notice that your script now has output. We captured the user's input and wrote it back out to them using the "name" variable we created. We also used what's called an "Environment Variable", something that is baked in to all instances of Powershell. In this case we used the "COMPUTERNAME" environment variable which pulls the name of our computer and we injected that into the output of our script. But what about that "age" variable? We are going to use that to learn about conditional logic.

### Part 3: Conditional Logic

Conditional logic is the foundation on which most scripting operates. There are many types of conditional logic but we are going to focus on the most basic: If Statements.

If statements allow us to test conditions and take different actions based on what the result of that test is. In this next step, we are going to test against our "age" variable and output different messages to the user based on what age they enter.

1. Begin by adding these next few lines to your script:

```bash
if ($age -lt 30) {Write-Host "You are young."}
if ($age -gt 30) {Write-Host "You are old."}
```

2. Click the green "Run Script" arrow. When the script runs, enter your name as usual and enter "20" for the age. Notice the output.
3. Test run the script again, this time entering "40" for the age. Notice the output.

Depending on what number you enter for the "age" variable, you will get a different output. There are a few problems with this logic, however.

4. Test run the script again. This time, when it asks for your age enter "Banana". What happens?

The script will likely output "You are old." even though you did not enter a number.

5. Now try running it again. This time, when it asks for your age enter "30". What happens?

You will likely not receive any output for the age question because the number you entered is neither greater than or less than 30.

This is a problem. We have not accounted for invalid data input by the user.

In our current example, accounting for invalid user data is fairly simple. Let's begin by fixing the easier issue.

6. Change the current line in your script:

```bash
if ($age -gt 30) {Write-Host "You are old."}
```

to

```bash
if ($age -ge 30) {Write-Host "You are old."}
```

7. Now test run the script again, entering "30" for the age. Notice that now the script gives an output for that number.

**Screenshot 6: Take a screenshot showing the contents of the Script Pane and the Powershell terminal below having test run your script with the two possible conditions.**

To fix the issue of the user not giving us a valid number, there are two things we have to worry about here. First, we don't want the user to give us something that isn't a number. Second, we don't want the user giving us a negative number since people can't be "negative" years old. To accomplish this, we are going to use "elseif" and "else" to restructure our script a little.

8. Replace your two if statement lines with the following:

```bash
if ($age -notmatch '^\d+$') {
    Write-Host "Please enter a valid non-negative number."
}
elseif ([int]$age -lt 30) {
    Write-Host "You are young."
}
else {
    Write-Host "You are old."
}
```

We have changed a few things here in our conditional logic to ensure that the user can ONLY give us valid and usable data.

Take a look at the first if statement:

```bash
if ($age -notmatch '^\d+$') {
    Write-Host "Please enter a valid non-negative number."
}
```

This tests to see if the user entered a regular number. It does this using the "-nomatch" flag and something called a "regular expression". You will learn more about these in your OPS145 class. This particular regular expression essentially matches only a string that begins with (^) and ends with ($) a number (\d) or a set of numbers (+). This condition isn't perfect because it will match _any_ regular number (635819 would be acceptable for example) so it won't disallow ages that are wildly unrealistic but for the purposes of our script it works nicely. If the user gives us anything else, a message is sent back to tell them their input was not valid.

The next condition is pretty much the same as our original if statements:

```bash
elseif ([int]$age -lt 30) {
    Write-Host "You are young."
}
```

The difference here is that we are using "elseif" instead of just "if" so the logic only processes if the first condition fails. The "[int]" ensures that whatever is stored in the "age" variable is converted to an integer. This isn't necessary in our example but it is a good idea when working with numbers.

Finally we have:

```bash
else {
    Write-Host "You are old."
}
```

Using "else" is the same as saying "if none of the above conditional statements succeeded, do this". Since we have two conditional statements that cover invalid inputs and any number under 30, all that's left are numbers over 30 and so we can safely attach our "You are old." message to the "else" since we know that if the script gets to this point, the user must have input a number of 30 or higher.

9. Test run your script to see the differences in output.

We can also expand on what conditions we are applying to our logic.

10. Change your conditions to match the following:

```bash
if ($age -notmatch '^\d+$') {
    Write-Host "Please enter a valid non-negative number."
}
elseif ([int]$age -lt 30) {
    Write-Host "You are young."
}
elseif ([int]$age -ge 30 -and [int]$age -lt 60) {
    Write-Host "You are old."
}
else {
    Write-Host "Oh wow, you are ANCIENT!"
}
```

11. Test run your script 3 more times, using 10, 40, and 70 for the ages and notice the different outputs.

We now have a functioning script that uses variables and conditional logic to output a few different pieces of information. However, we have skipped one of the most important parts of scripting: comments.

### Part 4: Commenting

Comments are important because they explain each part of our script. This is useful for us as the script creator for when we walk away from a partially completed script to come back to it later but also in case someone else wants to use our script. The comments ensure anyone looking through it understands what the script is meant to do and how it works. So let's add comments to our scritp. We will also clean up our script and add in a date capture while we are at it.

1. Change your entire script to:

```bash
#Script Purpose: Gather users name and age. Output user's name and computer name and a message based on age entered
#Author: Your Name (replace with your actual name)
#Date Created: Today's Date (Enter the actual date)

#Ask the user for their name and then store their input in a variable
$name= Read-Host "What is your name?"

#Ask the user for their age and then store their input in a variable
$age= Read-Host "How old are you?"

#Insert an empty line to clean up output
Write-Host ""

#Output a message to the user saying hello to them and welcoming them to the current computer
Write-Host "Hello $name. Welcome to $env:COMPUTERNAME"

#Insert an empty line to clean up output
Write-Host ""

#Store the current date in a variable and then use it to tell the user the current date
$date= Get-Date -Format D
Write-Host "The current date is: $date"

#Insert an empty line to clean up output
Write-Host ""

#Test the user's age and output an appropriate message based on how old they are (or ask them to provide valid data if they did not)
if ($age -notmatch '^\d+$') {
    Write-Host "Please enter a valid non-negative number."
}
elseif ([int]$age -lt 30) {
    Write-Host "You are young."
}
elseif ([int]$age -ge 30 -and [int]$age -lt 60) {
    Write-Host "You are old."
}
else {
    Write-Host "Oh wow, you are ANCIENT!"
}
```

Congratulations! You have created your first fully functioning, fully commented, clean looking PowerShell script! Feel free to play around with it a bit.

**Screenshot 7: Take a screenshot showing the entire contents of the Script Pane.**

### Part 5: Execution Policy

There are 4 PowerShell script policies:

- Unrestricted – Allow all scripts.
- RemoteSigned* – Allow local scripts and remote signed scripts. *default setting
- AllSigned – Allow only signed scripts (local or remote).
- Restricted – No scripts will run.

Script signatures are sort of like stamps or seals. At the risk of oversimplifying... adding one to a script means that is has been certified by its creator to operate a certain way and do a certain task. Changing the script invalidates the signature. So essentially, this is a way of ensuring only scripts that are safe are run on the system. With that said, a good administrator NEVER runs a script they don't understand.

1. You can run the following command to get your currently running execution policy:

```bash
Get-ExecutionPolicy
```

Your system should be set to "RemoteSigned" by default.

2. You can change the execution policy with:

```bash
Set-ExecutionPolicy Unrestricted
```

3. You will get a warning message about this. Click "Yes to All".
4. Run the following:

```bash
Get-ExecutionPolicy
```

**Screenshot 8: Take a screenshot showing the output of the Set-ExecutionPolicy command.**

5. Change your policy back to RemoteSigned:

```bash
Set-ExecutionPolicy RemoteSigned
```

6. Click "Yes to All" again.

## Lab 6 Sign Off

You should have 8 screenshots that you have been taking throughout this lab.

Put all 8 of these screenshots into a text document and upload it to the Lab 4 Submission page in Blackboard.
