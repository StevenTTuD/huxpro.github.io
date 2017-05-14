---
author: StevenTTuD
layout: post
title: "使用Github展示靜態網頁"
published: true
date: 2015-10-12 19:06
tags:
  - Github
  - HTML / CSS
comments: true

---
這篇很簡短，但還是筆記一下怎麼用，因為網路上找的資料有點繁雜。

### 原理

github的gh-pages分支可以用來展示靜態網頁，推上去就可以正確展示了。

### 步驟

1. 建立新的branch`git branch gh-pages`
1. 推上github`git push origin gh-pages`
1. 需要注意的是首頁要命名為index.html
1. 到`[github name].github.io/[repository name]`網址查看，你的網頁已經展示在這個網址。