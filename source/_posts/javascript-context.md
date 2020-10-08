---
title: JavaScript function context
date: 2020-10-08 16:03
tags: -javascript
---

## Javascript 的 function

今天在練習 js 的時候，一直卡在該用 this 還是 var。主要是 this 在 context 情境不同時，所指向的對象是不同的。然而這個情況在 普通 function 和箭頭函式的狀況下又有不一樣的變化。

1. arrow function 內會綁定當前 this， normal function 不會。
2. 可以利用 var 定義一個參數綁定當前的位置，進行呼叫。
3. var、this 所定義出來的變數是不同的。
4. this.variable 可以直接被外部存取、var variable 則需另外定義 getter 才行。

```javascript
function timer() {
  var self = this;
  var intervalId;
  this.intervalId = 666;

  this.normalLogFun = function () {
    // this 會因為 setInterval 而指向 window, self 則指向 timer
    console.log(this, self);

    clearInterval(intervalId);
    self.isSame();
  };
  this.arrowLogFun = () => {
    // 在 arrow function 下，this 會先行綁定當前的 context.
    console.log(this, self);

    clearInterval(intervalId);
    this.isSame();
  };
  this.start = function () {
    //把當前的 normal function 傳至 window.setInterval，所以 Context 會改變。
    intervalId = setInterval(this.normalLogFun, 1000);
  };
  this.start2 = () => {
    //把當前的 arrow function 傳至 window.setInterval，所以 Context 會改變。  (Arrow 不受 Context 改變影響)
    intervalId = setInterval(this.arrowLogFun, 1000);
    this.isSame();
  };
  this.isSame = function () {
    // setInterval 回傳的 id
    console.log(intervalId);
    console.log(this.intervalId); // 666
    console.log(window.intervalId); // undefined
    // 因為綁定的對象不同，所以是不同的東西
    console.assert(intervalId == this.intervalId, "It's different!!");
  };
}
//var t = new timer(); t.start(); t.start2()  可以使用這個測試
```
