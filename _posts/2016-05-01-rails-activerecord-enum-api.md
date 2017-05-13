---
layout: post
title: Rails 筆記 - 使用 ActiveRecord::Enum 設定狀態
published: true
date: 2016-05-01 12:05
tags:
  - Rails
comments: true

---
## 使用 Array 表達狀態

```rb
class Order
	KIND = [ ['未付款', 0], ['已付款', 1], ['已出貨',2] ]
end
```

代表 orders 這張表中的 kind 欄位如果是 1 ，則此訂單狀態是未付款的。
寫成這樣的好處是在使用 select view helper 的時候可以直接丟進去

```erb
<%= select_tag "訂單狀態", options_for_select( Order::KIND ) %>
```

把 KIND 打成這樣是 OK 的，但是我想把它打的好看一點。今天發現了有 ActiveRecord Enum 可以讓他變得有趣點。而且是只要有 ActiveRecord 就可以用，不需要裝其他的 gem。

## ActiveRecord::Enum 介紹

首先需要宣告對應 DB 裡面的值，第一個就代表對應 0 , 第二個就代表對應 1。

```rb
class Conversation < ActiveRecord::Base
  enum status: [ :active, :archived ]
end
```

除了可以用文字表示原本是數字的 conversation 的狀態以外，也加了 `active!` 和 `active? `這種方便的方法，`active!`用來改變 conversation 的狀態為 active ，而 `active?` 則用來辨識 conversation 狀態是否為 active。這樣寫語意上更棒，更好讀。

```rb

# conversation.update! status: 0
conversation.active!
conversation.active? # => true
conversation.status  # => "active"

# conversation.update! status: 1
conversation.archived!
conversation.archived? # => true
conversation.status    # => "archived"
```

當然你也可以直接指定值給 `conversation.status`

```rb
# conversation.status = 1
conversation.status = "archived"

conversation.status = nil
conversation.status.nil? # => true
conversation.status      # => nil
```

## 用 Hash 儲存 enum

宣告成 hash 型態的話，我們可以用 status 的複數型態 statuses 來取得這個 Hash，之後的取值也從 Conversation.active 變成了 Conversation.statuses[:active]，兩個得到的結果是一樣的。不過使用 Hash 時可以明確知道我現在在修改的是 status，屬性一多的時候不會造成混淆。

```rb
class Conversation < ActiveRecord::Base
  enum status: { active: 0, archived: 1 }
end

Conversation.statuses[:active]    # => 0
Conversation.statuses["archived"] # => 1
```

## 取得 options_for_select 所需的陣列

這樣宣告後，使用 status 的複數型態 `statuses` 可以取得剛剛輸入的陣列

```
[5] pry(main)> Conversation.statuses
=> {"active"=>0, "archived"=>1}
```

OK 現在要的東西很接近我們要的了，再做一點加工。

```
[11] pry(main)> Conversation.statuses.invert.to_a
=> [[0, "active"], [1, "archived"]]
```

done! 現在用 enum 也可以製作 options_for_select 專屬的陣列了。

## 參考資料
[ActiveRecord::Enum](http://edgeapi.rubyonrails.org/classes/ActiveRecord/Enum.html)
[What's new in edge Rails: Active Record enums](https://robots.thoughtbot.com/whats-new-in-edge-rails-active-record-enum)