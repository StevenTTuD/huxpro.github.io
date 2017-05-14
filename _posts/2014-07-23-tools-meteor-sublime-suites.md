---
author: StevenTTuD
layout: post
title: Meteor專用的Sublime套件：TernJs
published: true
date: 2014-07-23 18:31
tags:
  - Sublime
  - Text Editor
comments: true

---
終於安裝好這個Sublime套件了，安裝上有點麻煩，步驟很多，每次用的時候都要設定一下才能用。不過裝好之後爽度還是挺高的
[demo影片](https://www.youtube.com/watch?v=5cAHxpNEHTc)
[github](https://github.com/Slava/tern-meteor)
# 簡單記錄一下安裝過程
1. 從finder開啟~/Library/Application Support/Sublime Text 3/Packages資料夾
2. 把github上的meteor.js丟進這個資料夾中的TernJS/ternjs/plugin，如果沒有這三個資料夾，就創一個。
3. 打開sublime > project > save project as > 儲存project
4. sublime > project > edit project > 修改json檔案成（以官網為準）
```json
{
      "folders":
      [
               ... don't touch this part, leave it as it was ...
      ],
      // add this! ternjs object
      "ternjs": {
        "libs": ["browser", "underscore", "jquery"],
        "plugins": {
          "meteor": {}
        }
      }
}
```

# 以後要開發meteor專案時
1. 先open sublime-project
2. 把要開發的meteor專案的檔案全部加進來
3. 即可開始使用強大的ternjs套件

註：不然英打太慢關鍵字又記不清楚頭好暈阿＠＠