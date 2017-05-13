---
author: StevenTTuD
layout: post
title: Atom殺手級套件介紹
published: true
date: 2015-07-22 07:50
tags:
  - Atom
  - Text Editor
comments: true

---
這次來介紹兩個殺手級套件，以及其他的輔助的畫面跳躍套件來完善atom快速鍵不足的地方。

##jumpy
運用他你可以快速的跳躍到畫面中程式的任何一個地方，只需按下`shift + enter`。

![](https://raw.githubusercontent.com/DavidLGoldberg/jumpy/master/_images/jumpy.gif)

如果你使用`atom-Material-UI`這個套件的話可能畫面會變得像這樣。

![](https://lh3.googleusercontent.com/9vKXOSspuvaV4E6TsUFCEaNluWj0qcFVOrCds6qR5Bs=w1326-h824-no)

這時候在選單列選擇 atom > open your style sheet，加入以下內容，即可改善。
```less
atom-text-editor::shadow .jumpy {
    &.label {
      opacity: 0.75;
      color: black;
      font-weight: bold;
    }
    &.jump {
    }
}
```

完成畫面如下。已經修好了！

![](https://lh3.googleusercontent.com/IiOtg6wwvK7g_djMrapIKf94EO52Pc-tCiMrcJsfi7I=w1196-h869-no)

你也可以編寫自己喜歡的樣式。enjoy it!

##multi-cursor
接下來介紹第二個殺手級應用`multi-cursor`，這個套件用的人不多，但是我覺得非常的實用。按下`alt + shift + cmd + up or down`就等於用滑鼠一一點擊多行，同時新增相同的內容。
![](https://camo.githubusercontent.com/c6b86d97d1f83b748a51af958dd84ed8804e1808/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f662e636c2e6c792f6974656d732f32583339334d31753147304b305a3036314f30302f6d756c74692d637572736f722e676966)

##基礎篇沒有提到「垂直跳躍」
垂直跳躍可以使用幾個套件來達到。第一個介紹的套件是`Indentation Jumper`，他的效果是在同樣位置的開頭中跳躍。

![](https://i.github-camo.com/547e228420651b4cb6aaab55af668144d7aa5b7f/68747470733a2f2f636c6f75642e67697468756275736572636f6e74656e742e636f6d2f6173736574732f3335313539312f323637333030302f63343762636661322d633065642d313165332d396332662d3365646565646164626135612e676966)

另一個介紹的套件是`line-jumper`。按下`alt + up or down`就可以跳躍10行，行數可以依照個人喜好調整。

![](https://camo.githubusercontent.com/84af716d9da695bd2cf499b5b4cb77a37b984006/68747470733a2f2f662e636c6f75642e6769746875622e636f6d2f6173736574732f36393136392f323237343535312f64623565613764362d396631642d313165332d393065372d6136366430653039613032632e676966)

配合之前基礎篇介紹的`alt+left or right`和`cmd + left or right`可以應付絕大多數寫程式需要的情況。

###小結
這篇是atom快速上手系列的第二篇，看完本篇與基礎篇之後已經能應付大多數的情況。第三篇會介紹的是如何使用Key Binding Resolver觀察快速鍵，並進而修改成自己慣用的快速鍵。



