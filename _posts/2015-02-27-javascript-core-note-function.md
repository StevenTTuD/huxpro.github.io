---
layout: post
title: 'Javascript核心筆記：function '
published: true
date: 2015-02-27 09:55
tags:
  - Javascript
comments: true

---
###javascript的函數是一級物件(First-Class)
function是由Function的實例，並且在javascript中可以指定給變數，函式與數值的地位相同，並不會像有些語言中，無法像數值一樣地被指定，不會淪為二等公民，因此，對於支持函式可如數值一樣指定給變數的語言，我們稱函式在這個語言中是一級函數。
```js
var number = 10;        // Number literal
var obj = {x : 10};     // Object literal
var array = [1, 2, 3];  // Array literal
var func = function() { // Function literal
    // do something...
};
```
函式既然是物件，本身亦可擁有特性。例如函式有個 length 特性，代表其參數個數：
```js
var gcd = function g(num1, num2) {
    return num2 != 0 ? g(num2, num1 % num2) : num1;
};

console.log(gcd.length); // 2
```

###函數的回傳值

函式沒設定return時，預設回傳`undefined`。


###輸入的參數與引數不符合時
如果函式傳入的參數不足時是可以執行的，不足的部份會自動補上`undefined`。
```js
function func(a, b) {
    console.log(a);
    console.log(b);
}

func(10, 20);         // 10 20
func(10);             // 10 undefined
func();               // undefined undefined
func(10, 20, 30, 40); // 10 20
```
傳入比參數個數還多的引數也是可行的，在函式內部會自動宣告 arguments 名稱參考至具陣列外觀的物件，上頭帶有所有傳入的引數。例如，你可以如下設計一個加總數值的函式：
```js
function sum() {
    var sum = 0;
    for(var i = 0; i < arguments.length; i++) {
        sum += arguments[i];
    }
    return sum;
}

console.log(sum(1, 2));;      // 3
console.log(sum(1, 2, 3));    // 6
console.log(sum(1, 2, 3, 4)); // 10
```
arguments 不是 Array 實例，它只是具有數字作為特性，特性參考至傳入的引數，並具有 length 特性代表傳入引數的個數。你可以使用arguments.length來檢查引數個數。

在 `EMCAScript 5 前的版本`，參數只是具名的引數，你改變參數的值，arguments 對應索引的參考值也會相應的改變。然而，若採用 `EMCAScript 5 嚴格模式`，參數的值 與 arguments 的元素值彼此互不影響。

###函式宣告式、函式表達式與匿名函數

- 表達式
	- 具名表達式 var add = function add(a, b){return a+b;};
	- 匿名表達式 var add = function (a, b){return a+b;};
- 宣告式
	- 具名宣告 function add(a, b){return a+b;};
	- 匿名宣告 function (a, b){return a+b;};

宣告式的特性有：
- 在javascript引擎解析javascript程式碼時，會受到Function declaration Hoisting，提升到作用域的最前面執行。
- 後面加括號`不可以`立刻調用函數

表達式的特性有：
- 當javascript引擎執行到表達式的那行程式碼時，才會開始解析。
- 後面加括號`立刻`調用函數。


###立即函數
要在函數體後面加括號就能立即調用，則這個函數必須是函數表達式，不能是函數宣告式。所以立即函數的本質是函數表達式。立即函數可以讓括號內的變數變成區域變數，這部份請參考Scope的筆記。

##後記
除了本篇提到的性質以外，物件的建構式也是由function來構建，學習時一併理解以釐清不清楚的地方。


##參考資料
[js中(function(){…})()立即执行函数写法理解](http://dengo.org/archives/1004)