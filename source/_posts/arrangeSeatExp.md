---
title: ArrangeSeatExp
date: 2021-01-25 16:18
tags: -learning
---

## 寫一個簡單的排座位系統
這是一個挺簡單的系統,但是在實作過程中還是會卡在一些地方,算是當作筆記,讓之後自己在撰寫程式時,比較不容易採坑.
> 使用 Laravel, jQuery, 一點點的 Vue

### Jquery 和 Vue.js
起初想說此系統應該沒有很複雜,可能 Jquery 操作資料+DOM,弄一弄就好了,但是實際在撰寫的過程中,發現挺多問題的,因為要照顧的對象有兩個（Dom,Data）,所以時常遇到資料操作完了,又要回頭思考我要在哪裡綁定 Event 比較好,常常來回跑,且程式檔案變多之後,已經有點看不清楚資料是如何在我的 js 檔案之中流動了...  

後來將其中一個 Modal 畫面改寫成 Vue.js 搭配 vue dev tool, 此時可以先發現一個好處,我減少大量在 jquery console.log 的處境了!! 這一點很重要,不然在 jQuery 的世界只能不斷的 log 看資料在哪個步驟變成怎樣, Vue.js 偶爾還是需要 log 看看資料的轉移. 但減少很多.

下一次我個人會推薦嘗試看看資料導向的前端框架, 畢竟減少不少考慮 Dom 和找資料的時間.

### 亂數排序座位與學生
在亂數排序的過程中, 最重要的是亂數的機率,比起 version 1, 其實要使用 version 2 (Fisher-Yates Shuffle), 才能達到更好的隨機效果.
```javascript
//version 1, 
//it depends on browser's sorting method. 

function sort(array){
    array.sort(() => Math.random() - 0.5)
}


//version 2

/**
 * Fisher-Yates Shuffle
 * @param {array} array 
 * if u don't want to change original array, use array.slice(0)
 * to assign new data. var newArray = array.slice(0);
 */
export function shuffle(array) {
    for (var i = array.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
    }
    return array;
}
```
### 定義系統需求
寫到隨機排序座位功能時,心中一直有一種這樣可以,那樣做更好的想法,結果程式碼改來改去的,浪費不少時間... 

當初在構思時,其實也沒有想太多（座位,學生,亂數隨機）, 確實也是如此, 但是當畫面的呈現與實際撰寫還是有些微的出入, 譬如學生屬於某個班級,那是不是可以直接把整班的學生抓進來亂數隨機, 教室切換班級時,是不是應該保留先前的隨機資料,一些比較細節的地方,都值得考慮是否要作到那麼複雜

此外還可以繼續延伸下去,例如：加入`時段, 課程`的條件因素等等, 系統就會更加複雜, 所以明確的系統需求就佔有相當重要的成份, 如果一直做下去,其實沒完沒了...


### 成果網站參考
https://arrange-seat.herokuapp.com/
測試帳號: test@test.com
測試密碼: 12345678
