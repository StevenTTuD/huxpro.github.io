---
layout: post
title: EDX Linux Foundation Ch 9：User Environment Section 2 Environment Variables
published: true
date: 2014-09-14 09:04
tags: []
categories: []
comments: true

---

##Environment Variables
Environment variables are simply named quantities that have specific values and are understood by the command shell, such as bash. Some of these are pre-set (built-in) by the system, and others are set by the user either at the command line or within startup and other scripts. 

環境變數可以幫我們達到很多功能～包括家目錄的變換啊、提示字元的顯示啊、執行檔搜尋的路徑啊等等的， 還有很多很多啦！那麼，既然環境變數有那麼多的功能，問一下，目前我的 shell 環境中， 有多少預設的環境變數啊？我們可以利用兩個指令來查閱，分別是 env 與 export 呢！

###env和export
env是environment的簡寫，可以列出來所有的環境變數。
```
$ env
SSH_AGENT_PID=1892
GPG_AGENT_INFO=/run/user/me/keyring-Ilf3vt/gpg:0:1
TERM=xterm
SHELL=/bin/bash
...
```
如果使用 export 也會是一樣的內容～ 只不過， export 還有其他額外的功能就是了。（下面會講到）
```
$ export
declare -x COLORTERM=gnome-terminal
declare -x COMPIZ_BIN_PATH=/usr/bin /
declare -x COMPIZ_CONFIG_PROFILE=ubuntu
...
```
###set
用 set 觀察所有變數 (含環境變數與自訂變數)
```
$ set
BASH=/bin/bash
BASHOPTS=checkwinsize:cmdhist:expand_aliases:extglob:extquote:force_fignore
BASH_ALIASES=()
...
```
###環境變數與自定變數
By default, variables created within a script are only available to the current shell; child processes (sub-shells) will not have access to values that have been set or modified. Allowing child processes to see the values, requires use of the export command.

環境變數與自訂變數這兩者之間有啥差異呢？其實這兩者的差異在於**『 該變數是否會被子程序所繼續引用』**
子程序僅會繼承父程序的環境變數， 子程序不會繼承父程序的自訂變數啦！所以你在原本 bash 的自訂變數在進入了子程序後就會消失不見， 一直到你離開子程序並回到原本的父程序後，這個變數才會又出現！

##Setting Environment Variables

###Show the value of a specific variable	
```
echo $SHELL
```
###Export a new variable value	
export除了可以查看以外還可以用來設定環境變數
```
export VARIABLE=value (or VARIABLE=value; export VARIABLE)
```
###Add a variable permanently	
```
Edit ~/.bashrc and add the line export VARIABLE=value
```
Type source ```~/.bashrc``` or just ```. ~/.bashrc``` (dot ~/.bashrc); or just start a new shell by typing  ```bash```

##The HOME Variable
HOME is an environment variable that represents the home (or login) directory of the user.
輸入```echo $HOME```會顯示家目錄的路徑
輸入```cd```後面不加目錄名稱會切換到家目錄

##The PATH Variable
PATH is an ordered list of directories (the path) which is scanned when a command is given to find the appropriate program or script to run. Each directory in the path is separated by colons (:). A null (empty) directory name (or ./) indicates the current directory at any given time.

###show $Path
echo $PATH

###To prefix a private bin directory to your path:
```
$ export PATH=$HOME/bin:$PATH
$ echo $PATH
/home/me/bin:/usr/local/bin:/usr/bin:/bin/usr
```
延伸閱讀：[鳥哥：$Path](http://linux.vbird.org/linux_basic/0220filemanager.php#dir_path)

##The PS1 Variable
Prompt Statement (PS) is used to customize your prompt string in your terminal windows to display the information you want. 
用來改變終端機外觀的變數。

##The SHELL Variable
The environment variable SHELL points to the user's default command shell and contains the full pathname to the shell:
```
$ echo $SHELL
/bin/bash
```