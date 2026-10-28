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
3. Enter the following command in the command prompt:

```bash
ipconfig
```

Notice the IPv4 Address, Subnet Mask, and Default Gateway fields. What does this information tell us?

4. Now enter:

```bash
ipconfig /all
```

Notice that we are given a lot more information. So what are we looking for?

There are several pieces of information here that are useful to us. The result of the first command we ran gives us the IP address and its subnet mask, as well as the default gateway (or the router/device it looks to in order to send data outside of the local network.

The second command gives us more details about the networking information associate with this machine. Specifically for us, we are interested in the DHCP information and the DNS information.

The DNS server is the server that is providing translations for IP addresses to Fully Qualified Domain Names and vice versa. We will be creating our own DNS server later on in this lab.

The DHCP server is the server that is providing this machine with an IP address. When a computer with DHCP enabled comes online, it reaches out to the network via a broadcast to find a DHCP server. If it finds one, the two machines perform a DORA (Discover, Offer, Request, Acknowledge) exchange and the DHCP server assigns an IP address to the machine that has just come online. The computer will keep this IP address for as long as it has a lease for it. You can also find the lease information in the output that we got from the "ipconfig /all" command. Using the two lease time stamps provided, we can determine how long the DHCP server has leased this IP to this machine. When we are done with part 1 here, we will change this machine to use a static IP address instead of a DHCP assigned one.

5. Take a look at the "Lease Obtained" and "Lease Expires" lines in the output of "ipconfig /all". Now enter:

```bash
ipconfig /release
```

What happened?

6. Run "ipconfig /all" again and take a look at the "Lease Obtained" line. What changed?
7. Now enter:

```bash
ipconfig /renew
```

What happened?

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

11. Now try:

```bash
ping -n 10 google.com
```

What changed about the ping output? There are times when network stability cannot be guaranteed or ascertained. So you may send out a ping and the first 4 pings fail but later pings may succeed. This is not typical but there are times when running extended pings can be useful.

12. Finally, try:

```bash
ping -t google.com
```

What happened? How many pings are used? Press Ctrl+C when you get tired of watching pings.

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
10. Click the "Use the following DNS server addresses" button and fill in the two fields as such:
    Preferred DNS server: 10.10.xxx.10 (replace xxx with your dedicated number)
    Alternate DNS server: Leave Blank
11. Click "OK", then click "OK" again, then click "Close".
12. Bring up your command prompt and use "ipconfig" to confirm that the correct IP address and subnet mask are assigned to Server1.

**Screenshot 1: Take a screenshot of your Server1 command prompt showing the the correct IP address and subnet mask**

15. Shut down Server1.

### Part 3: Addressing Server2

Next, we will change the IP information on Server2. This is actually easier than Server1 because we will be using Powershell.

1. Power on and log into Server2
2. Enter "15" to go into the Powershell command line.
3. Enter the following commands:
   **- Note - Be careful with these commands. Typos will cause problems. Double check your commands before running them.**
   **- Note - Your network adapter may not be named "Ethernet0" the output of the "Get-NetAdapter" command will tell you your adapter name.**
   **- Note - Replace "xxx" with your assigned number.**

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
3. Open the command prompt and run "ipconfig"

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

Your ping should fail (likely with a "request could not find host..." error).

The primary reason is that Server1 does not have a default gateway. We intentionally left that configuration blank. Because of this, Server1 does not know where to send data if the target is not on its local network. Server1, Server2, and Client1 have all been configured in such a way that they can no longer connect with the Internet or anything external to their local network. However, they can still connect to each other. Let's try that next.

4. Power on Server2.
5. Log in and navigate to the Powershell command line.
6. Confirm that your IP address and subnet mask are still 10.10.xxx.20 and 255.255.255.0.
7. Try to ping Server1 from Server 2:

```bash
ping 10.10.xxx.10
```

What happened? Your ping requests should have timed out. The problem now does not have to do with our network setup. Our problem now is the Windows Firewall.
You will go into more detail regarding packet filtering and firewalls (both Windows and others) in other courses but right now our problem is that the Windows Firewall is blocking our ping requests. This will happen on all 3 VMs because all 3 have Windows Firewall running and the firewall will not allow ICMP traffic (the protocol used by pings) to get through. Let's fix this.

8. Still in Server2, run the following command:

```bash
New-NetFirewallRule -DisplayName "Allow Pings" -Direction Inbound -Protocol ICMPv4 -IcmpType 8 -Action Allow
```

9. Verify that the rule has been added successfully:

```bash
Get-NetFirewallRule -DisplayName "Allow Pings"
```

10. Switch over to Server 1, open Powershell as an Administrator and run the same commands:

```bash
New-NetFirewallRule -DisplayName "Allow Pings" -Direction Inbound -Protocol ICMPv4 -IcmpType 8 -Action Allow
Get-NetFirewallRule -DisplayName "Allow Pings"
```

11. Now power on Client 1, open Powershell as an Administrator and run the same commands:

```bash
New-NetFirewallRule -DisplayName "Allow Pings" -Direction Inbound -Protocol ICMPv4 -IcmpType 8 -Action Allow
Get-NetFirewallRule -DisplayName "Allow Pings"
```

12. We can now try to ping Server1 and Server2 from Client1:

```bash
ping 10.10.xxx.10
ping 10.10.xxx.20
```

13. Switch back over to Server1 and ping Server2 and Client1 from Server1:

```bash
ping 10.10.xxx.20
ping 10.10.xxx.100
```

**Screenshot 4: Take a screenshot showing successful pings from Server1 to Server2 and Client1**

**Screenshot 5: Take a screenshot showing successful pings from Client1 to Server1 and Server2**

Now that we have full connectivity between our VMs, we can start working on a few basic network services.

## Investigation 3: Configuring DNS and IIS

In lab 3, we installed the DNS and IIS roles onto our Server1 but we did not make any changes to those services. We are going to use these services to create a small, simple webpage that will be accessible from Client1.

Keep in mind that in order for this to work, Server1 must have a static IP address because it is hosting these services. The IP address cannot change otherwise other computers would not be able to reliably find it. This principle is the same whether we are talking about finding a web server on a tiny little network like ours or a website on the Internet.

### Part 1: Configuring IIS on Server1

First, we will make a small change to the default webpage that comes with IIS, just so we can differentiate it from the default one.

1. In Server Manager, from the Tools pull-down menu (in the upper-right corner of the Server Manager), find and run "Internet Information Services (IIS) Manager".
2. In the left pane, expand the tree under your server name until "Default Web Site" appears.
3. Right-click on the Default Web Site and choose Explore.
4. From the new Window you can modify your starter web site by opening the file iisstart (the HTML file, NOT the PNG file) in Notepad. You will be editing HTML code in this file. To make a basic modification to the site, change the title of the page by modifying the text that appears between the \<title\> tags to include your name. You may also make any other modifications you like (not required). Save and exit the file.
5. To view your web page locally, open a web browser, and type 127.0.0.1 in the browser address bar. (Remember, 127.0.0.1 is the loopback address and not your actual IP address.) The default IIS webpage should appear (an image with a bunch of blue squares and "Welcome" written in different languages), and the tab title should have your changes. As long as you can see your name in the tab title you have edited the HTML file correctly.

**_MAYBE USE AI HERE TO MAKE A SIMPLE HTML PAGE??_**

### Part 2: Configuring DNS on Server1

Now that our webpage is updated, we need to set up DNS so that we can access that webpage using a Fully Qualified Domain Name (FQDN) instead of an IP address. To accomplish this, we will be mapping the IP address of our Server1 to a domain.

1. In Server Manager, from the Tools pull-down menu, select DNS. The DNS Manager will appear, displaying your server name, which should be your SenecaID (not a name that starts with WIN) in both the left and right panes.
2. Expand the tree for your server [>], then right-click Forward Lookup Zone, and select New Zone.
3. Click Next. Make the zone a Primary Zone. Click Next.
4. Type in a zone name for your web server. Use your Seneca ID + .com (ex. hheim.com).
5. Click Next and allow the wizard to create the zone file for you. Click Next. Ensure "Do not allow dynamic updates" is selected. Click Next and click Finish.
6. Expand the Forward Lookup Zone in the left pane until you see your zone name (senecaID.com). Right-click on your new domain name and choose to Add New Host. The New Host dialog box will appear.
7. Enter "server1" in the "Host" field and add the IP address of your web server (the IP address that was manually configured for your installation of Windows Server on Server1) to the IP address field. You do not need to create a pointer record.
8. Click Add Host, and then OK when you get your success message. Click Done to close the window.

### Part 3: Testing our Web Server

Before we move on to testing our configurations, take a moment to recall how we configured Server1, Server2, and Client1's network settings. In addition to the IP address and subnet mask, we also entered in a preferred DNS IP address. This was the IP address of Server1 (10.10.xxx.10). What this means is that all 3 VMs will look to Server1 to resolve IP addresses to FQDNs and vice versa. Now that we have created a host (A) record, if we try to access the FQDN via a web browser we should connect to the Web Page that is being hosted on Server1 since its FQDN matches its IP address in the host (A) record.

1. To test the new domain name locally, open your web browser and type the domain name you entered above (http://server1.senecaID.com), in the address bar field. Your webpage should display as it did before. This proves that your DNS Server (that you just configured) is resolving the name (server1.senecaID.com) to the correct IP address to find the web page on your web server. (Note - make sure you specify http and not https. It will want to default to https.)

If your webpage does not appear, check your Preferred DNS settings on Server1 to make sure you have entered the correct IP address (10.10.xxx.10) and that you created the host (A) record correctly.

Now let's test to see if we can connect to this web server from Client1.

2. Log in to Client1, open a web browser and enter "http://server1.senecaid.com"

You should see the same web page you saw on Server1 and can confirm it is the same page if you see your name in the tab title.

If your webpage does not appear, check your Preferred DNS settings on Client1 to make sure you have entered the correct IP address (10.10.xxx.10).

**Screenshot 6: Take a screenshot showing that you have accessed the web page on Client1**

### Part 4: Adding Authentication to our Web page

Our example in this lab is very simple and straightforward. We mapped the IP address of our web server (Server1, 10.10.xxx.10) to the FQDN http://server1.senecaid.com. Also, this web page is only accessible on our local virtual network. We are not letting the outside world access it. Only our own VMs can access it so we aren't really worried about security at this point. However, security is ALWAYS something we need to worry about in the real world. So we are going to add a level of it to our web page by requiring authentication to access the website.

1. Using Server Manager on Server1, click IIS in the left pane. Your server name should appear in the right pane. Right-click the server and select Add Roles and Features from the menu.
2. Select Next a couple of times until you reach the screen with the roles displayed.
3. Scroll down to your Web Server Role and expand it [>] to see the available roles and features.
4. Select Web Server. Then select Security. Place a checkmark next to "Basic Authentication".
5. Click Next, Next, and Install. Close the window once the installation completes.
6. Open the IIS Manager (Tools >> IIS). Expand the tree to find your Default Web Site.
7. Double-click on the Authentication icon in the middle pane.
8. Disable Anonymous Authentication and Enable Basic Authentication.

We have now turned basic authentication on but we still need to create the credentials that will be allowed to access the web page.

9. Open a Command Prompt as an Administrator and type the following command:

```bash
net user webuser P@ssw0rd /ADD
```

This creates a new user account on the server, called "webuser" and the account has a password of "P@ssw0rd". 4. Test your website again on Client1 (you may have to refresh your web page). What is the difference now?

**Screenshot 7: Take a screenshot showing the authentication window appearing on the web page on Client1**

You should be able to use your new webuser account to login to gain access to the website.

## Investigation 4: Connecting from Client1 to Server2 using RDP

The last thing we are going to do is enable Remote Desktop on Server2 so that we can connect to it from our Client1 VM. This is very useful as we generally do not log in directly to Server Core machines. As a security measure, we stick to remote connecions.

1. Log into Server 2 and find "Remote desktop" on the Sconfig screen. It should be number 7. Enter "7".
2. Enter "E" to enable it and then enter "2".
3. Reboot your Server2.
4. When Server2 comes back up, do not log in. Switch over to Client1.
5. On Client1, press the Windows key, type "run" and press "Enter".
6. Enter "MSTSC" and hit "Enter". The "Remote Desktop Connection" window will appear.
7. Enter "10.10.xxx.20" (Server2) into the "Computer" field and click "Connect".
8. You will be asked for your Server2 credentials. Enter them.
9. Click "Yes" when it asks you about a certificate.
10. You should successfully connect to your Server2 from Client1 via RDP.

**Screenshot 8: Take a screenshot showing the successful RDP connection from Client1 to Server2**

## Lab 4 Sign Off

You should have 8 screenshots that you have been taking throughout this lab.

Put all 8 of these screenshots into a text document and upload it to the Lab 4 Submission page in Blackboard.
