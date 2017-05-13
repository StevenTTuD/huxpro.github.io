---
layout: post
title: "在 Centos OS 7 架設 Gitlab 筆記"
published: true
date: 2016-07-29 06:45
tags:
  - Gitlab
  - Centos
comments: true

---
## 安裝git

因為系統內建的git 是 1.8.3 版本太舊了，所以我們要先移除掉舊版的 git，並下載 2.7.4 版的 source code 重新編譯。


#### 錯誤1: Can’t locate ExtUtils/MakeMaker.pm” while compile git

安裝的途中可能會遇到類似以下的錯誤，因為官方從 source code 方式安裝的教學是使用 ubuntu 系統。所以有些套件沒法如預期的安裝，不過也不用慌張，等噴錯之後 google 一下就知道要裝什麼套件。

```
Can’t locate ExtUtils/MakeMaker.pm” while compile git
```

```
yum install perl-ExtUtils-MakeMaker -y
```

#### 錯誤2: -bash: /usr/bin/git: No such file or directory

```
-bash: /usr/bin/git: No such file or directory
```

發生這個錯誤主要問題是 git 的路徑 cache 在 `usr/bin`

輸入 `type git` 後系統會告訴你你的 git 目前快取的位置

```
type git
git is hashed (/usr/bin/git)
```

要解決這個問題你可以重開機或是清掉 git 快取

```
hash -d git
```

現在輸入`git --version`，可以看到新安裝的 git 已經可以用了

```
$git --version
git version 2.7.4
```



### 參考資料

git 安裝參考資料

[How To Install Git on CentOS 7 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-centos-7)
[Git - 安裝Git](https://git-scm.com/book/zh-tw/v1/%E9%96%8B%E5%A7%8B-%E5%AE%89%E8%A3%9DGit)
[Mad Coder's Blog | “Can’t locate ExtUtils/MakeMaker.pm” while compile git](https://madcoda.com/2013/09/cant-locate-extutilsmakemaker-pm-while-compile-git/)
[homebrew - Need help with update to latest Git using Terminal - Ask Different](http://apple.stackexchange.com/questions/162591/need-help-with-update-to-latest-git-using-terminal)