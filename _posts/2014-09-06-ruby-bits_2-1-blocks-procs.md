---
layout: post
title: Ruby bits 2 ( 1 )：BLOCKS, PROCS & LAMBDAS
published: true
date: 2014-09-06 07:42
tags:
  - Ruby
  - Ruby Bit
comments: true

---
雖然跟Ruby bit名字很像，但是內容的確比較進階點。
#two ways for storing blocks
##1.Proc
```rb
my_proc = Proc.new do
  puts "tweet"
end
my_proc.call # => tweet
```
等同於
```rb
my_proc = Proc.new { puts "tweet" }
my_proc.call # => tweet
```

##2.lambda
使用lambda來儲存又稱為static lambda。
```rb
my_proc = lambda { puts "tweet" }
my_proc.call  # => tweet
```
Ruby1.9以前的版本是這樣寫
```rb
my_proc = -> { puts "tweet" }
my_proc.call  # => tweet
```
#block to lambda
```rb
class Tweet
  def post
    if authenticate?(@user, @password)
      # submit the tweet
      yield
    else
      raise 'Auth Error'
    end
  end
end
```
```rb
tweet = Tweet.new('Ruby Bits!')
tweet.post { puts "Sent!" }
```
等同於
```rb
class Tweet
  def post(success)
    if authenticate?(@user, @password)
      # submit the tweet
      success.call
    else
      raise 'Auth Error'
    end
  end
end
```
```rb
tweet = Tweet.new('Ruby Bits!')
success = -> { puts "Sent!" }
tweet.post(success)
```

#multiple lambdas
```rb
class Tweet
  def post(success, error)
    if authenticate?(@uerser, @password)
      # submit the tweet
      success.call
    else
      error.call
    end
  end
end
```
```rb
tweet = Tweet.new('Ruby Bits!')
success = -> { puts "Sent!" }
error = -> { raise 'Auth Error' }
tweet.post(success, error)
```
#Using the ampersand
有兩種情況會用到ampersand也就是```＆```符號
##1.Calling a method with & in front of a parameter
```rb
tweets.each(&printer)
```
turn a proc into block
##2.Defining a method with & in front of a parameter
```rb
def each(&block)
```
turns a block into a proc so it can be assigned to parameter

這兩種用法很常同時使用
##example 1:
```rb
printer = lambda { |tweet| puts tweet }
tweets.each (printer) (
```
這樣會出現錯誤，因為each expects a block, not a proc.
改成這樣就沒問題了。
```rb
printer = lambda { |tweet| puts tweet }
tweets.each(&printer)
```
`&`turns proc into block

##example 2:
```rb
class Timeline
  attr_accessor :tweets
  def each(&block)       #block into proc
    tweets.each(&block)  #proc back into a block
  end
end
```
```rb
timeline = Timeline.new(tweets)
timeline.each do |tweet|
  puts tweet
end
```

#symbol to Proc

未完待續...