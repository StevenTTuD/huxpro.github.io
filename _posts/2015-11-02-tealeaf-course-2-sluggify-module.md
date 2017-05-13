---
layout: post
title: "從 Tealeaf 課程學習模組化 - Sluggify Module"
published: true
date: 2015-11-02 19:05
tags:
  - Rails
comments: true

---
因為 Post 與 Category 都的網址都需要 Sluggify 以便 SEO 的進行。所以我們把 Sluggify 模組化，讓同樣的程式碼只要寫一次就好。

###1. 建立module Sluggable，並引入之

在lib資料夾中建立一個名為`sluggable.rb`的檔案。加入`extend ActiveSupport::Concern`，這個技巧會讓模組間的耦合變得更加簡單。而一個class載入Sluggable時，會先做完include區塊中寫下的事情。

```rb
module Sluggable
  extend ActiveSupport:Concern

  include do

  end
end
```

打開`config/application.rb`加入路徑`config.autoload_paths << Rails.root.join('lib')`
> 還有另一個方法把rb檔initializers中，放在這個資料夾裡面代表app打開初始化時就會先跑過一遍。

###2. 跟sluggify有關的方法通通搬過來

接著我們要把原本model(post.rb,category.rb)跟sluggify有關的方法搬過來。

1. 把`after_validation :generate_slug!`放在include區塊。
1. 其他方法貼進model中。

這樣會出現幾個問題，接下來的步驟會解決他們並且解釋之。

```rb
module Sluggable
    extend ActiveSupport::Concern

    # executes this code when included only once
    included do
        after_validation :generate_slug!
    end

    def generate_slug
      the_slug = to_slug(title)
      post = Post.find_by slug: the_slug
      count = 2
      while post && post != self
        the_slug = append_suffix(the_slug, count)
        post = Post.find_by slug: the_slug
        count += 1
      end
      self.slug = str.downcase
    end

    def append_suffix(_the_slug, count)
      if str.split('-').last.to_i != 0
        return str.split('-').slice(0...-1).join('-') + '-' + count.to_s
      else
        return str + '-' + count.to_s
      end
    end

    def to_slug(name)
      str = name.strip
      str.gsub! /\s*[^A-Za-z0-9]\s*/, '-' # 將符號轉成"-"

      str.gsub! /-+/, '-' # 將多個"-"轉成一個"-"

      str
    end
end
```

###3. class_attribute特性新增屬性到model上

因為post與category所要轉換成網址的欄位一個是title、一個是name。所以我們必須想個方法讓module至換掉原本設定the_slug的這一行：

```rb
  def generate_slug
    the_slug = to_slug(title)
    .
    .
    .

  end
```

class_attribute這個ruby語言獨有的特性可以幫助我們解決這個問題，簡單的說class_attribute可以讓屬性繼承給子class使用。所以我們先在剛剛建立的模組`sluggable.rb`中加入

```rb
  include do
    before_save :generates_slug!
    class_attibute :slug_column
  end
```

接著在post.rb中加入



這樣一來就可以使用`post.slug_column`這個新的變數。

###4. 新增類別方法sluggable_cloumn讓model能夠初始化slug_column

```rb
module Sluggable
 .
 .
 .
  module ClassMethods
    def sluggable_column(col_name)
      self.slug_column = col_name
    end
  end
end
```

在Post中呼叫剛剛建立的`sluggable_column`方法，把title設成slug_column。

```rb
class Post
    .
    .

    sluggable_column :title

    .
    .
end
```

###5. 置換欄位

```rb
def generate_slug
    the_slug = to_slug(title)
    .
    .
    .
end
```

置換成

```rb
def generate_slug
    the_slug = to_slug(self.send(self.class.slug_column.to_sym))
    .
    .
    .
end
```

這句是什麼意思呢？以Post為例來解析一下它的意思

1. `self.class`就是Post，所以變成了`self.send(Post.slug_column.to_sym)`

1. `Post.slug_column`我們在post中設定成`slug_column: title`所以變成了`self.send("title".to_sym)`

1. title字串轉成symbol，變成`self.send(:title)`

1. self我們可以想像成一個新增的post物件`post=Post.new`，置換後`post.send(:title)`

1. `post.send(:title)`就等同於post.(:title)就等同於`post.title`，我們成功的呼叫了`post.title`屬性！


###6. 有了the_slug之後我們就可以來置換post與Post

1. Post用self.class來取代
1. post用obj來取代 => `obj = self.class.find_by slug: the_slug`

```rb
  def generate_slug!
    the_slug = to_slug(self.send(self.class.slug_column.to_sym))
   I obj = self.class.find_by slug: the_slug
    count = 2
    while obj && obj != self
      the_slug = append_suffix(the_slug, count)
      obj = self.class.find_by slug: the_slug
      count += 1
    end
    self.slug = the_slug.downcase
    # self.slug = self.title.sub(" ","-").downcase # prefer the following
    # self.slug = self.title.parameterize # rails way without gem
  end
```

###7. 完成品

lib/slugabble.rb

```rb
module Sluggable
  extend ActiveSupport::Concern

  # executes this code when included only once
  included do
    after_validation :generate_slug!
    class_attribute :slug_column
  end

  def generate_slug!
    the_slug = to_slug(self.send(self.class.slug_column.to_sym))
    obj = self.class.find_by slug: the_slug
    count = 2
    while obj && obj != self
      the_slug = append_suffix(the_slug, count)
      obj = self.class.find_by slug: the_slug
      count += 1
    end
    self.slug = the_slug.downcase
    # self.slug = self.title.sub(" ","-").downcase # prefer the following
    # self.slug = self.title.parameterize # rails way without gem
  end

  def append_suffix(str, count)
    if str.split('-').last.to_i != 0
      return str.split('-').slice(0...-1).join('-') + '-' + count.to_s

    else
      return str + '-' + count.to_s
    end
  end

  def to_slug(name)
    str = name.strip
    str.gsub! /\s*[^A-Za-z0-9]\s*/, '-'
    str.gsub! /-+/, '-'
    str.downcase
  end

  def to_param
    self.slug
  end
  module ClassMethods
    def sluggable_column(col_name)
      self.slug_column = col_name
    end
  end
end
```

Post.rb

```rb
class` Post < ActiveRecord::Base
  include Sluggable

  has_many :post_categories
  has_many :categories, through: :post_categories
  has_many :comments
  sluggable_column :title
end
```
