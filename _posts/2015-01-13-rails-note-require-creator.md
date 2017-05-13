---
author: StevenTTuD
layout: post
title: 'Rails note : Require Creator'
published: true
date: 2015-01-13 00:42
tags:
  - Rails
comments: true

---
增加一個叫做Creator的別名，讓我們在判別作者時文意更加通順。

##設定發文者
我們透過foreign key新增一個叫做creator的別名。作法是修改model/post.rb原本的
```rb
	belongs_to :user
```
改成
```rb
	belongs_to :creator, foreign_key: "user_id", class: "User"
```
原本我們要使用post.user來取得文章作者，現在可以使用post.creator來取得作者。


##只有作者可以新增或修改文章

1. 到posts controller加上`before_action :require_creator, only: [:edit, :update]`

1. 在application controller中加入一個access dined的方法，這個方法的目的是「如果不是creator來新增或修改文章，就會出現錯誤訊息」。
```rb
class ApplicationController < ActionController::Base
 .
 .
 .
      def access_denied
        flash[:error] = "You can't do that."
        redirect_to root_path
      end
end
```
1. 接著在posts controller加上剛剛使用before action驗證的方法，require_creator驗證了兩件事情，第一是你必須登入，第二則是你必須是文章的作者。
```rb
def require_creator
      access_denied unless logged_in? && (current_user == @post.creator)
end
```

這樣子就做完發文的驗證了。