---
author: StevenTTuD
layout: post
title: Ruby bits ( 5 )：Modules
published: true
date: 2014-09-05 08:52
tags:
  - Ruby
  - Ruby Bit
comments: true

---
Ruby bits的課程真的很不錯，講到的主題都是很重要的東西。有點相見恨晚的感覺，趕快把它做完吧!

##本節學習目標
1. module
2. activesupport ::Concern
3. 了解self的意義

#part1: module
##class method與instance method

###use extend to expose methods as class method
```rb
class Tweet
  extend Searchable
end
```
使用的時候，直接呼叫class（開頭大寫）。
```rb
Tweet.find_all_from('@GreggPollack')
```
###use include to expose methods as instance methods
instance method用include引進module
```rb
class Image
  include ImageUtils
end
```
```rb
image = user.image
image.preview
```


#part2 :how to include class method and instance method in the same time

##第一種方法：依照直覺該怎麼寫
第一種方法是在class中includ module並且extend classmothods。
下例中Image想要使用ImageUtils這個module的class method與instance method，所以用兩種方式各自引用。
```rb
class Image
  include ImageUtils
  extend ImageUtils::ClassMethods
end
```
```rb
module ImageUtils

  def preview
  end

  def transfer(destination)
  end

  module ClassMethods
    def fetch_from_twitter(user)
    end
  end

end
```
使用上可以按照之前所學的來呼叫class method與instance method
```rb
image = user.image
image.preview
Image.fetch_from_twitter('gregg')
```
##第二種方法:method hooks
這樣每次使用module的時候都需要explore兩種module實在太麻煩了，引入class module這個動作在module完成，這樣我們就不需要每次都引入兩種method，只要引入module就可以了。於是第二種方法method hook產生了：

```rb
class Image
  include ImageUtils
  #extend ImageUtils::ClassMethods       刪除掉引入class method的這一行
end
```

```rb
module ImageUtils
  def self.included(base)  #加入self.include （self就是ImageUtils這個module）
    base.extend(ClassMethods)
  end
  def preview
  end
  def transfer(destination)
  end
  module ClassMethods
    def fetch_from_twitter(user)
    end
  end
end
```
#part3: 使用Activesupport :: Concern解決相依性問題

> Activesupport :: Concern代表什麼意思
:: is basically a namespace resolution operator. It allows you to access items in modules, or class-level items in classes. For example, say you had this setup:
關鍵字double colon ruby
[What is Ruby's double-colon (::) all about?](http://stackoverflow.com/questions/3009477/what-is-rubys-double-colon-all-about)

##使用方式
在terminal下```gem install activesupport```安裝activesupport
在module檔中(xxx.rb)中require 'active_support/concern'

```rb
require 'active_support/concern' module ImageUtils

  extend ActiveSupport::Concern

  included do
    clean_up
	end

  module ClassMethods
    def fetch_from_twitter(user)
    end

    def clean_up
    end
  end
end
```
這樣就可以直接include ImageUtil的ClassMethods
```rb
class Image
	include ImageUtils
end
```
這樣看起來沒什麼了不起的對吧？只是換個寫法。實際上cocern的出現是為了解決更重要的問題，讓我們看下去。
##Acitvesupport::concern要解決的問題
```rb
module ImageUtils
  def self.included(base)      #base is ImageProcessing module
    base.extend(ClassMethods)
  end
  module ClassMethods
    def clean_up; end
	end
end
```
```rb
module ImageProcessing

	include ImageUtils

  def self.included(base)
    base.clean_up                #undefined method error
  end
end
```
```rb
class Image
   include ImageProcessing
end
```
這樣乍看之下好像沒問題，但是卻有個嚴重的問題導致無法執行，因為ImageUtils變成是由ImageProcessing所 include，所以對 ImageUtils 的 self.included 來說，他的參數 base 變成了 ImageProcessing 了，所以他就沒辦法存取到宿主 Host 的任何函式及變數，do_host_something 時就會失敗。

Okay，ActiveSupport::Concern 就是來幫助解決這個難題，我們希望宿主可以不需要知道 modules 之間的 dependencies 關係。dependencies 關係寫在 module 裡面就好了。
```rb
module ImageUtils

  extend ActiveSupport::Concern

  module ClassMethods
    def clean_up; end
  end
end
```
```rb
module ImageProcessing

  extend ActiveSupport::Concern
  include ImageUtils

  included do
    clean_up
  end
end
```
```rb
class Image
  include ImageProcessing
end
```
Dependencies are properly resolved！！

#part4 作答中遇到的問題
##5.1 宣告成class method
原本的
```rb
module GameUtils
  def lend_to_friend(game, friend_email)
  end
end
```
改成
```rb
module GameUtils
  def self.lend_to_friend(game, friend_email)
  end
end
```
這樣呼叫時就會從原本的
```rb
game = Game.new("Contra")
game.lend_to_friend(game, "gregg@codeschool.com")
```
變成
```rb
game = Game.new("Contra")
GameUtils.lend_to_friend(game, "gregg@codeschool.com")
```

##5.2 reopen game and include the gameutil module

##5.3 reopen Game and expose the method from module as class method of Game class
原本
```rb
class Game
end
```
加上GameUtils的class method
```rb
class Game
  extend GameUtils
end
```
##5.4 extend the single game object with Playable module
原本
```rb
game = Game.new("Contra")
game.play
```
加入module Playable的method到game這個instance中。
```rb
game = Game.new("Contra")
game.extend(Playable)
game.play
```
module長這樣
```rb
module Playable
  def play
  end
end
```
這個用法讓我有點困惑，特地查了一下[ruby-doc](http://ruby-doc.org/core-2.1.2/Object.html#method-i-extend)
extend的解釋如下
Adds to obj the instance methods from each module given as a parameter.
雖然是用extend這個字，但並非是繼承的意思，而是加入instance method。

##5.5
使用self.include初始化class method

##5.6
使用ActiveSupport::Concern代替self.included

##5.7 AcitveSupport::Concern part II
使用included class method

##5.8 AcitveSupport::Concern part III

#延伸閱讀：
[深入Rails3: ActiveSupport::Concern](http://ihower.tw/blog/archives/3949)
[Self - The current/default object](http://rubylearning.com/satishtalim/ruby_self.html)
[ActiveSupport::Concern](http://api.rubyonrails.org/classes/ActiveSupport/Concern.html)