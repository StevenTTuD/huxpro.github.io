---
author: StevenTTuD
layout: post
title: Landing Page 實作：來做個簡單的Jumbotron
published: true
date: 2015-10-19 19:53
tags:
  - HTML / CSS
comments: true

---

製作jumbotron有兩個重點

1. 垂直置中
2. 設定背景。

## Background

- 設定`position: center bottom`讓圖片顯示出需要的區塊
- 使用`background-size: cover;`讓圖片滿版

```css
section.intro{
  padding: $baseline * 2;
  background-image: url('../images/intro-bg.jpg') ;
  background-position: center bottom;
  height: 100%;
  width: 100%;
  -moz-background-size :cover ;
  -webkit-background-size :cover ;
  background-size: cover;
  background-attachment: scroll;
}
```

### 複習背景的用法

背景的屬性：
- background-repeat，常用的是`no-repeat`
- background-attachment，有fixed和scroll兩種，fixed圖片不會動，只有轉的空間會動。
- background-position

left 為水平靠左， right 為水平靠右， top 為垂直置頂， bottom 為垂直置底center 則是水平或垂直都置中。

前後需設定兩組，若是數字或百分比前為水平後為垂直，若是關鍵字則順序無關，例如
```css
background-position: 25% 50%;      /* 水平 25% ，垂直 50% */
background-position: 50px 25px;    /* 水平 50px ，垂直 25px */
background-position: center;       /* 水平垂直都 50% */
background-position: bottom right; /* 水平 100% ，垂直 100% */
```


## 使用Table-cell水平置中

1. 外層display設成table
1. 內層display設成table-cell，並加上`vertical-align: middel`即可

這種方法的優點是用起來很簡單，缺點是會增加html複雜度。

```html
<section class="intro">
  <div class="intro-body">
    <div class="container t-padding">
      <div class="row text-center">
        <div class="col-md-8 col-md-offset-2">
          <h1>welcome</h1>
          <h3><strong>Digital Agency</strong> is here all of your technology, advertising, SEO and marketing needs.</h3>
        </div><!-- col-md-8 offset-2 -->
      </div><!-- row -->
    </div><!-- container -->
  </div><!-- intro-body -->
</section><!-- intro -->
```

```css
.intro{
  display: table;
}

.intro-body{
  display: table-cell;
  vertical-align: middle;
}
```

## 置中小整理

1. table-cell可以用在jumbotron這種結構較單純的區塊上，因為使用這種方式會增加html的複雜度。
1. 單行多個置中可以使用偽元素的技巧
1. css3的技巧雖然很方便，但支援度可以能沒那麼高。
1. 行高的技巧簡單易用，但只能用在文字上。
