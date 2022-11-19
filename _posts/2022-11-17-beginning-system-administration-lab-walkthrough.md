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
3. 

##### pfSense

The beginning steps are to deploy virtual machines. We'll include a pfSense router, Windows 2022 Server, Windows 10 client, and a Rocky Linux client. pfSense allows access to the Internet while also keeping the virtual infrstructure isolated from the rest of the Internet/world, which is great for labs and practicing. Windows Server 2022 is a Windows Server Operating System from Microsoft, which supports enterprise-level management, communications, data storage, etc. Windows 10 clients and Rocky Linux clients are typical machines for every day use that you may see people use.

We'll start by configuring the virtual environment setting and manually configure network settings. This assumes the 4 virtual machines have been deployed. 

First, start by powering on the pfSense VM. When it has finished booting, it will display menu options like so: 
FIGURE1

You should now create a LAN segment for the entire virtual infrastructure. Remember, LAN stands for *Local Area Network* and the LAN segment is a portion/section of the entire LAN that is segmented - it's separated from the rest of the LAN through the use of a device such as a bridge, router, or switch. 

This can be done in VMware Workstation by right-clicking the VM tab, going into *Settings*, then changing the *Network Adapter* to be part of the LAN segment. If this is the first machine you are configuring, you'll have to add a new LAN segment. Give it any name. In this example, it will be `net01`. For pfSense in particular, it will likely have 2 *Network Adapter* options. One is the WAN interface (facing NAT) and the other is the LAN interface. You will want to use the LAN segment one. See Figure 2 below. 

To ensure the correct VMware adapter is associated with the right pfSense adaptor, the MAC address for the interfaces should be checked. On the same Virtual Machine Settings popup window, click *Advanced* next to the *LAN Segments* button and note the MAC address. 

For my example machines, the Network Adapter 2 MAC is `0:0C:29:94:AB:F6` (LAN) and the Network Adapter MAC is `00:0C:29:94:AB:EC` (WAN)

Knowing that information, we can now configure the interfaces on pfSense. 

Back on the pfSense VM, enter option `1`, which is *Assign interfaces*. Select no for setting up VLANs, and when it asks for the WAN interface name, check against the valid interfaces it shows you and match it to the MACs you recorded. Enter em0 or em1 accordingly, and do the other/same for the LAN interface name. Proceed. 

Since the WAN interface will receive its IP address from VMware, there is no need to manually configure it. The LAN interface, however, will need a static address. Use option `2`, *Set interface(s) IP address* from the menu to set an IP address. We will set the IP to be 192.168.1.254/24, meaning the network ID is 192.168.1.0/24. This is common convention - giving the gateway the high-end range of IP addresses (servers would get the low-end). Since it isn't a WAN interface, press Enter. Press enter again to skip past IPv6, and 'no' for enabling DHCP. 'Yes' for reverting back to HTTP. 

pfSense is all set! 

<br>
<br>

#### Windows Server 2022







## References & Resources
### References


### Extra Resources to Check Out!



Samba: https://www.linuxshelltips.com/install-samba-rockylinux-almalinux/

starting nfs service https://www.ibm.com/docs/ru/psfa/1.6?topic=software-configuring-nfs-export
