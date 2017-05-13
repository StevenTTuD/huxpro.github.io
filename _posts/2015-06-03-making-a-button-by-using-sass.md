---
author: StevenTTuD
layout: post
title: "使用SMACSS製作button"
published: true
date: 2015-06-03 03:55
tags: ["HTML / CSS", "SCSS"]
comments: true

---

## module抽離出常用元件，負責大致的外觀

SMACSS是Jonathan Snook所發表的一個CSS/SASS的設計模式。在製作button時我們會把它放在`module`資料夾底下，並存成檔名為_btn.scss。在modules資料夾中的_btn.scss裡面撰寫Button的外觀，需要注意的是**不包含顏色和其他特效**。這樣是為了將顏色和細部調整放到之後的_theme.scss檔案中做調整。為什麼要這樣做呢？這樣做可以在有多個佈景主題的時候很方便的切換。也比較知道現在在這個檔案我要做哪些事情。不會一堆css碼做了什麼事情要一一去解讀。

`modules/_btn.scss`負責大致上的外觀

```scss
.btn {
	display: inline-block;
	height: 3rem;
	padding: 0 1rem;
	outline: none;

	border: 1px solid;

	font-size: 1.125rem;

	@include border-radius(3px);
}
```

完成圖片

![](https://lh3.googleusercontent.com/nlDElyGFFKjzH0IcnR_6NMW1idUKClgZ_mHMUm0nd6s=w177-h49-no)

## theme負責顏色、陰影...等佈景主題需要的修飾

在`theme/_theme.scss`中將按鈕的顏色、陰影效果、hover效果補上。因為主題可能有多個，所以我們抽離出來撰寫。以後想要加上別的主題的時候CSS碼就步會搞的很混亂。

```scss
/* Buttons */
.btn {
	background-color: $color-1;

	color: $color-text;

	border-color: darken($color-1, 20%);

	@include box-shadow(0 -2px 0 0 darken($color-1, 20%) inset);

	&:hover {
		background-color: darken($color-1, 5%);
	}
}

.btn-primary {
	background-color: $color-link;

	color: #fff;

	border-color: darken($color-link, 20%);

	@include box-shadow(0 -2px 0 0 darken($color-link, 20%) inset);

	&:hover {
		background-color: darken($color-link, 5%);
	}
}

.btn-secondary {
	background-color: $color-2;

	color: #fff;

	border-color: darken($color-2, 20%);

	@include box-shadow(0 -2px 0 0 darken($color-2, 20%) inset);

	&:hover {
		background-color: darken($color-2, 5%);
	}
}
```

使用的技巧有

1. 利用`box-shadow`來製造陰影效果
2. box-shadow的顏色使用compass的darken函式來完成。使用方法是`darken($color-2, 5%)`，第一個參數是要跟黑色混和的顏色，第二個參數是要混和的比例。

完成畫面

![](https://lh3.googleusercontent.com/tGXeVpBNfHcKUYZ3KlSDG0YHtsZu9ApomgC3VnH6mBc=w210-h62-no)
