---
author: StevenTTuD
layout: post
title: "在 Mac OS X 下使用 brew 安裝 Go"
published: true
date: 2016-01-09 14:07
tags:
  - Golang
  - OSX
categories: []
comments: true

---
### 兩種安裝方法

在 Mac OS X 下安裝 Go 有兩種方法，一種是去官網下載安裝包，另一種是使用 Homebrew 來安裝，為了以後更新的便利性著想，我決定使用 Homebrew 來安裝 Golang。

*ps: 如果你還沒有 Homebrew 的話，需要先安裝 Homebrew 才能進行以下的安裝。*


### 更新 Brew 到最新狀態

打開你的 Terminal，輸入：

```
brew update && brew upgrade
```

### 安裝 Golang

使用 brew 安裝 go

```
brew install go
```

完成安裝，安裝完畢之後使用 `go run file_name` 指令時會出現 `path error` 因為我們還沒設定 $GOPATH 路徑。

### 設置 $GOPATH




#### bash

打開 bash 設定檔

```
vim ~/.bashrc
```

設定 GOPATH 參數

```
export GOPATH="${HOME}/go"
export PATH="${GOPATH}/bin:${PATH}"
```

重載參數

```
source ~/.bashrc
```

#### fish

因為我用的是 fish shell，所以也要設定 fish 的 GOPATH 路徑。輸入以下指令打開 fish 設定檔：

```
vim ~/.config/fish/config.fish
```

加上 GOPATH 參數

```
set -gx GOPATH "$HOME/Code/go"
set -gx PATH "$GOPATH/bin" $PATH
```

重載參數

```
. ~/.config/fish/config.fish
```

### 建立你的第一個 Go 程式

建立 hello.go 檔案

```
vim /tmp/hello.go
```

幫剛剛建立的檔案加上以下內容

```go
package main

import "fmt"

func main() {
  fmt.Println("Hello, World!");
}
```

儲存後在 terminal 中輸入`go run /tmp/hello.gov`

恭喜！ 你可以在 OS X 內使用 Golang 來開發了。



#### 參考資料

[Mac下安装Go和配置相应环境 | Arron.y's blog](http://blog.helloarron.com/2015/08/29/go/mac-install-go/)

[Getting Started with Golang on OS X](https://coolaj86.com/articles/getting-started-with-golang-osx/)