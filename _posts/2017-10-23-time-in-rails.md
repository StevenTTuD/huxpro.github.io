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

## Rails 中的時間

Rails 中的時間是個有趣的議題，如果不多加注意，很容易被雷到。
尤其是一般在本地端開發的的時候並不會有時區不同的問題，但是一放到 Server 上，就會忽然爆炸。
是不能不小心的一個問題。
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

## Time.now VS Time.current

接著來比較 Time.now 和 Time.current

```rb
Time.now.zone
=> "UTC"
Time.current.zone
=> "CST"
```

很明顯的可以看到，如果是 `Time.now` 的話回傳的會是格林威治時間 `UTC`, 但若是使用 `Time.current` 回傳的則會是我們在 `config/application.rb` 裡面設定的 Time.zone。究竟為什麼會有這個差異呢? 其實很簡單，因為 Time.now 是 Ruby 的原生物件，而 Time.current 是 Rails ActiveSupport 的物件。

```rb
Time.now.class
=> Time
Time.current.class
=> ActiveSupport::TimeWithZone
```

我們再來做個測試，來看看剛剛的 `user.created_at` 是不是真的如果們所想:

```rb
User.last.created_at.class
=> ActiveSupport::TimeWithZone
```

太棒了! 果然如此，這樣一來是不是很容易理解 Rails 的時間了。

## Rails 時間的另一個重點 - SQL

Rails 時間另一個需要注意的地方是 SQL 的時間。來比較一下使用 Time.now 和 Time.current 在 ActiveRecord Relation 中的時間:

```rb
User.where("created_at > ?", Time.now)
#=> SELECT "users".* FROM "users" WHERE "users"."deleted_at" IS NULL AND (created_at > '2017-11-17 14:46:01.698018')

User.where("created_at > ?", Time.current)
#=> SELECT "users".* FROM "users" WHERE "users"."deleted_at" IS NULL AND (created_at > '2017-11-17 14:46:09.655406')
```

筆者在測試的時候是台北時間(CST)晚上 22:46，可以發現兩者依賴的時區都是在 `config/application.rb` 設定的時區。
