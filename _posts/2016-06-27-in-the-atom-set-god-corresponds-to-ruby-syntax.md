---
layout: post
title: "在 Atom 設定 .god 對應至 Ruby Syntax "
published: true
date: 2016-06-27 11:23
tags: []
categories: []
comments: true

---
## 簡介 God

God 是以 Ruby 撰寫而成，但不僅限於使用於執行 Ruby 檔，常見的用途是維持程式的運行使程式不中斷，萬一中斷可以自動重啟。不過本篇的重點不在於 God 的使用方法，而是在 Atom 編輯器中於如何讓`.god`可以對應到 `.rb` 的語法高亮。

這邊就是設定的方法，打開 config.cson，可以經由 menu 列的 Atom > config 開啟。在 core 的部份加上以下的設定。即可讓 `.god` 類型的檔案對應至 `.rb` 的語法高光。

```cson
"*":
  core:
    customFileTypes:
      "source.ruby": [
        "god"
      ]
```

簡單的說明一下設定方法：外層輸入已知的檔案類型，內層輸入自定義的檔案類型，即可完成。

```cson
"*":
  core:
    customFileTypes:
      "source.已存在的檔案類型": [
        "自定義檔案類型"
      ]
```

[Make file type -> language mapping locally configurable · Issue #1718 · atom/atom](https://github.com/atom/atom/issues/1718)