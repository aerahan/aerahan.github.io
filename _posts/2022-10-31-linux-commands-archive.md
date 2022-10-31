---
title: "Linux Commands Archive"
layout: post
date: 2022-10-31
feature_image: images/linux_commands.png
tags: [linux]
---

A compiled list of linux commands that I'll keep adding to. Includes a general list section, and then an explanation/example section. They aren't in alphabetical order, so please use your browser's search/find function!

<!--more-->

### Linux Commands
- [**ls**](#ls) - list files and directories
- [**man**](#man) - manual page for commands
- [**sudo**](#sudo) - superuser do
- [**pwd**](#pwd) - prints current working directory
- [**cd**](#cd) - changes directory
- [**cat**](#cat) - concatenate
- [**cp**](#cp) - copy files or directories & content
- [**mv**](#mv) - move and rename files and directories
- [**mkdir**](#mkdir) - create directories 
- [**touch**](#touch) - create an empty file 
- [**find**](#find) - search for files in a directory
- [**grep**](#grep) - find a word and print pattern matched lines
- [**tail**](#tail) - display last 10 lines of a file 
- [**chmod**](#chmod) - change file/directory permissions 
- [**ping**](#ping) - checks whether network or server is reachable
- [**wget**](#wget) - downloads files from internet using HTTP, HTTPS, and FTP protocols
- [**uname**](#uname) - prints detailed information about Linux system/hardware
- [**clear**](#clear) - clears the terminal screen
- [**history**](#history) - lists history of commands
- [**echo**](#echo) - displays a line of text
- [**hostname**](#hostname) - returns system's hostname 
- [**su**](#su) - switches user
- [**useradd**](#useradd) - create a new account



### Explanations & Examples

##### ls
ls lists files and directories in the current directory, or the specified directory. The command accepts flags. 
Basic syntax: `ls [flags] [directory]`
General shortcuts: / for root, .. for parent directory one level above, ../.. for two levels above, ~ for home directory.

Example:
```sh
ls -l
```


##### man
man is a built-in manual for commands in linux. The scroll wheel/mouse, up and down keys, and PgDn and PgUp keys can all be used to navigate through the manual. Q/q is pressed to exit. The basic syntax is simply `man [command]`.
Man pages can include information such as the name, synopsis, configuration, description, examples, defaults, options, exit status, environment, files, authors, history, notes, bugs, etc. 

Example:
```sh
man -f mkdir
mkdir (1)       - make directories
mkdir (2)       - create a directory
```


##### sudo
sudo stands for superuser do, and allows you to perform admin tasks or have root privileges for a command. The general syntax is `sudo [command]`.


##### pwd
##### cd
##### cat
##### cp
##### mv
##### mkdir
##### touch
##### find
##### grep
##### tail
##### chmod
##### ping
##### wget
##### uname
##### clear
##### history
##### echo
##### hostname
##### su
##### useradd
