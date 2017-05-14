---
author: StevenTTuD
layout: post
title: Ruby bits ( 3 )：Class
published: true
date: 2014-09-05 06:41
tags:
  - Ruby
  - Ruby Bit
comments: true

---
上一個禮拜想要嘗試更多的動手記憶，經過實驗證明，還是需要筆記的輔助反覆記憶會比較好。經過這次的練習之後會更注重學習本身的強度。

## ENCAPSULATION封裝
跟物件本身有關的method，使用class會比使用許多的method來的好。

## VISIBILITY
有一些method你並不想給外面的人直接使用他，那麼宣告成private，限制這些private method不能給外面的人所使用。
```rb
class User
  def up_vote(friend)
    bump_karma
    friend.bump_karma
  end
  protected
  def bump_karma
    puts "karma up for #{name}"
  end
end
```
# INHERITANCE
重複的程式碼使用繼承來避免。
原來程式碼：
```rb
class Image
	attr_accessor :title, :size, :url
  def to_s
  	"#{@title},{@size}"
	end
end

class Video
	attr_accessor :title, :size, :url
  def to_s
  	"{@title}, {@size}"
  end
end
```
使用繼承後：
```rb
class Attachment
  attr_accessor :title, :size, :url
  def to_s
    "#{@title}, #{@size}"
end end
class Image < Attachment
end
class Video < Attachment
end
```
ruby內的繼承就用箭頭```<```來表示
## SUPER
ruby的super跟java的super只能夠繼承constructor一樣。（見下圖）
![](https://lh3.googleusercontent.com/AXTNO38rF_Z5-Bvn6HEX96KsojxntcHjnKFdkpT-5-4=w1755-h923-no)

### super的省略寫法
super不僅可以在method裡面用，而且有省略寫法。
不過一開始學習還是把參數加上去避免混淆。
![](https://lh4.googleusercontent.com/-3UdY2F0B16k/VAlinCbjwRI/AAAAAAAAC3c/aFwDasJziEo/w1755-h833-no/Screen%2BShot%2B2014-09-05%2Bat%2B15.12.39.png)

### overideing methods以加強執行效率
原來寫法：使用case來判斷。
```rb
class Attachment
  def preview
    case @type
    when :jpg, :png, :gif
			thumbnail
    when :mp3
      player
		end
  end
end
```
不如直接使用subclass，增加效率。
```rb
class Attachment
  def preview
    thumbnail
  end
end

class Audio < Attachment
  def preview
    player
	end
end
```

## HIDE INSTANCE VARIABLES
這節要討論的是如何簡化程式碼
原本
```rb
class User
  def tweet_header
    [@first_name, @last_name].join(' ')
  end
  def profile
    [@first_name, @last_name].join(' ') + @description
	end
end
```
可以看到method內有重複的地方。把他們包起來獨立出來。
```rb
class User
  def display_name
    [@first_name, @last_name].join(' ')
  end
  def tweet_header
    display_name
  end
  def profile
    display_name + @description
  end
end
```
更漂亮的寫法?
```rb
class User
  def display_name
    title = case @gender
      when :female
        married? ? "Mrs." : "Miss"
      when :male
        "Mr."
    end
    [title, @first_name, @last_name].join(' ')
  end
end
```
## Override
Override的方法很簡單，直接取相同的method名稱，就可以複寫掉父類別的method。

## 最後一關
重構程式碼 refactoring