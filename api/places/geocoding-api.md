# 台灣圖霸 | Map8 Platform 
# Application Programming Interface Specification
歡迎使用 **<img src="https://www.map8.zone/images/logo.png" /> 台灣圖霸 | Map8 Platform** 地圖平台 API


## Notation
1. 左右鍵符號 (大於、小於符號, 也就是 `<` `>`) 所描述的是一個變數 (variable) 的 formal parameter 形式。本文件底下若提及參數部分，使用到此表示法時，請讀者將之代換成實際的內容 (也就是代換為 actual parameter，並且，不留下 `<` `>` 符號)。例如, `https://api.map8.zone/find?keyword=<關鍵詞>` 若實際關鍵詞為 taiwan，則實際呼叫時，應代換為 `https://api.map8.zone/find?keyword=taiwan`。
2. 位於 **API** 欄位內格式的 `<參數>`，為標準的 URL 之 query string 格式編碼 (i.e., `name=value` 以 URL `%` 編碼, 並以 `&` 連接)
3. 除非另有指定，否則，地理經緯度座標 (lat 或 latitude 均指經度，lng 或 longitude 均指緯度) 以 WGS84 / EPSG:3857 為地理座標系統

## Version
- v1.0_2019-01-06 (the present document)


## Places > Geocoding API SPEC
功能 : 地址定位與反地址定位 

(geocoding 與 reverse geocoding : 將 `地址 / 門牌` 轉為地理座標 `經緯度`, 或反過來, 將地理座標 `經緯度` 轉為 `地址 / 門牌`)


## API Index
- [Places / Geocoding API (地址定位與反地址定位)](#geocoding-api)


### Geocoding API
- 地址定位 (geocoding, 也就是將 `地址 / 門牌` 轉為地理座標 `經緯度`).
- 反地址定位 (reverse geocoding, 反過來將地理座標 `經緯度` 轉為 `地址 / 門牌`)

- **API** :

    ```
    https://api.map8.zone/place/geocode/<輸出格式>?<參數>
    ```
- **HTTP Method** : 
    - **GET**
- **Synopsis**
    - **<輸出格式>**
        - 選擇性參數，本版本不理會此參數，response 一律以 JSON 格式回應
    - **<參數>**
        - **key**
            - 必要參數，請帶進您的 key
        - **address**
            - 欲轉換為 `經緯度` 座標之 `地址` (若欲執行的是 `地址定位` ，則此參數為必須)
        - **latlng**
            - 欲轉換為 `地址` 的 `經緯度` 座標 (格式為 `<緯度>,<經度>`)  (若欲執行的是 `反地址定位` ，則此參數為必須)
    - **(Migration 指南) 與 Google Maps 的 Geocoding API 相容性**
        1. 除以上 **參數** 章節所描述的參數外，其他 [Google Maps Geocoding API 的參數](https://developers.google.com/maps/documentation/geocoding/intro#geocoding) 包括 `components`, `bounds`, `language`, `region` 均 ignore (因此，同樣地，您可自行決定要刪除或留著)
     - **(Migration 指南) 與 Google Maps 的 Reverse Geocoding API 相容性**
         1. 除以上 **參數** 章節所描述的參數外，其他 [Google Maps Reverse Geocoding API 的參數](https://developers.google.com/maps/documentation/geocoding/intro#ReverseGeocoding) 包括 `language`, `result_type`, `location_type` 均 ignore (因此，同樣地，您可自行決定要刪除或留著)
- **Request Message Body**: None.
- **Response**
    - Status code : **200** OK
        - 表示成功完成您的 request
    - `地址定位`
        - **Response Message Body**:
            - type: text/json
            
            ```json-doc
            {
               "html_attributions" : [                 // 您必須向使用者顯示的本 API 所屬的圖資版權資訊
                   "台灣圖霸", 
                   "研鼎智能", 
                   "PAPAGO!"
               ], 
               "results" : [
                  {
                     "formatted_address" : <String>,   // 地址 (經整理、格式化過的)
                     "geometry" : {
                        "location" : {
                           "lat" : <float>,            // 緯度
                           "lng" : <float>             // 經度
                        },
                     },
                     "id" : <String>,                  // 此地點於台灣圖霸系統內的 record ID
                     "place_id" : <String>,            // 此地點於台灣圖霸系統內的 record ID
                     "name" : <String>,                // 本筆搜尋結果的名稱 (地名、道路名、景點名) (例如 "研鼎智能股份有限公司")
                     "city" : "<String>,               // 本筆搜尋結果所屬的城市 (例如 "台北市")
                     "town" : "<String>,               // 本筆搜尋結果所屬的行政區 (例如 "內湖區")
                     "type" : <String>,                // 本筆搜尋結果的類型，可為 : "景點", "地址", 或 "道路"
                 }
               ],
               "status" : <String>  // Status Code
            }
            ```
            - 上述每一筆搜尋結果內的各欄位, 若無值, 仍一律回傳, 但帶空值
            - 一般而言，本 API 的搜尋結果通常只會有一筆 (儘管實際上可能因為關鍵字的模糊性，而系統內部搜尋時其實有找到多筆 hits)
            - **(Migration 指南) 與 Google Maps 的 Geocoding API 相容性**
                1. 本 API 的 response 不會回傳任何 [Google Maps Geocoding API 的 response](https://developers.google.com/places/web-service/search#find-place-responses) 內的 optional 欄位包括 `address_components`, `geometry.location_type`, `geometry.viewport`, `types` 等欄位. 由於這些欄位原本即為 optional, 因此, suppose 不會對於您的 migration 造成影響
    - `反地址定位`
        - **Response Message Body**:
            - type: text/json
            
            ```json-doc
            {
               "html_attributions" : [                 // 您必須向使用者顯示的本 API 所屬的圖資版權資訊
                   "台灣圖霸", 
                   "研鼎智能", 
                   "PAPAGO!"
               ], 
               "results" : [
                  {
                     "formatted_address" : <String>,   // 地址 (經整理、格式化過的)
                     "geometry" : {
                        "location" : {
                           "lat" : <float>,            // 緯度
                           "lng" : <float>             // 經度
                        },
                     },
                     "id" : <String>,                  // 此地點於台灣圖霸系統內的 record ID
                     "place_id" : <String>,            // 此地點於台灣圖霸系統內的 record ID
                     "name" : <String>,                // 本筆搜尋結果的名稱 (地名、道路名、景點名) (例如 "研鼎智能股份有限公司")
                     "city" : "<String>,               // 本筆搜尋結果所屬的城市 (例如 "台北市")
                     "town" : "<String>,               // 本筆搜尋結果所屬的行政區 (例如 "內湖區")
                     "type" : <String>,                // 本筆搜尋結果的類型，可為 : "景點", "地址", 或 "道路"
                 }
               ],
               "status" : <String>  // Status Code
            }
            ```
            - 上述每一筆搜尋結果內的各欄位, 若無值, 仍一律回傳, 但帶空值
            - 請留意 reverse geocoding 通常會傳回多個結果
            - **(Migration 指南) 與 Google Maps 的 Reverse Geocoding API 相容性**
                1. 本 API 的 response 不會回傳任何 [Google Maps Reverse Geocoding API 的 response](https://developers.google.com/maps/documentation/geocoding/intro#reverse-example) 內的 optional 欄位包括 `address_components`, `geometry.location_type`, `geometry.viewport`, `types` 等欄位. 由於這些欄位原本即為 optional, 因此, suppose 不會對於您的 migration 造成影響
    - Status code : **400** Bad Request
        - 表示您的 request 系統偵測到有錯誤而無法完成您的要求。通常是給入的參數多了或少了，或是格式有錯誤，或必要參數卻沒給，等等
        - **Response Message Body**:
            - type: text/json

            ```json-doc
            {
                "status" : <String>     // Status Code
            }
            ```
    - 參見 [HTTP Status Code](#http-status-code) 一節說明本 API 回傳值之一般通則
    - 參見 ["status" 欄位](#status-欄位) 一節說明 "status" 欄位的一般性意義
- Example
    - `地址定位` (帶入 `address` 參數) : `https://api.map8.zone/place/geocode/json?key=<您的 key>&address=新湖三路189號`
    
        ```json-doc
        {
            "html_attributions" : [
                "台灣圖霸", 
                "研鼎智能", 
                "PAPAGO!"
            ],
            "results" : [
              {
                "formatted_address" : "台北市內湖區新湖三路189號6樓",
                "geometry" : {
                    "location" : {
                        "lat" : 25.065089,
                        "lng" : 121.580056
                    },
                },
                "id" : "R625AYXZ38324999QP10236181",
                "place_id" : "R625AYXZ38324999QP10236181",
                "name" : "新湖三路189號",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "地址",
              }
            ],
            "status" : "OK"
        }
        ```
    - `反地址定位` (帶入 `latlng` 參數) : `https://api.map8.zone/place/geocode/json?key=<您的 key>&latlng=25.065089,121.580056`
    
        ```json-doc
        {
            "html_attributions" : [
                "台灣圖霸", 
                "研鼎智能", 
                "PAPAGO!"
            ],
            "results" : [
              {
                "formatted_address" : "台北市內湖區新湖三路189號6樓",
                "geometry" : {
                    "location" : {
                        "lat" : 25.065089,
                        "lng" : 121.580056
                    },
                },
                "id" : "R625AYXZ38324999QP10236181",
                "place_id" : "R625AYXZ38324999QP10236181",
                "name" : "新湖三路189號",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "地址",
              }
            ],
            "status" : "OK"
        }
        ```
- [back to index](#api-index)


## 共同

### HTTP Status Code
以上 API，可能回傳的 HTTP status code 如后 : 
- 400 Bad Request : 表示您的 requset 解析有誤。通常是給入的參數多了或少了，或是格式有錯誤，或必要參數卻沒給，等等
- 401 Authorization Required : 表示您未給定您的 key，或是您給的 key 並非有效。請跟我們聯絡
- 503 Service Unavailable : 表示您的 request 已經超出與我們約定的 QoS (服務品質) 等級。通常過一會兒 (QoS 上限解除) 再重發一次即可成功。如果持續發生，請跟我們聯絡

### "status" 欄位
"status" 欄位的意義為 : 
- `OK` : 無發生任何錯誤；該地點被成功偵測，並且至少回傳一則結果
- `ZERO_RESULTS` : 表示搜尋雖然完成，但未得到任何有效結果。此狀況譬如可能發生在您對本 API 發出的 request 所給定的中心座標在一個偏遠地區
- `OVER_QUERY_LIMIT` : 表示您已經超出您的配額。請跟我們聯絡
- `REQUEST_DENIED` : 表示您的 request 無法進行；一般來說是您未給定您的 key，或是您給的 key 並非有效。請跟我們聯絡
- `INVALID_REQUEST` : 表示您的 requset 解析有誤。通常是給入的參數多了或少了，或是格式有錯誤，或必要參數卻沒給，等等
- `UNKNOWN_ERROR` : 表示是我們的伺服器端的錯誤；再重試一次可能就會成功。如果持續發生此問題，請跟我們聯絡
- **(Migration 指南) 與 Google Maps 的 Find Place API 相容性**
    - 以上 "status" 欄位與 Google Maps 完全相容


## 附註
- 當然，載在 URL 的 query string 的參數部分無順序性


<br/><br/>

----

<p align="center">
<img src="https://raw.githubusercontent.com/golife-sysop/map8-docs/master/images/logo_96x96.png" /> <br/> https://map8.zone
</p>
