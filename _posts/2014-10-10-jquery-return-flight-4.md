---
layout: post
title: jQuery Return Flight Ch4：Utility Methods
published: true
date: 2014-10-10 05:16
tags:
  - jQuery
  - Javascript
comments: true

---
##each
用each把result的物件讀出來，插入頁面元素之中。
要插入`<p></p>`之中使用的是html()
要插入img的src欄位，使用find('img').attr('src', city.image)
之前也有使用過attr()這個方法來找到data欄位。
```js
success: function(result){
  $.each(result, function(index, city) {
    var favorite = $('.favorite-'+index);
    favorite.find('p').html(city.name);
    favorite.find('img')
            .attr('src', city.image);
  });
}
```

```js
$.each(collection, function(<index>, <object>) {});
```

##getJSON
用ajax設定```dataType:'json'```
```js
('.update-status').on('click', function() {
  $.ajax('/status', {
    contentType: 'application/json',
    dataType: 'json',
    success: function(result) { ... }
	});
});
```
等於使用getJSON
```js
$('.update-status').on('click', function() {
  $.getJSON('/status', function(result) {
    name: 'JFK - New York, NY',
    var statusElements = ???
    $('.status-list').html(statusElements)
  });
});
```

##map
```js
var myNumbers = [1,2,3,4];
var newNumbers = $.map(myNumbers, function(item, index){ return item + 1 });
  //myNumbers => [1,2,3,4]
  //newNumbers => [2,3,4,5]
```

###使用map插入元素
```js
$.map(result, function(status, i) {
	var listItem = $('<li></li>');
	$('<h3>' + status.name + '</h3>').appendTo(listItem);
  $('<p>' + status.status + '</p>').appendTo(listItem);
  return listItem;
});
```
完整版
```js
$('.update-flight-status').on('click', function() {
   $.getJSON('/status', function(result) {
     var statusElements = $.map(result, function(status, i) {
       var listItem = $('<li></li>');
       $('<h3>'+status.name+'</h3>').appendTo(listItem);
       $('<p>'+status.status+'</p>').appendTo(listItem);
       return listItem;
});
     $('.status-list').html(statusElements);
   });
});
```
可以看出map需要另外指定一個陣列(statusElemets)來儲存所有的li。一次把一整包li，塞進ul中。
而each是一個一個塞進去。相似的部份用map是語意滿清楚的作法。

還有一個最大的不同是each回傳的陣列不會改變，而map回傳的陣列是經過處理的。請看下圖：
![](https://lh5.googleusercontent.com/9HcvHuOpheqWWH6tJ8NpLniNpnlyllb1SpLk510t8cg=w1755-h1123-no)

##detach
```js
$('.update-flight-status').on('click', function() {
  $.getJSON('/status', function(result) {
    var statusElements = $.map(result, function(status, index) {
      var listItem = $('<li></li>');
      $('<h3>'+status.name+'</h3>').appendTo(listItem);
      $('<p>'+status.status+'</p>').appendTo(listItem);
      return listItem;
    });
    $('.status-list').detach()
  });                .html(statusElements)
                     .appendTo('.status');
});
```
這邊使用detach而不使用remove來刪除元素，因為detach其實不會把元素真正的刪除，而會保留下來。這樣做效率比較好。
[detach與remvove的區別](http://www.jquery001.com/jquery-detach-remove.html)