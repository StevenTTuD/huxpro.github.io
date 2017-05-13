---
layout: post
title: 'Ruby bits ( 1 ) : Expression'
published: true
date: 2014-09-03 09:10
tags:
  - Ruby
  - Ruby Bit
comments: true

---
## Unless

### Unless is more intuitive
用 unless 代替 if! 更加直覺
![](https://lh6.googleusercontent.com/-5bBGYT1k2-A/VJ6nejJKOhI/AAAAAAAADSo/1-o4flu-7Ys/w1753-h923-no/Screen%2BShot%2B2014-12-27%2Bat%2B20.21.26.png)

### Unless with else is confusing
unless 和 else 一起用的時候令人困惑
![](https://lh6.googleusercontent.com/-5bBGYT1k2-A/VJ6nejJKOhI/AAAAAAAADSo/1-o4flu-7Ys/w1753-h923-no/Screen%2BShot%2B2014-12-27%2Bat%2B20.21.26.png)

##NIL
###NIL IS FALSE
因為 nil 的值等於 false 所以上面的程式可以簡化成下面這樣。
![](https://lh5.googleusercontent.com/-d-D-pBa2YDg/VJ6nep0yXhI/AAAAAAAADS0/0Xxjj3Nio6s/w1755-h913-no/Screen%2BShot%2B2014-12-27%2Bat%2B20.26.07.png)

###ONLY NIL IS FALSE
Rails 世界中只有 nil 的值是 false（除了 False 本身的值當然是 False 以外）。
![](https://lh3.googleusercontent.com/-mdIOdhlh8D4/VJ6nevdeyvI/AAAAAAAADSw/1BiBgZAB4EQ/w1650-h900-no/Screen%2BShot%2B2014-12-27%2Bat%2B20.29.07.png)

## inline condition

![](https://lh5.googleusercontent.com/-6w8mJFsayEk/VJ6nfsNXD6I/AAAAAAAADS4/cCaNA4Tc9AU/w1648-h870-no/Screen%2BShot%2B2014-12-27%2Bat%2B20.33.55.png)

## short circuit assignment
指的是使用 and(&&) 和 or(||)來縮減 if-else

###Ex1 : if nil, default to empty array
![](https://lh6.googleusercontent.com/-VGNyfeQzJ9o/VJ6tympvnJI/AAAAAAAADTc/lspZVQXK6cU/w1753-h805-no/Screen%2BShot%2B2014-12-27%2Bat%2B20.47.18.png)

###Ex2 : sign in
一個常見的例子是用在「判斷使用者有沒有登入」，如果 session 中有 user_id 就去找到相對的 user。 session 中沒有找不到 user_id 時才導向登入畫面。
```ruby
def sign_in
  current_session || sign_user_in
end
```

## conditional assignment
![](https://lh5.googleusercontent.com/N9asEu-OunIOpgTfsZhXbHzp9ryQbRF2T5cYHb--Xos=w1650-h813-no)

現在有一個敘述``` i_was_set ||= 2 ```它的意思是如果 i_was_set 這個變數還沒被 assign 值，那就設定為2(下方的例子)。如果已經被 assign了值，i_was_set 的值保持原來 assign 的值（上方的例子）。

![](https://lh3.googleusercontent.com/0BVEMiPeVwoOckMffgOffI50f7cPgrCeM81PXWn74SI=w1755-h783-no)
再看一個例子，如果變數未指定的話就會設成後面的值。
##conditional return values
利用 =if 來減少重複的程式碼。原理是再function中裸寫出一個值時，其實意義上等於 return 剛剛寫下的那個值。舉例來說
```rb
if list_name
  "/#{user_name}/#{list_name}"
else
  "/#{user_name}"
end
```
會回傳```"/#{user_name}/#{list_name}"```或```"/#{user_name}"```，所以如果寫下
```ruby
options[:path] = if list_name
  "/#{user_name}/#{list_name}"
else
  "/#{user_name}"
end
```
其實就等於
```ruby
if list_name
  options[:path] = "/#{user_name}/#{list_name}"
else
  options[:path] = "/#{user_name}"
end
```
上式先判斷listname的值，再指定給option[:path]，下式也是先判斷list_name的值，再指定給 option[:path]。可以發現兩者邏輯是相同的，只是書寫的方式看起來不同。

##conditional return values (ex2)
```rb
def list_url(user_name, list_name)
  if list_name
    url = "https://twitter.com/#{user_name}/#{list_name}"
  else
    url = "https://twitter.com/#{user_name}"
  end
  url
end
```
```rb
def list_url(user_name, list_name)
  if list_name
      "https://twitter.com/#{user_name}/#{list_name}"
  else
      "https://twitter.com/#{user_name}"
  end
end
```
##CASE
case 的用法跟java不太一樣，但是其實意義上有點類似，ruby裡面的case流程判斷的else就是java裡面switch流程判斷的default。然後java設定條件的地方用關鍵字 case 而 ruby 用關鍵字 when。
###CASE RANGES

```rb
popularity = case tweet.retweet_count
  when 0..9
    nil
  when 10..99
    "trending"
  else
    "hot"
end
```

###CASE - REGEXPS
```rb
tweet_type = case tweet.status
	when /\A@\w+/
		:mention
	when /\Ad\s+\w+/
		:direct_message
	else
		:public
end
```

###CASE - WHEN/THEN
```rb
tweet_type = case tweet.status
	when /\A@\w+/    then :mention
	when/\Ad\s+\w+/  then :direct_message
	else                  :public
  end
```
這個設定滿方便，用java寫至少要拆成兩個物件才能夠取得return的值。(看不懂不要緊，我沒有說的很清楚，總之就記得ruby這樣的寫法是比較簡潔的即可)。