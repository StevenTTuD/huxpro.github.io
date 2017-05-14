---
author: StevenTTuD
layout: post
title: Javascript核心筆記：this
published: true
date: 2015-02-27 04:14
tags:
  - javascript
comments: true

---

### call可以讓你決定this的參考對象
在 JavaScript 中，函式是 Function 的實例，Function 都會有個 call 方法，可以讓你決定 this 的參考對象。舉例來說，你可以如下呼叫：
```js
function toString(){
	return '[' + this.name + ',' + this.age + ',';
}

var p1 = {
	name:'Justin',
	age :35
};

var p2 = {
	name : 'momor',
	age : 32
};

console.log( toString.call(p1) ); //[Justin, 35]
console.log( toString.call(p2) ); //[momor, 32]
```
妳可以覆寫call方法，讓this參考別的物件。
```js
function toString() {
    return this.name;
}

var p1 = {
    name     : 'Justin',
    toString : toString
};

var p2 = {
    name     : 'momor',
    toString : toString
};

console.log(p1.toString());        // Justin
console.log(p2.toString());        // momor
console.log(p1.toString.call(p2)); // momor
```

### 當call遇到有參數的函式時

call 方法的第一個參數就是用來指定函式中的 this 所參考的物件。如果函式原本具有參數，則可接續在第一個參數之後。例如：
```js
function add(num1, num2) {
    return this.num + num1 + num2;
}

var o = {num : 10};

console.log(add.call(o, 20, 30)); // 60
```

### 同樣決定this的參考對象還有apply可以使用決定
差別在於apply必須將引數蒐集起成陣列，做為第二個參數來呼叫函數。
```js
function add(num1, num2) {
    return this.num + num1 + num2;
}

var o1 = {num : 10};
var o2 = {num : 100};
var args = [20, 30];

console.log(add.apply(o1, args)); // 60
console.log(add.apply(o2, args)); // 150
```

### this與全域物件
如果呼叫函式時，無法透過 . 運算、call、apply 等方法確定 this 的對象，如果不是嚴格模式，那麼 this 會直接轉為參考全域物件（Global object）。

全域物件是 JavaScript 執行時期全域可見的物件，在不同的環境中想要取得全域物件，會透過不同的名稱，像是 Node.js 中可以使用 global，瀏覽器中可以透過 window 或在全域範圍使用 this，Rhino（或 JDK8 的 Nashorn）可以在全域範圍使用 this 取得。

因此，如果你想統一全域物件的變數名稱，例如統一使用 global，可以透過類似以下的方式：
```js
var global = global || (function() {
    return this;
})();
```

## this參考的對象並非以附屬在哪個物件而定
this 實際參考的對象，是以呼叫方式而定，而不是它是否附屬在哪個物件而定。例如就算函式是附屬在函式上的某個特性，也可以這麼改變 this 所參考的對象：
```js
function toString() {
    return this.name;
}

var p1 = {
    name: 'Justin',
    toString: toString
};

var p2 = {
    name: 'momor',
    toString: toString
};

console.log(p1.toString()); // Justin
console.log(p2.toString()); // momor
console.log(p1.toString.call(p2)); // momor
```


#### 參考資料 & 延伸閱讀
- [JavaScript 語言核心（11）this 是什麼？ by caterpillar - CodeData](http://www.codedata.com.tw/javascript/essential-javascript-11-what-is-this/)
- [Function.prototype.call - JavaScript - MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Function/call)