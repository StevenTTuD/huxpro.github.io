---
layout: post
title: "保哥 javascript 實戰課程筆記"
published: true
date: 2015-02-22 06:09
tags:
  - Javascript
  - Course
comments: true

---

##javascript 語言特性
1. javascript是個動態型別語言，無法在開發期間宣告型別，意味著javascript的單一變數可能會隨時改變型別!

1. javascript是個弱型別的語言：意味著在開發時期無法指定javascript型別。只能在執行期間檢查型別。如果真的要知道這個變數的型態，只能在執行時透過typeof的方式將該變數目前的型別使用字串顯示。

1. javascript是個物件導向程式語言，除了五種 primitive type 以外其他都是物件。這部份會在下一節詳細的說明。



##javascript 是個物件導向程式語言
這節會從javascript是物件導向語言這個觀點延伸了解javascript的型別(type)。
1. 除了數值(number) 字串(string) 布林(boolean) null undefined 這五個是原始型別(Primitive Type)外，所有的東西都是物件。
![](https://lh5.googleusercontent.com/-kJ98NBVTZi4/VOltbCHXWAI/AAAAAAAAE9I/VxitLDd70sc/w734-h522-no/JavaScriptTypes1.jpg)
(引用自[Are You Still Confuse with Data Types in JavaScript ?](http://codesupport.info/are-you-confuse-with-data-types-in-javascript-yet/))

1. Primitive Wrapper 是將 primitive type 包裝成物件， 提供了許多方便的方法。但其實primitive type也可以調用這些方法，因為在使用這些方法時，javascript運作的環境會自動將 prinitive type 轉換成 Primitive Wrapper。
1. 下圖中橢圓形的是primitive type而方形的就代表的是物件，可以看出primitive wrapper也是一種物件。
![](https://lh5.googleusercontent.com/-yhcDRauPQxU/VOlwYklxcCI/AAAAAAAAE9Y/Oapb6t3gwz4/w817-h850-no/javascript%2Bprimitive%2Bwrapper.jpg)
（引用自[What is the difference between JavaScript null and undefined? – Wintellect DevCenter](http://www.wintellect.com/devcenter/mharpur/what-is-the-difference-between-javascript-null-and-undefined))

## javascript 的物件特性

了解javascript的型別之後，接下來我們要來了解javascript的物件特性。

###1. JavaScript物件是個容器(Container)。
每個物件包含了`屬性(Property)`和`方法(Method或Function)`。
```js
var car={};           //宣告物件(Object)
car.nume = "BMW";     //name是car的屬性(Property)
car.start=function(){ //函式(Function)
    return "OK";
}
var car_name = car['name'];
```
###2. JavaScript 物件其實就是 HashMap
- 所有屬性名稱一定是字串

- 取得car的屬性name的內容有兩種取法 一種是物件取法`car.name` 和 另一種是陣列取法`car['name']`。

- 需要注意的是不能用數字當變數，所以`cat.007` is undefined ，但是妳可以使用`cat['007']`。

（這部份code school的javascript課程有更多範例，未來有時間的時候再補充。）

###3. JavaScript沒有Class概念，所以你可以不需要constructor，就能建立物件
1. javascript的constructor(建構式)就是函式，又稱建構式函式。

1. 使用 new 關鍵字，透過建構式建立物件。
```js
function person(first, last, age, eyecolor) {
    this.firstName = first;
    this.lastName = last;
    this.age = age;
    this.eyeColor = eyecolor;
}
var myFather = new person("John", "Doe", 50, "blue");
var myMother = new person("Sally", "Rally", 48, "green");
```
```js
var myFather = new person("John", "Doe", 50, "blue");
var myMother = new person("Sally", "Rally", 48, "green");
```
1. 因為javascript沒有Class概念，所以必須借助prototype的特性。

###4. javascript的物件可以不需先行宣告就可以自由擴充屬性
1. 這點是我之前學習時沒有發現的微妙之處，一般物件導向程式語言如java一定要先行宣告變數才能使用，而javascript卻不需要。
```js
//物件範例
var obj = { 'a':1, 'b':2 };
//擴增屬性
obj.c = 3;
//這時候obj就會有a, b, c三個屬性了

//刪除屬性
elete obj.b;
//這時候obj又只剩下a, c三個屬性了
```
1. 因為原始型別無法自由擴增屬性，所以針對 number,string,boolean有提供原始型別包裹方式，包裹成物件後，即可透過該物件自由擴增屬性。

1. 物件也可以透過下列的方法轉換成原始型別。`valueOf()` : 物件型別轉成原始型別。`toString()` : 物件型別轉成字串型別。

###5. 原生物件與宿主物件
javascript有兩種物件：
- 原生物件(Native)，也可以叫做內建物件，例如Array、Date或是使用者自行定義(後面會提到)
- 宿主物件(Host)，例如window物件和所有DOM物件。
- 所有物件資料都從根物件(就是window)開始連結(chain)。

怎麼去分辨這兩種物件，只要看看物件能不能在瀏覽器底下執行就可以，如果只能在瀏覽器底下執行，就是Host物件，如果在非瀏覽器的地方也可以執行，那就是Native。



## 物件、變數與型別的關係
- 物件(object)
  - 記憶體中的資料
  - 僅存在執行時期

- 變數(variable)
  - 用來儲存物件的記憶體位址(指標)
  - 在開發時期進行宣告(使用var關鍵字)

- 型別(type)
  - 用來標示物件的種類
  - 不同型別可能會有不同的預設屬性與方法

![](https://lh5.googleusercontent.com/-dtWA3IrgWbw/VOmNEZi5u8I/AAAAAAAAE98/m3Xk4tBMpzY/w577-h388-no/01333w234234willcourse.jpg)
(引用自[梅西的小腦袋](http://windwaterbo-blog.logdown.com/posts/222722-baoge-typescript-combat-development-and-javascript-javascript-basic-beliefs-review))

###範例：物件、變數與型別之間的關係
請問以下出現過幾個記憶體物件? 出現過幾個變數? 出現過幾個型別?
```js
 var a;
 a=1;
 a="a";
 a="a"+a;
```
![](https://lh6.googleusercontent.com/-CLFAmse7xK8/VOmLPzb_mAI/AAAAAAAAE9s/0h3IJXlCU8o/w673-h500-no/02willcourse.jpg)
(引用自[梅西的小腦袋](http://windwaterbo-blog.logdown.com/posts/222722-baoge-typescript-combat-development-and-javascript-javascript-basic-beliefs-review))

ANS : 5個記憶體物件、1個變數、3個型別  '

## 函式物件( funciton )

函式是個特殊的物件(其實函式本身有一個內部物件，用來存放在函式中宣告的變數、物件與屬性，只是外部無法存取該內部物件。)，有兩大特色

1. 函式是Javascript的一級物件(first-class object)
  - 可以被動態建立
  - 可以指定給變數，也可以複製給其他變數
  - 可以擁有自己的屬性或方法(如同物件的特性)

2. 提供變數的作用域(scope)，換句話說，作用域是看function範圍，而不是 {} 的符號範圍，跟C#不一樣。


###函式表示法

- 表式式
  - 具名表示
	```
  var add = function add(a, b){return a+b;};
	```
  -	匿名表示
	```
  var add = function (a, b){return a+b;};
	```
-	宣告式
  -	具名宣告
    ```
    function add(a, b){return a+b;};
    ```
  -	匿名宣告
    ```
    function (a, b){return a+b;};
    ```

使用匿名的方式，在偵錯或是效能調教時，會無法看出確切的函式位置，盡量避免。

## 後記
保哥javascript實戰開發課程中還有更多的內容，本篇學習筆記重點放在理解 javascript 的重要特性和 javascript 的物件特性，函式部分只有稍稍帶過，更詳細的筆記將會在研讀[JavaScript語言核心系列](http://www.codedata.com.tw/category/javascript/5)的時候記錄。

## 參考資料
* [JavaScript觀念整理-保哥上課心得 (1) « Coding For Fun](http://sfcer0414.logdown.com/posts/193415-javascript-concept-of-finishing-school-experience-1)
* [JavaScript觀念整理-保哥上課心得 (2) « Coding For Fun](http://sfcer0414.logdown.com/posts/194246-javascript-concept-of-finishing-school-experience-2)
* [Wilson.S.Weng: JavaScript 基礎訓練](http://l7960261.blogspot.tw/2014/01/javascript.html)
