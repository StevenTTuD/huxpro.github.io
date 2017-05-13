---
layout: post
title: Ruby - 爬網頁時遇到的編碼錯亂問題
published: true
date: 2016-09-10 13:50
tags:
  - Ruby
  - Crawler
comments: true

---
### 問題描述

編碼問題是寫爬蟲常會遇到的問題。當你沒有處理好編碼問題，爬回來的網頁無法進行字串的切割，也無法使用 nokogiri 抽離需要的部份。

### 解決方法

1. 找到原始網頁的編碼`chartset='big'`
2. 把網頁 force_encoding 至原始格式
3. 將網頁轉換成 utf-8，這是 ruby 預設的編碼，也是 nokogiri 接受的編碼。

`force_encoding` 的意思是強制使用某種編碼格式，但是其實不會進行編碼的轉換，因為ruby預設是utf-8，
所以如果網頁是 big5 我們就得先幫網頁加上網頁原有的編碼格式。

設定好網頁原始的編碼之後，我們才可以將載下來的網頁轉成我們要的格式，所以最後得使用`encode`指令來轉換成我們要的utf-8格式。

完成以上步驟之後，你就可以順利的進行字串的處理了:)
