---
author: StevenTTuD
layout: post
title: Demo：實作捲動觸發CSS動畫
published: true
date: 2015-10-20 18:35
tags: ["HTML / CSS"]
comments: true

---

[demo](http://steventtud.github.io/CSS-Animation-Page-Demo/)

## 第一部分：使用CSS撰寫Slide In效果

這個單元使用的技巧有：

1. 使用`transition`讓屬性值改變時有動畫的效果
2. 使用`animation-fill-mode`設定結束狀態
3. 使用`transform:translate`移動元素

#### 1. 使用`transition`讓屬性值改變時有動畫的效果

transition 可以將CSS改變的過程變成動畫。詳細玩一下[CSS3 Transitions](http://www.w3schools.com/css/css3_transitions.asp)就懂了。

#### 2. 使用`animation-fill-mode`設定結束狀態
animation-fill-mode 結束後的狀態

- none：默认值。不设置对象动画之外的状态
- forwards：结束后保持动画结束时的状态，但当animation-direction为0，则动画不执行，持续保持动画开始时的状态
- backwards：结束后返回动画开始时的状态
- both：结束后可遵循forwards和backwards两个规则。

#### 3. 使用`transform:translate`移動元素

使用`transform: translate(x,y)`來移動元素。其他常用的還有：

- rotate(-20deg) 用來旋轉元素
- scale(x,y) 等比例放大或縮小元素

可以到W3C Scholl玩玩看
[CSS3 2D Transforms](http://www.w3schools.com/css/css3_2dtransforms.asp)

#### 程式碼(CSS部分)

```css
@-webkit-keyframes fadeIn { from { opacity:0; } to {opacity: 1;} }
@-moz-keyframes fadeIn { from { opacity:0; } to {opacity: 1;} }
@keyframes fadeIn { from { opacity:0; } to {opacity: 1;} }
```

```css
.animate{
	-moz-transition: 2s ease-in-out;
	-webkit-transition: 2s ease-in-out;
	transition: 2s ease-in-out;

	-moz-animation: fadeIn ease-in 1;
	-webkit-animation: fadeIn ease-in 1;
	animation: fadeIn ease-in 1;

	-webkit-animation-fill-mode: forwards;
	-webkit-animation-fill-mode: forwards;
	animation-fill-mode: forwards;

	-webkit-animation-duration: 1s;
	-moz-animation-duration: 1s;
	animation-duration: 1s;
}
```

```css
#phone.animate {
	-webkit-transform: translate(14em,0);
	-moz-transform: translate(14em,0);
	transform: translate(14em,0);
}

#monitor.animate {
	-webkit-transform: translate(3em,0);
	-moz-transform: translate(3em,0);
	transform: translate(3em,0);
}
```

#### 程式碼(JS部分)

用簡單的addClass方法即可完成。

```js
$(function () {
    $(window).scroll(function () {
        var y = $(this).scrollTop();
        if (y > 300) {
             $('#monitor').addClass('animate');
             $('#phone').addClass('animate');
        };

			.
			.
			.

    });
});
```

## 第二部分：使用CSS讓圖片永久旋轉

keyframe:用來編寫這個動畫的過程。設定好旋轉的動作命名為rotaRadial。

```css
 @-webkit-keyframes rotateRadial {
 	from {
 		-webkit-transform: rotate(0deg);
 	}
 	to {
 		-webkit-transform: rotate(360deg);
 	}
 }

 @keyframes rotateRadial {
 	from {
 		transform: rotate(0deg);
 	}
 	to {
 		transform: rotate(360deg);
 	}
 }
```

設定幾個參數來達到我們要的效果：

1. 動畫名稱指定剛剛創造的keyframe名稱`rotateRadial`。
1. 完成一次動畫的時間設為10s秒。
1. 永不停止。`animation-iteration-count: infinite;`
1. 將動畫轉變的加速曲線設為線性。`animation-timing-function: linear;`

```css
.always-rotate{
	-webkit-animation-name: rotateRadial;
	-webkit-animation-duration: 10s;
	-webkit-animation-iteration-count: infinite;
	-webkit-animation-timing-function: linear;

	animation-name: rotateRadial;
	animation-duration: 10s;
	animation-iteration-count: infinite;
	animation-timing-function: linear;
}
```

## 第三部分：讓圖片變大

原本圖片的大小，設成0。

```css
.anim-img{
	position: absolute;
	left: 0;
	opacity: 0;
}
```

後來圖片的大小

```css
img.grow-img{
	width: 200px;
	height: 200px;
}
```

用`.animate`設定了transition，因此會有動畫效果。

```js
$(function () {
    $(window).scroll(function () {
        var y = $(this).scrollTop();
        if (y > 300) {
            $('#monitor').addClass('animate');
            $('#phone').addClass('animate');
        };


        if(y > 400){
          $('#support').find('img').addClass('animate grow-img');
          $('#speed').find('img').addClass('animate grow-img');
          $('#smart').find('img').addClass('animate grow-img');
        };

    });
});
```

## 參考連結

大量範例展示CSS動畫可以做到的效果。

[Transitions & Animations - Learn to Code Advanced HTML & CSS](http://learn.shayhowe.com/advanced-html-css/transitions-animations/)

MDN 的 CSS 動畫說明頁，有不少的範例可以玩。

[CSS 動畫  MDN](https://developer.mozilla.org/zh-TW/docs/CSS_%E5%8B%95%E7%95%AB)

中文版的CSS字典

[CSS参考手册_web前端开发参考手册系列](http://css.doyoe.com/)
