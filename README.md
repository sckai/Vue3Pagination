# Vue 3 + Pagination

![ScreenShot](https://upload.cc/i1/2023/03/18/0u3swb.png "front")

![ScreenShot](https://upload.cc/i1/2023/03/18/2zoD9v.png "middle")

![ScreenShot](https://upload.cc/i1/2023/03/18/7XuOfN.png "later")

Attributes

參數 | 說明 | 類型 | 可選值 | 預設值
------------- | ------------- | ------------- | ------------- | ------------- 
currentPage | 當前所在頁數 | number | — | 1
total | 總頁數 | number | — | 1
middlePageDisplay | 按鈕中間顯示數量 | number | — | 3

<br>
<br>

Events

事件名稱 | 說明 | 返回參數
------------- | ------------- | -------------
GetPage | 點擊按鈕後獲得值 | (value: number)