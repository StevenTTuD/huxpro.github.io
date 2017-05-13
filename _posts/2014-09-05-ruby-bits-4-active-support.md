---
layout: post
title: Ruby bits ( 4 )：ActiveSupport
published: true
date: 2014-09-05 08:12
tags:
  - Ruby
  - Ruby Bit
comments: true

---

#install it and load it
##install it
gem install activesupport
gem install i18n

##load it
require 'active_support/all'

##core extensions: array
array.form()
array.to()
array.in_group_of
array.splite

![](https://lh5.googleusercontent.com/OB4R69JvbILh1yuu6vN19DE4F4N0D2x2ctwontchPpg=w1755-h930-no)

##core extensions: Date
.at_beginning_of_day
.at_end_of_monthr

![](https://lh4.googleusercontent.com/-gTpPXf3apRg/VAl8eIYhssI/AAAAAAAAC6I/gCGNlSzXD8E/w1755-h930-no/Screen%2BShot%2B2014-09-05%2Bat%2B17.03.04.png)
![](https://lh6.googleusercontent.com/-hrKJGiJ99Sk/VAl8eMcWUCI/AAAAAAAAC6E/cE1O4VGHvZ4/w1753-h913-no/Screen%2BShot%2B2014-09-05%2Bat%2B17.03.22.png)

##core extensions:hash
有兩個hash
options = {user: 'codeschool', lang: 'fr'}
new_options = {user: 'codeschool', lang: 'fr', password: 'dunno'}

###**options.diff(new_options)**
比較option與new_options的不同
###**options.stringify_keys**
turn keys into strings
![]( https://lh6.googleusercontent.com/e2_pcizuLi1OlUovuZiSojCpYSu6RN7By3sOxdyXlP8=w1755-h925-no)

###Merge hash
會以前面的為主
![](https://lh4.googleusercontent.com/bbHJCbvWR4bZkbWafn50FU-zEs_2mN961DVc7LCyRnE=w1518-h828-no)

###remove keys
new_options.except(:password)

###檢查是否有額外的key
new_options.assert_valid_keys(:user, :lang)，如果有額外的key的話就會出現警示訊息
![](https://lh3.googleusercontent.com/-BuXD1yd6ELg/VAlzlXcFaOI/AAAAAAAAC4U/HwFcTSDaNLA/w1472-h424-no/Screen%2BShot%2B2014-09-05%2Bat%2B16.25.34.png)

#core extensions: integer
##odd or even
index.odd?
index.even?
![](https://lh3.googleusercontent.com/-wqEF5xlbw6I/VAl4Aad_csI/AAAAAAAAC5A/SIbmaS-L2Nw/w1753-h853-no/Screen%2BShot%2B2014-09-05%2Bat%2B16.44.04.png)

##ordinallize
"#{1.ordinalize} place!"
"#{2.ordinalize} place!"
"#{3.ordinalize} place!"
![](https://lh6.googleusercontent.com/-UOYnfA1u4gs/VAl4AeYO7NI/AAAAAAAAC44/pLotT1TEP7A/w1755-h1050-no/Screen%2BShot%2B2014-09-05%2Bat%2B16.44.12.png)

##pluralize and singularize
轉換單字的單數與複數
![](https://lh5.googleusercontent.com/-eIRkulMno2c/VAl4AhVM0vI/AAAAAAAAC5E/xTXoDHRDGfk/w1755-h745-no/Screen%2BShot%2B2014-09-05%2Bat%2B16.44.24.png)

##titleize and humznize
![](https://lh4.googleusercontent.com/-qlpsYKkMB4U/VAl4BvlyLRI/AAAAAAAAC5I/uMGS4bSgBrk/w1753-h658-no/Screen%2BShot%2B2014-09-05%2Bat%2B16.44.29.png)

#延伸閱讀
看完還是有點霧煞煞，接著來看[ihower實戰聖經:active support](http://ihower.tw/rails3/activesupport.html)
