---
author: StevenTTuD
layout: post
title: Javascript核心筆記：建構式與prototype
published: true
date: 2015-02-27 13:25
tags:
  - javascript
comments: true

---
## 建立物件時，實際上做了什麼事?
使用 new 關鍵字時，JavaScript 會先建立一個空物件，接著設定物件的原型為函式的 prototype 特性所參考的物件，然後呼叫建構式並將所建立的空物件設為 this。接下來依照建構式設定實例上的特性，最後再由prototype補上未設定的特性。

## 建構式如何初始化實例
```js
var p1 = new Person('Justin', 35);
var p2 = new Person('Monica', 32);
var p3 = new Person('Irene', 2);

console.log(p1.toString());  // [Justin,35]
console.log(p2.toString());  // [Monica,32]
console.log(p3.toString());  // [Irene,2]
```
當你使用new產生Person物件時。其實是透過如下面例子的建構式function Person 初始化了建構式所設定的各個特性。

```js
function toString(){
	return '[' + this.name +',' + this.age + ']';
}

function Person(name, age){
	this.name = name;
	this.age = age;
	this.toString = toString;
}

var p = {}
Person.call(p, 'Justin', 35);

console.log(p.toString());
```

## constructor特性
每個透過 new 建構的物件，都會有個 constructor 特性，參考至當初建構它的函式。例如：
```js
function Person() {}
var p = new Person();
console.log(p.constructor == Person);  // true
```
事實上，每個函式實例建立時，都會在函式實例上以空物件建立 prototype，然而在空物件上設定 constructor 特性，也因此每個 new 建構的物件，都可以找到 constructor 特性。例如：
```js
function Some() {}
console.log(Some.prototype.constructor);  // [Function: Some]
```


## prototype特性
JavaScript 在尋找特性名稱時，會先在實例上找尋有無特性，以下例而言，p1 上會有 name 與 age 特性，所以可以直接取得對應的值。如果物件上沒有該特性，會到物件的原型上去尋找，以下例而言，p1 上沒有 toString 特性，所以會到 p1 的原型上尋找，而 p1 的原型物件此時也就是 Person.prototype 參考的物件，這個物件上有 toString 特性，所以可以 找到 toString 所參考的函式並執行。
```js
function Person(name, age) {
    this.name = name;
    this.age = age;
}
 
Person.prototype.toString = function() {
    return '[' + this.name + ', ' + this.age + ']';
};
 
var p1 = new Person('Justin', 35);
var p2 = new Person('Momor', 32);
 
console.log(p1.toString());   // [Justin, 35]
console.log(p2.toString());   // [Momor, 32]
```

要注意的是，只有在查找特性，而物件上不具該特性時才會使用原型，如果你對物件設定某個特性，是直接在物件上設定了特性，而不是對原型設定了特性。例如：
```js
function Some() {}
Some.prototype.data = 10;

var s = new Some();
console.log(s.data);                 // 10

s.data = 20;
console.log(s.data);                 // 20
console.log(Some.prototype.data);    // 10
```
在上例中可以看到，你對 s 參考的物件設定了 data 特性，但並不影響 Some.prototype.data 的值。

### 如何實現private
對熟悉物件導向私有（private）特性的人來說，可能覺得這不安全，這相當於在物件導向觀念中，每個類別成員都是公開成員的意味。JavaScript 本身並沒有支援物件導向私用特性的語法，如果你想模擬，則可以如下：
(...待補充)

## 參考資料
[JavaScript 語言核心（13）在 Scope chain 查找變數 by caterpillar CodeData](http://www.codedata.com.tw/javascript/essential-javascript-13-scope-chain/)
[JavaScript 語言核心（14）隱藏諸多細節的建構式 by caterpillar CodeData](http://www.codedata.com.tw/javascript/essential-javascript-14-constructor/)
