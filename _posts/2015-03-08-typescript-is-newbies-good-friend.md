---
layout: post
title: Typescript是新手的好朋友
published: true
date: 2015-03-08 14:17
tags: []
categories: []
comments: true

---
## 為什麼 Typescript 是新手的好朋友

typescript百分之百跟javascript相容，所有javascript的語法都可以在`.ts`typescript檔案中執行。因此就算是要javascrtip新手，也可以享受typescript帶來好處。typescript能夠讓

1. 目前暫時不需要去研究typescript的其他功能，只需要他可以宣告型態，靜態語言即時偵錯，與關連定義檔就可以檢查method，光是這幾項就很值得使用了

1. 需要注意的是因為typescript的許多特性，例如: class, module, interface 皆與原生javascript有所不同，如果對javascript還不了解，直接學習可能會更加的混淆。

1. 在javascript演進這麼快速的現在，要選擇任何一種framework或是語言都是一種巨大的投資，

## 選擇 Typescript 還有什麼好處？

1. 因為javascript的是個動態型別的語言，所以只能在執行時期偵錯，要真的執行到那一行，才會跳出錯誤訊息，這使的偵錯上非常的不容易。
1. typescript所能解決的問題：因為typescript提供了靜態型別檢查，在編譯的時候就可以檢查到！尤其是打錯字的問題和型別不一致的問題。
1. typescript新增了一些型別，讓我們可以宣告靜態型別。也保留了原本動態型別的特性，只要你設為any！。
1. function 也可以使用冒號設定回傳值。
1. module 類似namespace的存在，裡面可以包其他module、interface、class、function...
1. garbage collection(回收)是執行時期做的，只要參考的技術器是0的話，javascript就會自動回收他


## 其他
[Excelsior: [Java] 淺談 call by value 和 call by reference](http://brownydev.blogspot.tw/2011/06/java-call-by-value-call-by-reference.html)