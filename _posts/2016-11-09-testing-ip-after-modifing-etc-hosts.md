---
author: StevenTTuD
layout: post
title: "在 OSX 環境中測試 etc/hosts 是否成功修改"
published: true
date: 2016-11-09 08:02
tags:
  - OSX
comments: true

---
### 前言

透過修改 etc/hosts 讓我們不需要透過 DNS 也能模擬網域名稱連線到伺服器。
可是設定是否成功？這時候我們可以用一些測試工具來檢測之。

### OSX環境下

OSX 內建 `dscacheutil` 工具，可以用來檢測網址名稱對應 ip 的結果。
使用方法

```
dscacheutil -q host -a name 檢測的網域名稱
```

比如說我想要檢測 google.com 對應的 ip

```
$ dscacheutil -q host -a name google.com
name: google.com
ipv6_address: 2404:6800:4008:c03::66

lsname: google.com
ip_address: 74.125.203.101
ip_address: 74.125.203.100
ip_address: 74.125.203.138
ip_address: 74.125.203.139
ip_address: 74.125.203.113
ip_address: 74.125.203.102
```

如果我在 `etc/hosts` 將 `google.com` 設定至自定義的 ip。

```
111.111.111.111 google.com
```

檢測的結果即變成

```
$ dscacheutil -q host -a name google.com
name: google.com
ipv6_address: 2404:6800:4008:c03::64

name: google.com
ip_address: 111.111.111.111
```

### 參考資料

[mac - How can I install getent on Snow Leopard? - Ask Different](http://apple.stackexchange.com/questions/44567/how-can-i-install-getent-on-snow-leopard)

其他相關工具

[macos - OS X 10.10.1 /etc/hosts & /private/etc/hosts file is being ignored and not resolving - Ask Different](http://apple.stackexchange.com/questions/158117/os-x-10-10-1-etc-hosts-private-etc-hosts-file-is-being-ignored-and-not-resol)

Linux 環境

[dns - How to test /etc/hosts - Unix & Linux Stack Exchange](http://unix.stackexchange.com/questions/134143/how-to-test-etc-hosts)