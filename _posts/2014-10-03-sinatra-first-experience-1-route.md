---
author: StevenTTuD
layout: post
title: Sinatra初體驗( 1 )：Route
published: true
date: 2014-10-03 12:04
tags:
  - Ruby
  - Sinatra
comments: true

---
# First Sinatra App

1.輸入```gem install sinatra```安裝gem

2.建立app.rb檔
```rb
require "sinatra/base"

class App < Sinatra::Base
	get '/' do
		"Hello World!!"
	end
end
```
3.建立config.ru

```rb
	require "./app" # relative file path

	run App   #class Name
```
4.輸入```rackup```啟動server，在 http://localhost:9292即可看到Hello World

## 使用curl來模仿HTTP Verb - GET
1. 輸入```curl -v "http://localhost:9292"```
2. 可以看到以下畫面
/Users/Steven/Desktop/Screen Shot 2014-10-03 at 14.41.37.png

RESTful HTTP Post:
```
curl -X POST -d "http://localhost:9292"
```
[http-post-and-get-using-curl-in-linux](http://stackoverflow.com/questions/14978411/http-post-and-get-using-curl-in-linux)

[curl指令用法](http://evelynnote.blogspot.tw/2011/03/curl.html)

## 改寫app為post
```rb
require "sinatra/base"

class App < Sinatra::Base
	post '/' do
		"Hello World!!"
	end
end
```

輸入```curl -X Post -v -d "" http://localhost:9292```
這邊分號沒有寫錯，因為就是要傳一個空字串給server。
這樣server就會傳回http response

## 接著來大亂鬥
app.rb
```rb
require "sinatra/base"

class App < Sinatra::Base
	get '/' do
		"Hello World!!"
	end

	post '/' do
		"Hello World via POST!!"
	end

	put '/' do
		"Hello World via PUT!!"
	end

	delete '/' do
		"Hello World via DELETE!!"
	end
end
```
輸入```curl -X POST -v -d "" http://localhost:9292```
回應```Hello World via POST!!```
輸入```curl -X PUT -v -d "" http://localhost:9292```
回應```Hello World via PUT!!```
輸入```curl -X DELETE -v -d "" http://localhost:9292```
回應```Hello World via DELETE!!```

## 解析完整的POST

```
    > POST / HTTP/1.1
    > User-Agent: curl/7.30.0
    > Host: localhost:9292
    > Accept: */*
    > Content-Length: 0
    > Content-Type: application/x-www-form-urlencoded
    >
    < HTTP/1.1 200 OK
    < Content-Type: text/html;charset=utf-8
    < Content-Length: 22
    < X-Xss-Protection: 1; mode=block
    < X-Content-Type-Options: nosniff
    < X-Frame-Options: SAMEORIGIN
    * Server WEBrick/1.3.1 (Ruby/2.0.0/2013-11-22) is not blacklisted
    < Server: WEBrick/1.3.1 (Ruby/2.0.0/2013-11-22)
    < Date: Fri, 03 Oct 2014 07:58:10 GMT
    < Connection: Keep-Alive
    <
    * Connection #0 to host localhost left intact
    Hello World via POST!!%
```

比較重要的欄位有

1. 第一行，說明是使用哪種http動詞
2. ```Content-Type: text/html;charset=utf-8``` 說明是html
3. 最後輸出的就是route相對輸出的內容

## 接下來
```rb
get "/hello/:first_name/:last_name" do |first, last|
  "hello#{first}#{last}"
end
```
在網址列輸入http://0.0.0.0:9292/hello/steven/huang
網頁上就會印出hellostevenhuang

### 這樣寫的缺點
這樣寫的話有如果網址列輸入http://0.0.0.0:9292/hello/test
就會出現Sinatra doesn’t know this ditty.的錯誤，因為這樣的寫法比較沒有彈性，route並沒有match到網址。

### 修正選擇性欄位的問題
```rb
get "/hello/:first_name/?:last_name?" do |first, last|
  "hello#{first}#{last}"
end
```
這樣的話:last_name就變成選擇性欄位
有沒有輸入都會進入這個route
來測試看看：
輸入http://0.0.0.0:9292/hello/steven
果然輸出了hellosteven
