---
layout: post
title: Javascript核心筆記：namespace
published: true
date: 2015-02-27 14:39
tags:
  - javascript
comments: true

---
##靜態命名空間
1. 直接指定
```js
var myApp = {}

myApp.id = 0;

myApp.next = function() {
    return myApp.id++;
}

myApp.reset = function() {
    myApp.id = 0;
}

window.console && console.log(
    myApp.next(),
    myApp.next(),
    myApp.reset(),
    myApp.next()
); //0, 1, undefined, 0
```

2. 使用物件實字（Object Literal Notation）


3. 使用設計模式Module Pattern來建構

##動態命名空間
