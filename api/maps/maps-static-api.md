# 台灣圖霸 | Map8 Platform 
# Application Programming Interface Specification
歡迎使用 **![](images/logo.png) 台灣圖霸 | Map8 Platform** 地圖平台


## Notation
1. 左右鍵符號 (大於、小於符號, 也就是 `<` `>`) 所描述的是一個變數 (variable) 的 formal parameter 形式。本文件底下若提及參數部分，使用到此表示法時，請讀者將之代換成實際的內容 (也就是代換為 actual parameter，並且，不留下 `<` `>` 符號)。例如, `https://api.map8.zone/find?keyword=<關鍵詞>` 若實際關鍵詞為 taiwan，則實際呼叫時，應代換為 `https://api.map8.zone/find?keyword=taiwan`。
2. 位於 **API** 欄位內格式的 `<參數>`，為標準的 URL 之 query string 格式編碼 (i.e., `name=value` 以 URL `%` 編碼, 並以 `&` 連接)
3. 除非另有指定，否則，地理經緯度座標 (lat 或 latitude 均指經度，lng 或 longitude 均指緯度) 以 WGS84 / EPSG:3857 為地理座標系統

## Version
- v1.0_2019-01-06 (the present document)


## Maps Static API SPEC
功能 : 在您的網站中嵌入靜態地圖 (圖片)

## API Index
- [[Maps] Maps Static API (製作顯示地圖的圖檔)](#maps-static-api)


### Maps Static API
製作顯示地圖的圖檔，讓您在網站或其他任何素材中嵌入靜態地圖

- **API** :

    ```
    https://api.map8.zone/maps/staticmap?<參數>
    ```
- **HTTP Method** : 
    - **GET**
- **Synopsis**
    - **<參數>**
        - **key**
            - 必要參數，請帶進您的 key
        - **center**
            - 必要參數，欲製作成圖片的地圖中心點 (將與圖片四周等距離)，格式為 `<緯度>,<經度>`
        - **zoom**
            - 必要參數，整數，代表欲製作成圖片的地圖縮放層級 (比例尺) 
            - (數值小為小比例尺，數值越大則比例尺越大；譬如 10 可以看到整個中台灣，而 19 則已經是特寫部分街廓) 
            - (您可以從我們線上地圖平台 https://maps.map8.zone 的網址列井字號 `#` 後面的第一個數字評估此數值)
        - **size**
            - 必要參數，欲製作的圖片寬高。格式為 `<寬>,<高>` (單位為 pixel)
        - **format**
            - 選擇性參數，欲製作的圖片格式。可為 `png` 或 `jpg` 兩者之一。預設為 `png`。
    - **(Migration 指南) 與 Google Maps 的 Nearby Search API 相容性**
        1. **center** 參數僅支援 `<緯度>,<經度>` 格式，不支援 `地址`
        2. 其他 [Google Maps Static API 的參數](https://developers.google.com/maps/documentation/maps-static/dev-guide#URL_Parameters) 包括 `scale`, `maptype`, `language`, `region`, `markers`, `path`, `visible`, `style` 均 ignore (因此，同樣地，您可自行決定要刪除或留著)
- **Request Message Body**: 
    - None.
- **Response**
    - Status code : **200** OK
        - 表示成功完成您的 request
        - **Response Message Body**:
            - 圖片本身
    - 參見 [HTTP Status Code](#http-status-code) 一節說明本 API 回傳值之一般通則
- Example
    - `https://api.map8.zone/maps/static?key=<您的 key>&center=25.03745%2C121.547428&zoom=17&size=1024x768&format=jpg`
    - 結果 : ![](/images/maps-static-api-demo.jpg)
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
