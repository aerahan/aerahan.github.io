---
title: "Beginner Systems Administration"
layout: post
date: 2022-11-17
feature_image: images/linux_background.png
tags: [networking, sysadmin]
---

This is a walkthrough of basic systems administration tasks along with creating a lab with virtual machines on VMWare Workstation. It will include setting up a lab infrastructure, using AD, and RAID. 

<!--more-->

## Table of Contents
1. [Setting up infrastructure](#setting-up-the-lab-infrastructure)
2. [Active Directory](#active-directory)
3. 
4. [RAID Partitioning](#raid-partitioning)

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

<br>
<div align="center">»»——————————-　✼　-——————————««</div>
<br>

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
<div align="center">»»——————————-　✼　-——————————««</div>
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

<br>
<div align="center">»»——————————-　✼　-——————————««</div>
<br>

#### Rocky Linux

Create and deploy a linked clone for Rocky Linux. Make sure to connect it to the LAN segment. 

##### Hostname

Open the `Terminal`. Log in as root using `su` (normally not good!) and change the hostname with the command `hostnamectl set-hostname enterFQDNHere` where 'enterFQDNHere' is where the Fully Qualified Domain Name should go. Mine will be `rocky.aera123.com` so the hostname is `rocky` and the domain name is the previous `aera123.com`. 

To check and ensure the hostname has been set, use the command `hostname`. 

##### IP Address

Let's set the static IP address for now. Go to *Settings*, *Network*, and click the Gear button next to *Wired*. Click to the *IPv4* tab and select `Manual` for the IPv4 Method. Enter the information (we'll assign 192.168.1.2 for this machine).

Verify the IP address has been set correctly with the command `ip a` or `ifconfig`. 

<br>
<div align="center">»»——————————-　✼　-——————————««</div>
<br>

#### Windows 10

Create and deploy a linked clone for Windows 10. Make sure to associate the virtual NIC to the LAN segment. 

##### Hostname
Run Windows PowerShell as an administrator and use the command `Rename-Computer -NewName "enterNewNameHereInQuotes"` to assign a hostname. The system will have to be restarted for the hostname change to take place. 


##### IP address
The IP address can easily be assigned by following the same steps as the Windows 2022 Server by going through the Control Panel. I'll assign mine `192.168.1.3`. 

Verify that the correct hostname and IP address has been assigned by using the command `ipconfig /all`. See Figure 11 below. 

That's it for setting up! Time to move on to Active Directory! 


<br>
<br>
<div align="center">»»————————————————————————————-　✼　-————————————————————————————««</div>
<br>
<br>

### Active Directory

Active directory is a directory service used to manage information such as user accounts, devices, and authentication. You can read more about Active Directory in my upcoming post which I will link soon.

1. [Install Services](#installing-services)
2. [AD Services Configuration](#ad-services-configuration)
3. [DHCP Configuration](#dhcp-configuration)
4. [Create User Account](#create-user-account)
5. [Remote Active Directory](#remote-active-directory)


<br>
<div align="center">»»——————————-　✼　-——————————««</div>
<br>

#### Installing Services

On the Windows Server 2022 machine, Active Directory Domain Services (AD DS), Domain Name Service (DNS), and Dynamic Host Configuration Protocol (DHCP) needs to be installed. To do so, open *Server Manager*, and on the Dashboard, select *Manage* in the top right and click the *Add Roles and Features* dropdown. 

When asked for the Installation Type, select *Role-based or feature-based installation* and continue. On the next page, select the correct server and continue. The page now is for selecting server roles. The three roles, **DHCP**, **AD DS**, and **DNS Server** should be selected. For each service, when prompted, default features may be selected. Continue on until the end and keep in mind that a restart will need to be done after the services are all configured (no need to check the box on the last window to restart the server, as it is not needed for installation). 

After the installations are finished, the services can be seen in the dashboard of *Server Manager*. 

<br>
<div align="center">»»——————————-　✼　-——————————««</div>
<br>

#### AD Services Configuration

Now, in the top right, the flag will have a yellow triangle warning sign. The drop down will have an option titled *Promote this server to a domain controller*. Click it to launch the AD Services Configuration Wizard. On the Deployment Configuration screen, select *Add a new forest* as the deployment operation and enter the domain (in our example, it would be `aera123.com`. The next page with the Domain Controller Options will allow you to enter a password for Restore Mode - just in case. 

The third screen, DNS Options, will have you create a delegation, but for this lab, it is not needed. Go to the next page. 

The fourth screen, Additional Options, will auto-fill the NetBIOS domain name, so there is no need to touch it. The fifth screen, Paths, can just have the defaults used for where to place the ADDS files. 

The sixth screen, Review Options, will let you review your chosen settings. If you wish, you can click *View script* and save the file to reuse the options later on. Next! 

When the checks are finished, click install. You may be prompted to restart and change the Admin password. 

#### DHCP Configuration

Click on the flag with the yellow warning triangle in the top left again. This time, select *Complete DHCP Configuration* from the drop down. On the second screen, Authorization, you can choose who has authorization to make changes to the DHCP server. Select the Administrator account and finish. 

On the Server Manager dashboard, select *Tools* and click *DHCP* from the dropdown. 

Expand the server on the left, then expand the IPv4. Right click the IPv4 and click *New Scope*. 

In the New Scope Wizard, when asked for a name and description, enter anything such as "lab" and "private network". Next, you can define the range of IP addresses that are available to assign devices. An example is a start IP of `192.168.1.1` to the end of `192.168.1.254`. Enter the subnet mask, which in this case, would be `255.255.255.0`. 

On the next page, enter two excluded address ranges: one will be the gateway, and another will be a future server. First, exclude the IP address that was assigned to the default gateway, which is the pfSense LAN interface. We assigned it to be `192.169.1.254`, so exclude that address. The next exclusion will be a range for servers - such as 192.168.1.1 to 192.168.1.10. 

You'll want to select *Yes, I want to configure these options now* for configuring DHCP options. Next ,enter the pfSense default gateway address (we used `192.169.1.254`) and add it on the list. Click next. 

On the next page, a public server should be assigned for the secondary DNS. To do this, add the IP address. There should now be two listed. (`8.8.8.8` is a good one). 

Soon, select the option *Yes, I want to activate this scope now* on the Activate Scope page. Finish the configuration. It should now show as an *Active* status in the DHCP window.

<br>
<div align="center">»»——————————-　✼　-——————————««</div>
<br>

#### Create User Account

Windows Active Directory can be used to create and authenticate a user. On the Windows Server 2022 VM, go to the Server Manager dashboard. Select *Tools* in the top right and in the dropdown select *Active Directory Users and Computers*. In the window that pops up, right click on your domain, and in the dropdown select *New* then *User*. 

Enter the user information and a password. When finished, the user will appear in the domain. 

To add the user to the Domain Admin group, right click the user and select *Add to a group*. On the new window, type in *domain* in the *Enter the object names to select* text box and then click *Check Names*. From the resulting list, select *Domain Admins*. Add the user to the group.

To join the user to the Windows domain, first change the NIC settings on the two clients to receive their IP address via DHCP (automatically). On Windows 10, run Powershell as the administrator and use this command: `Add-Computer -DomainName aera123.com -Credential (Get-Credential)`. You can then input the username and password when prompted, then be asked to restart the system. 

Select Other User to log in and use the new account to check it works. 

On Rocky, ensure the system is updated as root with `sudo dnf -y update`. Join the user to the AD domain with the command `realm join --user=ahan aera123.com` where 'ahan' is the username. To verify, use `realm list`. 



<br>
<div align="center">»»——————————-　✼　-——————————««</div>
<br>

### Remote Active Directory

On Windows 10, install Windows Admin Center from [here](https://www.microsoft.com/en-us/evalcenter/download-windows-admin-center). Select the *Open Windows Admin Center* box before finishing the install. When prompted with the certificate, select the certificate then continue. Install the Active Directory extension. It will appear in the Tools column.

Now, connect to the server with the *Add* button in the top left, then the *Add* button in the Servers box. Enter the hostname of the Windows Server 2022, which should be `win22` if following along. It should be found by the system. Click add. Select the server then press the *Connect* button. 


<br>
<br>
<div align="center">»»————————————————————————————-　✼　-————————————————————————————««</div>
<br>
<br>

skip

### RAID Partitioning

1. [Deploy Storage Server](#deploy-a-storage-server)
2. [Format and Mount Drives](#format-and-mount-drives)


#### Deploy a Storage Server

Create another Rocky Linux VM to be a storage server. We will partition drives on this server. To the new machine, give it a hostname such as rockyServer, and a static IP address from one of the excluded addresses that was configured in [DHCP](#dhcp-configuration). Update the system then power it off, and within VMWare Workstation Pro, add eight drives. On the VM settings machine, click *Add* and select *Hard Disk*. Select *NVMe* as the disk type and select *Create a new virtual disk*. 

For the capacity, use 4 GB and store the image files in the same location as the VM linked clone files. Give it a meaningful name. 

When done deploying the eight drives, power on the VM. 

Using the command `which mdadm` and `lsblk` will show that the drives have been created. 

#### Format and Mount Drives


<br>
<br>
<div align="center">»»————————————————————————————-　✼　-————————————————————————————««</div>
<br>
<br>


## References & Resources
### References


### Extra Resources to Check Out!



Samba: https://www.linuxshelltips.com/install-samba-rockylinux-almalinux/

starting nfs service https://www.ibm.com/docs/ru/psfa/1.6?topic=software-configuring-nfs-export
