---
layout: post
title: Ruby metaprogramming - Method Lookup
published: true
date: 2016-09-10 19:55
tags: []
categories: []
comments: true

---
### 介紹

ruby metaprogramming 這本書除了教如何用 ruby 來生成其他的程式語言外，對語言特性的描述是比較深入的。因此在研讀這本書的同時，記錄下一些我覺得重要的部份。這些筆記不會依照章節的順序性。而是隨機記錄我需要的部分。

Method Lookup 即 Ruby 物件中查找方法的順序。從 Module 得到的方法與從 Class 繼承的方法其實是有順序性的。
知道其順序性後我們在編寫的程式碼的時候才可以比較清楚的預想程式行為的發生的情況。在追蹤原始碼的時候對語言特性多一份的理解追起來就會順利一點快速一點。

### prepend 與 include 

這邊是 Method Lookup 的順序圖，左邊是 instance method 右邊是 class method。使用 include 的時候可以發現，如果 class 中原本就有方法，那 include 進來優先權還是無法比 class 原有的方法高。但是如果使用 prepend ，則可以取代。我們用個小例子來證實這點。

![](https://lh3.googleusercontent.com/-CgYcBgfLE3M/V9ULwiOLqHI/AAAAAAAAKeU/x8xowPvc7k8/I/14735377495263.jpg)


ex1 - 使用 `include` 引入方法

```rb
module IAmModule
  def meow
    puts "Module Meow"
  end
end

class Cat

  include IAmModule

  def meow
    puts "Meow"
  end

end

c = Cat.new
c.meow
```

執行結果：

```
$ ruby ex1.rb
-> Meow
```

ex2 - 使用 `prepend` 引入方法

```rb
module IAmModule
  def meow
    puts "Module Meow"
  end
end

class Cat

  prepend IAmModule

  def meow
    puts "Meow"
  end

end

c = Cat.new
c.meow
```

```
$ ruby ex2.rb
-> Module Meow
```

我們可以使用 `ancestor` 方法來看看優先順序：

ex1 

```
[ Cat, IAmModule, Object, Kernel ,BasicObject ]
```

ex2 

```
[IAmModule, Cat, Object, Kernel, BasicObject]
```

ex1 使用 include 所以 Module 在 Class 之後，會優先呼叫 Class 的方法。
ex2 使用 prepend ， Module 引入的順序在 Class 之前，因此會優先執行 Module 的方法。

相關的還有與 refine 的比較，不過我想要把 refine 整理在 open class 獨立成一篇。敬請期待~

### 參考資料 & 圖片來源
[Get the Lowdown on Ruby Modules](https://www.sitepoint.com/get-the-low-down-on-ruby-modules/)