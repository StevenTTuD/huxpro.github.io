---
author: StevenTTuD
layout: post
title: Javascript核心筆記：callback
published: true
date: 2015-02-27 04:25
tags:
  - javascript
comments: true

---
##callback是一種設計模式

來看看callback的定義：

> [Callback (computer programming) - Wikipedia](http://en.wikipedia.org/wiki/Callback_(computer_programming))
In computer programming, a callback is a reference to a piece of executable code that is passed as an argument to other code.

> [jQuery Document: How jQuery Works#Callback_and_Functio...](http://docs.jquery.com/How_jQuery_Works#Callback_and_Functions)
A callback is a function that is passed as an argument to another function and is executed after its parent function has completed. The special thing about a callback is that functions that appear after the "parent" can execute before the callback executes. Another important thing to know is how to properly pass the callback. This is where I have often forgotten the proper syntax.

所以本質上callback是一種設計模式。當一段程式碼可以當做參數丟給其他程式碼執行就叫做callback。jQuery和許多其他的框架的設計原則都遵循這個模式。

##callback不限於非同步使用
同步時使用callback通常是希望程式能夠按照順序執行。通常callback在同步的情況下是最後執行的。
```js
var func1=function(callback){
    //do something.

    //如果callback存在就執行它
    callback && callback();
}

func1(func2);
    var func2=function(){
}
```
非同步的例子：
```js
$(document).ready(callback);
$.ajax({
  url: "test.html",
  context: document.body
}).done(function() {
  $(this).addClass("done");
}).fail(function() {
  alert("error");
}).always(function() {
  alert("complete");
});
```

##callback function的撰寫方式
####1.沒有其他引數時
```js
function basic( callback ){
  console.log( '作些事情' );
  var result = '我是等會要被傳送給 `do something` 的 callback 的函式結果';
  // 如果 callback 存在的話就執行他
  callback && callback( result );
}

basic( function( result ){
  console.log( '這個 callback 函式會在 terminal 上列出 `basic` 函式執行的結果' );
  console.log( result );
});

```
結果
```
作些事情
這個 callback 函式會在 terminal 上列出 `basic` 函式執行的結果
我是等會要被傳送給 `do something` 的 callback 的函式結果
```

####2.有其他引數時可以用call或apply來實作。用call來實作看看。
```js
function callbacks_with_call( arg1, arg2, callback ){
  console.log( '作些事情' );

  var result1 = arg1.replace( 'argument', 'result' ),
      result2 = arg2.replace( 'argument', 'result' );

  this.data = '這等會可以讓 callback 函式用 `this` 來調用';

  // 如果 callback 存在的話就執行他
  callback &amp;&amp; callback.call( this, result1, result2 );
}

( function(){
  var arg1 = '我是 argument1',
      arg2 = '我是 argument2';

  callbacks_with_call( arg1, arg2, function( result1, result2 ){
    console.log( '這一個 callback 函式將會列出 `callbacks_with_call` 的執行結果' );
    console.log( 'result1: ' + result1 );
    console.log( 'result2: ' + result2 );
    console.log( 'data from `callbacks_with_call`: ' + this.data );
  });
})();
```
執行結果
```
作些事情
這一個 callback 函式將會列出 `callbacks_with_call` 的執行結果
result1: i am result1
result2: i am result2
data from `callbacks_with_call`: 這等會可以讓 callback 函式用 `this` 來調用
```

##參考資料
[Javascript callbacks | DreamersLab](http://dreamerslab.com/blog/tw/javascript-callbacks/)
[JavaScript 回调函数怎么理解](http://segmentfault.com/q/1010000000140970)
