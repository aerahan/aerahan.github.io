---
title: "Linux Commands Archive"
layout: post
date: 2022-10-31
feature_image: images/
tags: [linux]
---

A compiled list of linux commands that I'll keep adding to. Includes a general list section, and then an explanation/example section. They aren't in alphabetical order, so please use your browser's search/find function!

<!--more-->

### Linux Commands
- [**ls**](#ls) - list files and directories
- [**man**](#man) - manual page for commands
- []

### Explanations & Examples

##### ls
ls lists files and directories in the current directory, or the specified directory. The command accepts flags. 
Basic syntax: 
```console
ls [flags] [directory]
```
General shortcuts: / for root, .. for parent directory one level above, ../.. for two levels above, ~ for home directory.

Example:
```console
ls -l
```

##### man
man is a built-in manual for commands in linux. The scroll wheel/mouse, up and down keys, and PgDn and PgUp keys can all be used to navigate through the manual. Q/q is pressed to exit. 

man pages can include information such as the name, synopsis, configuration, description, examples, defaults, options, exit status, environment, files, authors, history, notes, bugs, etc. 

Example:
```console
linux@aerahan:~$ man -f mkdir
mkdir (1)       - make directories
mkdir (2)       - create a directory
```


