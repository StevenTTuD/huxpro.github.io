---
author: StevenTTuD
layout: post
title: "CORS in Rails"
published: true
date: 2017-05-20 22:34
tags:
  - Rails
  - CORS
  - gem
comments: true

---

## Problem

跨網域存取的時候需要使用 CORS 來讓不同網域也可以存取相同網域的資源。請看下圖，Server 返回的 Response 必須明確指出哪些網域可以存取該 Server 的資源。
有這樣的行為模式的話，是不是可以在 Controller 中的 Response 加上 Header 來達成 CORS 呢？

答案是否定的。

因為當 PUT 或是 POST ... 等等的 HTTP Verb 使用 CORS 時，
需要先使用 `PATCH` 這個 HTTP Verb 來確認有沒有跨網域存取的權限。
當 PATCH 動作打到 Rails Route 的時候，就會發現，沒有對應的 Route，於是就回 404 了。

## Solution

所以如果以後有 CORS 需求的話，請使用 [gem 'rack-cors'](https://github.com/cyu/rack-cors)，從 middleware 層處理 CORS，會是比較萬用的解法。

![](https://lh3.googleusercontent.com/-_Pes10FnRo4/WSLrN-5YTJI/AAAAAAAAKyw/TS2_O2GiWJocUjlo15glwizmLVOW4JUXQCHM/I/14954599810526.jpg)

(圖片來源: MDN)
