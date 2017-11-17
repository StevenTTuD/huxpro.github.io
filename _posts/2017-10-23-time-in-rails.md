---
author: StevenTTuD
layout: post
title: "Time in Rails"
published: true
date: 2017-10-23 22:34
tags:
  - Rails
  - Time
comments: true
---

Rails 中時間是個有趣的議題，
如果你在 `config/application.rb` 設定 Time.zone，範例如下:

```rb
config.i18n.default_locale = "zh-TW"
```

如果透過 ActiveRecord 來存取，取得的會是你在 `application.rb` 裡面設定的 Time.zone
比如說你有一個 `User` 物件:

```rb
user = User.first
user.created_at.zone
# => "CST"
```

CST 即中原標準時間. it's right!

## Time.now VS Time.zone

Time.now.zone




