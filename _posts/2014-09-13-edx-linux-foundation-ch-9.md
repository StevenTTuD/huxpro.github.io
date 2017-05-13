---
author: StevenTTuD
layout: post
title: EDX Linux Foundation Ch 9：User Environment Section 1 Account
published: true
date: 2014-09-13 07:50
tags:
  - Linux
  - EDX Linux Foundation
comments: true

---
#Section 1 Account

##1. Identifying the Current User
###who
To list the currently logged-on users, type ```who```
###whoami
To identify the current user, type ```whoami```
###who -a
Giving ```who``` the ```-a``` option will give more detailed information.

#2. Basics of Users and Groups

Linux uses groups for organizing users. Groups are collections of accounts with certain shared permissions.

###UID and GID

All Linux users are assigned a unique user ID (```uid```), which is just an integer, as well as one or more group ID’s (gid), including a default one which is the same as the user ID.



###/etc/passwd
這個檔案的構造是這樣的：每一行都代表一個帳號，有幾行就代表有幾個帳號在你的系統中！ 不過需要特別留意的是，裡頭很多帳號本來就是系統正常運作所必須要的，我們可以簡稱他為系統帳號， 例如 bin, daemon, adm, nobody 等等，這些帳號請不要隨意的殺掉他
```
ubuntu@ip-172-31-27-94:~$ head -n 4 /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
```
1. 帳號名稱：
就是帳號啦！用來對應 UID 的。例如 root 的 UID 對應就是 0 (第三欄位)；

2. 密碼：
早期 Unix 系統的密碼就是放在這欄位上！但是因為這個檔案的特性是所有的程序都能夠讀取，這樣一來很容易造成密碼資料被竊取， 因此後來就將這個欄位的密碼資料給他改放到 /etc/shadow 中了。所以這裡你會看到一個『 x 』！

3. UID：
![資料來源：鳥哥](https://lh5.googleusercontent.com/kZDKBDkeEs8mmyEsJNC78JD5tTmtdtmU72h3ZnjMt1c=w1743-h1090-no)

4. GID
這個與 /etc/group 有關！其實 /etc/group 的觀念與 /etc/passwd 差不多，只是他是用來規範群組名稱與 GID 的對應而已！
5. 使用者資訊說明欄
這個欄位基本上並沒有什麼重要用途，只是用來解釋這個帳號的意義而已
6. 家目錄
root 的家目錄在 /root
預設的使用者家目錄在 /home/yourIDname
7. Shell
當使用者登入系統後就會取得一個 Shell 來與系統的核心溝通以進行使用者的操作任務。那為何預設 shell 會使用 bash 呢？就是在這個欄位指定的囉！ 這裡比較需要注意的是，有一個 shell 可以用來替代成讓帳號無法取得 shell 環境的登入動作！那就是 /sbin/nologin 這個東西！這也可以用來製作純 pop 郵件帳號者的資料呢！

###/etc/shadow
早期的密碼也有加密過，但卻放置到 /etc/passwd 的第二個欄位上！這樣一來很容易被有心人士所竊取的， 加密過的密碼也能夠透過暴力破解法去 try and error (試誤) 找出來！因為這樣的關係，所以後來發展出將密碼移動到 /etc/shadow 這個檔案分隔開來的技術， 而且還加入很多的密碼限制參數在 /etc/shadow 裡頭呢

詳細內容請看：
[鳥哥：/etc/shadow 檔案結構](http://linux.vbird.org/linux_basic/0410accountmanager.php#shadow_file)

###/etc/group
Control of group membership is administered through the ```/etc/group``` file, which shows a list of groups and their members.
```
ubuntu@ip-172-31-27-94:~$ head -n 4 /etc/group
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
```
這個檔案每一行代表一個群組，也是以冒號『:』作為欄位的分隔符號，共分為四欄，每一欄位的意義是：

1. 群組名稱：
就是群組名稱啦！

2. 群組密碼：
通常不需要設定，這個設定通常是給『群組管理員』使用的，目前很少有這個機會設定群組管理員啦！ 同樣的，密碼已經移動到 /etc/gshadow 去，因此這個欄位只會存在一個『x』而已；

3. GID：
就是群組的 ID 啊。我們 /etc/passwd 第四個欄位使用的 GID 對應的群組名，就是由這裡對應出來的！

4. 此群組支援的帳號名稱：
我們知道一個帳號可以加入多個群組，那某個帳號想要加入此群組時，將該帳號填入這個欄位即可。 舉例來說，如果我想要讓 dmtsai 也加入 root 這個群組，那麼在第一行的最後面加上『,dmtsai』，注意不要有空格， 使成為『 root:x:0:root,dmtsai 』就可以囉～



#3. Adding and Removing Users

Only the *root user* can add and remove users and groups.

###useradd

In the simplest form an account for the new user turkey would be done with:
```
$ sudo useradd turkey
```
which by default sets the home directory to ```/home/turkey```, populates it with some basic files (copied from /etc/skel) and adds a line to /etc/passwd such as:
```
turkey:x:502:502::/home/turkey:/bin/bash
```
and sets the default shell to ```/bin/bash```

###userdel and userdel -r
Removing a user account is as easy as typing userdel turkey However, this will leave the /home/turkey directory intact. This might be useful if it is a temporary inactivation. To remove the home directory while removing the account one needs to use the ```-r``` option to userdel.

###id
Typing id with no argument gives information about the current user, as in:
```
$ id
uid=500(george) gid=500(george) groups=106(fuse),500(george)
```
If given the name of another user as an argument, id will report information about that other user.

![](https://lh5.googleusercontent.com/-DlvXntyuZuo/VB5Yp-Bv8eI/AAAAAAAAC_M/na_IAEG8CLg/w1755-h153-no/Screen%2BShot%2B2014-09-21%2Bat%2B12.46.47.png)

###usermod
所謂這『人有失手，馬有亂蹄』，您說是吧？所以囉，當然有的時候會『不小心』在 useradd 的時候加入了錯誤的設定資料。或者是，在使用 useradd 後，發現某些地方還可以進行細部修改。 此時，當然我們可以直接到 /etc/passwd 或 /etc/shadow 去修改相對應欄位的資料， 不過，Linux 也有提供相關的指令讓大家來進行帳號相關資料的微調呢～那就是 usermod 囉～

[鳥哥：usermod](http://linux.vbird.org/linux_basic/0410accountmanager.php#usermod)

#4. Adding and Removing Groups

###groupadd

Adding a new group is done with groupadd:
```
$ sudo /usr/sbin/groupadd anewgroup
```
###groupdel

The group can be removed with
```
$ sudo /usr/sbin/groupdel anewgroup
```

###groups
輸入```groups turkey```查看turkey的group
```
$groups turkey
turkey : turkey
```
得到turkey : turkey，代表預設的群組是自己。

###使用usermod修改群組
Adding a user to an already existing group is done with usermod.
```
$ sudo /usr/sbin/usermod -G anewgroup turkey
$ groups turkey
turkey: turkey anewgroup
```

-g  ：後面接初始群組，修改 /etc/passwd 的第四個欄位，亦即是 GID 的欄位！
-G  ：後面接次要群組，修改這個使用者能夠支援的群組，修改的是 /etc/group 囉～


#5. The root Account
The root account is very powerful and has full access to the system. Other operating systems often call this the administrator account; in Linux it is often called the superuser account.

###su
switch or substitute user

###sudo
sudo 可以讓你以其他用戶的身份執行指令 (通常是使用 root 的身份來執行指令)

###/etc/sudoers

sudo configuration files are stored in the ```/etc/sudoers``` file and in the ```/etc/sudoers.d/``` directory. By default, the sudoers.d directory is empty.

#6.Startup Files

###/etc
In Linux, the command shell program (generally bash)  uses one or more startup files to configure the environment. Files in the ```/etc``` directory **define global settings for all users** while Initialization files in the user's home directory can include and/or override the global settings.

###/etc/profile(login shell reading it)
When you first login to Linux, /etc/profile is read and evaluated, after which the following files are searched (if they exist) in the listed order:

1. ~/.bash_profile
2. ~/.bash_login
3. ~/.profile

The Linux login shell evaluates whatever startup file that it comes across first and ignores the rest. This means that if it finds ~/.bash_profile, it ignores ~/.bash_login and ~/.profile. Different distributions may use different startup files.

###~/.bashrc (non-login shell reading it)
However, every time you create a new shell, or terminal window, etc., you do not perform a full system login; only the ~/.bashrc file is read and evaluated.


###~/.bash_history
還記得我們在歷史命令提到過這個檔案吧？預設的情況下， 我們的歷史命令就記錄在這裡啊！而這個檔案能夠記錄幾筆資料，則與 HISTFILESIZE 這個變數有關啊。每次登入 bash 後，bash 會先讀取這個檔案，將所有的歷史指令讀入記憶體， 因此，當我們登入 bash 後就可以查知上次使用過哪些指令囉。至於更多的歷史指令， 請自行回去參考喔！

##login shell 和 no-login shell
“login shell” 代表用戶登入, 比如使用“su -“ 命令, 或者用ssh 連接到某一個服務器上, 都會使用該用戶默認shell 啟動login shell 模式.該模式下的shell會去自動執行/etc/profile和~/.profile文件,但不會執行任何的bashrc文件,所以一般再/etc/profile或者~/.profile裡我們會手動去source bashrc文件.而no-login shell 的情況是我們在終端下直接輸入bash 或者bash -c “CMD” 來啟動的shell.該模式下是不會自動去運行任何的profile 文件.
