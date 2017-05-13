---
layout: post
title: Javascript Roadtrip 3 Closure
published: true
date: 2014-07-29 04:12
tags: []
categories: []
comments: true

---
這一章講的是closure，很奇妙的東西。
[JavaScript学习总结(十六)——Javascript闭包（Closure）](http://www.cnblogs.com/xdp-gacl/p/3703876.html)
這個網站寫的不錯，看來以後要拜讀一下他的文章。

##2.1 影片
```js
￼￼function buildCoveTicketMaker( transport ) {
         return function ( name ) {
alert("Here is your transportation ticket via the " + transport + ".\n" +
      "Welcome to the Cold Closures Cove, " + name + "!");
￼￼} }
```￼

```js￼
var getSubmarineTicket = buildCoveTicketMaker("Submarine");
var getBattleshipTicket = buildCoveTicketMaker("Battleship");
var getGiantSeagullTicket = buildCoveTicketMaker("Giant Seagull");
```￼

第一次呼叫的時候先初始化transport，
因為回傳的是一個function，這時候getSubmarineTicket儲存的已經是內部的inner function，又稱為closure。
下次可以直接使用closure這個內部函式，如下

```js￼
getSubmarineTicket("Mario");
￼￼￼￼￼￼getBattleshipTicket("Luigi");
getGiantSeagullTicket("Bowser");
```￼

即會顯示出

![Screen Shot 2014-07-29 at 13.19.36.png](http://user-image.logdown.io/user/6619/blog/6590/post/211815/AprhkyhjSF6LexQr6iGl_Screen%20Shot%202014-07-29%20at%2013.19.36.png)

##2.8
呼叫closure顯示訊息。

##2.11
將location存進陣列中。這題滿值得做的，能夠利用closure來儲存陣列，給五顆星。

##2.12
比想像中的複雜，先記錄起來。
```js
function warningMaker( obstacle ){
  var count = 0;
  var zones = [];
  return function ( number, location ) {
    count++;
    var isThere = false;
    for (var ii = 0;ii<zones.length;ii++){
      if(zones[ii][0]==location){
        zones[ii][1]=zones[ii][1]+number;
        isThere = true;
        break;
      }}
      if (!isThere){
        zones.push([location,number]);
      }
    var list = "";
    for(var i = 0; i<zones.length; i++){
        list = list + "\n" + zones[i][0] +
          " (" +zones[i][1]+")";
    }

    alert("Beware! There have been " +
          obstacle +
          " sightings in the Cove today!\n" +
          number +
          " " +
          obstacle +
          "(s) spotted at the " +
          location +
          "!\n" +
          "This is Alert #" +
          count +
          " today for " +
          obstacle +
          " danger.\n" +
          "Current danger zones are: " +
          list
         );
  };
}
```

##2.15
這題先跳一下

#closure的用途
1.