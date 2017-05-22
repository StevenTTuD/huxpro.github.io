---
author: StevenTTuD
layout: post
title: "EFK(6) - 使用 docker 包裝 Fluentd"
published: true
date: 2017-05-19 23:15
tags:
  - EFK
  - Fluentd
  - Devops
comments: true

---

## 1 Aggregator 與 Forwarder


![](https://lh3.googleusercontent.com/-ISAjtMLBHsQ/WSL5U_Aw87I/AAAAAAAAKzA/a5gcrr4jQ508MINa9Yy1aVR23CMFccYuwCHM/I/14954621237379.jpg)


## 2 使用 docker 建立 fluentd image

為了模擬需要的環境，我們來使用 docker 建立 fluentd image

參考[fluent/fluentd-docker-image](https://github.com/fluent/fluentd-docker-image)來製作個人化的 fluentd image

### 2.1 準備工作

建立 custom-fluentd 資料夾

```
mkdir custom-fluentd
```

建立 plugin 資料夾

```
cd custom-fluentd
mkdir plugins
```

下載 Dockerfile 範例

```
curl https://raw.githubusercontent.com/fluent/fluentd-docker-image/master/Dockerfile.sample > Dockerfile
```

下載 fluent.conf 範例

```
curl https://raw.githubusercontent.com/fluent/fluentd-docker-image/master/fluent.conf.erb > fluent.conf
```

#### 2.3 製作 Fluentd Log Aggregator Image

```
FROM fluent/fluentd:onbuild
StevenTTuD <steventtud@gmail.com>

USER root

RUN apk add --update --virtual .build-deps \
        sudo build-base ruby-dev \

 # cutomize following instruction as you wish
 && sudo -u fluent gem install \
        fluent-plugin-secure-forward \

 && sudo -u fluent gem sources --clear-all \
 && apk del .build-deps \
 && rm -rf /var/cache/apk/* \
           /home/fluent/.gem/ruby/2.3.0/cache/*.gem

USER fluent

EXPOSE 24284
```

因為我們需要輸出給 Elasticsearch Kibana 可以接收的格式，
所以需要安裝 `elasticsearch plugin`

在的下面`fluent-plugin-secure-forward \`加上`        fluent-plugin-elasticsearch \` 變成：

```
 # cutomize following instruction as you wish
 && sudo -u fluent gem install \
        fluent-plugin-secure-forward \
        fluent-plugin-elasticsearch \
```


Aggregator 與 Forwarder 不同之處會是 config 檔與 plugin 的安裝，因為 Forwarder 不負責過濾與輸出 log 格式，只負責運送 Log，以此觀念下我們來構建 Fluentd 的高可用架構。

編輯 fluntd.conf (官方 image 所設定路徑)
input 使用 forward 來接收從 Fowarder 傳過來的 log。
output 傳送到 elasticsearch。

```
# Input
<source>
  @type forward
  port 24224
</source>

# Output
<match fluentd.es.**>
  @type elasticsearch
  logstash_format true
  flush_interval 1s # for testing
</match>
```

產生 docker image 的方法是到剛剛我們編輯好的 Docker File 資料夾底下(包含 config 檔案)輸入

```
docker build -t my-fluentd-aggregator:1.0 ./
```

編譯完成後即會有叫做 `my-fluentd-aggregator` 的 image 可以使用。

接著輸入 `docker run -p 24224:24224 custom-fluentd` 即可啟動 container，並 bind 至本機 24224 port 上。

### 2.4 製作 Fluentd Log Aggregator Image

Forwarder 也是如法炮製，差別在於 Dockerfile 不需加上 Elasticsearch Output 套件。

並修改 fluent.conf 檔：

```
# input

<source>
  @type forward
  port 24224
</source>

# forward to aggregator

<match fluentd.forwarder.**>
  @type forward
  send_timeout 10s
  heartbeat_interval 1s
  heartbeat_type tcp
  # optional
  recover_wait 10s
  phi_threshold 16
  hard_timeout 10s

  buffer_type file
  buffer_path ~/splashtop/EFK/buffer/
  buffer_chunk_limit 8m
  buffer_queue_limit 4096
  flush_interval 5s
  retry_wait 20s
  <server>
    # primary host
    host xxx.xxx.xxx.xxx
    port 25225
  </server>
  <server>
    host 192.168.0.2
    port 24224
    standby
  </server>
</match>

```

完成後輸入

```
docker build -t my-fluentd-forwarder:1.0 ./
```

編譯完成後即會有叫做 `my-fluentd-forwarder` 的 image 可以使用。

接著輸入 `docker run -p 24225:24224 custom-fluentd` 即可啟動 container，並 bind 至本機 24225 port 上。

## 3.實際運作

實際運作的時候只需將 fluent.conf 中的 ip 位置，改成真實 Server IP，即可開始運作。
