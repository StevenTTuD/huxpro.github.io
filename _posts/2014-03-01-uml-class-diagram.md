---
author: StevenTTuD
layout: post
title: "UML - Class Diagram"
published: true
date: 2014-03-01 08:31
tags:
  - UML
comments: true

---
Design Pattern的學習中頻繁的出現著Class Diagram，如果不仔細地了解箇中意思，將會學得很辛苦，因此特別介紹Class Diagram，也就是類別圖。UML中的專有名詞與一般寫程式的專有名詞並不完全相同，例如UML用的一般化（generalization）這個名詞就等於是物件導向語言中「繼承」。而這些UML特有的名詞經常會出現在學習的過程中，如果能夠了解，對學習來說很有幫助，因此建議熟記在Class Diagram中出現的專有名詞。

最後簡單的介紹一下UML，UML是描述物件導向程式的語言，使用圖形來表示我們設計的軟體。從需求設計實作，都可以用UML來表示。而Class Diagram描述著系統的結構，是UML中唯一可以對應實際物件導向程式的UML圖。

### 能見度(Visibility)
加在Class中的變數或是方法前面。
public:大家都可以使用。
private:只能在自己本身的Class中使用。
protected:跟自己有繼承關係的class皆可共用。
package:在同package下可以使用。
![visibility.jpg](http://user-image.logdown.io/user/6619/blog/6590/post/182932/js4O4X8QuK0zy2iNhBHe_visibility.jpg)

### 多重性(multiplicity)

### 關係
類別間的關係是最重要也是最需要釐清地部分，小小的不同意思都會差的很多，請特別注意。

** Generalization 一般化**
說明：物件導向程式中稱為Inheritance-繼承。表示繼承了父類別。
![2014-03-07_010113.jpg](http://user-image.logdown.io/user/6619/blog/6590/post/182932/4LLXgIJpR9akjqn1ExfC_2014-03-07_010113.jpg)

** Realization 實現：**
說明：物件導向程式中的Implementation-實作。表示實作了介面。
![2014-03-07_010852.jpg](http://user-image.logdown.io/user/6619/blog/6590/post/182932/fSp6zw8yRHWzosqgUnSx_2014-03-07_010852.jpg)

** dependency 相依關係**
說明：類別依賴於另一個類別所提供的功能。
從程式語言角度來看：當一個類別使用到另一個類別的方法(method)。

** associations 關聯**
說明：程式語言角度：類別之間的關聯，通常代表著一個類別的變數參考至(refer to)另一個類別。
舉例來說：

**navigable assovciations 有方向性的結合關係**
箭頭開始的那類別，擁有箭頭指向類別的參考(reference) 。

** aggregation 聚合**
說明：弱關連 - 整體(Whole)消失後，部分(part)還是繼續存在。有包含的意思，英文為is-part-of。例如：學生是學校的一部分，學校不見了，學生還是可以到別的學校讀書，並不會因為學校不見跟著消失。
![2014-03-07_011238.jpg](http://user-image.logdown.io/user/6619/blog/6590/post/182932/dojgQxczTwS8niQKmcCh_2014-03-07_011238.jpg)

** Composition**
說明：強關連 - 比聚合更強的包含關係，整體消失，部分也會跟著消失。整體-部分的生命週期是一致的。
如果電話消失了，那麼按鍵也會跟著消失。
![2014-03-07_013507.jpg](http://user-image.logdown.io/user/6619/blog/6590/post/182932/p4ghACcQleb3VDv6Aahp_2014-03-07_013507.jpg)


### 其他
role 角色
說明：箭頭附近的說明文字，代表變數的名稱。
例如：

============================================
## 參考資料：
[游峰碩老師個人網頁](http://fengyu0318.myweb.hinet.net/ood.html)