---
author: StevenTTuD
layout: post
title: 'Rails - 如何解決ajax沒有CSRF Key的問題 '
published: true
date: 2015-11-12 16:12
tags:
  - Rails
comments: true

---
## 錯誤訊息

當瀏覽器發生422錯誤，很有可能是缺少CSRF Key所引起的。這時候可以到 Log 確認一下是否是缺少 CSRF Key的情形。

## 方法一：用js抓mete的csrf-token

```js
$(document).ajaxSend(function(e, xhr, options) {
  var token =$j("meta[name='csrf-token']").attr("content");
  xhr.setRequestHeader("X-CSRF-Token", token);
});
```

想要學習處理csrf可以看看jquery-ujs的source code

[jquery-rails/jquery_ujs.js at master · rails/jquery-rails](https://github.com/rails/jquery-rails/blob/master/vendor/assets/javascripts/jquery_ujs.js#L69)


## 方法二：在controller加上一個方法

[[SOLVD] Rails: How Does csrf_meta_tag Work? | DevSolvd](http://devsolvd.com/questions/rails-how-does-csrf_meta_tag-work)

## 配合ajax 使用
```js
      $.ajax({
        url: "your_url",
        type: "DELETE",
        dataType: "json",
        beforeSend: function(xhr) {xhr.setRequestHeader('X-CSRF-Token', $('meta[name="csrf-token"]').attr('content'))},
        data: {id: attr.ta_attr_id },
        success: function(data) {
          console.log(data);
          attr.is_added = false;
          attr.position = undefined;
        }
      });

```

## 結論

這是在前端做的事情，所以我會選擇使用js來抓
