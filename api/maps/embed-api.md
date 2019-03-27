# 台灣圖霸 | Map8 Platform 
# Application Programming Interface Specification
歡迎使用 **![](images/logo.png) 台灣圖霸 | Map8 Platform** 地圖平台


## Notation
1. 左右鍵符號 (大於、小於符號, 也就是 `<` `>`) 所描述的是一個變數 (variable) 的 formal parameter 形式。本文件底下若提及參數部分，使用到此表示法時，請讀者將之代換成實際的內容 (也就是代換為 actual parameter，並且，不留下 `<` `>` 符號)。例如, `https://api.map8.zone/find?keyword=<關鍵詞>` 若實際關鍵詞為 taiwan，則實際呼叫時，應代換為 `https://api.map8.zone/find?keyword=taiwan`。
2. 位於 **API** 欄位內格式的 `<參數>`，為標準的 URL 之 query string 格式編碼 (i.e., `name=value` 以 URL `%` 編碼, 並以 `&` 連接)
3. 除非另有指定，否則，地理經緯度座標 (lat 或 latitude 均指經度，lng 或 longitude 均指緯度) 以 WGS84 / EPSG:3857 為地理座標系統

## Version
- v1.0_2019-03-16 (the present document)


## Maps Embedded API SPEC
功能 : 在您的網站中嵌入互動地圖

## API Index
- [[Maps] Maps Embedded API (在您的網頁中嵌入一個互動地圖)]


### Embedded API
讓您在網站或其他任何素材中嵌入互動地圖

- **API** :

    ```
    https://maps.map8.zone/?<參數>
    ```
place=<Your Place>
&
title=CLBC%E5%A4%A7%E7%9B%B4%E8%82%A5%E7%9A%82%E7%AE%B1
&
address=%E5%9C%B0%E5%9D%80:%E5%8F%B0%E5%8C%97%E5%B8%82%E4%B8%AD%E5%B1%B1%E5%8D%80%E6%98%8E%E6%B0%B4%E8%B7%AF569%E8%99%9F
&
description=CLBC%E5%A4%A7%E7%9B%B4%E8%82%A5%E7%9A%82%E7%AE%B1-%E5%B1%95%E6%BC%94%E5%BB%B3C%20(%E7%B6%BA%E9%BA%97%E5%BB%A3%E5%A0%B4B2)
#
18.6/25.0796124/121.5516343/-30/60
    
- **HTTP Method** : 
    - **GET**
- **Synopsis**
    - **<參數>**
        - **key**
            - 必要參數，請帶進您的 key
        - **title**
            - 選擇性參數，欲顯示之標題 ，格式為 text
        - **address**
            - 選擇性參數，欲顯示之地址 ，格式為 text
        - **description**
            - 選擇性參數，欲顯示之描述 ，格式為 text
        - **display**
            - 必要參數，格式為 #<zoom>/<緯度>/<經度>/角度(option)/視角(option)
                - zoom: 必要參數，數值 1~20 (數值小為小比例尺，數值越大則比例尺越大；譬如 10 可以看到整個中台灣，而 19 則已經是特寫部分街廓) 
                    - (您可以從我們線上地圖平台 https://maps.map8.zone 的網址列井字號 `#` 後面的第一個數字評估此數值)
                - 緯度: 必要參數，WGS84 / EPSG:3857 之緯度
                - 經度: 必要參數，WGS84 / EPSG:3857 之經度
                - 角度: 選擇性參數，數值-180~180，單位：度
                - 視角: 選擇性參數，數值0~60，單位：度
    - **(Migration 指南) 與 Google Maps 的 Nearby Search API 相容性**
        1. TBD
- **Request Message Body**: 
    - None.
- **Response**
    - None.
- Example
    - `https://maps.map8.zone/?key=<您的 key>&title=研鼎智能&address=台北市內湖區新湖三路189號6樓&description=台灣圖霸，有口皆碑#15.6/25.065089/121.580056/0/50`
    - `<iframe src="https://maps.map8.zone/?key=<您的 key>&title=研鼎智能&address=台北市內湖區新湖三路189號6樓&description=台灣圖霸，有口皆碑#15.6/25.065089/121.580056/0/50" width="640" height="480"></iframe>`
    - <iframe src="https://maps.map8.zone/?key=<您的 key>&title=研鼎智能&address=台北市內湖區新湖三路189號6樓&description=台灣圖霸，有口皆碑#15.6/25.065089/121.580056/0/50" width="640" height="480"></iframe>
- [back to index](#api-index)


## 共同

### HTTP Status Code
以上 API，可能回傳的 HTTP status code 如后 : 
- 400 Bad Request : 表示您的 requset 解析有誤。通常是給入的參數多了或少了，或是格式有錯誤，或必要參數卻沒給，等等
- 401 Authorization Required : 表示您未給定您的 key，或是您給的 key 並非有效。請跟我們聯絡
- 503 Service Unavailable : 表示您的 request 已經超出與我們約定的 QoS (服務品質) 等級。通常過一會兒 (QoS 上限解除) 再重發一次即可成功。如果持續發生，請跟我們聯絡
- 404 Not Found : 表示您的 request 的 URL 路徑 / 參數有誤


## 附註
- 當然，載在 URL 的 query string 的參數部分無順序性


<br/><br/>

----

<p align="center">
<img src="https://raw.githubusercontent.com/GO-LiFE/map8/master/images/logo_96x96.png" /> <br/> https://map8.zone
</p>
