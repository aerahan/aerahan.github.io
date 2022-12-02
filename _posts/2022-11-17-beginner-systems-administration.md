---
title: "Beginner Systems Administration"
layout: post
date: 2022-11-17
feature_image: images/linuxintro.png
tags: [networking, sysadmin]
---

basic systems administration tasks

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

### Active Directory

Active directory is a directory service used to manage information such as user accounts, devices, and authentication. You can read more about Active Directory in my upcoming post which I will link soon.

1. [Install Services](#installing-services)
2. [AD Services Configuration](#ad-services-configuration)
3. [DHCP Configuration](#dhcp-configuration)
4. [Create User Account](#create-user-account)
5. [Remote Active Directory](#remote-active-directory)

<br>
<br>
<div align="center">»»————————————————————————————-　✼　-————————————————————————————««</div>
<br>
<br>

skip

### RAID Partitioning

1. [Deploy Storage Server](#deploy-a-storage-server)
2. [Format and Mount Drives](#format-and-mount-drives)
3. [RAID 1 & MBR Table](#raid-1-&-mbr-partition-table)
4. [RAID 5 & GPT Table](#raid-5-&-gpt-partition-table)
5. [Persistent Mounts](#persistent-mount)



Commands: 
```
crontab -l    // lists user's crontab
crontab -e    // edits user's crontab
crontab -r    // deletes user's crontab
```

logger command: 
`logger Test Message!`
`00 11, 16 * * * logger Hello!`






To verify correctly working use ‘firewall-cmd -list-services’ and then reload 
Firewall-cmd –permanent –add-service=samba
Firewall-cmd –reload (firewall not running? Systemctl start firewalld)
Sudo useradd hbin -s /sbin/nologin 
	Cat /etc/passwd
Firewall-cmd –permanent –add-port=21/tcp


## References & Resources
### References


### Extra Resources to Check Out!



Samba: https://www.linuxshelltips.com/install-samba-rockylinux-almalinux/

starting nfs service https://www.ibm.com/docs/ru/psfa/1.6?topic=software-configuring-nfs-export
