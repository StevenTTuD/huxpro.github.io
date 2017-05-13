---
layout: post
title: Discover meteor Ch4：Collection
published: true
date: 2014-08-05 03:04
tags:
  - Meteor
  - Javascript
comments: true

---
1. 新增一個collection
```
collections/posts.js
```
```js
Posts = new Meteor.Collection('posts');
```
除了client與server以外的資料夾會在兩者都執行。
所以Posts collection在client和server都可以使用。
這裡的Posts前面不加var，這樣整個app都可以存取到Posts。

2. Commit 4-1
```
git commit -m "Added a posts collection"
```

#Collection的性質
On the server, the collection has the job of talking to the Mongo database, and reading and writing any changes. In this sense, it can be compared to a standard database library. On the client however, the collection is a secure copy of a subset of the real, canonical collection. The client-side collection is constantly and (mostly) transparently kept up to date with that subset in real-time.

##Server-Side Collection
On the server, the collection acts as an API into your Mongo database.

##Client-Side Collection
client side Mongo稱為MiniMongo，如字面所述，它不支援所有的Mongo的特徵。
###三個理解重點
1. we talk about a client-side collections being a “cache”, we mean it in the sense that it contains a subset of your data, and offers very quick access to this data.
2. In general, a client side collection consists of a subset of all the documents stored in the Mongo collection
3. Secondly, those documents are stored in browser memory, which means that accessing them is basically instantaneous.

> Mongo on Meteor.com
當你的app deploy上meteor之後，輸入```meteor mongo myApp```就可以存取網路上的mongodb。

#正式開始
1. 輸入meteor reset重置資料庫。確保資料庫是乾淨的。
2. 建立fixture.js
```
server/fixtures.js
```
```js
if (Posts.find().count() === 0) {
      Posts.insert({
        title: 'Introducing Telescope',
        author: 'Sacha Greif',
        url: 'http://sachagreif.com/introducing-telescope/'
      });
      Posts.insert({
        title: 'Meteor',
        author: 'Tom Coleman', url: 'http://meteor.com'
      });
      Posts.insert({
        title: 'The Meteor Book', author: 'Tom Coleman',
        url: 'http://themeteorbook.com'
      });
}
```
3. Commit 4-2
Added data to the posts collection.

4. 修改posts_list.js
```
client/views/posts/posts_list.js
```
```js
Template.postsList.helpers({ posts: function() {
        return Posts.find();

      }
});
```

5. Commit 4-3
```
git commit -m "Wired collection into postsList template."
```
> Inspecting DOM Changes這邊看不太懂

6.  刪除autopulish這個package
```
$ meteor remove autopublish
```

7. 新增publication.js
```
server/publications.js
```
```js
Meteor.publish('posts', function() {
  		return Posts.find();
});
```

8. 在main.js中subscribe posts
```
client/main.js
```
```js
Meteor.subscribe('posts');
```

9. commit 4-4
Removed autopublich and set up a basic publication.

還是不懂在幹嘛？ 接著要解釋的就是publication與subscription

#4.5 Publications and Subscriptions

##Rails App的做法
1. 當user造訪你的網頁，client送出一個request給server。
2. 你的app的第一個工作是找到user所需要的資料。
3. 當找到以後，server會把資料轉換成人類可讀的html。
4. 最後server會將html送給瀏覽器，也就是client。
5. 這樣一個動作就完成了，你的app繼續等待下一個request。

##Meteor的做法
根據剛剛的介紹可以了解到Rails app只能在server處理資訊。而Meteor在Client也就是你的瀏覽器中就可以處理資訊。
這就像書店的店員不只是針對你的需求把書給你，而且他還跟著你回家把書的內容讀給妳聽。

###This has two big implications:
first, instead of sending HTML code to the client, a Meteor app will send the actual, raw data and let the client deal with it (data on the wire).
Second, you'll be able to access that data instantaneously without having to wait for a round-trip to the server (latency compensation).

##Publishing
We'll need a way to tell Meteor which subset of data can be sent to the client, and we'll accomplish this through a publication.

##Whats is DDP?
這種利用publication/subscription系統的protocol，就稱為Distributed Data Protocol。

##Subscribing
###how you make a meteor app scalable in client-side
1. 修改publication(server)
```js
Meteor.publish('posts', function(author) {
  		return Posts.find({flagged: false, author: author});
});
```


2. 在client subscribe
```js
// on the client
Meteor.subscribe('posts', 'bob-smith');
```

##Autopulish
Automatically mirroring all data from the server on the client

###實際運作
如果你去看[Publish and subscribe](http://docs.meteor.com/#publishandsubscribe)的說明，你會發現實際使用的方法並不是那麼的簡潔，那是因為meteor提供了一個方變的method叫做_publishCursor()。當你在return一個cursor的時候，就會使用到它（例如：  return Posts.find({flagged: false, author: author});）。
###_publishCursor() 做了哪些事？
- It checks the name of the server-side collection.
- It pulls all matching documents from the cursor and sends it into a client-side collection of the same name. (It uses .added() to do this).
- Whenever a document is added, removed or changed, it sends those changes down to the client-side collection. (It uses .observe() on the cursor and .added(), .changed() and removed() to do this).