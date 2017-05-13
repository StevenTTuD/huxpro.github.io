---
layout: post
title: jQuery vs Ruby - 取得倒數的元素並組成字串
published: true
date: 2016-05-31 03:55
tags: []
categories: []
comments: true

---
## 摘要

本篇利用把陣列轉成字串這個題目，探討 jquery 和 ruby 中，map 和 join 行為的差異。

### 1. 在 jQuery 中可以用類似 Ruby 的方式取得尾部的倒數第幾個元素。

```js
$('span[itemprop=title]').eq(-2)
```

對照 ruby 語法

```rb
arr=[1,2,3,4,5,6]
arr[-2]
=> 5
```


### 2. 也可以取出倒數的三個元素

剛剛取出的是倒數的`第二個`元素，而現在我們要把倒數的三個元素都取出來

```js
$('span[itemprop=title]').slice(-3)
```

對照的 ruby 語法

```rb
arr=[1,2,3,4,5,6]
arr[-3..-1]
=> [4, 5, 6]
```


### 3.1 取出元素中的text後，存到陣列中

先把要的字串存到陣列裡面

```js
arr = [];
$('span[itemprop=title]').slice(-3).each(function(i,f){ 
	arr.push( $(f).text()) 
});
```

### 3.2 反轉陣列內容並用逗號 join

這邊是純 js 陣列的操作。

```js
arr.reverse().join(",");
```

### 4. 另一種方法：使用 map 加上 join 

```js
arr = [];
$('span[itemprop=title]').slice(-3).map(function(i,f){ 
	return $(f).text();
}).get().join(",") ;
```

跟 Ruby 的 map 有些許不同：

1. 需要 return 處理過後的值。
2. map完的陣列型態會`是預設的jquery物件集合而不是Array`。需要先使用 get() 取得 return 的值，再進行 join 。

如果你在 ruby 中使用 return，會出現錯誤。

```rb
[9] pry(main)> ["1", "2", "2"].map{|x| return x.to_i }
LocalJumpError: unexpected return
```

直接寫出要的結果就沒問題了

```rb
["1", "2", "2"].map{|x| x.to_i + 1 }
=> [2, 3, 3]
```

因為輸出的結果是 array，所以直接 join 即得串聯後的字串。

```rb
[13] pry(main)> ["1", "2", "2"].map{|x| x.to_i + 1 }.join(",")
=> "2,3,3"
```