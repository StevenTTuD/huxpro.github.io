---

layout: post
title: 'Javascript核心筆記：scope '
published: true
date: 2015-02-27 12:40
tags:
  - javascript
comments: true

---
1. 每個function在呼叫時都會建立新的 Function execution context，有個物件用來代表 `Execution context`，而區域變數則是 context 物件上的特性。

1. JavaScript 在查找變數時，會先在目前 context 物件上找，若找不到指定名稱，則會到外層 context 物件上找，若找不到，就再到更外層 context 物件找，直到全域物件為止，這樣的順序形成變數查找的 `Scope chain`。

1. closure是典型應用scope chain的例子，在內部的 f 函式中 context 物件上找有無 x 特性時，並沒有找到，於是在包裹 f 的 doSome 呼叫物件上查找有無 x，也就是查找 f.__parent__ 上有無 x，此時找到了。
```js
function doSome() {
    var x = 10;
    function f(y) {
        return x + y;
    }
    return f;
}
```

##參考資料
[JavaScript 語言核心（13）在 Scope chain 查找變數 by caterpillar | CodeData](http://www.codedata.com.tw/javascript/essential-javascript-13-scope-chain/)