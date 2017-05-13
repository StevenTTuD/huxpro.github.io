---
layout: post
title: EDX Linux Foundation Ch11：Local Security Principle section 4~5
published: true
date: 2014-09-22 08:07
tags: []
categories: []
comments: true

---
#Section 4: Working with passwords
##How Passwords are Stored
 On modern systems, passwords are actually stored in an encrypted format in a secondary file named ```/etc/shadow```. Only those with root access can modify/read this file.

##Password Encryption
Most Linux distributions rely on a modern password encryption algorithm called **SHA-512** (Secure Hashing Algorithm 512 bits), developed by the U.S. National Security Agency (NSA) to encrypt passwords.

可以玩玩看下面這個指令，加密test這個字。
```
echo -n test | sha512sum
```

##Good Password Practices
IT professionals follow several good practices for securing the data and the password of every user.

###1.Password aging 
**Password aging** is a method to ensure that users get prompts that remind them to create a new password after a specific period. This can ensure that passwords, if cracked, will only be usable for a limited amount of time. This feature is implemented using **chage**, which configures the password expiry information for a user.



###2.PAM
Another method is to force users to set strong passwords using Pluggable Authentication Modules (PAM). PAM can be configured to automatically verify that a password created or modified using the passwd utility is sufficiently strong. PAM configuration is implemented using a library called pam_cracklib.so, which can also be replaced by pam_passwdqc.so for more options.

###3.John the ripper
One can also install password cracking programs, such as John The Ripper, to secure the password file and detect weak password entries. It is recommended that written authorization be obtained before installing such tools on any system that you do not own.

#Seciton 5: Securing the Boot Process and Hardware

##Requiring Boot Loader Passwords
You can secure the boot process with a secure password to prevent someone from bypassing the user authentication step. 

###GRUB version 1（older version）
you can invoke **grub-md5-crypt**  which will prompt you for a password and then encrypt as shown on the adjoining screen.

You then must edit ```/boot/grub/grub.conf``` by adding the following line below the timeout entry:
```
password --md5 $1$Wnvo.1$qz781HRVG4jUnJXmdSCZ30
```
You can also force passwords for only certain boot choices rather than all. 
舊的作法是直接修改```/boot/grub/grub.conf```

###GRUB version 2（older version）
For the now more common GRUB version 2 things are more complicated, and you have more flexibility and can do things like use user-specific passwords, which can be their normal login password.  Also you never edit the configuration file, ```/boot/grub/grub.cfg```, directly, rather you edit system configuration files in ```/etc/grub.d``` and then run **update-grub**. One explanation of this can be found at https://help.ubuntu.com/community/Grub2/Passwords.

新的作法不直接修改```/boot/grub/grub.cfg```，而是先修改```/etc/grub.d```然後跑update-grub。

##Hardware Vulnerability
**When hardware is physically accessible, security can be compromised by:**
1. Key logging: Recording the real time activity of a computer user including the keys they press. The captured data can either be stored locally or transmitted to remote machines
2. Network sniffing: Capturing and viewing the network packet level data on your network
3. Booting with a live or rescue disk
4. Remounting and modifying disk content

**The guidelines of security are:**
1. Lock down workstations and servers
2. Protect your network links such that it cannot be accessed by people you do not trust
3. Protect your keyboards where passwords are entered to ensure the keyboards cannot be tampered with
4. Ensure a password protects the BIOS in such a way that the system cannot be booted with a live or rescue DVD or USB key
