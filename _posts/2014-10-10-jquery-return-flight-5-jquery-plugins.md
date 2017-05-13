---
layout: post
title: jQuery Return Flight Ch5 ( 1 )：Advanced Event
published: true
date: 2014-10-10 07:27
tags:
  - jQuery
  - Javascript
comments: true

---
#advanced event handler
##trigger
使用trigger就像是你按下那個按鈕。範例:
```js
function picture() { console.log('Show Plane'); }
function status() { console.log('In Service'); }
function zoom() { console.log('Zoom Picture'); }

$(document).ready(function() {
    $('button').on('click.image', picture);
    $('button').on('click.details', status);
    $('button').on('mouseover.image', zoom);
    ...
    $('button').trigger('click');
});
```
使用trigger觸發事件(等於使用者按下按鈕)
```js
$('button').trigger('click');
```
會觸發這兩個
```js
$('button').on('click.image', picture);
$('button').on('click.details', status);
```

##自訂`<event>`同時觸發多個事件
照理說evnt的格式是：
```js
$(<dom element>).on("<event>.<namespace>", <method>)
```
但其實jquery可以自訂event，讓我們做更多方便的操作。現在我們來自訂一個event。
```js
$('.vacation').on('show.price', showPrice);
```
可以看到本來應該是'click'的部份，已經用`show.price`這個我們新創的event所代替。
這樣我們就可以透過這個event來trigger所有的vacation。
```js
$('.vacation').trigger('show.price');
```
或是透過這個event來trigger最後一個vacation。
```js
$('.vacation:last').trigger('show.price')
```
完整範例：
```js
var showPrice = function() {...};
$('.vacation').on('click.price', 'button', showPrice)
$('.vacation').on('show.price', showPrice);
$('show-prices').on('click', function(event) {
  event.preventDefault();
  $('.vacation').trigger('show.price');
};
```
下面這個例子是，要在點擊show photo按鈕時，也會顯示天氣。這
```js
$(document).ready(function(){
  // Get Weather
  $('button').on('show.weather', function() {
    var results = $(this).closest('li').find('.results');
    results.append('<p>Weather: 74&deg;</p>');
    $(this).off('show.weather');
  });

  // Show Photos
  $('button').on('click.photos', function() {
    var tour = $(this).closest('li');
    var results = tour.find('.results');
    results.append('<p><img src="/assets/photos/'+tour.data('loc')+'.jpg" /></p>');
    $(this).off('click.photos');
    $(this).trigger('show.weather');
  });
});
```
這樣就不會像原來一個button綁兩個click listener，而是綁定一個click listener依序執行兩個函數。
```js
$(document).ready(function(){
  // Get Weather
  $('button').on('click.weather', function() {
    var results = $(this).closest('li').find('.results');
    results.append('<p>Weather: 74&deg;</p>');
    $(this).off('click.weather');
  });

  // Show Photos
  $('button').on('click.photos', function() {
    var tour = $(this).closest('li');
    var results = tour.find('.results');
    results.append('<p><img src="/assets/photos/'+tour.data('loc')+'.jpg" /></p>');
    $(this).off('click.photos');
    $(this).trigger('click.weather');

  });
});
```

##off
將符合event.namespace的handler移除
![](https://lh4.googleusercontent.com/-Pponyl2bHfo/VDeZTWsGPlI/AAAAAAAADHc/8_d1EMnLjzc/w1650-h530-no/Screen%2BShot%2B2014-10-10%2Bat%2B15.31.35.png)
刪除符合event的handler
![](https://lh4.googleusercontent.com/-OjeqhJM-sAQ/VDeZTcVpIZI/AAAAAAAADHk/RcghF-py-X4/w1650-h685-no/Screen%2BShot%2B2014-10-10%2Bat%2B15.31.45.png)
也可以指定namespace，符合namespace的handler都移除
![](https://lh6.googleusercontent.com/-mA0cKkXEn74/VDeZTYFMVgI/AAAAAAAADHg/ffIlXNo3iuU/w1650-h688-no/Screen%2BShot%2B2014-10-10%2Bat%2B15.32.45.png)

