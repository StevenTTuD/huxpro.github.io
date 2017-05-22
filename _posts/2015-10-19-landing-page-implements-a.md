---
author: StevenTTuD
layout: post
title: Demo：使用scrollspy讓畫面更生動
published: true
date: 2015-10-19 20:16
tags:
  - "HTML / CSS"
comments: true

---
[Demo展示頁](http://steventtud.github.io/Scollspy-Landing-Page/#who)

## 作法說明

### 1.幫body加工

將body加上`display: relative`，如果還是不行使用的話加上`width: 100%`與`height: 100%`。

```css
display: relative;
width: 100%;
height: 100%;
```

### 2. 宣告navbar並設定target
使用js宣告target，這次我使用的target是整個navbar，用`.navbar-custom`來指定之，有需要的話可以調整offset修正navbar的觸發情形。

```js
	$(document).ready(function(){
    $("body").scrollspy({
        target: ".navbar-custom",
        offset: 370
    })
	});
```

```html
nav class="navbar navbar-default navbar-custom navbar-fixed-top" role="navigation">
  <div class="container">
    .
    .
    .

  </div><!-- /.container -->
</nav>
```
### 3. 設定個區塊的id，並關聯到導覽列的連結


每個區塊設定id，例如：

```html
<section class="client t-padding" id="clients">
.
.
.
</section>
```

幫導覽列的連結加上`href="#id"`

```html
    <div id="navbar"class="collapse navbar-collapse navbar-right navbar-main-collapse">
      <ul class="nav navbar-nav ">
        <li class="page-scroll"><a href="#why">why us?</a></li>
        <li class="page-scroll"><a href="#who">who are we?</a></li>
        <li class="page-scroll"><a href="#clients">clients</a></li>
        <li class="page-scroll"><a href="#contact">contact</a></li>
      </ul>
    </div><!-- /.navbar-collapse -->
```


### Bonus. 自行撰寫點擊導覽列連結就可以緩緩移動的JS

將li加上`.page-scroll`的屬性，以便之後選取。

```html
    <div id="navbar"class="collapse navbar-collapse navbar-right navbar-main-collapse">
      <ul class="nav navbar-nav ">
        <li class="page-scroll"><a href="#why">why us?</a></li>
        <li class="page-scroll"><a href="#who">who are we?</a></li>
        <li class="page-scroll"><a href="#clients">clients</a></li>
        <li class="page-scroll"><a href="#contact">contact</a></li>
      </ul>
    </div><!-- /.navbar-collapse -->

```

當點擊到連結時，畫面會垂直位移到相對硬的區塊。

```js
	$('.page-scroll > a').click(function(){
		var $anchor = $(this);
		$('html, body').stop().animate({
			scrollTop: $($anchor.attr('href')).offset().top-100
		}, 1500, 'easeInOutExpo');
		event.preventDefault();
	});
```

加上CSS的轉場效果，讓效果更漂亮。

```css
.page-scroll{
   -moz-transition-property: -webkit-transform ;
   -webkit-transition-property: -webkit-transform;
   transition-property: -webkit-transform;
   transition-duration: 1s;
   -moz-transition-duration: 1s;
 	-webkit-transition-duration: 1s;
 }
```


### 參考連結
[JavaScript · Bootstrap](http://getbootstrap.com/javascript/#scrollspy)

[How to Create Bootstrap 3 Scrollspy - Tutorial Republic](http://www.tutorialrepublic.com/twitter-bootstrap-tutorial/bootstrap-scrollspy.php)


