---
author: StevenTTuD
layout: post
title: "EFK(2) - 介紹 EFK (Elasticsaerch + Fluentd + Kibana)"
published: true
date: 2017-05-15 21:15
tags:
  - Fluentd
  - OSX
  - EFK
  - Devops
comments: true

---

## Fluentd 介紹

![](https://lh3.googleusercontent.com/-j70_YXz_sk4/WNoTF7xoWDI/AAAAAAAAKvI/2jTCdWgspPs/I/14906863744809.jpg)

Fluentd 跟 Logstash 扮演的角色相同。
過去 Logstash 的歷史有效能不佳的記錄，Fluentd 在效能上的口碑更好。
如下圖所示，Fluentd 可以將蒐集 Log 負責的過程統一規格化。我們在使用的時候，選用想要蒐集Log樣式的 input plugin，
例如：要蒐集 apache 的 log 我們就選用 fluentd 的 apache input plugin。
輸出的時候選擇 output plugin，例如: logstash output plugin，這樣就能夠讓 Elasticsearch 和 Kibana 接受。
有著這種特性我們可以用 Fluentd 來取代 Logstash，讓原本的 ELK Stack (Elasticsearch + Logstash + Kibana) 變成 `EFK`。
(Elasticsearch + Fluentd + Kibana)。

![](https://lh3.googleusercontent.com/-BOUMzF0Cmtg/WNoTGAw0YeI/AAAAAAAAKvM/ZrdJjwU8rQg/I/14906863534401.jpg)

---

## Fluentd 是值得信賴的

Ruby 之父 Matz 與 Heroku co-founder 推薦

![](https://lh3.googleusercontent.com/-m44IbmNOduw/WNoTGfUDV9I/AAAAAAAAKvQ/L9rkWT9NDXg/I/14906859399847.jpg)

企業推薦

![](https://lh3.googleusercontent.com/-SeZ7MagNN_I/WNoTGlswbXI/AAAAAAAAKvU/ZNwwJ_hsn0E/I/14906865411614.jpg)

隸屬 CNCF 聯盟

![](https://lh3.googleusercontent.com/-E-X1fwq1UOw/WNoTHMyWrzI/AAAAAAAAKvY/vfTXYe45n3U/I/14906865828825.jpg)

---

### Fluentd 與 Logstash 比較

這邊有兩篇很棒的比較：

[Loom Systems - AI Log Analysis for All Your Applications](https://www.loomsystems.com/single-post/2017/01/30/A-Comparison-of-Fluentd-vs-LogStash-Log-Collector)

[Log Aggregation with Fluentd, Elasticsearch and Kibana](http://dev.haufe.com/log-aggregation/)

---


### 其他資源

[List of All Plugins](https://www.fluentd.org/plugins/all)
