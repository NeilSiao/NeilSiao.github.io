---
title: Avoid AWS GCP Azure Cost Disasters
date: 2021-03-15 21:26
tags: -cloud 
---

# 雲端服務可能意外造成非預期的費用


## Algorithm
程式碼的效率，如果頁面上有類似於統計或複雜運算的資料，例如募資網站，當前的募資金額，如果每一個新的使用者進入該頁面，都會重新計算，當該筆金額是由數百萬人所捐款的總額，其雲端的運算量將會非常的驚人，也因此造成雲端成本暴漲。

## Secure Cloud Key
不要把 AWS key 放到雲端，否則很可能會被偷走，拿去跑複雜的運算...

## You don't need to scale yet
雲端服務都有預設 auto scale ，當設定此選項時，須注意預設的數量是否過大，導致寫出錯誤程式碼時，瘋狂的加開 instance 導致費用暴漲

## dont' create infinite loops
避免在雲端上，寫出具有無窮迴圈的程式碼，例如: 上傳檔案觸發 Cloud function，Cloud function 又觸發上傳檔案，無窮迴圈，費用飆漲。

## Set up budget alert
在帳戶設定預算上限，AWS 宣布可以開始設定預算上限，當超過預算，可以自動關閉。GCP 則是需要自己在撰寫程式偵測當收到預算超過的 Email 時，自動關閉服務。

## 實際案例: Announce today
新創軟體 Announce today 因為程式碼問題，GCP 的 Cloud Run 以每分鐘五百美金的價錢持續飆漲，短短幾個小時內費用來到 $72,000 美金。最後通報 Google 這是場意外後，Google 取消了這筆費用。

## 總結

雲端服務，雖然讓我們省去許多機器投入成本和維護的麻煩，但是也帶來許多需要特別注意的地方，畢竟雲端服務的收費方式非常多元，網路流量、以次數計算、自動擴充等，如果有環節沒顧慮到，就有可能發生超出預算的情況。

雖然意外總會發生，GCP、AWS 的態度也傾向保護客戶的角度，只要不是故意，就不會特意去收取此項費用，但是客戶還是需要謹慎的使用，避免造成不必要的麻煩。

[How to Burn Money in the Cloud //Avoid AWS, GCP, Azure Cost Disasters](https://www.youtube.com/watch?v=N6lYcXjd4pg)
[Reddit $72,000 美元的參考連結](https://www.reddit.com/r/googlecloud/comments/kaa5ew/we_burnt_72k_testing_firebase_cloud_run_and/)
[how to not get a 30k firebase bill](https://www.youtube.com/watch?v=Lb-Pnytoi-8)