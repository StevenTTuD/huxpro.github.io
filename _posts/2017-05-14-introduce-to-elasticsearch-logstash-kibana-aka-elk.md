---
author: StevenTTuD
layout: post
title: "EFK (1) - 介紹 ELK Stack( Elasticsearch + Logstash + Kibana)"
published: true
date: 2017-05-14 20:02
tags:
  - Fluentd
  - EFK
  - ELK
  - Devops
comments: true

---

# ELK

## 1. 整體架構

- 最左邊的 Logstash Shipper 用來運送 Log 到 Log 處理中心。
- Redis 當做 Buffer 來緩衝資料量瞬間爆量的問題。
- 右邊的 Logstash 將 Log 加工成 Elasticserach、Kibana 可以處理的格式。
- Kibana 是一個後台，可以看到我們所有蒐集的 Log，輸入搜尋條件後就可以很快的找到需要的資料。

為什麼我們要蒐集 log?

- 想像一下在被攻擊時我想列出某一ip的所有記錄或是某個使用者的行為蹤跡，以做對應的處理。
- 伺服器在記憶體標高的那段時間系統到底做了些什麼事情，可以幫助我們迅速找到系統問題、對症下藥。

![](https://lh3.googleusercontent.com/-exOBSKTu4UA/WNoMlrXpgGI/AAAAAAAAKu4/7OhqQpvsR0c/I/14906715245146.jpg)

---

## 2. Logstash

Logstash 是一套 Log 分析框架，可以幫助我們處理各式各樣的 Log

![](https://lh3.googleusercontent.com/-r7elMh7APLA/WNoMk-QolvI/AAAAAAAAKus/4GRMGwJnEiY/I/14906672318390.jpg)

### 2.1 Beats 介紹

Beat 即為 logstash forwarder 又稱為 shipper，因為之前的 logstash forwarder 有著效能問題，elastic.co 公司推出 Beat 來取代之。除了原本的 log 蒐集([filebeat](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-overview.html))以外，也可以監控系統狀態([metricbeat](https://www.elastic.co/guide/en/beats/metricbeat/5.2/metricbeat-overview.html))，或是伺服器網路是否正常運作([heartbeat](https://www.elastic.co/guide/en/beats/heartbeat/current/heartbeat-configuration.html))
除此之外，社群上也有大量各式各樣的 Beat ([Community Beats | Beats Platform Reference 5.2 | Elastic](https://www.elastic.co/guide/en/beats/libbeat/current/community-beats.html)) 可以使用


![](https://lh3.googleusercontent.com/-H7VboeauWWE/WNoMlJwJpbI/AAAAAAAAKuw/AbpFk96DpIY/I/14906726298053.jpg)

## 3. Elasticsearch



## 4. Kibana

Kibana 是一個可以顯示 Logstash 處理後格式的後台。資料儲存於 Elasticsearch。
可以顯示漂亮的圖表
