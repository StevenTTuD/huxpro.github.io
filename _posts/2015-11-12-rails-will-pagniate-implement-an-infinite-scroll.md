---
author: StevenTTuD
layout: post
title: Rails - 使用 will pagniate 搭配 ajax 實作無限捲動
published: true
date: 2015-11-12 05:29
tags:
    - Rails
    - javascript
comments: true

---
## 概念：

will paganiate 由送到 controller 的 params[:page] 決定回傳的`@posts`。

可以由我們在 controller 中定義`@posts`所知道。

```rb
@posts = Post.paginate(:page => params[:page])
```

## inifite scroll event

```js
$(window).scroll(function () {
   if ($(window).scrollTop() >= $(document).height() - $(window).height() - 10) {
      //Add something at the end of the page
   }
});
```

記得要減10，或任何你覺得適合的數，可以幫助捲動正確觸發。

[javascript - infinite-scroll jquery plugin - Stack Overflow](http://stackoverflow.com/questions/5059526/infinite-scroll-jquery-plugin)

## 程式碼 - HTML

因為我們使用 will pagniate 也就是分頁的GEM來實作，所以我們必須用 hidden field 來儲存目前捲動的最後一頁是哪一頁。image_tag 是 rails 特有的用法，會對應到 `assets/images` 資料夾下的圖片。

index.html.erb

```html
<div class="page-header">
  <h1>My Post</h1>
</div><!-- page-header -->

<div id="my-posts">
  <%= render @posts %>
</div><!-- my-posts -->

<div id="loadmoreajaxloader" style="display:none;"><center><%= image_tag "ajax-loader.gif" %></center></div>

<%= hidden_field_tag "current_page", "1" %>

```

_post.html.erb

```html
<div>
  <h2><%= link_to post.title, post %></h2>
  <small><em><%= post.timestamp %></em></small>
  <p><%= truncate(strip_tags(post.body), length: 600) %></p>
</div>

```

## 程式碼 - JS

實作了兩種觸發的方式，先完成較簡單的 click ，之後再把 click 事件替換成捲動至頁尾會觸發的事件即完成無限下拉式瀏覽。

```js
$(function () {
    // Load more
    var $loadmore = $("#loadmore");
    var $current_page = $("#current_page");
    var $my_posts = $("#my-posts");
    var addPostToList = function (post) {
        var postStri = "";
        postStri += "<div>";
        postStri += "  <h2><a href=\"#\">" + post.title + "<\/a><\/h2>";
        postStri += "  <small><em>" + post.created_at + "<\/em><\/small>";
        postStri += post.body;
        postStri += "<\/div>";
        $my_posts.append(postStri);
    };
    var addPosts = function (posts) {
        $.each(posts, function () {
            addPostToList(this);
        });
    };
    $(window).on("scroll", function () {
        if ($($(window).scrollTop() >= $(document).height() - $(window).height() - 50)) {
            var current_page = parseInt($current_page.val());
            current_page++;
            console.log("111");
            $('div#loadmoreajaxloader').show();
            $.ajax({
                url: "/posts.json",
                type: "GET",
                dataType: "json",
                data: {
                    page: current_page
                },
                success: function (data) {
                    console.log(data);
                    $current_page.val(String(current_page));
                    var posts = data;
                    $('div#loadmoreajaxloader').hide();
                    addPosts(posts);
                }
            });
        }
    });
    // var $loadmore_gif = $("a.loading-gif");
    $loadmore.click(function (e) {
        e.preventDefault();
        var current_page = parseInt($current_page.val());
        current_page++;
        $loadmore.hide();
        console.log("click");
        $.ajax({
            url: "/posts.json",
            type: "GET",
            dataType: "json",
            data: {
                page: current_page
            },
            success: function (data) {
                console.log(data);
                $current_page.val(String(current_page));
                var posts = data;
                addPosts(posts);
                $loadmore.show();
            }
        });
    });
});
(function ($) {
})(jQuery);
```


## 其他：javascript小技巧 - 過濾url獲取id

```js
var str_sub = str.substr(str.lastIndexOf("=")+1);
```

[Javascript: 標籤過濾從url獲取id - 數碼維基](http://codex.wiki/question/1933026-9917)



