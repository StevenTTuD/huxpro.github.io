---
layout: post
title: "排版練習2：PSD to html with Bootstrap"
published: true
date: 2015-02-12 12:33
tags:
  - HTML
  - CSS
  - PSD to HTML
comments: true

---
第二個練習一樣學習tuts的課程，選擇課程的好處是做到一半不知道該怎麼做時，有video解答可以看。看看高手怎麼做總是比自己亂弄的好的多。我選擇的課程是需要付費的，tuts還有一些免費的教材可以學習，影片或是文章教學都有，有興趣的自己發掘囉。第二個練習跟第一個練習不同之處在於本練習會使用bootstrap的grid system來排版，並且icon與logo會使用sprite css的方式來製作。

##Sprite sheets
sprite 說的是利用將 icon 集中在一張圖片上，藉由CSS設定不同顯示區域，來實現每一個不同的icon。這樣的好處是很可降低圖片request數，減少server負擔。現在我們要使用sprite的方法實作的是右上角的五個social-icon和左上角的mumbo logo。

##Create Sprite Sheets
1. 首先在photoshop中用方框工具圈選5個icon
1. 點選`file > copy merge`複製五個icon
1. `file > new` 建立新檔案，圖像大小設定為250 X 250。
1. 開啟ruler(view > ruler)
1. 從左側ruler拉進隔線每50px拉一條，這是為了等等區隔複製過來的icon，每個間隔會是50px。
1. 使用方框工具(第二個工具)選取後，用移動工具(第一個工具)將icon對齊左上角(如果右下角的layer選錯的話，方框工具會選不到圖案）。
1. 接著將mumbo logo也複製進來，放在第二排，對齊左上角，製作好的photoshop檔案會是長得像這樣。
註:
![](https://lh5.googleusercontent.com/-kZxU_a73YTk/VNyjQ0_8wfI/AAAAAAAAE6g/LXCLe6pwyL4/w1410-h628-no/sprite.jpg)
1. 將photoshop檔案 save for web，存成檔名為sprite.png的檔案。放在imgs資料夾下。
1. 製作好圖片之後來實際做做看social-list，關鍵是將background設成sprite.png。再利用個別的class鎖定不同的位置，這樣就會顯示不同的icon。
```css
.social-list li{
  display: inline-block;
  margin: 0 2px;
}
.social-list li a {
  display: inline-block;
  width: 37px;
  height: 37px;
  background: url(../imgs/sprite.png) no-repeat 0 0;
}
.social-list li a.vimeo {
  background-position: -50px 0;
}
.social-list li a.lastfm {
  background-position: -100px 0;
}
.social-list li a.linkedin {
  background-position: -152px 0;
}
.social-list li a.dribble {
  background-position: -200px 0;
}
```
1. 這樣就完成social-icon了。
![](https://lh5.googleusercontent.com/-uBsU7-pyW8U/VNy7yRTObPI/AAAAAAAAE7A/DMfdyVFwG1w/w734-h198-no/02mumbo.jpg)
1. 接著要把左上方的logo用sprite的手法改寫。
```css
header hgroup h1 a{
  display: block;
  width: 172px;
  height: 25px;
  background: url(../imgs/sprite.png) no-repeat 0 -50px;
}
```
1. 如此一來便大功告成。使用photoshop手動製作sprite是比較沒效率的作法，現在已經有很多方便的工具可以使用，藉由這個練習了解sprite的原理，之後再學習快速開發的工具便容易許多。
![](https://lh4.googleusercontent.com/-4Np5E53Y3Bs/VNy7xCZCx8I/AAAAAAAAE60/EcqUdwQ5P30/w992-h219-no/01mumbo.jpg)

##學習筆記
1. 先來進行header的h1與h2排版，要達成的效果如下圖。還在苦惱怎麼使用純CSS設定長寬和box-model讓他服服貼貼的時候，突然發現有`.pull-left`可以用。真是太方便了。
![](https://lh4.googleusercontent.com/-4Np5E53Y3Bs/VNy7xCZCx8I/AAAAAAAAE60/EcqUdwQ5P30/w992-h219-no/01mumbo.jpg)

1. 使用`font-style: italic;`就可以讓標題有傾斜的效果。
![](https://lh3.googleusercontent.com/-SJrEQRDSV8k/VN2vuvZQLeI/AAAAAAAAE7U/k4DQG9hf34U/w738-h118-no/titile.jpg)

1. 如何讓li前面的dot變色？對li加上`color: #ff6b39;`就可以將dot一同變色。
![](https://lh6.googleusercontent.com/ORaHhrfnK-RVQ7sUVN_6HgtGUMoww-gRJ5a8hFXqgPY=w733-h138-no)