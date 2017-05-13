---
layout: post
title: jQuery Return Flight Ch1 ( 1 )：Ajax Basics
published: true
date: 2014-08-25 01:26
tags:
  - Ajax
  - Javascript
  - jQuery
comments: true

---
這是之前在try jQuery做過的例子。
![](https://lh6.googleusercontent.com/-HjoupHhCEzM/U_qSn5aBcsI/AAAAAAAACxQ/tnBo1gSjLCM/w1755-h860-no/Screen%2BShot%2B2014-08-25%2Bat%2B09.33.46.png)
之前我們只能顯示localhost的圖片，現在我們要從server載圖片下來，並顯示之。
接下來來完成我們第一個Ajax程式。
```js
$('.confirmation').on('click', 'button', function(){
  $.ajax('http://example.org/confirmation.html',  {
     success: function(response) {
       $('.ticket').html(response).slideDown();
	   }
	});
});
```
使用Ajax回傳的網頁並不是完整的網頁，而是只有局部更新。
```html
<div> ...
  <strong>Boarding Pass: </strong>
  <a href='#' class='view-boarding-pass'>View Boarding Pass</a>
  <img src='ticket.png' alt='Your boarding pass' class='boarding-pass' />
</div>
```


##Aax使用的是相對網址(relative URL)
```js
$('.confirmation').on('click', 'button', function(){ $.ajax('confirmation.html', {
     success: function(response) {
    	 $('.ticket').html(response).slideDown();
     } });
});
```
跟http://example.org/confirmation.html一樣的效果。

##使用$.get減少程式碼
使用$.ajax
```js
$.ajax('confirmation.html', {
	 success: function(response) {
      $('.ticket').html(response).slideDown();
    }
});
```
使用$.get
```js
$.get('confirmation.html', function(response) {
   $('.ticket').html(response).slideDown();
});
```
兩種方法效果完全一樣，用get的寫法比較精簡。格式是```$.get(url, success callback)```

##Sending parameters with requests
有兩種寫法，作用是相等的。
第一種把網址填入url
```js
$.ajax('confirmation.html?confNum=1234', { success: function(response) {
     $('.ticket').html(response).slideDown();
   }
});
```
第二種將網址寫在data項目之下
```js
$.ajax('confirmation.html', {
   success: function(response) {
     $('.ticket').html(response).slideDown();
   },
   data: { "confNum": 1234 }
   }
});
```
這兩種方法都是寫死的，正常來說資料是會變動的，所以應該要用以下寫法。

```html
//html
<div class='ticket' data-confNum='1234'>
```

```js
//js
$.ajax('confirmation.html', {
     success: function(response) {
       $('.ticket').html(response).slideDown();
     },
     data: { "confNum": $(".ticket").data("confNum")
     }
});
```

###1.6 Ajax Data
原本會顯示全部的圖片，透過修改data對應的url，變成只顯示london這張圖片
我的答案是這樣
```js
$(document).ready(function() {
  $("#tour").on("click", "button", function() {
    $.ajax('/photos.html', {
      data: {location:'london'},
      success: function(response) {
        $('.photos').html(response).fadeIn();
      }

    });
  });
});
```
這時候出現了以下的錯誤
![](https://lh6.googleusercontent.com/zmzYKG0zPFrQtQuetAmUASzkGppMbCiTAMpblaBAhcY=w1748-h223-no)
因為我把值寫死了，使用data來對應是比較好的作法。
```js
$(document).ready(function() {
  $("#tour").on("click", "button", function() {
    var el = $("#tour");
    $.ajax('/photos.html', {
      data: {location: el.data('location')},
      success: function(response) {
        $('.photos').html(response).fadeIn();
      }
    });
  });
});
```

#Ajax options
##使用error處理失敗的request
![](https://lh5.googleusercontent.com/-Ho8DXLjqFms/U_qfnpPOAOI/AAAAAAAACyU/0Bm0X2lg8zI/w1755-h885-no/Screen%2BShot%2B2014-08-25%2Bat%2B10.24.28.png)
##使用beforeSend和Complete製作Loading效果
![](https://lh6.googleusercontent.com/-CiUYsr6XJpg/U_qfny5n5vI/AAAAAAAACyk/pxPMu-k_URk/w1755-h925-no/Screen%2BShot%2B2014-08-25%2Bat%2B10.23.49.png)

##使用delegation解決錯誤
因為DOM在一進入頁面的時候就載入，所以如果用第一個寫法會發生錯誤，因為根本就還沒有這個物件。
![](https://lh4.googleusercontent.com/-F0Ta3-SweHU/U_qfowcrybI/AAAAAAAACys/nwHPzkl773c/w1755-h875-no/Screen%2BShot%2B2014-08-25%2Bat%2B10.28.02.png)
這時候需要delegate，在觸發事件的後面跟著設定一個selector，符合這個selector才會執行。
![](https://lh4.googleusercontent.com/-U03mkyStXcs/U_qfpEdT5FI/AAAAAAAACyo/pth2HCRfKFM/w1753-h423-no/Screen%2BShot%2B2014-08-25%2Bat%2B10.28.33.png)
[delegate官方文件](http://api.jquery.com/delegate/)

##1.8 在.photos內顯示新的```<li>```
```html
<div id="tour" data-location="london">
  <button>Get Photos</button>
  <ul class="photos">

  </ul>
</div>
```

```js
 $('.photos').html('<li>There was a problem fetching the latest photos. Please try again.</li>');
```
