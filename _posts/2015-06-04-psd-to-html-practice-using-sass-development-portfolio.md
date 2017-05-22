---
author: StevenTTuD
layout: post
title: "練習：使用SASS開發Portfolio (1) Header 與 Footer"
published: true
date: 2015-06-04 16:56
tags:
  - "HTML / CSS"
  - SCSS
comments: true

---
## 標題列(Header)
### 固定的標題

![](https://lh3.googleusercontent.com/dVVKl8lHg0M4TExKiNyp_TeO6saBUx15ljAfVI-0ipQ=w903-h346-no)

往下捲動時仍然會固定在上方

![](https://lh3.googleusercontent.com/y0v7AtVVUbhYHDjl6v5qT1Rd_aGmdSbRHL8F8uSdz4Y=w907-h429-no)

幫header加上`position: fixed`可以做到這個效果。透明的效果則是使用`background: rgba(0, 0, 0, 0.8)`。

### 標題列的水平置中
這邊使用的水平置中技巧是使用[CSS 垂直置中的3種方法](http://www.oxxostudio.tw/articles/201408/css-vertical-align.html)中的**設定行高(line-height)**的方法。

而行高我們可以在_typography.scss中統一設置，typography.scss顧名思義就是用來處理一些排版的問題，常見的設定有body、anchor、paragraph和header等等常用到的東西。如果沒有使用scss的習慣，可以利用註解區隔出一個專門處理排版的區域。

```scss
body {
	color: $color-text;
	font: $body-font-size~"/"$baseline $body-font;

	-webkit-font-smoothing: antialiased;
}
```

font 的使用方法可以看看CSS官方文件

```css
/* Set the font size to 12px and the line height to 14px. Set the font family to sans-serif */
p { font: 12px/14px sans-serif }
```
第一個欄位為font-size，而第二個則為行高。但其實我們不是完全的利用行高來達成，還有設定header的padding來幫助我們實現置中。因為要置中的不全部是文字，還有其他的元素例如ul需要考慮。

## Footer
### 重疊效果製作
我們要把信封圖案置中並且讓信封圖案在背景的中間。還未做任何修飾的時候如下：
![](https://lh3.googleusercontent.com/gSZf80g6aWX0x59JGNa5lSmM1HusPvz0rjQdHv25xrU=w545-h215-no)
要怎麼做重疊的效果呢？很簡單，把下方的`.section-subscribe`加上負的margin即可。
```rb
.section-subscribe{
	margin-top: -18px;
}
```

加上後的效果如下：

![](https://lh3.googleusercontent.com/OixTFiR5im7GI263YaQZAZFRXbXNgAd029pdnWuGD80=w748-h212-no)

### 讓段落置中
我想讓段落分行並置中，所以我要做兩件事情：第一件事情是我要設定段落的寬度為`65%`第二件事情我要利用margin來讓段落置中。想要做到的效果：

![](https://lh3.googleusercontent.com/nVx8qy43AgvV3FpPZr129nM97iEo8G2HUc31z_Ubh-A=w1013-h73-no)

```scss
.section-subscribe{
  p{
    margin: 0 auto;
    color: #fff;
    width: 65%;
  }
}
```
需要注意是`display`為`block`才能使用`margin:auto`的方式置中。
