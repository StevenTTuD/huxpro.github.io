---
layout: post
title: Ruby - 利用 ARGV 特性執行指定的方法
published: true
date: 2016-08-26 16:58
tags: []
categories: []
comments: true

---
### 情境

我想手動執行 class 的某個方法。這邊的例子是當我想移動的時候，我可以選擇走路、跑步或是游泳。

###方法一： 在 irb 中引入 Ruby 檔

```rb
class Move

  def self.walking
    puts "walking"
  end
	
  def self.running
    puts "running"
  end

  def self.swimming
    puts "swimming"
  end
end

```

打開 irb ，輸入 `require_relative argv_ex1.rb`，argv_ex1.rb 是上面程式碼的檔名。

```rb
[1] pry(main)> require_relative 'argv_ex1.rb'
=> true
[2] pry(main)> Move.swimming
swimming
=> nil
```

用這樣的方式我們可以執行 Class 中的某個方法，不過還是麻煩了點。現在我們來試試透過 ARGV 來執行 Move 中的方法。


### 方法二：使用 ARGV 執行指定的 method 

#### 2.1 簡單介紹 ARGV

當你在 Command line 模式中輸入除了原本檔名以外的參數，會自動被儲存成一個 ARGV 陣列。

```rb
argv = ARGV

puts "ARGV Type is: " + argv.class.to_s
puts "array elements are: "
puts argv
```

打開 Terminal 輸入`ruby argv_ex1.rb cat dog rabbit snake`，會得到：

```
ARGV Type is:Array
array elements are:
cat
dog
rabbit
snake
```

簡單的說參數的第一個會對應 ARGV[0]，第二個會對應 ARGV[1]，運用這個特性我們可以依照需求來決定要執行的方法或內容。

#### 2.2 應用到例子

```rb
class TryARGV
  def self.execute( command )
    if command.to_sym == :walk
      self.walking
    elsif command.to_sym == :swim
      self.swimming
    else
      puts "swim or walk?"
    end
  end

  def self.walking
    puts "walking"
  end

  def self.running
    puts "running"
  end

  def self.swimming
    puts "swimming"
  end
end

TryARGV.execute( ARGV[0] )
```

現在我們只要在 terminal 中輸入 `swim` 或是 `walk` 即會呼叫對應的方法。

```
$ argv_practice ruby argv.rb swim
swimming
$ argv_practice ruby argv.rb walk
walking
```

### 結論

可以用來呼叫類別方法的還有 rake 也可以達到相同效果。不過如果要使用的話還需要另外設定 `.rake` 檔。ARGV 的方式在輕量使用的時候是個不錯的選擇。