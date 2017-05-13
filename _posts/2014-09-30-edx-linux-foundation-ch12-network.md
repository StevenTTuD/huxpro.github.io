---
author: StevenTTuD
layout: post
title: EDX Linux Foundation Ch12：Network
published: true
date: 2014-09-30 04:17
tags:
  - Linux
  - EDX Linux Foundation
comments: true

---
##IP and package
IP(Internet Protocol) address is essential for routing packets of information through the network.These packets contain **data buffers** together with **headers** which contain information about where the packet is going to and coming from, and where it fits in the sequence of packets that constitute the stream.

##Decoding IPv4 Addresses
A 32-bit IPv4 address is divided into four 8-bit sections called octets.

![](https://lh5.googleusercontent.com/-B9UGhE1NesY/VCoF-IowH0I/AAAAAAAADDQ/0QZbRLRjUlk/w1653-h213-no/Screen%2BShot%2B2014-09-30%2Bat%2B09.15.39.png)

**IP address** are divided into five classes

![](https://lh6.googleusercontent.com/-rgUCVtxI9gk/VCoF-H3gCzI/AAAAAAAADDU/sJFdNhvXi9k/w1655-h803-no/Screen%2BShot%2B2014-09-30%2Bat%2B09.18.47.png)

Classes A, B, and C are classified into two parts: **Network addresses(Net ID)** and **Host address (Host ID)**. The Net ID is used to identify the network, while the Host ID is used to identify a host in the network.
Class D is used for special multicast applications (information is broadcast to multiple computers simultaneously)
Class E is reserved for future use.

##IP Address Allocation
Typically, a range of IP addresses are requested from your **Internet Service Provider(ISP)** by your organization's network administrator

You can assign IP addresses to computers over a network manually or dynamically.

![](https://lh3.googleusercontent.com/-Gdkko-JytG4/VCoK16007yI/AAAAAAAADDs/BKfxn8tDBfA/w1278-h660-no/Screen%2BShot%2B2014-09-30%2Bat%2B09.43.14.png)

When you assign IP addresses manually, you add static (never changing) addresses to the network. When you assign IP addresses dynamically (they can change every time you reboot or even more often), the **Dynamic Host Configuration Protocol (DHCP)** is used to assign IP addresses.

##Manually Allocating an IP Address
??
ipcalc

##Name Resolution
Name Resolution is used to convert numerical IP address values into a human-readable format known as the **hostname**.**text** For example, 140.211.169.4 is the numerical IP address that refers to the linuxfoundation.org hostname. Hostnames are easier to remember.
![](https://lh5.googleusercontent.com/2VU-chSrk_oMUIEB0vphQlRJ43uY73L016WiEjS-JkI=w1490-h1140-no)

##Using Domain Name System (DNS) and Name Resolution Tools
Domain Name System (DNS) translates Internet domain and host names to IP addresses.
私有IP對應的主機名稱```cat /etc/hosts```
顯示DNS IP```cat /etc/resolv.conf```

```
host linuxfoundation.org
```
```
nslookup linuxfoundation.org
```
```
dig linuxfoundation.org
```
look up domain name information

![](https://plus.google.com/photos/106207382048371838527/albums/6044136604797521137/6061411979084535810?pid=6061411979084535810&oid=106207382048371838527)

##Network Interfaces
Network interfaces are a connection channel between a device and a network.

Physically, network interfaces can proceed through a **network interface card (NIC)** or can be more abstractly implemented as software.

A list of currently active network interfaces is reported by the ifconfig utility which you may have to run as the superuser.

##Network Configuration Files


 /etc/init.d/network start
 ???

##Network Configuration Commands
To view the IP address:
$ /sbin/ip addr show

To view the routing information:
$ /sbin/ip route show

**ip** is a very powerful program that can do many things. Older (and more specific) utilities such as **ifconfig** and **route** are often used to accomplish similar tasks. A look at the relevant man pages can tell you much more about these utilities.

##ping
**ping** is used to check whether or not a machine attached to the network can receive and send data; i.e., it confirms that the remote host is online and is responding.
To check the status of the remote host, at the command prompt, type ```ping <hostname>```.
![](https://lh3.googleusercontent.com/-WWCJScptKVw/VCoaHRUmzLI/AAAAAAAADEY/LDb4BD_pTZY/w1517-h653-no/Screen%2BShot%2B2014-09-30%2Bat%2B10.48.06.png)


##route
Servers maintain **routing tables** containing the addresses of each node in the network.**route** is used to view or change the IP routing table.
![](https://lh6.googleusercontent.com/-8Xy4nDL37cE/VCocCP4OY1I/AAAAAAAADEs/XiRhyVmMkEc/w1653-h490-no/Screen%2BShot%2B2014-09-30%2Bat%2B10.56.54.png)

##traceroute
？？？
traceroute is used to inspect the route which the data packet takes to reach the destination host which makes it quite useful for troubleshooting network delays and errors.

Check the route which the data packet takes to reach the destination host (google.com). Type ```traceroute <domain>```.

##More Networking Tools
Now, let’s learn about some additional networking tools. Networking tools are very useful for monitoring and debugging network problems, such as network connectivity and network traffic.
![](https://lh6.googleusercontent.com/-kGM0kxMWbts/VCotWIqGvwI/AAAAAAAADFc/OlpwAh0CkQ4/w1653-h708-no/Screen%2BShot%2B2014-09-30%2Bat%2B12.10.45.png)

##Using More Networking Tools
sudo ethtool eth0
netstat -r
sudo nmap -sP 10.0.2.15/24

先看自己啟動哪些service
service stats
lsof socket_name

###strace ls
可以知道command怎麼run的

###netstat grep某個port
看某個server的port

##其他指令
###fork
operating
###cli

fd: 識別馬
lsof 可以茶城市到底開了多少個fd

