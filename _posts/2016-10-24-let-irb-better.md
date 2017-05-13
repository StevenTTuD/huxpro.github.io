---
layout: post
title: Ruby - 讓 irb 更好用
published: true
date: 2016-10-24 23:56
tags: []
categories: []
comments: true

---
### 前言

在使用`pry`的時候我們可以回到上一個輸入的指令，擁有記錄指令歷史的功能。其實`irb`也可以！

### 開始修改

修改`~/.irbrc`

```rb
require 'irb/ext/save-history'
IRB.conf[:SAVE_HISTORY] = 200
IRB.conf[:HISTORY_FILE] = "#{ENV['HOME']}/.irb-history"
```

修改完之後按下方向鍵上和下即可使用上一個用過的指令。而實際上，歷史紀錄是儲存在`~/.irb_history`。

另一個常用的功能 - `autocomplete ` 我們也順便把它開起來。
在`~/.irbrc`中`irb/completion`即可，現在你的 irb 按下 tab 即可以自動補完。

```rb
require 'irb/ext/save-history'
require 'irb/completion'
IRB.conf[:SAVE_HISTORY] = 200
IRB.conf[:HISTORY_FILE] = "#{ENV['HOME']}/.irb-history"
IRB.conf[:AUTO_INDENT] = true
```



#### 參考資料

[Module: IRB (Ruby 2.0.0)](http://ruby-doc.org/stdlib-2.0.0/libdoc/irb/rdoc/IRB.html)

[Have ruby irb console save history | kitt hodsden's nags of a similar ilk](https://kitt.hodsden.org/comment/1656#comment-1656)