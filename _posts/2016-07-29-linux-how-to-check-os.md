---
layout: post
title: Linux - 如何查詢 OS 版本
published: true
date: 2016-07-29 04:37
tags:
  - Linux
comments: true

---
要接手 Server 第一件事情就是要了解 server 的環境啦。
這邊要記錄的是如何判別 Linux 系統類別的方式

### 1. 確認 Kernel 版本

使用 `uname -or` 可以取得 kernel 的版本

```
$ uname -or
=> 3.10.0-327.el7.x86_64 GNU/Linux
```

如果要知道詳細的資訊，輸入 `uname -a`，但即使這樣也無法清楚的看出 OS 的種類。

```
$ uname -a
=> Linux username 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

### 2. 針對不同 OS 查找對應的說明系統名稱的文件

如果是 Ubuntu，輸入 `cat /etc/lsb-release`。有找到的情況會出現以下訊息

```
$ cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=14.04
DISTRIB_CODENAME=trusty
DISTRIB_DESCRIPTION="Ubuntu 14.04.5 LTS"
```

如果你的系統不是 Ubuntu，則會出現以下訊息

```
$ cat /etc/lsb-release
cat: /etc/lsb-release: No such file or directory
```

Debian

雖然跟 Ubuntu 同體系但是存放版本號文件的位置不太一樣，
輸入`cat /etc/debian_version`

```
$  cat /etc/debian_version
8.0
```

Fedora, Red Hat and CentOS have:

```
Fedora: $ cat /etc/fedora-release
Fedora release 10 (Cambridge)

Red Hat/older CentOS: $ cat /etc/redhat-release
CentOS release 5.3 (Final)

newer CentOS: $ cat /etc/centos-release
CentOS Linux release 7.1.1503 (Core)
```


最後用表格來整理以上資訊

| 系統   | 位置 |
| :---: | :--- |
| ubuntu | /etc/lsb-release |
| debian | /etc/debian_version |
| Fadoara | /etc/fedora-release |
|  Red Hat/older CentOS |  /etc/redhat-release |
|  newer CentOS | /etc/centos-release |

如果你的 OS 上面列表都找不到的話，可以找 etc 資料夾內的有 release 這個詞的

```
cat /etc/*{release,version}
```


## Reference

[command line - How do I find out what version of Linux I'm running? - Super User](http://superuser.com/questions/11008/how-do-i-find-out-what-version-of-linux-im-running)
[How to Check CentOS Version Number](https://www.rackaid.com/blog/check-your-centos-version-number/)