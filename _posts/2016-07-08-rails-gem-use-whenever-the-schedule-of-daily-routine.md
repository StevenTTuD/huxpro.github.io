---
layout: post
title: 'Rails: 使用 Whenever 產生 Cronjob '
published: true
date: 2016-07-08 04:53
tags: []
categories: []
comments: true

---
### Part 1 - Crontab 介紹

Crontab 是 Linux 中用來管理例行事務的程式，而 whenever 實際上的作用就是用來產生 cronjob 的，所以先介紹一下 Linux 系統中的 crontab 如何操作。

### crontab 指令

比較常用指令的有以下幾項：

`crontab -l` 顯示目前排程 (list cron)
`crontab -e` 編輯排程 (edit cron)
`crontab -r` 移除排程 (remove cron)
`crontab -u` 改變排程的執行身分: crontab -u user filename

### crontab 的欄位與對應的意義

整個指令會長得像樣子，這個指令做的事情是先移動到某個資料夾後執行`User.delete_three_day_ago!`。

```
0 4 * * * /bin/bash -l -c 'cd /Users/oceanttd/rails_whenerver && bundle exec bin/rails runner -e development '\''Users.delete_three_day_ago!'\'''
```

第一到六項分別是：

1. 分鐘 1～59
2. 小時 0～23 
3. 日 1～31
4. 月 1～12
5. 0～6（0表示星期天）
6. 要運行的命令

需要注意的是`6`要運行的命令必須輸入`絕對路徑`，輸入相對路徑是沒有任何效果的。

#### 使用 crontab 產生器產生 cronjob 

看完上面規則後一定覺得很難撰寫，不直覺。來推薦一個網站 [crontab.guru - the cron schedule expression editor](http://crontab.guru/)輸入 cronjob 後可以立即顯示正確時間。

### Part 2 - 使用 Whenver 產生 cronjob

Whenever 是一個 Ruby 的 Gem，沒錯，你不在 Rails 環境下也可以使用。他的功能是讓 cronjob 變得很好撰寫。

#### 安裝

在 Gemfile 中加入`whenever`

或是輸入`gem install whenever` 

#### 初始化

移動到 Rails 資料夾輸入  `wheneverize .`，會幫你建立`config/schedule.rb`，如果你要建在非 Rails 的資料夾，可能需要自己創一個 config 資料夾來避免發生錯誤。

#### 把 whenever 中的內內容轉為 crontab 

後面可以指定環境

```
whenever --update-crontab --set environment=development
```

接著用`crontab -l`指令就可以查看創建的 cronjobs。

#### 來看看 whenever 怎麼撰寫

原本難以閱讀的 crontab 與 cronjob，變得很容易閱讀，很且可以跟 rake 或是 ActiveRecord 一起作用。這邊在 Rails 環境下為例。他的原理就是先幫你 cd 到 Rails 資料夾後，再執行 Rake 指令，或是執行 model 的方法。

rake 就直接用 `rake` 指令執行就好

```rb
# run this task only on servers with the :app role in Capistrano
# see Capistrano roles section below
every :day, :at => '12:20am', :roles => [:app] do
  rake "app_server:task"
end
```

```rb
every 3.hours do
  runner "MyModel.some_process"
  rake "my:rake:task"
  command "/usr/bin/my_great_command"
end
```

如果是執行 model 方法要用 `runner` 

```rb
every 1.day, :at => '4:30 am' do
  runner "MyModel.task_to_run_at_four_thirty_in_the_morning"
end

every :hour do # Many shortcuts available: :hour, :day, :month, :year, :reboot
  runner "SomeModel.ladeeda"
end

every :sunday, :at => '12pm' do # Use any day of the week or :weekend, :weekday
  runner "Task.do_something_great"
end
```

call system

```rb
every '0 0 27-31 * *' do
  command "echo 'you can use raw cron syntax too'"
end
```

如此一來 cronjob 是不是變得很好寫了呢:D
 

### 參考資料

[javan/whenever: Cron jobs in Ruby](https://github.com/javan/whenever)
[Linux / UNIX Crontab File Location](http://www.cyberciti.biz/faq/where-is-the-crontab-file/)