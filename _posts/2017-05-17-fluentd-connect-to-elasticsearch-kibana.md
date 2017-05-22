---
author: StevenTTuD
layout: post
title: "EFK(4) - 讓 Elasticsearch 與 Kibana 能夠接收 Rails 的 log"
published: true
date: 2017-05-17 21:33
tags:
  - EFK
  - Fluentd
  - Devops
comments: true

---

### 2.1 架構

![](https://lh3.googleusercontent.com/-d5gQhHd4ylM/WSKepoR8vPI/AAAAAAAAKyc/0BxPY3iDqEkpUttT907x5q__p1z2Cgy1wCHM/I/14943188485356.jpg)

## 2.2 安裝步驟

### 2.2.1 安裝 elasticsearch

```
$ curl -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.0.2.tar.gz
$ tar zxvf elasticsearch-5.0.2.tar.gz
$ cd elasticsearch-5.0.2
```

啟動 elasticsearch

```
$ ./bin/elasticsearch
```

### 2.2.2 安裝 Kibana

```
curl -O https://artifacts.elastic.co/downloads/kibana/kibana-5.0.2-darwin-x86_64.tar.gz
$ tar zxvf kibana-5.0.2-darwin-x86_64.tar.gz
$ cd kibana-5.0.2-darwin-x86_64
```

啟動 kibana

```
$ ./bin/kibana
```

## 2.2.2 fluentd

透過fluentd 的 elasticsearch 套件`fluent-plugin-elasticsearch`我們可以讓 fluentd 輸出的 log 變成 logstash 輸出的格式，這樣一來 kibana 就可以顯示我們儲存的 log。

安裝 fluent-plugin-elasticsearch

```
fluent-gem install fluent-plugin-elasticsearch --no-document
```

設定 fluentd 設定檔

```
mkdir td-agent
touch ./td-agent/td-agent.conf
```

編輯 `td-agent.conf`
因為 Rails 是使用 forward input，我們只需要開啟 forward input 就好。
如果你想監測其他的 log 類型例如：syslog，也可以透過設定 source 來達成。

```
# get logs from syslog
#<source>
#  @type syslog
#  port 42185
#  tag syslog
#</source>

# get logs from fluent-logger, fluent-cat or other fluentd instances
<source>
  @type forward
</source>

<match syslog.**>
  @type elasticsearch
  logstash_format true
  flush_interval 10s # for testing
</match>
```

執行 fluentd

```
fluentd -c ./fluent/td-agent.conf -vv
```

## 參考資料

[Free Alternative to Splunk Using Fluentd - Fluentd](http://docs.fluentd.org/v0.12/articles/free-alternative-to-splunk-by-fluentd)
