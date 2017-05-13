---
layout: post
title: Ruby 爬蟲小技巧 - 處理 Html Entity
published: true
date: 2016-08-12 07:45
tags:
  - Ruby
  - Crawler
comments: true

---
這是一個在工作上遇到的小問題。
把網頁爬回來的時候有 HTML Entity 的編碼，看起來很不美觀。
舉例來說，爬回來的標題如果含有 HTML Entity 會是這個樣子:

```
PURUS空氣清淨器&#40;鴻海集團創星出品&#41;
```

如果我想要使用資料建立自己資料庫的時候勢必要對 html entity 做一些處理
這時候 Gem`htmlentities`就派上用場了。使用方法：

```rb
require 'htmlentities'

str = "PURUS空氣清淨器&#40;鴻海集團創星出品&#41;"
puts HTMLEntities.new.decode(str)
=> PURUS空氣清淨器(鴻海集團創星出品)
```

現在你可以把品名存進資料庫了 :D
其實這篇只是想記錄一下，HTML Entity這個名詞。歸類到編碼的類別方便以後查找。


### 參考資料：
[htmlentities.rubyforge.org](http://htmlentities.rubyforge.org/)