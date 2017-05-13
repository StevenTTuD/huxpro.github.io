---
author: StevenTTuD
layout: post
title: EDX Linux Foundation Ch11：Local Security Principle section 1~3
published: true
date: 2014-09-22 07:53
tags:
  - Linux
  - EDX Linux Foundation Course
comments: true

---
#Section 1 Understanding Linux Security

使用者權限的管理在個人電腦上可以放輕鬆使用，但如果是多人共用的電腦，就必須限制的嚴格，最好只給用戶他所可以用的權限就好，不要多給，本章將會以最嚴格的標準來說明如何管理使用者的權限以增加安全性。

##User Accounts
For each user, the following seven fields are maintained in the /etc/passwd file:
![](https://lh5.googleusercontent.com/Qizgf3rUafom1Uf72hKqjWWcp911th-wiySVy0Rct4o=w1483-h1185-no)

##Types of Accounts
By default, Linux distinguishes between several account types in order to isolate processes and workloads. Linux has four types of accounts:
- root
- System
- Normal
- Network

 The ```last``` utility, which shows the last time each user logged into the system, can be used to help identify potentially inactive accounts which are candidates for system removal.

##Understanding the root Account

 root is the most privileged account on a Linux/UNIX system. This account has the ability to carry out all facets of system administration, including adding accounts, changing user passwords, examining log files, installing software, etc. Utmost care must be taken when using this account. It has no security restrictions imposed upon it.

#section 2: Understanding the usage of root
##Operations that Require root Privileges
 root privileges are required to perform operations such as:
- Creating, removing and managing user accounts.
- Managing software packages.
- Removing or modifying system files.
- Restarting system services.

一般用戶也可以安裝或是更新軟體，但是只有root帳號可以讓軟體做一些跟系統有關的事情。
![](https://lh4.googleusercontent.com/IFzh7nozJnb-TvQqkzSRYh7acu0Hfyv_fYdNMIJTsDE=w1443-h948-no)

##Creating a New User in Linux
useradd <username>
passwd <username>

##Operations That Do Not Require root Privileges
###SUID（Set owner User ID upon execution）
SUID (similar to the Windows "run as" feature)is a special kind of file permission given to a file. SUID provides temporary permissions to a user to run a program with the permissions of the file owner  (which may be root) instead of the permissions held by the user.

當 s 這個標誌出現在檔案擁有者的 x 權限上時，例如```/usr/bin/passwd``` 這個檔案的權限狀態：『-rwsr-xr-x』，此時就被稱為 Set UID，簡稱為 SUID 的特殊權限。明明 ```/etc/shadow``` 就不能讓 vbird 這個一般帳戶去存取的，為什麼還能夠修改這個檔案內的密碼呢？ 這就是 SUID 的功能!

1. vbird 對於 ```/usr/bin/passwd``` 這個程式來說是具有 x 權限的，表示 vbird 能執行 passwd；
2. passwd 的擁有者是 root 這個帳號；
3. vbird 執行 passwd 的過程中，會『暫時』獲得 root 的權限；
4. ```/etc/shadow``` 就可以被 vbird 所執行的 passwd 所修改。

另外，SUID 僅可用在binary program 上， 不能夠用在 shell script 上面！這是因為 shell script 只是將很多的 binary 執行檔叫進來執行而已！

#Section 3:Comparing sudo and su
Users' authorization for using sudo is based on configuration information stored in the ```/etc/sudoers``` file and in the ```/etc/sudoers.d``` directory.

##The sudoers File
Whenever sudo is invoked, a trigger will look at /etc/sudoers and the files in /etc/sudoers.d to determine if the user has the right to use sudo and what the scope of their privilege is. Unknown user requests and requests to do operations not allowed to the user.

###Edit sudoers
 You can edit the ```sudoers``` file by using ```visudo```, which ensures that only one person is editing the file at a time.

The basic structure of an entry is:
```
who where = (as_whom) what
```

###/etc/sudoers.d
Most Linux distributions now prefer you add a file in the directory ```/etc/sudoers.d``` with a name the same as the user. This file contains the individual user's sudo configuration, and one should leave the master configuration file untouched except for changes that affect all users.

##Command Logging
By default, sudo commands and any failures are logged in ```/var/log/auth.log``` under the Debian distribution family, and in ```/var/log/messages``` or ```/var/log/secure``` on other systems.This is an important safeguard to allow for tracking and accountability of sudo use. A typical entry of the message contains:

Running a command such as sudo whoami results in a log file entry such as:
Dec 8 14:20:47 server1 sudo: op : TTY=pts/6 PWD=/var/log USER=root COMMAND=/usr/bin/whoami

##Process Isolation
Linux is considered to be more secure than many other operating systems because processes are naturally isolated from each other. One process normally cannot access the resources of another process, even when that process is running with the same user privileges.

##Hardware Device Access
###device special file (often called a device node) under the /dev directory that corresponds to the device being accessed.

###/dev/sd*
Hard disks, for example, are represented as /dev/sd*. While a root user can read and write to the disk in a raw fashion (for example, by doing something like:
```
$ echo hello world > /dev/sda1
```
寫入divice node很容易會毀掉整個檔案系統，所以絕對不要直接存取device node。

##Keeping Current
The best practice is to take advantage of your Linux distribution's mechanism for automatic updates and never postpone them. It is extremely rare that such an update will cause new problems.
