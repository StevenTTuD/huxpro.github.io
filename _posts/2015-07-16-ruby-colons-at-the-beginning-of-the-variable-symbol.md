---
author: StevenTTuD
layout: post
title: Ruby中冒號開頭Symbol與&:to_s用法解釋
published: true
date: 2015-07-16 05:53
tags:
  - Ruby
comments: true

---
本篇介紹Ruby中特有的寫法，將解答`&:to_s`和`:symbol`這兩種寫法的意義。並依序介紹以下議題：

### Symbol和String的不同之處
分別宣告兩個symbol與string。

```
[22] pry(main)> sym = :abc
=> :abc
[23] pry(main)> str = "abc"
=> "abc"
[24] pry(main)> sym2 = :abc
=> :abc
[25] pry(main)> str2 = "abc"
=> "abc"
```

用object_id來看看是否為同一個物件。從下面結果可以發現，在參照同樣的symbol`:abc`時，不同的變數參考的是同一個物件。而String參考的物件是不同的。

```
[26] pry(main)> sym.object_id
=> 1172168
[28] pry(main)> sym2.object_id
=> 1172168
[29] pry(main)> str.object_id
=> 70232488314360
[30] pry(main)> str2.object_id
=> 70232505015640
[31] pry(main)>
```

### all_symbols 與 include? 搭配運用

在終端機輸入`Symbol.all_symbols`可以列出目前所有的Symbol。

```
[34] pry(main)> Symbol.all_symbols
=> [:freeze,
 :inspect,
 :intern,
 :object_id,
 :const_missing,
 :method_missing,
 :method_added,
 :singleton_method_added,
 :method_removed,
 :singleton_method_removed,
 :method_undefined,
 :singleton_method_undefined,
 :length,
 :size,
 :gets,
 :succ,
 :each,
 :proc,
 :lambda,
 :send,
 :__send__,
 :__attached__,
 :initialize,
 .
 .
 .
```

我們把用all_symbols取出的陣列用變數`symbols`存起來。這樣我們就可以用`include?`來檢查某個symbol是否已經宣告。


```
[23] pry(main)> all_symbols.include?(:sym)
=> true
# :sym是系統預設的所以顯示true
[24] pry(main)> all_symbols.include?(:abc)
=> false
# :abc是我亂輸入的所以顯示false
[25] pry(main)>
```

### `&:to_s`的用法與原理
以下兩個寫法效果相同，`:`代表一個Symbol，當Symbol遇上`&`符號時，會把`to_s`方法轉變為proc，而是否能轉變為proc則是看Class中有沒有`to_proc`方法，這是ruby中的duck typing特性所致。我會另外寫一篇來說明duck typing是如何運作。

```
[72] pry(main)> [1,2,3,4].map(&:to_s)
=> ["1", "2", "3", "4"]
[73] pry(main)> [1,2,3,4].map{|i| i.to_s}
=> ["1", "2", "3", "4"]
```

### String、Symbol和Integer的互相轉換

Symbol可以轉換成String

```
[74] pry(main)> :a.to_s
=> "a"
```

String可以轉換成Symbol

```
[75] pry(main)> "a".to_sym
=> :a
```

Fixnum(數字的類別)不能轉換成Symbol，會出現錯誤。

```
[76] pry(main)> 1.to_sym
NoMethodError: undefined method `to_sym' for 1:Fixnum
from (pry):98:in `__pry__'
```

## 參考資料
誘人的ruby-符號篇