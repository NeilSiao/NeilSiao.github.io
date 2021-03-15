---
title: JavaScript 陣列操作
date: 2021-01-26 11:24
tags: javascript
---

## Javascript Array 筆記

紀律常常會使用到的陣列操作，方便開發。

```javascript
var array = [];
var arrayObject = new Array();
//判斷空陣列
array == 0; //true
arrayObject == 0; //true

array === 0; //false
arrayObject === 0; //false

array.length == 0; //true
arrayObject.length == 0; //true

array.length === 0; //true
arrayObject.length === 0; //true

// array 常見的操作有哪些
//基本款
for (let i = 0; array.length > i; i++) {
  console.log(array[i]);
}

//進階款, 陣列本身提供的 function
// 1. foreach
var result = array.forEach((item, index, array) => {
  //效果跟上面的基本款相同, 針對資料進行遍歷操作
});
// 沒有回傳值
// result = undefined

// 2. map((item, index, array) => {})
var result = array.map(function (item, index, array) {
  return item;
});
// 回傳值為操作後的陣列
// result = array

// 3. filter((item, index))
var result = array.filter(function (item, index) {
  if (condition) {
    return true | false;
  }
});
// result = 條件過濾後的Array. 可以去除不要的原數

// 4.every((item, i))
// 檢查每個元素的結果為何,除非全部都符合才回傳 True
var result = array.every(function (item, i) {
  return condition; // true || false
});
//result = true || false;

// 5. some((item, i))
// 只要某一個條件成功, 即回傳 true
var result = array.some(function (item, i) {
  return condition; // true || false
});

// 6.reduce((previous, item, index)=>{}, initial);
//比大小
var result = array.reduce(function (pre, item, index) {
  return Math.max(prev, item.Number);
}, 0);
// result = 陣列最大值

// 7.indexOf(key)
var index = array.indexOf("key");
// result 會傳傳 index 的位置.
// array[index] 抓取值

// 8.find(function(item, index, array){})
var result = array.find(function (item, index, array) {
  return condition; // true || false;
});
// result = 找到符合條件的第一個 Element
```

## Javascript PassByValue PassByRef

除了陣列操作外, Js 的 傳值, 傳址也是非常重要的概念,像 Vue 很多地方參數的宣告都是使用 {} Object 型態,想了解原理就必須學習此概念, 下方文章寫得很清晰,收錄起來放著.
[JavaScript 是「傳值」或「傳址」Kuro's Blog](https://kuro.tw/posts/2017/12/08/JavaScript-%E6%98%AF%E3%80%8C%E5%82%B3%E5%80%BC%E3%80%8D%E6%88%96%E3%80%8C%E5%82%B3%E5%9D%80%E3%80%8D/)
