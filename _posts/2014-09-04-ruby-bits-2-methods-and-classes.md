---
author: StevenTTuD
layout: post
title: 'Ruby bits ( 2 ) : Methods and Classes'
published: true
date: 2014-09-04 05:05
tags:
  - Ruby
  - Ruby Bit
comments: true

---
## Hash Argument
![](https://lh4.googleusercontent.com/-hdUCbd8-xxI/VJ7OabbDJzI/AAAAAAAADVE/IVpvhmF0jME/w1755-h965-no/Screen%2BShot%2B2014-12-27%2Bat%2B23.18.02.png)
每一個不同的欄位就多一個 argument不是個好方法，會造成很多欄位空在那邊。
![](https://lh6.googleusercontent.com/-Gn4gNk7tPCE/VJ7ObUCXt5I/AAAAAAAADVQ/DmSN6NOzNLw/w1755-h958-no/Screen%2BShot%2B2014-12-27%2Bat%2B23.18.18.png)
用 Hash argument 來解決這個問題。
![](https://lh6.googleusercontent.com/-Gn4gNk7tPCE/VJ7ObUCXt5I/AAAAAAAADVQ/DmSN6NOzNLw/w1755-h958-no/Screen%2BShot%2B2014-12-27%2Bat%2B23.18.18.png)
使用方法，因為 hash 是由 key-value 所組成，所以可以把需要的屬性設成 symbol ，也就是```:lat```這種樣子。後面指定它的 value，形成 key-value 的形式。
![](https://lh5.googleusercontent.com/-6bgtAU3Clbg/VJ7ObPXoEWI/AAAAAAAADVM/0UnTaLC-y8o/w1755-h933-no/Screen%2BShot%2B2014-12-27%2Bat%2B23.18.31.png)
改寫成 1.9 以後的 hash![](https://lh5.googleusercontent.com/-Kig_SWyG0Bc/VJ7Odqbjh0I/AAAAAAAADVY/fZtWyQP4WPQ/w1755-h855-no/Screen%2BShot%2B2014-12-27%2Bat%2B23.19.22.png)
hash 中的 key 可以省略，只寫出某幾個就可以，就連hash argument 本身都可以省略，。
![](https://lh6.googleusercontent.com/-SoTADybAdlA/VJ7OfHNTvJI/AAAAAAAADVg/bWIor1roiY8/w1755-h663-no/Screen%2BShot%2B2014-12-27%2Bat%2B23.19.42.png)

## Exceptions
![](https://lh6.googleusercontent.com/-q7ZbxrQ8BHU/VJ7ZaBfnJ2I/AAAAAAAADV4/7G0z1kMaJXk/w1753-h933-no/Screen%2BShot%2B2014-12-28%2Bat%2B00.05.23.png)
當 tweets 是空的時候，我希望我會知道這個訊息，這時候 Exception 就派上用場了。
```rb
def get_tweets(list)
  unless list.authorized?(@user)
    raise AuthorizationException.new
	end
	list.tweets
end
```
現在我有一個 method 叫做 get_tweets(list)，當他發現 user 沒有認證的時候，它就會丟出AuthorizationException這個例外，對應的程式碼是```raise AuthorizationException.new```，接下來再用另外一段程式碼，把剛剛上面的程式碼包起來，因為我們接下來要做的是對拋出的例外做一些處理，好讓我們可以藉由 exception 更輕鬆的了解到錯誤出現在哪裡。
```rb
begin
  tweets = get_tweets(my_list)
rescue AuthorizationException
  warn "You are not authorized to access this list."
end
```
1~2行，先執行我們剛剛建立的method 叫做 get_tweet(my_list)，接著 3~4 行的意思是當發現有認證例外（AuthorizationException）產生時，就會跳出警告```"You are not authorized to access this list."```。
延伸閱讀：
[Ruby Exceptions](http://rubylearning.com/satishtalim/ruby_exceptions.html)
[tutorial point ruby exception](http://www.tutorialspoint.com/ruby/ruby_exceptions.htm)
[ruby exceptions and error handling](http://www.skorks.com/2009/09/ruby-exceptions-and-exception-handling/)



## Splat Argument
![](https://lh3.googleusercontent.com/--O8ZvriOtYI/VJ7frL2bYtI/AAAAAAAADWU/Vudg3_RSJnY/w1748-h260-no/Screen%2BShot%2B2014-12-28%2Bat%2B00.34.16.png)
前面有加 * 字號的變數代表傳入的會是 Array，依序傳入為 Array[0]、Array[1]、Array[2]...以此類推。
![](https://lh3.googleusercontent.com/m_IMEQvAM5O6a-_I801MNkMBy3b6Ksklu_-wnlDo5T8=w1755-h415-no)

## You Need a Class When ...
![](https://lh4.googleusercontent.com/-A_2YsWJ-dzQ/VJ7j1AQW5kI/AAAAAAAADW4/pkW88PelvLU/w1755-h1005-no/Screen%2BShot%2B2014-12-28%2Bat%2B00.48.34.png)
![](https://lh6.googleusercontent.com/-koGBR2kSY74/VJ7jz1uHhMI/AAAAAAAADW0/J4GF70Xeeg4/w1753-h903-no/Screen%2BShot%2B2014-12-28%2Bat%2B00.49.01.png)
![](https://lh4.googleusercontent.com/-qcgWOVQOWl0/VJ7jz_THkII/AAAAAAAADWs/GiSTz8Od5uE/w1755-h855-no/Screen%2BShot%2B2014-12-28%2Bat%2B00.49.23.png)

## OverSharing
![](https://lh4.googleusercontent.com/-ySIcZkVxL1k/VJ7j3olxddI/AAAAAAAADXA/LGct57OmEoU/w1753-h973-no/Screen%2BShot%2B2014-12-28%2Bat%2B00.50.40.png)
![](https://lh4.googleusercontent.com/-G881X3MSHo8/VJ7j67Nk_vI/AAAAAAAADXM/ZkKMYHmRlY8/w1755-h960-no/Screen%2BShot%2B2014-12-28%2Bat%2B00.50.48.png)


## Reopen Classes
![](https://lh5.googleusercontent.com/-oHFPEKrFU0o/VJ7j71YNvoI/AAAAAAAADXQ/Y3IIxyyihG0/w1755-h968-no/Screen%2BShot%2B2014-12-28%2Bat%2B00.51.02.png)
- You can re-open and change any class.
- Beware! You don't know who relies on the old functionality.
- You should only re-open classes that you yourself own.

## Self
![](https://lh6.googleusercontent.com/-gJ4xawZHjFY/VJ7j9YxS8bI/AAAAAAAADXY/UKRNKNzoTdA/w1755-h980-no/Screen%2BShot%2B2014-12-28%2Bat%2B00.51.54.png)