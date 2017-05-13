---
layout: post
title: javascript核心筆記：Object
published: true
date: 2015-02-24 02:50
tags:
    - Javascript
comments: true

---
在 JavaScript 中，物件是 Object 的實例。你可以如下建立一個新的物件：
```js
var obj = new Object();
```
實際上，現在已經很少人這麼撰寫了，使用物件實字（Object literal）語法就可以建立一個物件：
```js
var obj = {};
```
上面你所看到的函式撰寫方式，稱之為`函式實字（Function literal）`，這就像你寫下一個數值實字、物件實字或陣列實字，會產生數值或物件等：

###搜尋物件上的特性

想要知道物件上有哪些自定義特性，可以使用 for in 語法，逐一取出物件的特性名稱。例如：
```js
> for(var prop in obj) {
...     console.log(prop);
...     console.log(typeof prop);
... }
x
string
y
string
undefined
> 'x' in obj;
true
>
```

###物件個性化
在 JavaScript 中，每個物件都可以是獨一無二，不一定是由其建構式來規範，這能力稱為物件個性化（Object individuation），你可以隨時為物件新增特性（Properties），也可以隨時用 delete 運算子來刪除特性。

有些內建特性無法被刪除，舉例來說，Array 實例有個 length 特性，你無法刪除它：
```js
> var arr = [];
undefined
> arr.length;
0
> delete arr.length;
false
>
```

###js物件的本質就像是Map物件

JavaScript的物件本質上，其實是個特性與值的群集（Collection），要比喻的話，有點像是 Java 中的 Map 物件。如果你要使用 for in 取得物件上的特性與值，則可以如下：
```js
> var obj = {
...     x : 10,
...     y : 20
... };
undefined
> for(var prop in obj) {
...     console.log(prop + ': ', obj[prop]);
... }
x:  10
y:  20
undefined
>
```

使用 [] 運算子的場合之一，就是當你的特性會包括空白、`.`字元、數字...等時。

###特性偵測
特性便是javascript物件中的key。我們要偵測物件是否存在某特性時可以用以下兩種方式：

1. 使用 in 測試物件上是否存在特性之外。
2. 由於物件上不存在某個特性時，你試圖存取時會傳回 undefined，而undefined若在判斷是否成立時會被當作 false，所以就有了特性偵測的作法：
```js
> var obj = {};
undefined
> obj.x ? 'has x' : 'has no x';
'has no x'
> obj.x = 10;
10
> obj.x ? 'has x' : 'has no x';
'has x'
>
```

###javascript是個弱型別語言

JavaScript是個弱型別語言，在需要將物件的型別(Type)轉為number的時候，會呼叫 `valueOf` 方法。例如：
```js
> var obj = {
...     valueOf : function() {
.....       return 100;
.....   }
... };
undefined
> 100 + obj;
200
> obj + 200;
300
> obj > 100;
false
> obj >= 100;
true
>
```
在需要將物件的Type轉換為字串的場合，則會呼叫`toString`方法。例如：
```js
> var caterpillar = {
...     name : 'Justin Lin',
...     url  : 'openhome.cc',
...     toString : function() {
.....         return '[name: ' + this.name + ', url: ' + this.url + ']';
.....     }
... };
undefined
> 'My info: ' + caterpillar;
'My info: [name: Justin Lin, url: openhome.cc]'
>
```

###比較"==="與"=="

`===` 用在物件比較時，是比較參考的對象是否為同一物件，而不是物件實際內含值（`==` 得考慮型態轉換後的結果)。

如果你要比較兩個物件實際上是否為同一物件，必須自行定義專屬方法。可以命名為`equals`。
```js
function equals(other) {
    return (this.name === other.name) &&  (this.url === other.url);
}

var man1 = {
    name : 'Justin Lin',
    url  : 'openhome.cc',
    equals : equals
};

var man2 = {
    name : 'Justin Lin',
    url  : 'openhome.cc',
    equals : equalsÂ
};

var man3 = {
    name : 'Justin Lin',
    url  : 'openhome.cc',
    equals : equals
};

console.log(man1.equals(man2));  // true
console.log(man1.equals(man3));  // true
```

如果了解prototype的概念，可以改寫成下面這樣。
```js
function Man(name, url) {
    this.name = name;
    this.url = url;
}

Man.prototype.equals = function(other) {
    return (this.name === other.name) &&  (this.url === other.url);
};

var man1 = new Man('Justin Lin', 'openhome.cc');
var man2 = new Man('Justin Lin', 'openhome.cc');
var man3 = new Man('Justin Lin', 'openhome.cc');

console.log(man1.equals(man2));
console.log(man1.equals(man3));
```
##參考資料
[JavaScript 語言核心（6）鍵值聚合體的物件 by caterpillar | CodeData](http://www.codedata.com.tw/javascript/essential-javascript-6-object/)