---
id: lab1
title: Lab 1 - Preparing for the course
sidebar_position: 1
description: Preparing for the course, gathering necessary materials
---

# Lab 1: Preparing for the course

## Overview

This week's lab will cover the following:

- Gathering the necessary materials to run this course:
  - External hard drive
  - Windows Server 2025 ISO
  - Windows 11 ISO   
- Establishing a networking scheme for our virtual machines
- Using KVM to replicate physical network with a virtual one

## Lab 1 Notes

Most of the work in MST100 will be done on 3 Virtual Machines. It is highly recommended that you install these virtual machines using the Seneca lab computers as that is the environment that this (and all other) labs have been written and tested in. If you have a powerful enough laptop, you can install VMWare Workstation onto it and install the VMs on it. This can be useful as you will be able to complete the lab work at home as well as on campus.

But a word of warning...

If you choose to use your own laptop for this course, you assume all responsibility for ensuring the stability of your system. Your teacher will not be able to help you if you run into problems with your laptop.

## Objectives

By the end of this lab, you will be able to:

- Acquire Windows Server 2025 Datacenter and Windows 11 Education installation media and your individual product key from Azure Education and store them securely.
- Access VMware Workstation on a Seneca Lab PC (or install it on a personal PC).
- Establish a networking scheme for your virtual machines

## Investigation 1: Downloading Installation Media

In this investigation, you will download both ISOs required to install all our virtual machines for this course, along with serial keys assigned to your Seneca username.

### Part 1: Windows Server 2025 Datacenter

In this part, you will be downloading your Windows Server OS installation media by logging into your Seneca-based Azure account. You will also generate your personal serial keys for your copy of Server.

1. Navigate to the Microsoft Azure site:  [portal.azure.com](portal.azure.com)
2. Use your Seneca e-mail address and password to login.
3. Once on the main Azure page, look for the Search bar at the top of the page.
4. In the Search bar, type: Education, then hit Enter.
5. In the Education | Overview page, look to the left. You will see menu items already displayed on screen. (Overview, Learning resources, etc.)
6. Inside Learning resources in the left menu, click on Software.
7. In the main Software page, there is a Search bar just below the word Software (it says "Search" inside it.)
8. In that search field, type and enter: Windows Server 2025 Datacenter
9. In the item that appears below (there should only be one), click the link for Windows Server 2025 Datacenter.
10. On the right, an information box appears describing the software. Using your mouse to hover over this information box, scroll down to the bottom.
11. You should now see two items: View Key and Download
12. Click on Download first to begin downloading the Server 2025 ISO. You will need this for your operating system installation. (Don't forget where you've saved it!)
13. While the ISO file is downloading, click on View Key.
14. Copy this key into a text file that you save locally on your computer. You will need this for the Server installation and for any reinstalls later in the semester. DO NOT FORGET WHERE YOU SAVED THIS. We will be moving it to a secure location shortly.

### Part 2: Windows 11 Education

This part is similar to Part 1. We'll follow many of the same steps and download a copy of Windows 11 instead.

1. Log back into the Microsoft Azure website.
2. Go to: Education > Learning Resources > Software
3. In the search field, type and enter: Windows 11 Education
4. Download the ISO.
5. Click on View Key to save your Windows 11 Education serial key in a secure text file.
6. Reminder: Always store all serial keys in a secure location only you have access to.

Do not lose this key and do NOT share it with anyone!

### Part 3: Creating Your MST100 Working Directories and Storing the ISOs Inside

Now that we have our ISOs we are going to store them on our external hard drive so that we can use them to create our virtual machines in Lab 2. Before we actually move them, we are going to create a nice, organized directory structure for all of our MST100 lab work.

1. Plug your external hard drive into the computer you downloaded your ISOs onto and open the File Explorer.
2. Navigate to your external hard drive.
3. Create a directory called "MST100" and move into that directory.
4. Inside the MST100 directory create the following 5 directories:
  - "ISOs"
  - "Notes"
  - "Server1"
  - "Server2"
  - "Client1"
5. Finally, find the Windows Server 2025 Datacenter and Windows 11 Education ISOs on your computer (probably in your Downloads directory) and copy them into your newly created "ISOs" directory.
6. Find the text file that has your Windows security key in it and copy it into the "Notes" directory on your external hard drive.

**DO NOT LOSE THIS KEY. DO NOT SHARE THIS KEY WITH ANYONE.** You may want to also store a copy of this key in your OneDrive just in case your hard drive is rendered unusable.

Now you have your ISOs and security key in the right place and you have your directories ready to install to in next week's lab.

## Investigation 2: MST100 Pre-Lab Chart

The last thing we have to do is gather some information about our virtual machines and virtual network that we will be using later in the semester.

Below this picture of the chart, you will find a link to a downloadable file of the MST100 Pre-Lab Chart.

![MST100 Pre-Lab Chart](/img/mst100prelabchart2025pic.png)

[Download MST100 Pre Lab Chart](/files/MST100PreLabChart2025.docx)

You must download this file and fill it out completely.

## Lab 1 Sign Off

Upload your fully completed MST 100 Pre-Lab Chart to the Lab 1 submission page in Blackboard.
