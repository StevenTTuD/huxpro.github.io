---
author: StevenTTuD
layout: post
title: Deploy Redmine to VPS by Rails way in Ubuntu 14.04
published: true
date: 2016-02-13 10:30
tags:
  - Rails
  - Redmine
comments: true

---
## 前言

Redmine 是一套以 Ruby 為基底的專案管理網頁應用程式。而且正好就是使用 Ruby on Rails 開發的。所以如果你是 Rails 工程師，使用一般部屬 Rails 專案的方式就可將 Redmine 架起來。

本文記錄下來的方法，是我覺得最簡單，並且沒有副作用的架設方法。如果你要架設的不是 Redmine ，而是其他的 Rails 專案，也同樣適用。

本文採用的環境：
OS: Ubuntu 14.04
VPS: Digital Ocean

## 在 Ubuntu 創造新的使用者

Linux 是非常嚴謹的系統，不同資料夾放的東西都有其規範。而 root 帳號在 Linux 系統中是超人，可以做任何的事情。為了維持 Linux 系統的乾淨度，我們要建立一個專用的使用者 oceanttd 而不是直接使用 root 帳號來部屬。這樣在有多人要同時使用同一個 Linux 作業系統的時候會減少很多麻煩。

創造部屬的帳號 `oceanttd`

```
sudo adduser oceanttd
```

把 oceanttd 帳號加到 sudo 群組[(?)](http://askubuntu.com/questions/7477/how-can-i-add-a-new-user-as-sudoer-using-the-command-line)

```
sudo adduser oceanttd sudo
```

切換到 oceanttd 帳號

```
su oceanttd
```

## 更新 Ubuntu apt-get 的套件

```
sudo apt-get update
```


## 安裝 Mysql 或是其他資料庫

```
sudo apt-get install mysql-server
```

## 安裝 RVM

輸入指令安裝 RVM，過程中可能會出現一些錯誤訊息，因為我沒有預先使用 `apt-get` 安裝需要的套件。不過不要緊，跟著系統會告訴哪些還沒裝好，並且會給你安裝的指令，跟著系統指示很快的就可以完成。

```
curl -L https://get.rvm.io | bash
```

重新登入 vps，輸入 `rvm -v` 查看 rvm 是否有裝好。


## 在 RVM 中安裝 Ruby

```
rvm install 2.2.3
```

將 ruby 2.2.3 設定成預設的 Ruby 語言，這個動作很重要一定要做，因為預設的 Ruby 會是 Ubuntu 系統中預裝的版本，換成 RVM 的版本我們才好處理 Gem 之類的安裝問題

```
rvm use 2.2.3 --default
```

檢查系統中的 ruby 是否使用 rvm 的 ruby

輸入`ruby -v`檢查版本
輸入`which ruby` 檢查路徑，路徑裡面有 rvm 的才是正確

## 安裝 Rails

記得加上 `--no-ri --no-rdoc` ，意思是不要裝文件，因為我們上網查就好了。可以省下很多時間。

```
gem install rails --no-ri --no-rdoc
```

## 把 Redmine 專案載下來，

( 如果你要部屬的是自己的專案，請 git clone 你自己的專案 )

到 Redmine 官方的[下載頁面](http://www.redmine.org/projects/redmine/wiki/Download)下載最新版本或是你想要的版本的 Redmine。我的版本是 3.2.0 有支援 markown。就以 3.2.0 為例好了，如果你想要用別的版本可以自行替換。

```
wget http://www.redmine.org/releases/redmine-3.2.0.tar.gz
```

解壓縮

```
tar xzf redmine-3.2.0.tar.gz
```

現在你有一個 Redmine 的 Rails 專案了。

## 對 Rails 專案的一些處理

bundle 一下。可能會有一些 Ubuntu 的套件沒有裝會噴錯誤。不過都還滿簡單的。

```
bundle
```

缺少 imagemagick 的話可以下下面指令。

```
$ sudo apt-get install imagemagick
$ sudo apt-get install libmagickwand-dev
```

### Rails 資料庫處理

```
rake db:create
rake db:migrate
```

建之前要更新一下 `config/database.yml` 的內容，把 VPS Server 上的 mysql 帳號密碼寫進去。

## 安裝 Passenger

```
gem install passenger --no-ri --no-rdoc
```

## 使用 Passenger 安裝 nginx

```
rvmsudo passenger-install-nginx-module
```

## 安装 Nginx init script

```
$ cd ~/
$ git clone git://github.com/jnstq/rails-nginx-passenger-ubuntu.git
$ sudo mv rails-nginx-passenger-ubuntu/nginx/nginx /etc/init.d/nginx
$ sudo chmod +x /etc/init.d/nginx
```

開機自動啟動

```
$ sudo update-rc.d nginx defaults
```

(內容來自 ruby china 文尾有連結)

## 設定 nginx.conf

打開 nginx.conf

```
sudo vim /opt/nginx/conf/nginx.conf
```

```conf
user jason; # 修改成你的系统帐号名，不然项目目录 /home/jason/www 这里没有权限
worker_processes 8; # 修改成和你 CPU 核数一样
pid /var/run/nginx.pid;

http {
  include       mime.types;
  default_type  application/octet-stream;

  client_max_body_size 50m;

  sendfile        on;

  access_log /var/log/nginx/access.log;
  error_log /var/log/nginx/error.log;

  gzip on;
  gzip_disable "msie6";

  ## ------------ 重点修改内容 --------

  server {
    # 此处用于防止其他的域名绑定到你的网站上面
    listen 80 default;
    return 403;
  }

  server {
    listen       80;
    server_name  you.host.name; # 请替换成你网站的域名
    rails_env    production;
    root         /home/jason/www/gitlab/public;
    passenger_enabled on;

    location ~ ^(/assets) {
      access_log        off;
      # 设置 assets 下面的浏览器缓存时间为最大值（由于 Rails Assets Pipline 的文件名是根据文件修改产生的 MD5 digest 文件名，所以此处可以放心开启）
      expires           max;
    }
  }

  ## ---------------------------------
}
```

重新啟動 Nginx

```
sudo /etc/init.d/nginx start
```

## 到 Rails project 下重啟 nginx

```
touch tmp/restart.txt
```

大功告成，現在你應該可以透過你的 server ip，連接到你架設的 Redmine 上了^^



## 在過程中可能遇到的錯誤
### Incomplete response received from application

打開瀏覽器看到 `Incomplete response received from application` 時。這時候我們可以去看看`/opt/nginx/logs/error.log`。發現有以下訊息：

```
*** Exception RuntimeError in Rack application object (Missing `secret_token` and `secret_key_base` for 'production' environment, set these values in `config/secrets.yml`) (process 5076, thread 0x007fd841f79d58(Worker 1)):
```

這時候我們在 secret.yml 中加入 secret_token 即可。

## 參考連結

[Deploy Ruby On Rails on Ubuntu 14.04 Trusty Tahr - GoRails](https://gorails.com/deploy/ubuntu/14.04)

[雨蒼的終端機: 如何在Ubuntu 13.04 Server上部署Ruby on Rails app](http://billy3321.blogspot.tw/2013/09/ubuntu-1304-serverruby-on-rails-app.html)

[在 Ubuntu 14.04 Server 上安装部署 Ruby on Rails 应用 - Wiki » Ruby China](https://ruby-china.org/wiki/install-rails-on-ubuntu-14-04-server)

[Blog of Ryan Bigg - Deploying a Rails application on Ubuntu: Passenger Edition](http://ryanbigg.com/2015/07/deploying-a-rails-application-on-ubuntu-passenger-edition/)

