---
layout: post
title: Rails - Sortable Table With Ajax
published: true
date: 2015-11-12 15:32
tags: []
categories: []
comments: true

---
1. 前端使用sortable
1. 後端在幫要排序的類別(這邊是Memo)加上position欄位

##原理與流程

jquery-ui 的 sortable 套件內建了 serilize ，它的功用是把 id 變成 query string 依順序回傳，所以我們只要找出規則就可以知道移動的是哪些欄位，把移動後的結果用 ajax 存進資料庫即完成拖曳的動作。


##程式結構(html)

html指定要排序的Container為`id="memo-table"`，讓js可以幫他加上`sortable()`方法。要排序的欄位加上`id="id-<%= memo.id %>"`，這樣 jquery-ui sortable 套件的 serialize 時就會以 query string 的方式把排序送到server。

```html
<p id="notice"><%= notice %></p>

<h1>Listing Memos</h1>

Query string: <span></span>

<table class="table">
  <thead>
    <tr>
      <th>Content</th>
      <th>Position</th>
      <th colspan="3"></th>
    </tr>
  </thead>
  <tbody id="memo-table">
    <% @memos.each do |memo| %>
      <tr id="id-<%= memo.id %>">
        <td><%= memo.content %></td>
        <td><%= memo.position %></td>
        <td><%= link_to 'Show', memo %></td>
        <td><%= link_to 'Edit', edit_memo_path(memo) %></td>
        <td><%= link_to 'Destroy', memo, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>

<%= link_to 'New Memo', new_memo_path %>

```

##程式結構(js)

```js
$(function() {
  $('#memo-table').sortable({
    axis: 'y',
    stop: function(event,ui) {
      var data  = $(this).sortable('serialize');
      $('span').text(data);
      $.ajax({
        data: data,
        type: 'POST',
        url: '/memos/batch_update'
      });
    }
  });
});

```

##Controller

memo_controller 中新增一個action來接這個ajax。因為要表示的是一個列表(List)中許多欄位(column)的順序，所以回傳的`params[:id]`會是一個陣列，我們把它存成變數`ids`。我們把改變過的欄位一一更新資料庫的資料。這邊我有點偷懶，沒有比對是否順序不一樣就全部存進去，不過第一次做就先用笨一點的方法，把功能做出來，之後再改進效能。

```rb
  def batch_update
    @memos = Memo.all
    ids = params[:id]
    ids.each.with_index do |id, index|
      memo = @memos[id.to_i-1]
      memo.position = index + 1
      memo.save
    end

    respond_to do |format|
      format.js {
        render nothing: true
      }
    end
  end
```

##Route

因為batch_update沒有針對特定的memo，而是一次插入多筆post，所以我把url設定為`/posts/batch_update`而不是`/posts/:id/batch_update`。

```rb
  resources :memos do
    collection do
      post :batch_update
    end
  end
```


##遇到的Bug與解決方法

###Post 422 
[javascript - POST 422 (Unprocessable Entity) in Rails? Due to the routes or the controller? - Stack Overflow](http://stackoverflow.com/questions/27098239/post-422-unprocessable-entity-in-rails-due-to-the-routes-or-the-controller)


###Ajax造成的HTTP 500 Server Error

處理 missing template

```rb
  def batch_update
   .
   .
    respond_to do |format|
      format.js {

        render nothing: true
      }
    end
  end
```

##參考資源

* [php - jQuery UI Sortable, then write order into a database - Stack Overflow](http://stackoverflow.com/questions/15633341/jquery-ui-sortable-then-write-order-into-a-database)
* [Extending the jQuery Sortable With Ajax & MYSQL](https://gist.github.com/linssen/2773872)


