---
layout: post
title: Demo：使用Matirialize框架實現視差滾動
published: true
date: 2015-10-12 18:47
tags:
    - HTML / CSS
comments: true

---
[完成品展示頁](http://steventtud.github.io/matirialize_parral_design/)

Materialize是除了Bootstrap以外現今相當熱門的前端框架，可以製作的網頁類型相當廣泛，未來將使用Materialize來開發一些小作品，是今天練習的主要目的。

##實現Materialize框架提供的視差滾動效果

視差滾動的效果由materialize提供的[Parallax JS 套件](http://materializecss.com/parallax.html)來完成，效果可以讓圖片滿板，並且有視差效果。步驟如下：

1. 自行撰寫`.parallax-container`包覆需要跟圖片有視差效果的區域。
1. 第二層的最下方加上`.parallax`並包覆圖片即可完成視差效果。

```html
<section class="slogan-section  margin-bottom parallax-container valign-wrapper ">
    <div class="container">
        <h3 class="center-align slogan-description">A modern responsive front-end framework based on Material Design</h3>
    </div><!-- container -->
    <div class="parallax"><img src="images/background2.jpg" alt="Unsplashed background img 2"></div>
</section><!-- slogan -->
```

```css
 .parallax-container {
     min-height: 380px;
     line-height: 0;
     height: auto;
     color: rgba(255,255,255,.9);
     position: relative;
     overflow: hidden;
 }
```

###其他學習記錄

1. 一般區塊的垂直置中使用materialize的valign-wrapper來達到垂直置中的效果。

1. Footer使用width+line-height的垂直置中技巧。垂直置中的對象必須是外層的`footer.copyright`，包在container裡面會造成多餘的空白。

1. 修飾某些元件時除了class名稱以外，需加上元件名稱，來解決沒有蓋過原本framework的CSS的問題。例如：使用`.footer-copyright`無法順利設定背景顏色，但若改成`footer.footer-copryright`則可以順利完成。

1. 如果有以下的突出狀況，可以加上`overflow: hidden;`即可解決。
![](https://lh3.googleusercontent.com/Ng8tF0ab5j9ciT522tYNr1hKUWkKu4N43ZZUImQx3_SUUHjK3XguW1xfHHMebWExf6TG-acW7EunFVLkqJ0zfCD9Hng-99xCbVQrEfTEGE1O00vi6qVGHJvtH5LZSgPUx1vyrsfPAWs-WoSnjgmN2m0L_HCbNFDhJ5FV2qvyhPhO8u4HJcEiycl6MP4Vz-p7j284-oOCCY3nAsFAdbN6486y3lrDXVuw81qGpLxiW44IQjB1oSXPLj1KNbowTgLK3Cpj_KcsEtrNmrDDOpWpNpSdsJn4vGKMKl9ITgrhVofbNM2T0wMW3kCTM5NIVs2oqdE0Mi3LCIKFYJAH0q4jXvouJFywxpRkpi1svOlciNwbFHKInPwKDsojqkkEzsORXKayKj95i3jf_8puXH_RPuJzc8lBpXZT5l1Eyb221f3istv0nB3CUt4eedfBDsYh0O-doVyhVb8C2B2wByYDPaYqjEIt8wY7L1HsyB9MgKid8kSouDKZNJK4rBXrLFcVaIRBksKvOybB3v-TnSedyXkIGucdDNIn7tIDBzEDH40=w1084-h468-no)

