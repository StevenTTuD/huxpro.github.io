---
layout: post
title: Ruby bits ( 6 )：Blocks 學習筆記
published: true
date: 2014-09-06 05:13
tags: []
categories: []
comments: true

---
不得不再說一次，這些主題都超級重要，不先學Ruby直接學Rails感覺很卡。果然要從基礎開始打才是王道。在看這篇之前請先看完[method / block / yield / Proc / lambda](http://bombersnote.logdown.com/posts/2014/09/06/231080)，很多觀念都是從這邊的延伸。


#conventions
block有兩種表達方式
###single block
braces if the block is a single line
```rb
words.each { |word| puts word }
```
###multiblock
do/end if it's multiple lines
```rb
words.each do |word|
  backward_word = word.reverse
  puts backward_word
end
```
#yield
這邊有一個yield的範例
```rb
def call_this_block_twice
	yield
	yield
end
```
```rb
call_this_block_twice{ puts "twitter"}
  #=>twitter twitter
```
一開始看一定看不懂他是怎麼做的，因為他省略了很多東西，其實call block可以拆解成三個部分，第一個部分是定義呼叫block的function，在這個例子中就是
```rb
def call_this_block_twice
	yield
	yield
end
```
使用yield的時候可以省略傳入的&block與呼叫的.call方法，上面這段程式碼等同於
```rb
def call_this_block_twice(&block)
	block.call
	block.call
end
```
第二個部分是宣告block
在這個例子中省略了這一部分，其實"twitter"就是一個傳入的block。

第三部分是傳入block到function之中輸出結果
把"twitter"這個block傳進能夠處理block的function之中。
```rb
call_this_block_twice{ puts "twitter"}
  #=>twitter twitter
```

##yield - arguments
再來看另一個範例
```rb
def call_this_block
  yield "tweet"
end
```
yield中傳入了參數"tweet"，也就等於block.call("tweet")
```rb
call_this_block { |myarg| puts myarg }
  #=>tweet
```
```rb
call_this_block { |myarg| puts myarg.upcase }
  #=>TWEET
```
這時候如果要在call block的時候呼叫參數，要用```|```和```|```把輸入的參數包起來。接著後面的陳述式就可以對輸入的進行一些處理。

#Your own "each"
現在我的class中有兩個method，一個要列出朋友的tweet，一個要儲存朋友的tweet。可以發現```@user.friends.each do |friend| ```這段iteration重複了。
```rb
class Timeline
  def list_tweets
    @user.friends.each do |friend|  
      friend.tweets.each { |tweet| puts tweet }
    end 
  end
  
  def store_tweets
    @user.friends.each do |friend|
      friend.tweets.each { |tweet| tweet.cache }
    end 
  end
end
```
於是我們來著撰寫自己的each
```rb
class Timeline
  def each
    @user.friends.each do |friend| 
      friend.tweets.each { |tweet| yield tweet }
    end 
  end
end
timeline = Timeline.new(user)
timeline.each { |tweet| puts tweet }
timeline.each { |tweet| tweet.cache }
```
加入Enumerable擴充更多的method
```rb
class Timeline
  def each
...
end
  include Enumerable
end
```
在include Enumerable這個module之後，瞬間多了很多method（如下），要解了更多就去看看[ruby-doc Enumerable](http://ruby-doc.org/core-2.1.2/Enumerable.html)吧。
```rb
timeline.sort_by  { |tweet| tweet.created_at }
timeline.map      { |tweet| tweet.status }
timeline.find_all { |tweet| tweet.status =~ /\@codeschool/ }
```

#再來重構一個例子
```rb
def update_status(user, tweet)
  begin
    sign_in(user)
    post(tweet)
  rescue ConnectionError => e
    logger.error(e)
ensure
    sign_out(user)
	end 
end
```
```rb
def get_list(user, list_name)
  begin
    sign_in(user)
    retrieve_list(list_name)
  rescue ConnectionError => e
    logger.error(e)
ensure
    sign_out(user)
	end 
end
```
有裡個method裡面除了核心邏輯以外全部都一樣。這樣的話我們需要進行重構來實現DRY原則。因為Proc本身就是匿名函數也就是一段未執行程式碼，所以特別適用這個情況。
##把重複的地方獨立出來，核心邏輯用yield代替。
```rb
def while_signed_in_as(user) begin
    sign_in(user)
    yield
  rescue ConnectionError => e
    logger.error(e)
  ensure
    sign_out(user)
  end 
end
```
使用do的時候會省略傳入的&block，所以這段程式碼其實是對block做些處理，並顯示在block.call也就是yield的部份。
```rb
while_signed_in_as(user) do
  post(tweet)
end
```
```rb
tweets = while_signed_in_as(user) do
  retrieve_list(list_name)
end
```
##最後可以改寫的精簡一點
去掉不必要的begin和end
```rb
def while_signed_in_as(user) 
  sign_in(user)
  yield
  rescue ConnectionError => e
    logger.error(e)
  ensure
    sign_out(user)
end
```

#作業實作
##6.1
使用each代替for迴圈。原來使用for迴圈的程式：
```rb
def list
  for i in 0...(games.length)
    game = games[i]
    puts game.name
  end
end
```
改寫成each
```rb
def list
  games.each do|game|
    puts game.name
  end
end
```
##6.2
現在我們有一個class叫做Game裡面裝著每場比賽的資訊。
有一個陣列叫做Games，裡面蒐集了很多場的比賽。
現在我們要在Library中寫一個方法叫做each_on_system(system)，讓他可以讀出Games陣列裡面符合輸入的system的比賽。
```rb
class Library
  attr_accessor :games

  def initialize(games = [])
    self.games = games
  end

  def each_on_system(system)

  end
end
```
使用範例example.rb
```rb
library = Library.new(GAMES)
library.each_on_system("SNES") { puts "Found a Super Nintendo game" }
```
將library修正後，即為所得。
```rb
class Library
  attr_accessor :games

  def initialize(games = [])
    self.games = games
  end

  def each_on_system(system)
    games.each do|game|
      yield if game.system == system
    end
  end
end
```
##6.3 Passing Argument to Blocks
讓產生的block能夠使用iterator的參數。如下圖可以使用|game|。
```rb
library = Library.new(GAMES)
library.each_on_system("SNES") { |game| puts game.name }
```
##6.4 Return Value From block
Modify the list method to yield to a block and print whatever the block returns.
除了能夠在block中使用以外還要可以直接印出來。
```rb
library = Library.new(GAMES)
library.list { |game| "#{game.name} (#{game.system}) - #{game.year}" }
```
```rb
class Library
  attr_accessor :games

  def initialize(games = [])
    self.games = games
  end

  def list
    games.each do |game|
      puts yield game
    end
  end
end
```
##6.5include Enumerable module

##6.6重構以避免重複
```rb
class Game
  attr_accessor :name, :year, :system
  attr_reader :created_at

  def initialize(name, options={})
    self.name = name
    self.year = options[:year]
    self.system = options[:system]
    @created_at = Time.now
  end

  def play
    begin
      emulator = Emulator.new(system)
      emulator.play(self)
    rescue Exception => e
      puts "Emulator failed: #{e}"
    end
  end

  def screenshot
    begin
      emulator = Emulator.new(system)
      emulator.start(self)
      emulator.screenshot
    rescue Exception => e
      puts "Emulator failed: #{e}"
    end
  end
end
```
```rb
class Game
  attr_accessor :name, :year, :system
  attr_reader :created_at

  def initialize(name, options={})
    self.name = name
    self.year = options[:year]
    self.system = options[:system]
    @created_at = Time.now
  end

  def play
    emulate do |emulator|
      emulator.play(self)
    end
  end

  def screenshot
    emulate do |emulator|
      emulator.start(self)
      emulator.screenshot
    end
  end

  private

  def emulate
    begin
      emulator = Emulator.new(system)
      yield emulator
    rescue Exception => e
      puts "Emulator failed: #{e}"
    end
  end
end
```
```rb
class Emulator
  def initialize(system)
    # Creates an emulator for the given system
  end
 
  def play(game)
    # Runs the given game in the emulator
  end
 
  def start(game)
    # Loads the given game but doesn't run it
  end
 
  def screenshot
    # Returns a screenshot of the currently loaded game
  end
end
```