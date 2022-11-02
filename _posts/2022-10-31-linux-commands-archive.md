---
title: "Linux Commands Archive"
layout: post
date: 2022-10-31
feature_image: images/linux_commands.png
tags: [linux, archive]
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



<br />

### Explanations & Examples

<div align="center">────────❀*̥˚──────❀*̥˚───────</div>
#### ls
`ls` lists files and directories in the current directory, or the specified directory. The command accepts flags. `-R` stands for recursive and will return all files in the subdirectories too, while `-a` will show hidden files that start with `.`. 
Basic syntax: `ls [flags] [directory]`

General shortcuts: `/` for root, `..` for parent directory one level above, `../..` for two levels above, `~` for home directory.

Example:
```sh
ls -l
drwxr-xr-x. 1 aerahan aerahan 48 Oct 26 16:21 Documents
```
<br />
<div align="center">────────❀*̥˚──────❀*̥˚───────</div>
<br />

#### man
`man` is a built-in manual for commands in linux. The scroll wheel/mouse, up and down keys, and PgDn and PgUp keys can all be used to navigate through the manual. Q/q is pressed to exit. The basic syntax is simply `man [command]`.

Man pages can include information such as the name, synopsis, configuration, description, examples, defaults, options, exit status, environment, files, authors, history, notes, bugs, etc. 

Example:
```sh
man -f mkdir
mkdir (1)       - make directories
mkdir (2)       - create a directory
```
<br />
<div align="center">────────❀*̥˚──────❀*̥˚───────</div>
<br />

#### sudo
`sudo` stands for superuser do, and allows you to perform admin tasks or have root privileges for a command. The general syntax is `sudo [command]`.

Example:
```sh
[aerahan@fedorahat ~]$ rmdir Documents
This command requires root privileges.
[aerahan@fedorahat ~]$ sudo !!
```

`sudo !!` is a helpful shortcut to repeat the previous command typed and entered but with sudo, so you don't have to retype the whole command.

<br />
<div align="center">────────❀*̥˚──────❀*̥˚───────</div>
<br />

#### pwd
`pwd` prints the path of your current working directory. It can take the `-L` / `--logical` and `-P` / `--physical` options to print environment variable content (with symbolic links included) or the actual path of the current directory, respectively. 

Example:
```sh
[aerahan@fedorahat ~]$ pwd
/home/aerahan
```

<br />
<div align="center">────────❀*̥˚──────❀*̥˚───────</div>
<br />

#### cd
`cd` changes the current directory. Running it by itself as `cd` will take the user to the home folder. If you wish to go to a child directory, simply the directory name will do. If you wish to go somewhere completely else, the command will need the full path. 

Example:
```sh
cd /home/aerahan/Documents/schoolFiles
```
Shortcuts: `cd ..` will move one directory up. `cd-` will go to the previous directory.

<br />
<div align="center">────────❀*̥˚──────❀*̥˚───────</div>
<br />

#### cat
concatenate will write file content to standard output so you can read it easily. 
`cat` can also be used to create a new file and combine files. Use `cat > filename` to create a new file and `cat filename filename2 > filename3` to store a merged file. 

Example: 
```sh
cat hello.txt
hello world!

cat > hello2.txt // creates a new file 
cat file1.txt file2.txt > combined.txt // merges file1.txt and file2.txt and stores as combined.txt
```
Use with command [`tac`](#tac) to display content in reverse order. 

<br />
<div align="center">────────❀*̥˚──────❀*̥˚───────</div>
<br />

#### cp
`cp` can copy files and directories and directory content. Files can be copied to other files or other directories. The `-R` flag will copy the entire directory.

Basic syntax: `cp [source] [destination]`

Example: 
```sh
cp tester.txt /home/aerahan/Documents/website/hello.txt
```
<br />
<div align="center">────────❀*̥˚──────❀*̥˚───────</div>
<br />

#### mv
`mv` can be used to move or rename files and directories. 

Basic syntax: `mv [source] [destination]`

Example:
```sh
mv tester.txt /home/aerahan/Documents/testFiles
mv helloworld.txt HelloWorld.txt
```
<br />
<div align="center">────────❀*̥˚──────❀*̥˚───────</div>
<br />

#### mkdir
`mkdir` creates directories and can set permissions at the same time. 

Basic syntax: `mkdir [option] [directoryName]`

Basic options: `-m` can set file permissions. `-m777` will set full read, write, and execute permissions. `-v` will print a message when the directory is created. By default, no message is printed. `-p` / `--parents` will make a directory in between two folders.

Example: Create 'rap' directory inside music, create 'test' directory with full read/write/execute permissions, and create 'songs' directory between 'music' and 'rap'.
```sh
mkdir /music/rap
mkdir -m777 test
mkdir -p /music/songs/rap
```
<br />
<div align="center">────────❀*̥˚──────❀*̥˚───────</div>
<br />

#### touch
`touch` can be used to create a new, empty file, and to also change file timestamps. Using it on an existing file will change the access and modification times to the current time, while using it on an argument that doesn't exist will create an empty file. 

Common flags: `-a` to change only the access time, `-m` to change only the modifiction time, `-c` to not create any files.

Example:
```sh
[aerahan@fedorahat ~]$ touch newFile.txt
```
<br />
<div align="center">────────❀*̥˚──────❀*̥˚───────</div>
<br />

#### find

<br />
<div align="center">────────❀*̥˚──────❀*̥˚───────</div>
<br />

#### grep

`grep` prints lines that matches patterns. It searches for a pattern in each file, and each line that matches will be printed. 

Common flags: `-i` / `--ignore-case` will ignore case when searching for a pattern. On the flip side, `--no-ignore-case` will do the opposite - and is the default option. `-v` / `--invert-match` will invert the matching and print non-matching lines instead. `-c` / `--count` will print the number of matching lines instead. 

<br />
<div align="center">────────❀*̥˚──────❀*̥˚───────</div>
<br />

#### tail
#### chmod
#### ping
#### wget
#### uname
#### clear
#### history
#### echo
#### hostname
#### su
#### useradd
