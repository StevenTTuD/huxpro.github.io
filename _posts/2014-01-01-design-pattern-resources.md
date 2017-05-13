---
layout: page
title: "Design Pattern - 資源總整理"
published: true
date: 2014-01-01 20:10
tags:
  - Design Pattern
comments: true

---

### 要學Design Pattern之前, 一定要先搞懂的物件導向基礎:

強烈建議先閱讀[91Design Pattern學習的心得](http://www.dotblogs.com.tw/hatelove/archive/2012/07/31/object-oriented-training-lsp-lkp-isp-dip-introduction.aspx)之後，再開始學習Design Pattern。文中提到學Design Pattern時必須具備下列的物件導向知識。強調Design Pattern只是幫助物件導向程式達到「高內聚、低耦合」的目的。並且需遵照設計原則來使用。

- 三大特性：[封裝、繼承、多型](http://ithelp.ithome.com.tw/question/10053333)
- 兩種抽象：[interfact、abstract](http://www.dotblogs.com.tw/hatelove/archive/2010/10/15/abstract-interface-introduction-and-compare.aspx)
- 目的：[高內聚、低耦合](http://ithelp.ithome.com.tw/question/10053842)
- SOLID 原則：[單一職責原則](http://ithelp.ithome.com.tw/question/10054102)、[開放封閉原則](http://ithelp.ithome.com.tw/question/10054337)、[里氏替換原則](http://ithelp.ithome.com.tw/question/10055044)、[最小知識原則](http://ithelp.ithome.com.tw/question/10054636)、[介面隔離原則](http://ithelp.ithome.com.tw/question/10054846)、[依賴反轉原則](http://ithelp.ithome.com.tw/question/10055368)
- 設計原則：DRY、KISS、YAGNI
- 設計方式：interface-driven、intention-driven、生成物件與使用物件分開
- 延伸閱讀：
[Kent Beck 的四個簡單程式設計原則](http://ihower.tw/blog/archives/7181)
[一些軟件設計的原則](http://blog.game2.tw/%E4%B8%80%E4%BA%9B%E8%BB%9F%E4%BB%B6%E8%A8%AD%E8%A8%88%E7%9A%84%E5%8E%9F%E5%89%87-2#.UsUW6pAW3Zg)

文中提到，濫用Design Pattern的傷害很可能比不用Design Pattern還要大。使用時須根據需求來運用Design Pattern，而非無節制的使用Patterns。

### Pin 點部落
[Design Pattern目錄](http://www.dotblogs.com.tw/pin0513/category/3265.aspx)
這是一個我很推薦的學習資源，內容以深入淺出設計模式的學習筆記為主要骨幹。深入淺出設計模式寫得很好，但稍嫌囉嗦，故事敘述往往花了很長的篇幅，因此我選擇精簡整理過的筆記，也就是這個網站的資源來學習。

### Teddy搞笑談軟工
Teddy提到坊間的書籍，如大話設計模式、設計模式之禪......等等的書籍，以範例來說明Design Patern，以實際
程式來解說Desgin Pattern。諸如此類以「操作型定義」來學習，對於入門來說是個好方法。然而由GOF所著作的Design Pattern書中提到的抽象型定義，卻沒有書籍著重在這方面。因此Teddy使用6大元素，將抽象型定義囊括其中，藉以探究Design Pattern。

* Teddy流 - 物件導向核心概念
[亂談軟體設計（1）：Cohesion and Coupling](http://teddy-chen-tw.blogspot.tw/2011/12/1.html)
[亂談軟體設計（2）：Open-Closed Principle](http://teddy-chen-tw.blogspot.tw/2011/12/2.html)
[亂談軟體設計（3）：Single-Responsibility Principle](http://teddy-chen-tw.blogspot.tw/2011/12/3.html)
[亂談軟體設計（4）：Liskov Substitution Principle](http://teddy-chen-tw.blogspot.tw/2012/01/4.html)
[亂談軟體設計（5）：Dependency-Inversion Principle](http://teddy-chen-tw.blogspot.tw/2012/01/5dependency-inversion-principle.html)
[亂談軟體設計（6）：Single Choice Principle](https://www.google.com.tw/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&ved=0CDoQFjAB&url=http%3A%2F%2Fteddy-chen-tw.blogspot.com%2F2012%2F01%2F6single-choice-principle.html&ei=8A_mUoGxF4jNkwW_2oCQDg&usg=AFQjCNHlleGb1DuNYItfrOtDbszN0pHiuQ&sig2=c9fw9g48hCient0437ihzQ)

* Teddy流 - 設計與設計模式
[設計的定義](http://teddy-chen-tw.blogspot.tw/2013/06/blog-post_25.html)
[盤點 Design Patterns ](http://teddy-chen-tw.blogspot.tw/2012/01/design-patterns.html)
[設計模式的逆襲：種子篇](http://teddy-chen-tw.blogspot.tw/2012/10/blog-post_2.html)
[Force是什麼](http://teddy-chen-tw.blogspot.tw/2012/09/force.html)

* Teddy流 - 從Force出發
[了解Force讓你做出好設計（1）：自然界與設計界的Force](http://teddy-chen-tw.blogspot.tw/2013/07/force1force.html)
[了解Force讓你做出好設計（2）：一個軟體設計範例](http://teddy-chen-tw.blogspot.tw/2013/07/force2.html)
[尋找Force實驗1：Observer篇](http://teddy-chen-tw.blogspot.tw/2012/09/forceobserver.html)
[尋找Force實驗2：State Pattern篇](http://teddy-chen-tw.blogspot.tw/2012/09/force2state-pattern.html)
[尋找Force實驗3：Adapter Pattern篇](http://teddy-chen-tw.blogspot.tw/2012/10/force3adapter-pattern.htm)
[尋找Force實驗4：Facade篇](http://teddy-chen-tw.blogspot.tw/2012/10/force4facade.html)
[尋找Force實驗5：Proxy篇](http://teddy-chen-tw.blogspot.tw/2012/10/force5proxy.html)
[尋找Force實驗6：Singleton篇](http://teddy-chen-tw.blogspot.tw/2012/10/force6singleton.html)

* Teddy流-重新整理系列
[重新整理Adapter Pattern](http://teddy-chen-tw.blogspot.tw/2013/08/adapter-pattern.html)
[重新整理Singleton Pattern](http://teddy-chen-tw.blogspot.tw/2013/08/singleton-pattern.html)
[重新整理Factory Method Pattern](http://teddy-chen-tw.blogspot.tw/2013/08/factory-method-pattern.html)
[重新整理Abstract Factory Pattern](http://teddy-chen-tw.blogspot.tw/2013/08/abstract-factory-pattern.html)
[重新整理Template Method Pattern](http://teddy-chen-tw.blogspot.tw/2013/08/template-method-pattern.html)
[重新整理Composite Pattern](http://teddy-chen-tw.blogspot.tw/2013/08/composite-pattern.html)
[重新整理Observer Pattern](http://teddy-chen-tw.blogspot.tw/2013/08/observer-pattern.html)
[重新整理Facade Pattern](http://teddy-chen-tw.blogspot.tw/2013/08/facade-pattern.html)
[重新整理Strategy Pattern](http://teddy-chen-tw.blogspot.tw/2013/08/strategy-pattern.html)
[重新整理State Pattern](http://teddy-chen-tw.blogspot.tw/2013/07/state-pattern.html)


* Teddy流-要解決什麼問題系列
[Creational Patterns要解決什麼問題(上)?](http://teddy-chen-tw.blogspot.tw/2012/10/creational-patterns.html)
[Creational Patterns要解決什麼問題(中)?](http://teddy-chen-tw.blogspot.tw/2012/11/creational-patterns.html)
[Creational Patterns要解決什麼問題(下)?](http://teddy-chen-tw.blogspot.tw/2012/11/creational-patterns_26.html)
[Structural Patterns要解決什麼問題(上)?](http://teddy-chen-tw.blogspot.tw/2012/11/structural-patterns.html)
[Behavioral Patterns要解決什麼問題(一)?](http://teddy-chen-tw.blogspot.tw/2013/03/behavioral-patterns.html)

### 91的站
 很棒的站，對於物件導向觀念一直延伸至Design Pattern，一整套的學習教材。進階版有實際範例將程式重構為Design Pattern的型式。

 [iT邦](http://ithelp.ithome.com.tw/search/search_result?p=++91%E4%B9%8BASP.NET%E7%94%B1%E6%B7%BA%E5%85%A5%E6%B7%B1+%E4%B8%8D%E8%B2%A0%E8%B2%AC%E8%AC%9B%E5%BA%A7+&field=date&page=2)
 [ASP.NET由淺入深 不負責講座系列](http://www.dotblogs.com.tw/hatelove/category/4218.aspx)
 [重構之路系列](http://www.dotblogs.com.tw/hatelove/category/5036.aspx)

### 參考書籍

* 深入淺出設計模式
最多人推薦的教材，我也不例外，從最直覺但有缺陷的方式開始思考，慢慢推導致Design Pattern的產生。並不時檢視著SOLID五大原則。非常適合打基礎。缺點是故事篇幅啥在太長，常常看到恍神。書中只講解到其中幾種的Design Pattern並非全部。

* 大話設計模式
引導讀者進入情境，讓讀者思考該用甚麼方法解決問題，類似深入淺出設計模式。優點是故事較短，很容易閱讀。缺點是有些例子對Pattern的了解有限，程式碼有些是中文，較難閱讀。

* 設計樣式的解析與活用
Teddy在網站中推薦的教材，少數有提到抽象型定義的書籍。
