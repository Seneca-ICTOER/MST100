---
id: lab4
title: Lab 4 - Introduction to Windows Networking and Network Services
sidebar_position: 4
description: Configuring basic network settings in Windows Server and Windows 11 and setting up DNS and IIS.
---

# Lab 4: Introduction to Windows Networking and Network Services

## Overview

This week's lab will cover the following:

- Analyzing and configuring network settings on Windows Server and Windows 11
- Configuring and testing DNS
- Configuring and testing IIS
- Using RDP


## Lab 4 Notes



## Objectives

By the end of this lab, you will be able to:
- Configure Server1, Server2, and Client1 with static IP addresses
- Test connectivity between your VMs
- Configure DNS and add DNS records
- Create and connect to a basic web page using IIS
- Use RDP to log into Server2 from Client1

## Investigation 1: Exploring Network information and Assigning Static IP Addresses to your Virtual Machines

Right now, our virtual machines are all set to receive their IP addresses automatically via DHCP from the host OS. We are going to change them all to use a static IP address so that their IP address never changes.

### Part 1: Finding IP information on Server1

1. Begin by powering on and logging in to Server1.
2. Click the blue windows icon at the bottom of the screen, type "cmd", and press "Enter". A Windows Command Prompt window will appear.
3. Type in "ipconfig" and press "Enter". Notice the IPv4 Address, Subnet Mask, and Default Gateway fields. What does this information tell us?
4. Now enter "ipconfig /all". Notice that we are given a lot more information. So what are we looking for?

There are several pieces of information here that are useful to us. The result of the first command we ran gives us the IP address and its subnet mask, as well as the default gateway (or the router/device it looks to in order to send data outside of the local network.

The second command gives us more details about the networking information associate with this machine. Specifically for us, we are interested in the DHCP information and the DNS information. 

The DNS server is the server that is providing translations for IP addresses to Fully Qualified Domain Names and vice versa. We will be creating our own DNS server later on in this lab.

The DHCP server is the server that is providing this machine with an IP address. When a computer with DHCP enabled comes online, it reaches out to the network via a broadcast to find a DHCP server. If it finds one, the two machines perform a DORA (Discover, Offer, Request, Acknowledge) exchange and the DHCP server assigns an IP address to the machine that has just come online. The computer will keep this IP address for as long as it has a lease for it. You can also find the lease information in the output that we got from the "ipconfig /all" command. Using the two lease time stamps provided, we can determine how long the DHCP server has leased this IP to this machine. When we are done with part 1 here, we will change this machine to use a static IP address instead of a DHCP assigned one.

5. Take a look at the "Lease Obtained" and "Lease Expires" lines in the output of "ipconfig /all". Now enter "ipconfig /release". What happened?
6. Run "ipconfig /all" again and take a look at the "Lease Obtained" line. What changed?
7. Next, run "ipconfig /renew". What happened?
8. Run "ipconfig /all" again and take a look at the "Lease Obtained" line. What happened?

The "ipconfig /release" and "ipconfig /renew" commands do similar things. The "/release" option releases the DHCP assigned IP address from the machine. The "/renew" option does the same thing but then also immediately asks for a new one. So why did Server1 still have a DHCP IP address when we ran "ipconfig /all" after running "ipconfig /release"?. The short answer is: VMware. There are many small factors that differ between actual physical networks and virtual networks and this is one of them. You will discover several others as you proceed through this program. 

Either way, it is important to remember the distinction between those two commands. 

Keep in mind that right now, VMware is effectively acting as the default gateway, DNS server, and DHCP server. Due to the virtual nature of our network, VMware and the host OS control all of these networking services (again, another intricacy of a virtual network). 
  
Now let's do some basic network testing.

9. Enter "ping google.com" into the command prompt. You should see 4 replies appear. This tells you that your Server1 machine was able to contact the website "google.com" (probably hosted on a 142.x.x.x IP address). Pings are a useful tool to test network connectivity to devices that are on both our internal network and networks external to our own.
10. Try the following 3 commands:
```bash
ping localhost
ping loopback
ping 127.0.0.1
```
What were the outputs of these commands? What were you pinging?

The loopback address (referenced as 127.0.0.1 or localhost or loopback) is a special IP address that is on all systems. It represents the system that is running it so when you ping it you are having Server1 ping itself. This may not sound super useful (and with pings it isn't) but it is very useful when testing network services that the server is providing. We will be using it later in this lab.

11. Now try inputting "ping -n 10 google.com". What changed about the ping output? There are times when network stability cannot be guaranteed or ascertained. So you may send out a ping and the first 4 pings fail but later pings may succeed. This is not typical but there are times when running extended pings can be useful.
12. Finally, try inputting "ping -t google.com". What happened? How many pings are used? Press Ctrl+C when you get tired of watching pings.

Now that you have a general idea of how to get basic network information, we are going to change our network a little so that we have better control over it and so that we can begin to work with network services.

### Part 2: Addressing Server1 

1. Minimize your command prompt window. 
2. Click the blue windows icon at the bottom of the screen, type "control panel", and press "Enter".
3. In the top-right of the Control Panel window, change from "Category" view to "Large Icons".
4. Click on "Network and Sharing Center".
5. Click "Change adapter settings" on the left hand side of the window.
6. You will see one adapter labeled "Ethernet". Double-click it.
7. Click "Properties" in the window that appears.
8. Double-click "Internet Protocol Version 4 (TCP/IPv4)".
9. Click the "Use the following IP address: button and fill in the three fields as such:
  IP address: 10.10.xxx.10 (replace xxx with your dedicated number)
  Subnet Mask: 255.255.255.0
  Default Gateway: Leave Blank
11. Click the "Use the following DNS server addresses" button and fill in the two fields as such:
  Preferred DNS server: 10.10.xxx.10 (replace xxx with your dedicated number)
  Alternate DNS server: Leave Blank
13. Click "OK", then click "OK" again, then click "Close".
14. Bring up your command prompt and use "ipconfig" to confirm that the correct IP address and subnet mask are assigned to Server1.  

**Screenshot 1: Take a screenshot of your Server1 command prompt showing the the correct IP address and subnet mask**

15. Shut down Server1.

### Part 3: Addressing Server2

Next, we will change the IP information on Server2. This is actually easier than Server1 because we will be using Powershell.

1. Power on and log into Server2
2. Enter "15" to go into the Powershell command line.
3. Enter the following commands:
- Note - Be careful with these commands. Typos will cause problems. Double check your commands before running them.
- Note - Your network adapter may not be named "Ethernet0" the output of the "Get-NetAdapter" command will tell you your adapter name.
- Note - Replace "xxx" with your assigned number.

```bash
Get-NetAdapter
Set-NetIPInterface -InterfaceAlias "Ethernet0" -Dhcp Disabled
Get-NetIPAddress -InterfaceAlias "Ethernet0" -AddressFamily IPv4 | Remove-NetIPAddress -Confirm:$false
New-NetIPAddress -InterfaceAlias "Ethernet0" -IPAddress 10.10.xxx.20 -PrefixLength 24
```

4. Reboot Server2.
5. When it comes back up log in, go back to the command line, and run "ipconfig" to confirm your IP settings have saved.

**Screenshot 2: Take a screenshot of your Server2 command prompt showing the the correct IP address and subnet mask**

6. Shut down Server2.

### Part 3: Addressing Client1

1. Power on and log in to Client1.
2. Using the same instructions you used for Server1, give Client1 the following configurations:
  IP address: 10.10.xxx.100
  Subnet mask: 255.255.255.0
  Default gateway: Leave Blank
  Preferred DNS Server: 10.10.xxx.10 (replace xxx with your dedicated number)
  Alternate DNS Server: Leave Blank 
4. Open the command prompt and run "ipconfig"

**Screenshot 3: Take a screenshot of your Client1 command prompt showing the the correct IP address and subnet mask**

5. Power off Client1

## Investigation 2: Windows Firewall and Packet Filtering

Earlier in this lab, you used pings from Server1 to test connectivity with the outside world. However, that was when the IP addressed was still being assigned by DHCP. Let's see what happens now that we have statically assigned an IP address to our VMs.

1. Power on Server1, log in, and bring up the command prompt.
2. Confirm that your IP address and subnet mask are still 10.10.xxx.10 and 255.255.255.0.
3. If they are, try pinging google.
```bash
ping google.com
```
Your ping should fail (likely with a "request could not find..." error).

The primary reason is that Server1 does not have a default gateway. We intentionally left that configuration blank. Because of this, Server1 does not know where to send data if the target is not on its local network. Server1, Server2, and Client1 have all been configured in such a way that they can no longer connect with the Internet or anything external to their local network. However, they can still connect to each other. Let's try that next.

4. Power on Server2.
5. Log in and navigate to the Powershell command line.
6. Confirm that your IP address and subnet mask are still 10.10.xxx.20 and 255.255.255.0.
7. 
