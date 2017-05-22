---
author: StevenTTuD
layout: post
title: "EFK(3) - Rails 與 Fluentd 的串接方法"
published: true
date: 2017-05-16 21:23
tags:
  - Fluentd
  - OSX
  - EFK
  - Devops
comments: true

---

## 1. 幾種可以跟 Rails 串接的方式

1. gem 'fluent-logger'
    [Centralize Logs from Ruby Applications](http://docs.fluentd.org/v0.12/articles/ruby)

2. 自定解析
    [fluent/fluent-plugin-grok-parser: Fluentd's Grok parser](https://github.com/fluent/fluent-plugin-grok-parser)

3. 使用 Rails-logger 來蒐集
    [Collecting and Analyzing Ruby on Rails Logs | Fluentd](http://www.fluentd.org/datasources/rails)

這篇我們採用的方式是 1，優點是可以自定義 log 的形式，缺點是需要自行設計 log 系統。如果你只是想把原本存在於 `log/prodcution.log` 搜集到 EFK Stack，那可以使用 3 的方式來整合 Rails-app 的 log。而如果你想蒐集 Rails 以外的 log 如 apache 或是 nginx 的 log，則可以使用 2 的方式。


## 2. Fluentd agent 與 Rails 的串接

### step 1: 使用 Gem 安裝 Fluentd

這邊使用的方法是使用 gem 安裝，
配合 rvm 方便管理，不會污染環境

```
gem install fluentd -v "~> 0.12.0" --no-ri --no-rdoc
```

> Error: 因為我的開發環境用的 Rails 版本較舊，bundler 使用的是 1.14.4，在開發的時候遇到錯誤訊息`gems/bundler-1.14.4/lib/bundler/rubygems_ext.rb:45:in full_gem_path'`。
Solution：輸入 `gem update --system` 更新系統 gem 的版本。

### step 2: 產生設定檔

輸入以下指令

```
fluentd --setup ./fluent
```

資料夾會變成這樣

```
$ tree .
.
└── fluent
    ├── fluent.conf
    └── plugin
```

### step 3: 啟動 fluentd

```
fluentd -c ./fluent/fluent.conf -vv &
```

輸入後 fluentd 就可以順利跑起來了

![](media/14943115845575/14943128690870.jpg)

## step 4: 測試 fluentd 功能是否正常

```rb
require 'fluent-logger'
```

初始化 Fluentd

```
Fluent::Logger::FluentLogger.open(nil, :host=>'localhost', :port=>24224)
```

來測試一下訊息

```rb
Fluent::Logger.post("fluentd.test.follow", {"from"=>"userA", "to"=>"userB"})
```

結果

```
2017-05-09 15:09:00 +0800 [warn]: fluent/agent.rb:170:emit: no patterns matched tag="fluentd.test.follow"
```

可以看到 fluentd 並沒有成功收到我們想記錄的 log，並發現警告訊息，原因是 Fluentd 蒐集 log pattern 並沒有包含 `fluentd.test.follow`

### step 5: 讓 Fluentd 可以蒐集到我們要的 log pattern

來修改 Fluentd 的設定檔，打開設定檔：

```
vim ./fluent/fluent.conf
```

加入 pattern

```
<match fluentd.test.follow.**>
  @type stdout
  @id stdout_output
</match>
```

重新啟動 fluentd，再記錄一次 log 看看：

```rb
Fluent::Logger::FluentLogger.open(nil, :host=>'localhost', :port=>24224)
Fluent::Logger.post("fluentd.test.follow", {"from"=>"userA", "to"=>"userB"})
```

結果

```
2017-05-09 15:16:19 +0800 fluentd.test.follow: {"from":"userA","to":"userB"}
```

這樣就完成單機上的 fluentd-agent 與 Rails Application 的串接了。


## 3. 介紹 Forward Input Plugin

Fluentd 蒐集 Rails 的 Log 的時候，並不是使用讀取檔案的方式。
而是使用 `forward input plugin` 來幫助蒐集。
forward input plugin 是一個可以蒐集 tcp 或是 utp 協定的 fluentd plugin。

[forward Input Plugin - Fluentd](http://docs.fluentd.org/v0.12/articles/in_forward)

## 參考資料

[Quickstart Guide - Fluentd](http://docs.fluentd.org/v0.12/articles/quickstart)
[Installing Fluentd Using Ruby Gem - Fluentd](http://docs.fluentd.org/v0.12/articles/install-by-gem)
[Centralize Logs from Ruby Applications - Fluentd](http://docs.fluentd.org/v0.12/articles/ruby)
