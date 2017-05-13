---
layout: post
title: 'Ruby metaprogramming - Open Class and Refine '
published: true
date: 2016-09-11 13:49
tags:
  - Ruby
  - Metaprogramming
comments: true

---
### 簡介

Open Class 是 Ruby 常用的技巧，指的是我們可以覆寫已經存在的方法，來修改物件或類別的行為。
在這邊我沒有要講詳細的作法，要介紹的是在 Ruby 2.0 中新增的 `refine`

### Refine

Refine 是 ruby 2.0 之後加入的元素。
會使用 Refine 的原因是使用一般 Open Class 的技巧時並不會有明顯的提示。
所以我們如果沒有用比較清楚的方法標示 Open Class 作用的地方，造成的後果是非常難以維護且可怕的。
透過 refine 我們可以更安全的覆寫原本的 Class，尤其是系統中本來存在的一些方法。
用法是先定義一個 Module ，Module 裡面寫下你要覆寫的 Class 與要覆寫的方法。
並且用 Using 決定在何時使用。

```rb
class Cat
  def meow
    puts "Meow"
  end
end

module IAmRefineModule
  refine Cat do
    def meow
      puts "meow in Refine Module"
    end
  end
end

c = Cat.new
c.meow

using IAmRefineModule

c = Cat.new
c.meow

```

執行結果：

```
$ ruby refine_ex.rb
-> Meow
-> meow in Refine Module
```

要呼叫`using IAmRefineModule`後，我們定義的 refine Module，那麼 Class 方法才會被覆寫到原有的 class。

### Method Lookup

接下來我們來跟 Module 的 Prepend 來比較看看，看看 refine 定義的方法，是否會優先於 prepend module 所引入的方法。

```rb
module IAmModuleInclude
  def meow
    puts "Module include Meow"
  end
end

module IAmModulePrepend
  def meow
    puts "Module prepend Meow"
  end
end

class Cat
  prepend IAmModulePrepend
  # include IAmModuleInclude
  def meow
    puts "Meow"
  end
end

module IAmRefineModule
  refine Cat do
    def meow
      puts "meow in Refine Module"
    end
  end
end

c = Cat.new
c.meow

using IAmRefineModule

c = Cat.new
c.meow
```

來執行看看

```rb
$ ruby refine_prepend_compare.rb
-> Module prepend Meow
-> meow in Refine Module
```

我們發現，即使 prepend Module 之後，只要我們使用 refine，就會優先使用 refine 所定義的方法。
實際上使用 refine 的時候，因為語意上非常明顯，作為 open class 的選擇我覺得是很棒的。