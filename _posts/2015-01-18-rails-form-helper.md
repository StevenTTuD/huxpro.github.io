---
layout: post
title: Rails - 拆解 Form Helper 以 Checkbox 為例
published: true
date: 2015-01-18 05:25
tags:
  - Rails
comments: true

---
使用Rails Form Helper時，很重要的一點是要知道Form Helper會產生什麼樣的Html Code，了解之，觀察之，這樣你才能修改成自己需要的樣子。如果要在form中加入check box，可以使用collection_check_boxes這個在rails 4新加入的form helper來完成。我們就以checkbox為例，來練習觀察html吧！

##Collection Check Box
第一個來使用到的是collection_check_boxes，完整的參數是`collection_check_boxes(object, method, collection, value_method, text_method, options = {}, html_options = {}, &block)`，一看覺得眼花撩亂，沒關係，直接來看例子。首先我們有兩個model，分別為Post和Cateogry，他們是多對多的關係。現在我們想要在新增Post的時候可以用checkbox選擇Category。

```erb
<%= f.label "Categories" %>
<%= f.collection_check_boxes :category_ids, Category.all, :id, :name do |cb| %>
		<%= cb.label( class:"checkbox inline"){cb.check_box(class:"checkbox")+cb.text} %>
<% end %>
```
產生的html是
```html
<input id="post_category_ids_1" name="post[category_ids][]" type="checkbox" value="1">
<label for="post_category_ids_1">food</label>
<input id="post_category_ids_2" name="post[category_ids][]" type="checkbox" value="2">
<label for="post_category_ids_2">Play</label><input name="post[category_ids][]" type="hidden" value="">
```
使用binding.pry來看看create送出的params有哪些東西。

![](https://lh5.googleusercontent.com/prEm9rc0sIHt53jGcgC-dy7Ej3hjESh1-XymMINIzWE=w1743-h143-no)

用binding pry把form傳出的資訊抓出來看可以發現，params中傳送的key正是使用category_ids來做為名稱，因為checkbox的name都是相同的，所以只會有一個key(這邊不懂的話請去了解一下checkbox的html與select的html有什麼不一樣)。而value就是選擇的選項，因為我在form中勾選了第一個選項，所以顯示為"1"。label的for屬性，代表這是給哪一個專屬的`id`所使用的label，for與id常常搭配使用。

##手刻html版的checkbox
知道會產生怎麼樣的form和知道會傳出怎麼樣的params以後我就可以手刻一個html版的checkbox。
```erb
    <% Category.all.each do |category| %>
      <input id="post_category_ids_<%= category.id %>" name="post[category_ids][<%= category.id %>]"
            type="checkbox" value="<%= category.id %>" />
      <label for="post_category_ids_<%= category.id %>"><%= category.name %></label>
    <% end  %>
```
刻完之後更加了解了form中attribute的涵意(每個都要用手打阿)，在ruby on rails還沒有form helper時也是得要用手刻，這樣做的好處是可以對html更加的印象深刻，當然，如果閉著眼睛都記得這些tag怎麼使用了，再來使用form helper也不遲:)