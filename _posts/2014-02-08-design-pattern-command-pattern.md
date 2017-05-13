---
author: StevenTTuD
layout: post
title: "[Design Pattern] - Command Pattern"
published: true
date: 2014-02-08 19:33
tags: ["Design Pattern"]
comments: true

---

定義:

Encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations

粗分為兩部分:
1. 把request封裝成物件。
2. 實踐queue、日誌、以及支援復原功能。
先來介紹的是沒有undo功能的Simple Command Pattern。

## Simple Command Pattern

初步了解:
老方法，用Headfirst Design Pattern的範例熟悉一下Command Pattern。
這一章HeadFirst書上並沒有使用UML將範例畫出來，於是我稍作整理，以下是範例的UML:
![2014-02-13_095001.jpg](http://user-image.logdown.io/user/6619/blog/6590/post/178686/oGDG35GWRPmuif65ChcF_2014-02-13_095001.jpg)
圖中有幾個主要角色:
Invoker:調用者，負責調用client端需要的命令，並執行之。
Command << interface >>:透過這個介面Invoker可以調用各種實作這個介面的Command執行之。
Reciver:接收者，reciver接收到命令後執行相符合的動作。
Client:客戶端，發出命令者。

```java
public class SimpleRemoteControl {
	Command slot;

	public SimpleRemoteControl() {}

	public void setCommand(Command command) {
		slot = command;
	}

	public void buttonWasPressed() {
		slot.execute();
	}
}
```

Command:
命令接收者(reciver)做出實際行動，Class名稱就是它要reciver做的事情。例如:

```java
//LightOffCommand.java
public class LightOffCommand implements Command {
	Light light;

	public LightOffCommand(Light light) {
		this.light = light;
	}

	public void execute() {
		light.off();
	}
}
```

可以很清楚的知道，這個命令是要Light這個reciver去做開燈這件事。

從這個Simple Command pattern中可以觀察到幾個特性:
1. 原本需由Client端直接面對各種Reciver不同的API，現在只要setCommand，然後執行即可。讓Client端的程式碼更具有可讀性。
2. 要設成Command的行為，必須要有一定的相似性，通常是輸入的變數都是同一類型。如開燈與開電視機他們都不需要輸入參數。
3. 使用Command的行為盡量不要回傳值，或者回傳值即是需要的結果。簡化Client程式碼。

##Meta Command Pattern
什麼是meta command pattern?
剛剛的simple command pattern，invoker使用setCommand()方法，一次可以設定一個command對吧。
現在我們把command存在陣列中，最後再使用executeAll()方法一次執行所有儲存的命令。
來做個比較便一目了然:

```java
// SimpleCommandInvoker.java
public class SimpleCommandInvoker {

	Command command;
	public SimpleCommandInvoker() {
	}

	public void setCommand(Command command){
		this.command = command;
	}

	public void execute(){
		command.execute();
	}

}
```

```java
// MetaCommandInvoker.java
public class MetaCommandInvoker {

	List<Command> commands = new ArrayList<Command>();
  //把command儲存在List中，由executeAll一一執行。

	public MetaCommandInvoker() {
		// TODO Auto-generated constructor stub
	}

	public void setCommand(Command command){
		commands.add(command);
	}

	public void removeCommand(Command command){
		commands.remove(command);
	}

	public void executeAll(){
		for(Command c:commands){
			c.execute();
		}
	}

}
```
## Undo與NoCommand
這部分先行跳過，擇日補上，如果對復原有需要的朋友，請參閱Head First Design Pattern一書。

## 延伸思考:

1. 如果我們把Command用Abstract class或是Concrete Class實作一些方法，而非interface會怎麼樣?



2. Command Pattern or Stragery Pattern
一個情境，在登入時，能選擇google登入、facebook登入或者使用網站本身資料庫來登入，這種情形下，用Command Pattern比較好還是用Stragery Pattern比較好呢?

3. Command pattern與Facade Pattern
同樣都是在實際執行的物件和Client中間加上一個物件來降低藕合，差別在於facade必須為所有的method命名，例如開電視。而Command要使用哪一個才調用並執行，可以看得出Command Pattern適用於大量且不一定會使用的指令，而facade是讓你很明顯的看到，我這個系統有這些功能，扮演著介面的角色。

======================================================================

## 參考資源:
Head First Design Pattern
[搞笑談軟工 重新整理Command Pattern](http://teddy-chen-tw.blogspot.tw/2013/08/command-pattern.html)
[Command與Stragery Pattern的差異](http://rettamkrad.blogspot.tw/2013/06/commandstrategypattern.html)
[Rico 技術農場 Command Pattern](http://www.dotblogs.com.tw/ricochen/archive/2012/08/03/73801.aspx)