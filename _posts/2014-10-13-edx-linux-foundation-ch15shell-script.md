---
layout: post
title: EDX Linux Foundation Ch15：Shell Script ( Ch14跳過 )
published: true
date: 2014-10-13 14:30
tags: []
categories: []
comments: true

---
這是參加開源社群Tossug Linux讀書會的心得筆記，部分內容經過大大的補充，讓學習上更完整:）
Ch14講的是Printer，因為實際使用上較多Bug所以跳過這一章節，等有需要的時候再來學習。

#Shell
linux系統可分為三個重要的部份：
1.kernel
2.shell
3.application

因為kernel的部份太過低階，所以需要透過一個友善介面(interface)來讓使用者操作的時候更為方便，這個介面便是shell。涵蓋範圍包括但不限於：

CLI (Command line interface)
GUI (Graphical user interface)

常見的CLI有以下幾種。
![](https://lh4.googleusercontent.com/-Bxs23-huhzU/VDzh7zDaaII/AAAAAAAADII/sFNHHEnQwKk/w1618-h1185-no/Screen%2BShot%2B2014-10-14%2Bat%2B16.32.01.png)

sh是最古老的shell，大部分的linux系統使用上sh時，會直接使用其他shell的對應功能來取代，所以使用時需特別注意，就算語法是sh，其實實際上運作的是bash或者其他的shell如zsh...等等。

csh 用c寫shell

其他shell如zsh功能較bash強大，但是bash是一般linux系統最常見最基本的shell，所以撰寫 Shell Script 時，使用 Bash 是較好的選擇。

GUI的話，略過不提，在linux下還是學習Command Line來的踏實。

##Bash的兩種工作模式
1. interactive mode
2. shell script mode

interacitive mode 就是在終端機中一一輸入指令，等待一個指令執行完接著輸入下一個指令。而shell script mode顧名思義就是寫成.sh檔並執行裡面的shell script code，而shell script code就是由一連串的命令所組成。

##第一支bash Scripts
來試試看寫下第一個bash程式
```sh
$ cat > exscript.sh
  #!/bin/bash
  echo "HELLO"
  echo "WORLD"
```
輸入`chmod +x exscript.sh`讓檔案可以執行。

##重點整理

###1.Defensive BASH Programming
http://www.kfirlavi.com/blog/2012/11/14/defensive-bash-programming/
好的coding style可以大幅降低錯誤發生的機率。

###2.double brackets
http://mywiki.wooledge.org/BashFAQ/031
在使用判斷式的時候 (例如：if) 使用`[[]]`代替`[]`，這樣可以減少很大出錯的原因。

###3.如何實現function回傳值？
Shell Script 可視為是一種字串取代的過程。呼叫 echo 輸出參數可以就可以讓其他不同scope的函數使用該參數。

###4. shell script中的遞迴
遞迴在shell scipt中並不會佔用太多的記憶體。這是由於shell script的語言特性是一連串的字串取代。

##你不知道的shell script
Shell Script 可以 import libary。可以把程式拆成許多函式庫。
Shell Script 可以 include Module。可以把程式拆解成許多模組。
Shell Script 有 Test Unit 叫做shunit。


#參考連結
[tossug linux 讀書會 第11週](https://tossug.hackpad.com/Linux-11-10142014-b6y4uC6HCCw)