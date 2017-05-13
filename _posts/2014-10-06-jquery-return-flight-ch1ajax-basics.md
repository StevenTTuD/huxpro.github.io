---
layout: post
title: jQuery Return Flight Ch1 ( 2 )：補充筆記
published: true
date: 2014-10-06 03:06
tags:
  - jQuery
  - javascript
comments: true

---
##使用.html()插入元素
要使用的ajax長這樣，點下#tour之下的button後，如果出現錯誤，就會顯示錯誤訊息。
```js
$(document).ready(function() {
  var el = $("#tour");
  el.on("click", "button", function() {
    $.ajax('/photos.html', {
      data: {location: el.data('location')},
      success: function(response) {
        $('.photos').html(response).fadeIn();
      },
      error: function() {
        $('.photos').html('<li>There was a problem fetching the latest photos. Please try again.</li>');
      }
    });
  });
});
```
在錯誤發生的時候，在```.photo```底下插入```<li>```元素
```js
      error: function() {
        $('.photos').html('<li>There was a problem fetching the latest photos. Please try again.</li>');
```
ajax回傳之後的html
```html
<li>
  <img src="/assets/photos/paris1.jpg">
  <span style="display: none;">Arc de Triomphe</span>
</li>
<li>
  <img src="/assets/photos/paris2.jpg">
  <span style="display: none;">The Eiffel Tower</span>
</li>
<li>
  <img src="/assets/photos/london.jpg">
  <span style="display: none;">London</span>
</li>
```

#event delegate
錯誤寫法
```js
$('.confirmation .view-boarding-pass').on('click', function(){ ... });
```
正確寫法
```js
$('.confirmation').on('click', '.view-boarding-pass', function(){ ... })
```
因為當頁面載入時，```.view-boarding-pass```並不存在，所以必須使用把要選擇的元素寫在event handler裡面又稱為**event delegate**的作法。

#做出Loading特效
這是一個使用event delegate概念完成的特效。
```js
$('.photos').on('mouseenter', 'li', showPhotos)
            .on('mouseleave', 'li', showPhotos);
```
完整js檔如下
```js
$(document).ready(function() {
  function showPhotos() {
    $(this).find('span').slideToggle();
  }
  $('.photos').on('mouseenter', 'li', showPhotos)
              .on('mouseleave', 'li', showPhotos);

  var el = $("#tour");
  el.on("click", "button", function() {
    $.ajax('/photos.html', {
      data: {location: el.data('location')},
      success: function(response) {
        $('.photos').html(response).fadeIn();
      },
      error: function() {
        $('.photos').html('<li>There was a problem fetching the latest photos. Please try again.</li>');
      },
      timeout: 3000,
      beforeSend: function() {
        $('#tour').addClass('is-fetching');
      },
      complete: function() {
        $('#tour').removeClass('is-fetching');
      }
    });
  });
});
```