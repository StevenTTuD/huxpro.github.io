---
layout: post
title: "排版練習1：PSD to HTML Porfolio"
published: true
date: 2015-02-12 04:51
tags:
  - HTML
  - CSS
  - PSD to HTML
comments: true

---
最近在訓練排版的熟練度，要快速打造protype基本的前端技能是必須的。我選擇的教材是tuts的psd to html porfolio。課程中介紹了詳細的切版流程，從切圖到建構html到用css排版都有詳細的介紹。本篇是psd to html也就是排版練習第一篇，練習的是使用純css來刻網站。接下來會練習更多題目，直到我掌握基本的排版技巧為止。

##photoshop基本操作
psd to html 顧名思義就是把設計師做好的 photoshop 檔案，用 html 和 css 刻成網頁。這個練習很棒的地方就是模擬了一個設計師跟你合作。所以首先你的電腦裡需要安裝photoshop。我使用的是photoshop cc 2014版本。
1. 測量元素距離
1. 查看字型、字體大小和行高
1. 查看顏色

##chrome基本操作
1. 使用page ruler測量網頁元素距離，可以看看
1. 在dev tool修改css屬性即時更新畫面效果。
1. 在dev tool中觀看html結構（雖然是自己刻的但是還是會忘記呀-.-）。
1. 使用 livereload 套件即時更新修改結果。

##開始實作
1. 首先是header的部份，使用具有透明度的圖片格式會是png，加上背景色之後就會呈現以下效果。設定好背景與圖片之後使用float進行排版即完成。
![](https://lh6.googleusercontent.com/-8Q9iZXAZiqk/VNw2LXZr5bI/AAAAAAAAE6A/ktsL_idFtFM/w1423-h62-no/03.jpg)
1. 第二個部分是`section .feature`，使用的排版技巧是將三個`li`的寬度(width)設成33.333%，再使用`float:left`進行排版。需要注意的是，加上border時會產生寬度大於原本container寬度的bug，使用`box-sizing:border-box`將內部寬度大於原本ul寬度的問題修正掉。中間image的置中的效果其實是用anchor先包覆住image與h3，接著設定`padding-top:100px`來固定他的位置。
![](https://lh3.googleusercontent.com/-6LXa00qZ29Y/VNw2KO3IG-I/AAAAAAAAE50/12hFpINuOxE/w1518-h464-no/04.jpg)
1. 第三部分是`section .porfolio`，使用的排版技跟剛剛的`.feature有所不同`。首先設定`.container`的大小，本練習中`.container`的寬度設為900px。因為需要三個20px的間隔，所以900px-20px * 3=840px。最後除以四210px就是個元素的寬度
![](https://lh3.googleusercontent.com/-IyV80Ie1x_o/VNw2H5tbK4I/AAAAAAAAE5o/StvGRpSkwL8/w1438-h450-no/01.jpg)
1. 最後介紹的是footer，這邊使用的排版技巧跟feature相同，寬度設定為33.333%後使用`float:left`進行排版。Registration Form使用的排版技巧：設定input的寬度為100%，這樣就會看起來一列有一個input。對input設定padding會讓placeholer跟border有所空間，看起來更美觀。

##練習後心得
這個練習難易度較低，但是也因為如此可以熟練chrome、text-editor和photoshop等必備的開發工具。對我來說是很適合的練習。練習過後對一些常用的CSS屬性更加熟悉，對開發速度是很有幫助的。