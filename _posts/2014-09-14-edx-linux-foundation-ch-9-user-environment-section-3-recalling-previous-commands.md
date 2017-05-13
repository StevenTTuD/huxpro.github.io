---
layout: post
title: EDX Linux Foundation Ch 9：User Environment Section 3  Recalling Previous Commands
  &4 Command Aliases
published: true
date: 2014-09-14 09:41
tags:
  - Linux
  - EDX Linux Foundation Course
comments: true

---
#Section 3:Recalling Previous Commands
###Up and Down
 you can recall previously used commands simply by using the Up and Down cursor keys.
###histroy
 To view the list of previously executed commands, you can just type history at the command line.
###~/.bash_history.
The list of commands is displayed with the most recent command appearing last in the list. This information is stored in ~/.bash_history.

##Using History Environment Variables
**$HISTFILE** stores the location of the history file.
**$HISTFILESIZE** stores the maximum number of lines in the history file.
**$HISTSIZE** stores the maximum number of lines in the history file for the current session.
```
ubuntu@ip-172-31-27-94:~$ echo $HISTFILE
/home/ubuntu/.bash_history
ubuntu@ip-172-31-27-94:~$ echo $HISTFILESIZE
2000
ubuntu@ip-172-31-27-94:~$ echo $HISTSIZE
1000
ubuntu@ip-172-31-27-94:~$
```

##Finding and Using Previous Commands
###Up/Down arrow key
Browse through the list of commands previously executed
###!! (Pronounced as bang-bang)
Execute the previous command
###CTRL-R
Search previously used commands

##Executing Previous Commands
![](https://lh5.googleusercontent.com/-2D4GK51OD00/VB6ZUJyvS2I/AAAAAAAADAM/DpVtbXnDTBA/w1753-h628-no/Screen%2BShot%2B2014-09-21%2Bat%2B17.22.50.png)

#Section 4:Command Aliases
##Creating Aliases
You can create customized commands or modify the behavior of already existing ones by creating aliases. Most often these aliases are placed in your ``` ~/.bashrc```  file so they are available to any command shells you create.
###alias
列出所有的縮寫(alias)
###alias 縮寫＝'指令'
設定新的縮寫
###範例：
![](https://lh3.googleusercontent.com/-QFhJn8bGUes/VB6cgWaY8bI/AAAAAAAADAY/5yPSqpMnpxA/w1695-h1185-no/Screen%2BShot%2B2014-09-21%2Bat%2B17.36.55.png)