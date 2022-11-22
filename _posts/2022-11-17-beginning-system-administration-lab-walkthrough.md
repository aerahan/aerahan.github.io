---
title: "Beginning System Administration Lab Walkthrough"
layout: post
date: 2022-11-17
feature_image: images/
tags: [networking, sysadmin]
---

This is a walkthrough of basic systems administration tasks along with creating a lab with virtual machines on VMWare Workstation. It will include setting up a lab infrastructure, using AD, and RAID. 

<!-- more -->

## Table of Contents
1. [Setting up infrastructure](#setting-up-the-lab-infrastructure)
2. 

<br>
<br>
<div align="center">»»————————————————————————————-　✼　-————————————————————————————««</div>
<br>
<br>

### Setting up the Lab Infrastructure (4 VMs)

1. [pfSense](#pfsense)
2. [Windows Server 2022](#windows-server-2022)
3. [Rocky Linux](#rocky-linux)
4. [Windows 10](#windows-10)

#### pfSense

##### Deploy

The beginning steps are to deploy virtual machines. We'll include a pfSense router, Windows 2022 Server, Windows 10 client, and a Rocky Linux client. pfSense allows access to the Internet while also keeping the virtual infrstructure isolated from the rest of the Internet/world, which is great for labs and practicing. Windows Server 2022 is a Windows Server Operating System from Microsoft, which supports enterprise-level management, communications, data storage, etc. Windows 10 clients and Rocky Linux clients are typical machines for every day use that you may see people use.

We'll start by configuring the virtual environment setting and manually configure network settings. This assumes the 4 virtual machines have been deployed. 

First, start by powering on the pfSense VM. When it has finished booting, it will display menu options like so: 
FIGURE1

##### LAN segment

You should now create a LAN segment for the entire virtual infrastructure. Remember, LAN stands for *Local Area Network* and the LAN segment is a portion/section of the entire LAN that is segmented - it's separated from the rest of the LAN through the use of a device such as a bridge, router, or switch. 

This can be done in VMware Workstation by right-clicking the VM tab, going into *Settings*, then changing the *Network Adapter* to be part of the LAN segment. If this is the first machine you are configuring, you'll have to add a new LAN segment. Give it any name. In this example, it will be `net01`. For pfSense in particular, it will likely have 2 *Network Adapter* options. One is the WAN interface (facing NAT) and the other is the LAN interface. You will want to use the LAN segment one. See Figure 2 below. 

##### Interfaces

To ensure the correct VMware adapter is associated with the right pfSense adaptor, the MAC address for the interfaces should be checked. On the same Virtual Machine Settings popup window, click *Advanced* next to the *LAN Segments* button and note the MAC address. 

For my example machines, the Network Adapter 2 MAC is `0:0C:29:94:AB:F6` (LAN) and the Network Adapter MAC is `00:0C:29:94:AB:EC` (WAN)

Knowing that information, we can now configure the interfaces on pfSense. 

Back on the pfSense VM, enter option `1`, which is *Assign interfaces*. Select no for setting up VLANs, and when it asks for the WAN interface name, check against the valid interfaces it shows you and match it to the MACs you recorded. Enter em0 or em1 accordingly, and do the other/same for the LAN interface name. Proceed. 

Since the WAN interface will receive its IP address from VMware, there is no need to manually configure it. The LAN interface, however, will need a static address. Use option `2`, *Set interface(s) IP address* from the menu to set an IP address. We will set the IP to be 192.168.1.254/24, meaning the network ID is 192.168.1.0/24. This is common convention - giving the gateway the high-end range of IP addresses (servers would get the low-end). Since it isn't a WAN interface, press Enter. Press enter again to skip past IPv6, and 'no' for enabling DHCP. 'Yes' for reverting back to HTTP. 

Figure 5

The gateway, pfSense, is all set! 

<br>
<br>

#### Windows Server 2022

##### Deploy

First, power on and log into a deployed Windows 2022 Server. Common passwords to use for practice are "student". You'll want to right click the VM tab and connect it to the previously made `lan01` LAN segment under *Network Adapter* (this should be done for every machine in order to connect to the Internet). 

##### Assign IP address, mask, gateway

It's time to assign an IP address, mask, and gateway statically. 

The gateway is the IP address that was assigned to pfSense earlier for the LAN interface. See figure 5 above for a reminder! It should likely be `192.168.1.254`. You can assign any static IP address in your range - let's do `192.168.1.1`, which leaves the subnet mask to be `255.255.255.0`. For the DNS server, we can input the loopback address `127.0.0.1`. 

You can edit these settings within the *Internet Protocol Version 4 (TCP/IPv4) Properties*, which can be found by going to the Control Panel, *Network and Internet*, and then *Network and Sharing Center*. From there, 'Change adapter settings', which will show you your Ethernet connections. Right click and select 'Properties'. Refer to Figure 6 below.

From there, double click on *Internet Protocol Version 4 (TCP/IPv4)* and enter the information previously decided. This is modeled in Figure 7. 

##### pfSense Dashboard

Open a web browser and in the search bar, enter the IP address of the pfSense LAN interface, which for us is our default gateway address `192.168.1.254`. It will take you to the pfSense Dashboard, where you can log in with the credentials "admin" and "pfsense" for username and password (if using RIT's Virtual Machine deployments for labs). You will be led through the setup - which you can select defaults for all except step 2, where the pfSense hostname and domain information should be added in. the Hostname is probably 'pfSense', and for domain, I'll be using `aera123.com`. 

When done, you'll be taken back to the dashboard. Time to deploy the two client machines!


#### Rocky Linux

Create and deploy a linked clone for Rocky Linux. Make sure to connect it to the LAN segment. 

##### Hostname

Open the `Terminal`. Log in as root using `su` (normally not good!) and change the hostname with the command `hostnamectl set-hostname enterFQDNHere` where 'enterFQDNHere' is where the Fully Qualified Domain Name should go. Mine will be `rocky.aera123.com` so the hostname is `rocky` and the domain name is the previous `aera123.com`. 

To check and ensure the hostname has been set, use the command `hostname`. 

##### IP Address

Let's set the static IP address for now. Go to *Settings*, *Network*, and click the Gear button next to *Wired*. Click to the *IPv4* tab and select `Manual` for the IPv4 Method. Enter the information (we'll assign 192.168.1.2 for this machine).

Verify the IP address has been set correctly with the command `ip a` or `ifconfig`. 


#### Windows 10

Create and deploy a linked clone for Windows 10. Make sure to associate the virtual NIC to the LAN segment. 

##### Hostname
Run Windows PowerShell as an administrator and use the command `Rename-Computer -NewName "enterNewNameHereInQuotes"` to assign a hostname. The system will have to be restarted for the hostname change to take place. 


##### IP address
The IP address can easily be assigned by following the same steps as the Windows 2022 Server by going through the Control Panel. I'll assign mine `192.168.1.3`. 

Verify that the correct hostname and IP address has been assigned by using the command `ipconfig /all`. See Figure 11 below. 

That's it for setting up! Time to move on to Active Directory! 



## References & Resources
### References


### Extra Resources to Check Out!



Samba: https://www.linuxshelltips.com/install-samba-rockylinux-almalinux/

starting nfs service https://www.ibm.com/docs/ru/psfa/1.6?topic=software-configuring-nfs-export
