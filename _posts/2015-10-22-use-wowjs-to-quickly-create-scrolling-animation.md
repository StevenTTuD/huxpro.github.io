---
layout: post
title: Demo：使用wow.js快速打造捲動式動畫網頁
published: true
date: 2015-10-22 08:45
tags: []
categories: []
comments: true

---
我用兩個不同方法個別製作出網站來讓大家比較看看：

[純CSS打造捲動式動畫網站](http://steventtud.github.io/CSS-Animation-Page-Demo/)
[wow.js打造的捲動式動畫網站](http://steventtud.github.io/wowjs-scroll-based-animation-/)

接著來介紹這wowjs的製作方法與其優缺點。

##使用方法：

第一步要做的是animate.css與wow.js載進網頁中。接著幫要使用動畫的部份加上`.wow .animate動畫名稱`屬性。wow.js用來負責偵測捲動到的位置，animate.css用來決定呈現的動畫。

HTML

```html
  <section class="wow slideInLeft"></section>
  <section class="wow slideInRight"></section>
```

JavaScript

```js
new WOW().init();
```

就是這麼簡單。還有其他客製化的設定，不過彈性也不大，使用最基本的功能cp值最高。

##跟手工打造的CSS動畫的不同
- wow.js + animate.css易於撰寫但彈性不大。
- wow.js + animate.css 製作的動畫位置是預先設定好的。不能移動也不能重疊。
- 使用純CSS需要撰寫兩份CSS，一份是改變前的狀態，一份是改變後的狀態。而使用 wow.js + animate.css 只需要撰寫結束後的狀態，只要在div加上`.wow`與`.slideInLeft`等等的動畫名稱即可完成。撰寫的CSS少了許多。

##結論
wow.js + animate.css 能夠打造的捲動式動畫角色很鮮明。但是彈性不大。如果要更加生動的畫面還是得自己設計CSS。

##參考資料

[Animate.css](https://daneden.github.io/animate.css/)
[matthieua/WOW](https://github.com/matthieua/WOW)



