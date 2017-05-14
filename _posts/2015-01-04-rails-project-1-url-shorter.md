---
author: StevenTTuD
layout: post
title: Rails Project 1 URL Shortener
published: true
date: 2015-01-04 01:02
tags:
  - Rails
comments: true

---
## 前言

接下來我要藉由實作一些比較小型的Rails Project，來練習工程師必備的技能，其中最主要訓練的目標是能抓出需要實作的use case與如何從use case中實踐功能。因為沒有網頁的相關背景，如何磨練Html css javascript的基本實作能力也是這一階段的重點。

熟悉了如何使用Rails完成所要的需求與一些常見功能的開發之後，下一階段可以是學習Rails如何跟API互動或是開始學習Design Patterns。學習Dessign Pattern的原因有：一是可以讀的懂程式碼，二是了解為什麼要這麼寫。anyway，先把眼前的事情做好最重要let's do it！

## Use Case , Page Flow and Data

在練習實作Project的過程中，我會格外地注意Use Case、Page Flow和Data。一個網站從無到有，便是先抓出使用者的需求，也就是Use Case，可以表達出身為不同的使用者，各需要哪些需求。
### Use Case
Url Shortener顧名思義要能夠縮減網址，我們可以抓出幾個需求。
- 使用者可以使用URL Shortener縮短網址
- 輸入URL Shortener產生的網址時必須連接到原網址。

### Page Flow

### Data


# URL Shortener in Rails

1. 建立專案
```
rails new url_shortener
```

1. route中加入url的resource，並指定首頁為`url#new`
```rb
root to: :redirect('/urls/new')
resources :urls
```

1. 建立controller，controller要加s
```
rails generate controller urls
```

## Model

1. 建立model，名詞是單數。要使用的Model有兩個欄位，original用來記錄原始網址，random_id用來記錄縮址。
```
rails g model url original:string random_id:string
```

1. 在model加上validates，這邊的意思是儲存時一定要有「原網址」的欄位。
```rb
validates :original, presence: true
```



## Controller
#### Index
繼續完成controller，從index開始著手，在index我們希望可以看到全部儲存的連結。

```ruby
class UrlsController < ApplicationController
  def  index
	  @urls = Url.all
  end
end
```

#### Show
在action show我們希望可以做到的功能是「當使用者輸入縮址時，會轉址到原網址」，這也是也這個URL Shortener的核心功能。

```rb
class UrlsController < ApplicationController

  .
  .
  .

  def show
  	url = Url.where( random_id: params[:id] ).first

    if url
    	redirect_to url.original
    else
    	render "index"
    end
  end

end
```

#### New
 在action new要做的事情是「當使用者輸入原始網址，會產生相對應的縮網址」

```rb
class UrlsController < ApplicationController

  .
  .
  .

  def new
  	@url = Url.new

    letters = [('a'..'z'),('A'..'Z')].map{|i| i.to_a}.flatten
    @url.random_id = (0...8).map{ letters[rand(letters.length)]}.join
  end
end
```
letter陣列用來裝所有的字母包含大寫A-Z與小寫a-z。flatten使屬於Array的method是用來把許多陣列合併成單一的陣列。第二行

[產生隨機小寫英數字 幾種方法 and 效能](http://railsfun.tw/t/and/46)


#### Create
儲存一筆url資料，記得要使用strong parameter核對欄位。
```rb
class UrlsController < ApplicationController
	.
  .
  .
  def urls_params
    params.require(:url).permit(:original, :random_id)
  end

  def create
    @url = Url.new(urls_parmas)
    if @url.save
      redirect_to url_path
    else
      render "new"
    end
	end
end
```
>比較show與new可以發現，前面有加@的變數，是view會使用到的變數。如果view不會用到，那就不要添加@，以免造成混淆，這樣在閱讀程式碼的時候，可以更清楚的知道這些變數作用在哪些地方。

>來複習一下render的用法主要有四種：第一種是直接回傳結果，回傳的格式可以是xml,json,text...等等檔案格式。範例：`render text: "hello world"`、`render json: @event.to_json`。第二種是render template。可以直接指定template的路徑如：`render "/events/index.html.erb"`。如果是同controller的action可以寫成`render "index"`。第三種是回傳status code例如：`render status: 500`。第四種是回傳某template使用的layout，例如：`render layout: "special_layout"`。
其他用法請參考：[Layouts and Rendering in Rails](http://guides.rubyonrails.org/layouts_and_rendering.html#using-render)

## Views
因為URL Shortener的action show是轉址，並不需要view，所以我們要實作的view只有index.html.erb和new.html.erb。

#### index.html.erb

```html.erb
<h1>Url Shortener</h1>
<% @urls.each do |url| %>
  <p>
    <%= link_to url.random_id, url_path(id: url.random_id) %> --> <%= url.original %>
  </p>
<% end %>

<%= link_to "Shorten another URL", new_url_path %>
```

#### new.html.erb

```
<h1>Shorten a URL</h1>
erb
<% @url.errors.each do|attr,msg| %>
  <%= msg %> on <%= attr %>
<% end %>

<%= form_for @url do |f| %>
  <p>
    <%= f.label :original %> : <%= f.text_field :original %>
  </p>
  <%= f.hidden_field :random_id, value: @url.random_id %>
  <%= f.submit "Shorten my URL" %>
<% end %>
```
## Summary

URL Shortener這個小Project需求上跟一般實作的留言板有點不一樣，但還是可以用new和index兩個action來達成了Use Case所抓出來的需求。這非常有趣，多做點不同專案的好處就是能夠用不同的角度來累積經驗。
