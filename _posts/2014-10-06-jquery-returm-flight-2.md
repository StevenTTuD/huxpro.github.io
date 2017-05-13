---
author: StevenTTuD
layout: post
title: jQuery Returm Flight Ch2：Javascript Object & Function
published: true
date: 2014-10-06 06:20
tags:
  - jQuery
  - javascript
comments: true

---
#Part 1. javascript Object
將改原本的程式重構，所有的function都變成Object的Method，增加可讀性。

這是原本的code
```js
var confirmation = {
  init: function() {
    $('.confirmation').on('click', 'button', function() {
      $.ajax('confirmation.html', { ... });
    });
    $('.confirmation').on('click', '.view-boarding-pass', function(event) { ... }); }
  };
  $(document).ready(function() {
  	confirmation.init();
  });
```
把function全部獨立出來，用this呼叫，增加可讀性。
```js
var confirmation = {
  init: function() {
    $('.confirmation').on('click', 'button', this.loadConfirmation);
    $('.confirmation').on('click', '.view-boarding-pass', this.showBoardingPass);
  },
  loadConfirmation: function() { ... }
  showBoardingPass: function(event) { ... }
};

$(document).ready(function() {
  confirmation.init();
});
```

#Part2 javascript Function
##Object vs Function
這是物件，只允許一個vacation。
```js
var vacation = {
  init: function() {
    // init vacation
}
$(document).ready(function() {
  vacation.init();
});
```
這是function，允許多個vacation。
```js
function Vacation(destination) {
  // init vacation to destination
}
$(document).ready(function() {
  var paris = new Vacation('Paris');
  var london = new Vacation('London');
});
```

