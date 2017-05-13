---
author: StevenTTuD
layout: post
title: Atom 防止 Snippet Package 更新
published: true
date: 2014-10-29 09:57
tags:
  - Atom
  - Text Editor
comments: true

---
  使用 Snippet 最重要的針對自己的習慣客製化快捷鍵。 Atom 有許多內建的 Snippet，建議「不要」使用，因為那些設定會存在「Atom 程式」裡面，無法儲存在 .atom 資料夾中。所以一旦 Atom 版本更新之後，全部的設定都會不見。如果要使用 Snippet 請使用 Package 安裝，這樣用 git 來控管 .atom 資料夾時，就不會發生意外，全部的修改都能由 git 救回。

  回到正題，下載別人的 snippet package 後，經過客製化才會變成自己慣用的 snippet。這些自訂的 snippet ，在 package 更新後會全部不見，所以我們要防止 package 出現更新訊息，改成手動修改這些 snippet package 的更新。作法很簡單：
  以 atom-rails-snippet 來說：打開`package.json`

```
  {
{
  "name": "rails-snippets",
  "version": "1.6.0",
  "main": "./lib/rails-snippets",
  "activationEvents": [
    "rails-snippets:toggleErb"
  ],

  ......

}
```
將最上面的 name，從 "rails-snippets 改成 "my-rails-snippets，這麼一來原本會顯示的 update 訊息
![](https://lh4.googleusercontent.com/Ek3vjV7b1lSugbidKFdlXsjovF0xg6Epx_1ivAO83g4=w1518-h502-no)
就會消失如下
![](https://lh3.googleusercontent.com/-kR4FkYAcxSw/VFC9gOxVPmI/AAAAAAAADKI/dH8R_9SpKys/w1518-h454-no/Screen%2BShot%2B2014-10-29%2Bat%2B18.10.42.png)

但是在 package search 仍然可以搜尋的到
![](https://lh6.googleusercontent.com/-vBJhu5pfLLo/VFC9f_VEzGI/AAAAAAAADKM/u-nHOLV0jwI/w554-h850-no/Screen%2BShot%2B2014-10-29%2Bat%2B18.10.57.png)
還是可以對 package 修改成自己要的樣子，超級方便的。

一直在思考要如何管理自己的 snippet ，目前想到的解法是使用 package 加上用 git 來管理 .atom 資料夾，達到「備份」與「自訂」兩個重要的需求。