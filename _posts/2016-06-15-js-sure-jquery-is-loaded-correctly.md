---
layout: post
title: JS - 確認 jQuery 是否正確載入
published: true
date: 2016-06-15 12:32
tags: []
categories: []
comments: true

---
### 使用情境

有時候你不能確定環境是否已經載入 jQuery，所以你也不能使用 jQuery.ready()。
例如：你想要在別人的網頁上執行一段 javascript 程式碼。
這時候你就可以用本篇筆記記錄的方法等待 jQuery 載入後再執行自己的程式碼。

程式碼：

```js

//如果沒有載入的話，會再執行一次直到 jQuery 正確載入。

function defer(method) {
    if (window.jQuery)
        method();
    else
        setTimeout(function() { defer(method) }, 1000);
}

//要執行內容用 function 包起來，丟進 defer 裡面執行。

defer(function () {
    alert("jQuery is now loaded");
});
```

不過這樣如果 jQuery 一直找不到的話就會變成無限迴圈，所以我們加個機制 - 如果重試超過五次，我們就結束程式。

```js
var i = 0;
function defer(method) {
    if ( i > 5 ) return ; 
    if (window.jQuery)
        method();
    else{
        i++;
        setTimeout(function() { defer(method) }, 1000);
    }
}

//要執行內容用 function 包起來，丟進 defer 裡面執行。
defer(function () {
    alert("jQuery is now loaded");
});
```

> 這邊因為是簡單例子，全域變數就不做處理，一般來說要盡量避免全域變數的使用。


[How to check if jQuery library is loaded?](https://www.mkyong.com/jquery/how-to-check-if-jquery-library-is-loaded/)

[javascript - How to make script execution wait until jquery is loaded - Stack Overflow](http://stackoverflow.com/questions/7486309/how-to-make-script-execution-wait-until-jquery-is-loaded)