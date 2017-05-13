---
layout: post
title: "細讀 Bootstrap 3 文件"
published: true
date: 2015-03-15 03:54
tags:
  - bootstrap
comments: true

---
使用Bootstrap好一段時間了，卻沒有好好的把官方文件讀過一遍，雖然寫的出來但是速度不盡理想，所以這兩天花些時間將幾個不太理解的常用元件與一些以前有使用到卻不太了解的data-attribute用法寫下筆記，好提高生產力。

##Part 1: Navbar

這是一個bootstrap官網上的完整navbar範例。
```html
<nav class="navbar navbar-default|navbar-inverse">
  <div class="container-fluid">
    <!-- Brand and toggle get grouped for better mobile display -->
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="#">Brand</a>
    </div>

    <!-- Collect the nav links, forms, and other content for toggling -->
    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      <ul class="nav navbar-nav">
        <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu" role="menu">
            <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li class="divider"></li>
            <li><a href="#">Separated link</a></li>
            <li class="divider"></li>
            <li><a href="#">One more separated link</a></li>
          </ul>
        </li>
      </ul>
      <form class="navbar-form navbar-left" role="search">
        <div class="form-group">
          <input type="text" class="form-control" placeholder="Search">
        </div>
        <button type="submit" class="btn btn-default">Submit</button>
      </form>
      <ul class="nav navbar-nav navbar-right">
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu" role="menu">
            <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li class="divider"></li>
            <li><a href="#">Separated link</a></li>
          </ul>
        </li>
      </ul>
    </div><!-- /.navbar-collapse -->
  </div><!-- /.container-fluid -->
</nav>
```


####外部的nav
navbar的外部由nav包覆，但是這樣的作法其實不太妥當，因為有時候navbar中的元素並不完全是用來導覽(navigate)整個網站。所以我們將範例修改成使用`div`配合`role="navigation"`來避免這個問題。

```html
<nav class="navbar navbar-default|navbar-inverse">
  <div class="container-fluid">

    .
    .
    .

  </div>
</nav>
```

> Use `.container-fluid` for a full width container, spanning the entire width of your viewport.
>

####Brand與手機版本的元素
除了Brand以外的上半部程式碼顯示的是手機版本的畫面。
```html
<!-- Brand and toggle get grouped for better mobile display -->
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>

      <!-- Brand 在這-->
      <a class="navbar-brand" href="#">Brand</a>
    </div>
```


####容器
navbar的容器有幾種：
1. `div.navbar-header`裡面常裝brand與手機版網頁元素
2. `ul.navbar-nav`
3. `form.navbar-form` ：在navbar中可以使用form，加上 `.navbar-form`可以讓form垂直置中。

```html
	<div class="navbar-header">

	    .
	    .
	    .

	</div>

<!-- Collect the nav links, forms, and other content for toggling -->
    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">

      <!-- 我是ul.navbar-nav -->
      <ul class="nav navbar-nav">
        .
        .
        .
      </ul>

      <!-- 這是一個nav-form -->
      <form class="navbar-form navbar-left" role="search">
        <div class="form-group">
          <input type="text" class="form-control" placeholder="Search">
        </div>
        <button type="submit" class="btn btn-default">Submit</button>

      <!-- 向右對齊的非依序清單 -->
      <ul class="nav navbar-nav navbar-right">
        .
        .
        .
      </ul>
    </div><!-- /.navbar-collapse -->
  </div><!-- /.container-fluid -->
```
最外層`div class="collapse navbar-collapse"`讓這個navbar套用了reponsive design。reponsive navbar必須要有collapse plugin，不過不用擔心，bootstrap已經內建了collapse js plugin。


###容器一：navbar-nav

`ul.navbar-nav`本質上是unorder list ( ul )，裡面可以裝的元素必須為List item( li ) 。裡面可以裝：
1. Link
2. Dropdown

link的例子如下：內部可以加上連結
```html
<li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
```
dropdown本身是unorder list，因為他屬於`ul.navbar-nav`的其中之一個選項，所以外層必須由`li`包覆。形成巢狀。
```html
   <ul class="nav navbar-nav">
        <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu" role="menu">
            <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li class="divider"></li>
            <li><a href="#">Separated link</a></li>
            <li class="divider"></li>
            <li><a href="#">One more separated link</a></li>
          </ul>
        </li>
      </ul>
```

###容器二：navbar-form
form的使用跟一般的form是相同的，特別注意的是加上`.navbar-form`讓form在navbar中可以垂直置中。
```html
      <form class="navbar-form navbar-left" role="search">
        <div class="form-group">
          <input type="text" class="form-control" placeholder="Search">
        </div>
        <button type="submit" class="btn btn-default">Submit</button>
      </form>
```
詳細form的使用方法會開另一篇來做記錄。

###配合Snippet加快開發效率

我使用的是 atom-bootstrap3 和 bootstrap-3-snippetset 這兩個package。

1. 使用指令`navbar`建構出navbar雛形 ，包含brand、navbar-form。
2. 要加上dropdwon時使用指令`navbar-dropdown`
3. 各式各樣input :
	- `radiobs`
	- `checkboxbs`
	- `btn`
	- `textareabs`
	- `selectbs`

####參考資料：
[官方Doc - navbar](http://getbootstrap.com/components/#navbar)

##Part 2: Form

`input type="text"`常跟label成對出現，並且用form-group裝起來。form-control讓input有百分之百的長度，form-group則讓form的空間看起來更合適。
```html
<div class="form-group">
    <label for="recipient-name" class="control-label">Recipient:</label>
    <input type="text" class="form-control" id="recipient-name">
</div>
```
checkbox因為不需要佔有一整行的空間，所以也不需要加上`.form-group`調整表單間隔。
```html
 <div class="checkbox">
    <label>
      <input type="checkbox"> Remember me
    </label>
  </div>
```
同理radio也是一樣
```html
<div class="radio">
  <label>
    <input type="radio" name="optionsRadios" id="optionsRadios1" value="option1" checked>
    Option one is this and that&mdash;be sure to include why it's great
  </label>
</div>
```
select加上form-control可以讓選項延伸到該列的100%
```html
<select class="form-control">
  <option>1</option>
  <option>2</option>
  <option>3</option>
  <option>4</option>
  <option>5</option>
</select>
```
需要文字說明的時候，也可以搭配form-group使用
```html
    <div class="form-group">
      <label for="disabledSelect">Disabled select menu</label>
      <select id="disabledSelect" class="form-control">
        <option>Disabled select</option>
      </select>
    </div>
```
form的最後使用submit，使用class修飾外觀。
```html
<button type="submit" class="btn btn-default">Send invitation</button>
```

###水平形式的form
要達成水平的form可以用兩種方式，第一種是form-inline。缺點是不能自行設定label的長度。
```html
<form class="form-inline">
  <div class="form-group">
    <label for="exampleInputName2">Name</label>
    <input type="text" class="form-control" id="exampleInputName2" placeholder="Jane Doe">
  </div>
  <div class="form-group">
    <label for="exampleInputEmail2">Email</label>
    <input type="email" class="form-control" id="exampleInputEmail2" placeholder="jane.doe@example.com">
  </div>
  <button type="submit" class="btn btn-default">Send invitation</button>
</form>
```
另一種方式是使用Horizontal form便可以對label和input使用grid system。
```html
<form class="form-horizontal">
  <div class="form-group">
    <label for="inputEmail3" class="col-sm-2 control-label">Email</label>
    <div class="col-sm-10">
      <input type="email" class="form-control" id="inputEmail3" placeholder="Email">
    </div>
  </div>
  <div class="form-group">
    <label for="inputPassword3" class="col-sm-2 control-label">Password</label>
    <div class="col-sm-10">
      <input type="password" class="form-control" id="inputPassword3" placeholder="Password">
    </div>
  </div>
  <div class="form-group">
    <div class="col-sm-offset-2 col-sm-10">
      <div class="checkbox">
        <label>
          <input type="checkbox"> Remember me
        </label>
      </div>
    </div>
  </div>
  <div class="form-group">
    <div class="col-sm-offset-2 col-sm-10">
      <button type="submit" class="btn btn-default">Sign in</button>
    </div>
  </div>
</form>
```
這樣子基本的需求大概都可以cover到了。CSS class的查詢就直接看官方文件。

####參考資料
http://getbootstrap.com/css/#forms

##Part 3: JS Libarays

###Carousel

HTML分為三個部分：
1. indecator : 下面用來代表目前頁面的小圈圈
2. wrapper : 投影片資料放這裡面
3. Controls : prev 和 next
操作上需注意，投影片最外層需要加上`data-ride="carousel"`套用carousel javascript plugin。
```html
<div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
     .
     .
     .
</div>
```
indecator和controls需要跟外層的`#carousel-example-generic"`一致。
```html
<!-- Indicators -->
  <ol class="carousel-indicators">
      <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
      <li data-target="#carousel-example-generic" data-slide-to="1"></li>
      <li data-target="#carousel-example-generic" data-slide-to="2"></li>
  </ol>

<!-- Wrapper for slides -->
      .
      .
      .

<!-- Controls -->
      <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
            <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
            <span class="sr-only">Previous</span>
      </a>
      <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
          <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
          <span class="sr-only">Next</span>
      </a>
</div>
```



###modal
```html
<!-- Button trigger modal -->
<button type="button" class="btn btn-primary btn-lg" data-toggle="modal" data-target="#myModal">
  Launch demo modal
</button>
```
`data-toggle="modal"`代表這個button是用來觸發modal。`data-target="#myModal"`則指定了要用id為myModal的元素來作為跳出的視窗。完整範例如下：
```html
<!-- Modal -->
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title" id="myModalLabel">Modal title</h4>
      </div>
      <div class="modal-body">
        ...
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
```

###Collapse
```html
<a class="btn btn-primary" data-toggle="collapse" href="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
  Link with href
</a>

<div class="collapse" id="collapseExample">
  <div class="well">
    ...
  </div>
</div>
```
bootstrap使用data-attribute的方式來綁定js plugin，讓我們不需撰寫js檔也能使用javascript效果。每個tag中可以有一個`data-`開頭的屬性，根據使用的狀況不同`data-`後面接的會有所不同，Collapse用來展開或收起區域，所以後面接的是toggle字樣。(如果對toggle這個單字不太了解，可以玩玩看jQuery的toggle()方法。)`data-toggle`除了用在modal以外，也會使用在同樣有展開/收起特性的dropdown。來看看dropdown的button：
```html
<button id="dLabel" type="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
```
button中的`data-toggle="dropdown"`代表這個button觸發的會是dropdown menu。

Collapse的第一個例子中的`aria-controls="collapseExample"`輸入的「要跳出文字的id」。如此一來簡簡單單的完成了一個效果。



##ScrollSpy
Scrollspy可以讓你滾動到哪個div時，就讓navbar選擇到相對應的button。使用方式除了需要在html中加上`data-spy="scroll"`以外，css必須對body使用`position: relative`才能夠使用。css檔長這樣：
```css
body {
  position: relative;
}
```
html檔者這樣：
```html
<body data-spy="scroll" data-target=".navbar-example">
  ...
  <div class="navbar-example">
    <ul class="nav nav-tabs" role="tablist">
      ...
    </ul>
  </div>
  ...
</body>
```
如果不想使用data-attribute的方式，也可以透過javascript來使用。
```js
$('body').scrollspy({ target: '.navbar-example' })
```

###jumbto
```html
  <div class="form-group">
    <div class="col-sm-offset-2 col-sm-10">
      <button type="submit" class="btn btn-default">Sign in</button>
    </div>
  </div>
```

###Cheetsheet
http://getbootstrap.com/css/#buttons
http://getbootstrap.com/css/#tables
http://getbootstrap.com/css/#helper-classes
http://getbootstrap.com/components/#dropdowns
bootstrap 3 不使用 pull-right 而使用dropdown-menu-right
http://getbootstrap.com/components/#jumbotron
http://getbootstrap.com/components/#thumbnails
http://getbootstrap.com/javascript/#modals
http://getbootstrap.com/components/#pagination

##其他

###常見的data-attribute
1. Hide an element to all devices except screen readers with `.sr-only.`
2.  All textual `<input>`, `<textarea>`, and `<select>` elements with `.form-control` are set to width: 100%; by default. Wrap labels and controls in `.form-group` for optimum spacing.

3. 要使用sass開發的話有bootstrap for sass。


###Tag role
甚麼時候要用到role，根據官方文件的說法，當你所編寫的 tag 代表的意義已不符合本身的 default implicit roles，那你便需要再加上 role 屬性來說明其正確用途。而在W3C的官方文件中也載明了一個表格(見參考資料2 標題 Recommended ARIA usage by HTML language feature) 其中詳列的各 HTML Tag 的預設隱性 role，以及其可支援的其他 role

(引用自 [ARIA role 相關筆記 « Lobster 亂七八糟筆記](http://lobster0429.logdown.com/posts/144753-aria-role-related-notes) )


###container
Use `.container` for a responsive fixed width container.

```html
<div class="container">
  ...
</div>
```
Use `.container-fluid` for a full width container, spanning the entire width of your viewport.
```html
<div class="container-fluid">
  ...
</div>
```