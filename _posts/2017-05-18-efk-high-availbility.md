---
author: StevenTTuD
layout: post
title: "EFK(5) - Fluentd 高可用架構(High Availibity)"
published: true
date: 2017-05-18 22:15
tags:
  - EFK
  - Fluentd
  - Devops
comments: true

---

## 1 架構

之前我們的架構是直接由 fluentd td-agent 傳送到 elasticsearch (如下圖)

![](https://lh3.googleusercontent.com/-Fw9_PcnHOB4/WSKUWe9olJI/AAAAAAAAKyI/H2z06ueuQYsPC6SxezL40tdTQS1jaME6gCHM/I/14943188485356.jpg)

為了更高的可用性(High Availibity)，我們在中間加入 aggregator 的角色，讓在 td-agent 扮演 forwarder 角色，職責更加單一，forwarder 只負責「傳送資料給 aggregator」。過濾(filter)資料的工作轉由 aggregator 負責，這樣的架構下降低了原本應用程式伺服器(application server)的負擔，提供了更高的可用性。架構如下：

![](https://lh3.googleusercontent.com/-xndXkgeo0mg/WSKUWpAiPKI/AAAAAAAAKyM/jX3_xCaRC3YMEURD4kCgBmCdi0rjcGw3QCHM/I/14943830727801.jpg)

## 2 實際配置方式

## 2.1 如何配置 Forwarder

```
# TCP input
<source>
  @type forward
  port 24224
</source>

# HTTP input
<source>
  @type http
  port 8888
</source>

# Log Forwarding
<match mytag.**>
  @type forward

  # primary host
  <server>
    host 192.168.0.1
    port 24224
  </server>
  # use secondary host
  <server>
    host 192.168.0.2
    port 24224
    standby
  </server>

  # use longer flush_interval to reduce CPU usage.
  # note that this is a trade-off against latency.
  flush_interval 60s
</match>
```

## 2.2 如何配置 Aggregator

```
# Input
<source>
  @type forward
  port 24224
</source>

# Output
<match mytag.**>
  ...
</match>
```

## 參考資料

[Fluentd High Availability Configuration | Fluentd](http://docs.fluentd.org/v0.12/articles/high-availability#network-topology)
