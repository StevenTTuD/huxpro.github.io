---
layout: post
title: Atom 套件整理
published: true
date: 2015-02-02 14:35
tags:
  - Atom
  - Text Editor
comments: true

---

  `atom-color-highlight`
  在編輯器中即時顯示色碼對應的顏色。
  `atom-development-server`
  一個很強大的東西，自己玩玩看吧。
  `atom-html-preview`
  老實說我不太會用這個套件，但是很多人下載，我就跟著下載了＠＠
  `autocomplete-plus`
  可以即時提示可用snippet或是路徑的套件。
  `autocomplete-snippets`
  搭配autocomplete-plus可以提示套件內的內容。懶人可以多多使用，爽度很高。
  `bootstrap3-snippets`
  我所使用的bootstrap3-snippet
  `color-picker`
  顏色選擇器
  `docblockr`
  很好用的加強comment的套件，在block comment中按下enter就會自動多加個星號。很方便。建立block comment的快捷鍵是`/**`加上`tab`。
  `emmet`
  這就不用多做介紹了，網頁工程師必裝套件，如果不太清楚的話請google一下有很多介紹的好文。值得注意的是我會將預設的tab鍵改成慣用的按鍵，以免與snipppet相衝。
  `fancy-new-file`
  快速建立檔案的套件，可以提示目前有的資料夾路徑。
  `gist-it`
  用來將目前檔案或是選取的範圍製作成gist的工具。
  `gistom`
  可以從自己github呼叫想要使用的gist的工具。
  `git-history`
  配合tree-view使用可以顯示沒有commit過的檔案。
  `highlight-column`
  `highlight-line`
  以上兩者可以幫助寫程式對齊的問題，有時候會看到眼花很好用的XD。
  `highlight-selected`
  選取的地方highlight顯示。
  `javascript-snippets`
  `language-haml`
  `linter`
  可以當做debugger的好用工具，配合以下幾個套件使用，可以選擇性安裝你所撰寫的語言。
  `linter-csslint`
  `linter-erb`
  `linter-htmlhint`
  `linter-jshint`
  `livereload`
  可以即時更新網頁的套件。
  `minimap`
  右方出現小圖示讓我們知道目前畫面在檔案的哪一區段。
  `minimap-color-highlight`
  可以顯示目前畫面在minimap的哪一區段。
  `open-in-browser`
  可以在預設瀏覽器打開目前檔案的套件。
  `package-sync`
  創造目前package的清單，值得注意的是如果之前已經建立過了，他將不會覆蓋，要手動刪除`.atom/package.cson`檔案，在進行新增。
  `rails-navigation`
  配合vim-mode使用，可以快速切換model view controller。
  `rails-snippets`
  我慣用的rails snippet。
  `save-session`
  可以記錄目前檔案修改到哪邊的套件，跟另一套remember-session比起來的好處是啟動速度快非常多，所以我使用這套。
  `script`
  可以在atom中直接執行程式碼的工具。
  `seti-syntax`
  `seti-ui`
  我所使用的佈景主題。
  `tabs-to-spaces`
  可以將tab轉為空格。
  `vim-mode`
  可以在atom中使用vim的指令。
  `zentabs`
  限制開啟的分頁最多不超過5個。

 最後附上我的package清單，
```
	##packages.cson
  'packages': [
  'atom-color-highlight'
  'atom-development-server'
  'atom-html-preview'
  'autocomplete-plus'
  'autocomplete-snippets'
  'bootstrap3-snippets'
  'color-picker'
  'docblockr'
  'emmet'
  'fancy-new-file'
  'gist-it'
  'gistom'
  'git-history'
  'highlight-column'
  'highlight-line'
  'highlight-selected'
  'javascript-snippets'
  'language-haml'
  'linter'
  'linter-csslint'
  'linter-erb'
  'linter-htmlhint'
  'linter-jshint'
  'livereload'
  'minimap'
  'minimap-color-highlight'
  'open-in-browser'
  'package-sync'
  'rails-navigation'
  'rails-snippets'
  'save-session'
  'script'
  'seti-syntax'
  'seti-ui'
  'tabs-to-spaces'
  'vim-mode'
  'zentabs'
]
```