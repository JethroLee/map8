# 台灣圖霸 | Map8 Platform 
# Application Programming Interface Specification

歡迎使用 **<img src="https://www.map8.zone/images/logo.png" /> 台灣圖霸 | Map8 Platform** 地圖平台 API


## Notation
1. 左右鍵符號 (大於、小於符號, 也就是 `<` `>`) 所描述的是一個變數 (variable) 的 formal parameter 形式。本文件底下若提及參數部分，使用到此表示法時，請讀者將之代換成實際的內容 (也就是代換為 actual parameter，並且，不留下 `<` `>` 符號)。例如, `https://api.map8.zone/find?keyword=<關鍵詞>` 若實際關鍵詞為 taiwan，則實際呼叫時，應代換為 `https://api.map8.zone/find?keyword=taiwan`。
2. 位於 **API** 欄位內格式的 `<參數>`，為標準的 URL 之 query string 格式編碼 (i.e., `name=value` 以 URL `%` 編碼, 並以 `&` 連接)
3. 除非另有指定，否則，地理經緯度座標 (lat 或 latitude 均指經度，lng 或 longitude 均指緯度) 以 WGS84 / EPSG:3857 為地理座標系統

## Version
- v1.0_2019-01-06 (the present document)


## Places > Places API SPEC
功能 : 搜尋台灣圖霸的地圖圖資

此外，跟 Google Maps 不一樣的地方是，我們直接在結果內回傳距離給您!!!


## API Index
- [[Places] Places API / Place Search / Find Place API (給定關鍵字，搜尋地點)](#find-place-api)
- [[Places] Places API / Place Search / Nearby Search API (給定座標，搜尋周遭)](#nearby-search-api)
- [[Places] Places API / Place Search / Text Search API (以任意的關鍵字組合進行搜尋)](#text-search-api)
- [[Places] Places API / Place Autocomplete API (給定關鍵字，回應出推測的可能清單)](#place-autocomplete-api)


## Place Search API

## Find Place API
給定關鍵字，搜尋地點

- **API** :

    ```
    https://api.map8.zone/place/findplacefromtext/<輸出格式>?<參數>
    ```
- **HTTP Method** : 
    - **GET**
- **Synopsis**
    - **<輸出格式>**
        - 選擇性參數，本版本不理會此參數，response 一律以 JSON 格式回應
    - **<參數>**
        - **key**
            - 必要參數，請帶進您的 key
        - **input**
            - 必要參數，欲搜尋的地點的關鍵詞 (無任何空白分隔)。可以是地址、地名、商店名稱、或電話
        - **inputtype** : 
            - 選擇性參數，應為 `textquery` 或 `phonenumber` 兩者之一。若未給，則由台灣圖霸系統自動智慧判斷
        - **locationbias** : 
            - 選擇性參數，可為 : 
                - `ipbias` : 要求系統依照您發出此 request 的 IP address 來決定本 API 進行搜尋時的中心點
                - `<緯度>,<經度>` : 要求系統依據給定的地理經緯度座標為搜尋中心點
            - 此參數若未給，則系統預設採用 `ipbias` 
            - (注意) **採用 `ipbias` 方式將增加額外的系統動作，可能因網路而造成搜尋速度相當幅度的延遲。因此，建議您盡可能不要採用 `ipbias`。強烈建議您一律採 `locationbias=<緯度>,<經度>` 參數**
    - **(Migration 指南) 與 Google Maps 的 Find Place API 相容性**
        1. 台灣圖霸一次就把全部欄位回應給您，因此，[Google Maps Find Place API 的 fields 參數](https://developers.google.com/places/web-service/search#Fields) 台灣圖霸見到會直接 ignore -- 因此，如果您本來有使用此參數，那您可以選擇放著不改 (如果您認為這樣能降低您改動的 effort 的話)
        2. `language`, `locationbias.circular`, `locationbias.rectangular` 也均 ignore (因此，同樣地，您可自行決定要刪除或留著)
- **Request Message Body**: None.
- **Response**
    - Status code : **200** OK
        - 表示成功完成您的 request
        - **Response Message Body**:
            - type: text/json
            
            ```json-doc
            {
               "html_attributions" : [                 // 您必須向使用者顯示的本 API 所屬的圖資版權資訊
                   "台灣圖霸", 
                   "研鼎智能", 
                   "PAPAGO!"
               ], 
               "candidates" : [
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
                     "tel" : <String>,                 // 本筆搜尋結果的電話號碼 (例如 "02-87921567")
                     "city" : "<String>,               // 本筆搜尋結果所屬的城市 (例如 "台北市")
                     "town" : "<String>,               // 本筆搜尋結果所屬的行政區 (例如 "內湖區")
                     "type" : <String>,                // 本筆搜尋結果的類型，可為 : "景點", "地址", 或 "道路"
                     "chain" : <String>,               // 本筆搜尋結果若屬於某連鎖機構, 則此欄位為該機構名稱 (例如 "研勤集團")
                     "branch" : <String>,              // 本筆搜尋結果若屬於某連鎖機構, 則此欄位為該分店名稱 (例如 "台中辦公室")
                     "cat" : <String>,                 // 本筆搜尋結果於台灣圖霸系統內所歸屬的景點類型 (例如 "公司行號")
                     "distance" : float,               // 本筆搜尋結果與輸入的中心點座標間的直線距離, 單位為 km
                 }
               ],
               "status" : <String>  // Status Code
            }
            ```
            - 上述每一筆搜尋結果內的各欄位, 若無值, 仍一律回傳, 但帶空值
            - **(Migration 指南) 與 Google Maps 的 Find Place API 相容性**
                1. 本 API 的 response 不會回傳任何 [Google Maps Find Place API 的 response](https://developers.google.com/places/web-service/search#find-place-responses) 內的 optional 欄位包括 `geometry.viewport`, `opening_hours`, `photos`, `rating`, `debug_log` 等欄位. 由於這些欄位原本即為 optional, 因此, suppose 不會對於您的 migration 造成影響
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
    - `https://api.map8.zone/place/findplacefromtext/json?key=<您的 key>&input=研鼎智能&locationbias=25.06102,121.58790`
    
        ```json-doc
        {
            "html_attributions" : [
                "台灣圖霸", 
                "研鼎智能", 
                "PAPAGO!"
            ],
            "candidates" : [
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
                "name" : "研鼎智能股份有限公司",
                "tel" : "02-87921567",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "景點",
                "chain" : "", 
                "branch" : "",
                "cat" : "公司行號",
                "distance" : "0.3"
              }
            ],
            "status" : "OK"
        }
        ```
- [back to index](#api-index)


## Nearby Search API
給定座標，搜尋周遭

- **API** :

    ```
    https://api.map8.zone/place/nearbysearch/<輸出格式>?<參數>
    ```
- **Synopsis**
    - **<輸出格式>**
        - 選擇性參數，本版本不理會此參數，response 一律以 JSON 格式回應
    - **<參數>**
        - **key**
            - 必要參數，請帶進您的 key
        - **location**
            - 必要參數，要求系統依據給定的地理座標為搜尋中心點。格式為 `<經度>,<緯度>`，分別為緯度與經度。
        - **radius** : 
            - 選擇性參數，指定以上述 `location` 為中心，以本參數 `radius` 指定方圓半徑之距離作為搜索範圍 (最大為 50.000 公里)。若未給，則由台灣圖霸系統自動智慧判斷
    - **(Migration 指南) 與 Google Maps 的 Nearby Search API 相容性**
        1. 除以上 **參數** 章節所描述的參數外，其他 [Google Maps Nearby Search API 的參數](https://developers.google.com/places/web-service/search#PlaceSearchRequests) 包括 `rankby`, `keyword`, `language`, `minprice`, `name`, `opennow`, `type`, `pagetoken` 也均 ignore (因此，同樣地，您可自行決定要刪除或留著)
- **Request Message Body**: None.
- **Response**
    - Status code : **200** OK
        - 表示成功完成您的 request
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
                     "tel" : <String>,                 // 本筆搜尋結果的電話號碼 (例如 "02-87921567")
                     "city" : "<String>,               // 本筆搜尋結果所屬的城市 (例如 "台北市")
                     "town" : "<String>,               // 本筆搜尋結果所屬的行政區 (例如 "內湖區")
                     "type" : <String>,                // 本筆搜尋結果的類型，可為 : "景點", "地址", 或 "道路"
                     "chain" : <String>,               // 本筆搜尋結果若屬於某連鎖機構, 則此欄位為該機構名稱 (例如 "研勤集團")
                     "branch" : <String>,              // 本筆搜尋結果若屬於某連鎖機構, 則此欄位為該分店名稱 (例如 "台中辦公室")
                     "cat" : <String>,                 // 本筆搜尋結果於台灣圖霸系統內所歸屬的景點類型 (例如 "公司行號")
                     "distance" : float,               // 本筆搜尋結果與輸入的中心點座標間的直線距離, 單位為 km
                 },
                 ... (more results) ...
               ],
               "status" : <String>  // Status Code
            }
            ```
            - 上述每一筆搜尋結果內的各欄位, 若無值, 仍一律回傳, 但帶空值
            - **results** 陣列帶回本次搜尋的查詢結果；而且，為一口氣回足所有的查詢結果。讓您無須分數次查詢。本 API 的查詢，最多將一次回應多達 60 筆資料
            - **(Migration 指南) 與 Google Maps 的 Find Place API 相容性**
                1. 本 API 的 response 不會回傳 [Google Maps Nearby Search 與 Text Search API 的 response](https://developers.google.com/places/web-service/search#nearby-search-and-text-search-responses) 內 `results[i]` 的 optional 欄位包括 `icon`, `opening_hours`, `photos`, `scope`, `alt_ids`, `reference`, `types`, `vicinity` 等欄位. 由於這些欄位原本即為 optional, 因此, suppose 不會對於您的 migration 造成影響
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
    - `https://api.map8.zone/place/nearbysearch/json?key=<您的 key>&location=25.06102,121.58790`
    
        ```json-doc
        {
            "html_attributions" : [
                "台灣圖霸", 
                "研鼎智能", 
                "PAPAGO!"
            ],
            "results" : [
              {
                "formatted_address" : "台北市內湖區南京東路六段465號",
                "geometry" : {
                    "location" : {
                        "lat" : 25.060955,
                        "lng" : 121.587635
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "南京東路六段465號",
                "tel" : "",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "地址",
                "chain" : "", 
                "branch" : "",
                "cat" : "",
                "distance" : 0.028
              },
              {
                "formatted_address" : "台北市內湖區南京東路六段463號",
                "geometry" : {
                    "location" : {
                        "lat" : 25.060904,
                        "lng" : 121.587504
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "光屋攝影工作室",
                "tel" : "02-87919199",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "景點",
                "chain" : "", 
                "branch" : "",
                "cat" : "購物商場",
                "distance" : 0.0419
              }, 
              {
                "formatted_address" : "台北市內湖區南京東路六段463號",
                "geometry" : {
                    "location" : {
                        "lat" : 25.060904,
                        "lng" : 121.587504
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "南京東路六段463號",
                "tel" : "",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "地址",
                "chain" : "", 
                "branch" : "",
                "cat" : "",
                "distance" : 0.0419
              },
              {
                "formatted_address" : "台北市內湖區南京東路六段487號",
                "geometry" : {
                    "location" : {
                        "lat" : 25.061352,
                        "lng" : 121.588254
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "南京東路六段465號",
                "tel" : "",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "地址",
                "chain" : "", 
                "branch" : "",
                "cat" : "",
                "distance" : 0.0513
              },
              {
                "formatted_address" : "南京東路六段489號",
                "geometry" : {
                    "location" : {
                        "lat" : 25.060955,
                        "lng" : 121.587635
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "南京東路六段489號",
                "tel" : "",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "地址",
                "chain" : "", 
                "branch" : "",
                "cat" : "",
                "distance" : 0.0534
              }, 
              {
                "formatted_address" : "台北市內湖區南京東路六段461號",
                "geometry" : {
                    "location" : {
                        "lat" : 25.060904,
                        "lng" : 121.587504
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "108 CUSTOM",
                "tel" : "02-27908383",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "景點",
                "chain" : "", 
                "branch" : "",
                "cat" : "租車公司",
                "distance" : 0.055
              }
            ],
            "status" : "OK"
        }
        ```
- [back to index](#api-index)


## Text Search API
以任意的關鍵字組合進行搜尋

- **API** :

    ```
    https://api.map8.zone/place/textsearch/<輸出格式>?<參數>
    ```
- **HTTP Method** : 
    - **GET**
- **Synopsis**
    - **<輸出格式>**
        - 選擇性參數，本版本不理會此參數，response 一律以 JSON 格式回應
    - **<參數>**
        - **key**
            - 必要參數，請帶進您的 key
        - **query**
            - 必要參數，給定任意的關鍵字組合 (請把這個當作搜尋引擎) 關鍵字以空白分隔, 例如 "加油站 台中" 這樣即可得到滿意的結果
        - **location**
            - 選擇性參數，要求系統依據給定的地理座標為搜尋中心點。格式為 `<經度>,<緯度>`，分別為緯度與經度。
            - 此參數若未給，則系統預設依照您發出此 request 的 IP address 來決定本 API 進行搜尋時的中心點。但請注意 : **此方式將增加額外的系統動作，可能因網路而造成搜尋速度相當幅度的延遲。因此，強烈建議您一律給 `location=<緯度>,<經度>` 參數**
        - **radius** : 
            - 選擇性參數，指定以上述 `location` 為中心，以本參數 `radius` 指定方圓半徑之距離作為搜索範圍 (最大為 50.000 公里)。若未給，則由台灣圖霸系統自動智慧判斷
    - **(Migration 指南) 與 Google Maps 的 Text Search API 相容性**
        1. 除以上 **參數** 章節所描述的參數外，其他 [Google Maps Text Search API 的參數](https://developers.google.com/places/web-service/search#TextSearchRequests) 包括 `region`, `language`, `minprice`, `opennow`, `type`, `pagetoken` 也均 ignore (因此，同樣地，您可自行決定要刪除或留著)
- **Request Message Body**: None.
- **Response**
    - 同 Nearby Seach, 請參見 [Nearby Search](#nearby-search) 的 **Response** 章節
- Example
    - 搜尋 `加油站 內湖 台北`
    - `https://api.map8.zone/place/textsearch/json?key=<您的 key>&query=加油站 內湖 台北&location=25.06102,121.58790`

        ```json-doc
        {
            "html_attributions" : [
                "台灣圖霸", 
                "研鼎智能", 
                "PAPAGO!"
            ],
            "results" : [
              {
                "formatted_address" : "台北市內湖區新明路92號",
                "geometry" : {
                    "location" : {
                        "lat" : 25.059615,
                        "lng" : 121.589523
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "統一速邁樂加油站內湖一站",
                "tel" : "02-27929031",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "景點",
                "chain" : "統一速邁樂加油站", 
                "branch" : "內湖一站",
                "cat" : "加油站",
                "distance" : 0.226
              },
              {
                "formatted_address" : "台北市內湖區南京東路六段463號",
                "geometry" : {
                    "location" : {
                        "lat" : 25.060904,
                        "lng" : 121.587504
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "中油加油站協記站",
                "tel" : "02-27915409",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "景點",
                "chain" : "中油加油站", 
                "branch" : "協記站",
                "cat" : "加油站",
                "distance" : 0.543
              }, 
              {
                "formatted_address" : "台北市內湖區新明路305號",
                "geometry" : {
                    "location" : {
                        "lat" : 25.05699,
                        "lng" : 121.584842
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "台灣宅配通-中油加油站協記站-代收店",
                "tel" : "",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "景點",
                "chain" : "台灣宅配通", 
                "branch" : "中油加油站協記站-代收店",
                "cat" : "貨運站",
                "distance" : 0.0544
              },
              {
                "formatted_address" : "台北市南港區南港路三段49號",
                "geometry" : {
                    "location" : {
                        "lat" : 25.053272,
                        "lng" : 121.590197
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "全國加油站南港站",
                "tel" : "02-27828306",
                "city" : "台北市", 
                "town" : "南港區", 
                "type" : "景點",
                "chain" : "全國加油站", 
                "branch" : "南港站",
                "cat" : "加油站",
                "distance" : 0.892
              },
              {
                "formatted_address" : "台北市內湖區民權東路六段50號",
                "geometry" : {
                    "location" : {
                        "lat" : 25.068355,
                        "lng" : 121.583351
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "gogoro電池交換站中油內湖加油站",
                "tel" : "",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "景點",
                "chain" : "gogoro充電站", 
                "branch" : "中油內湖加油站",
                "cat" : "充電站",
                "distance" : 0.934
              }, 
              {
                "formatted_address" : "台北市內湖區民權東路六段50號",
                "geometry" : {
                    "location" : {
                        "lat" : 25.068355,
                        "lng" : 121.583351
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "中油加油站內湖站",
                "tel" : "02-27920678",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "景點",
                "chain" : "中油加油站", 
                "branch" : "內湖站",
                "cat" : "加油站",
                "distance" : 0.936
              }
            ],
            "status" : "OK"
        }
        ```
- [back to index](#api-index)


## Place Autocomplete API
依據給定的搜尋關鍵字，回應出推測的可能清單 (通常運用在需要極佳使用體驗, 逐字逼近搜尋目標物的場景上)

- **API** :

    ```
    https://api.map8.zone/place/autocomplete/<輸出格式>?<參數>
    ```
- **HTTP Method** : 
    - **GET**
- **Synopsis**
    - **<輸出格式>**
        - 選擇性參數，本版本不理會此參數，response 一律以 JSON 格式回應
    - **<參數>**
        - **key**
            - 必要參數，請帶進您的 key
        - **input**
            - 必要參數，欲搜尋的地點的關鍵詞 (無任何空白分隔)。可以是地址、地名、商店名稱、或電話
        - **location**
            - 選擇性參數，要求系統依據給定的地理座標為搜尋中心點。格式為 `<經度>,<緯度>`，分別為緯度與經度。
            - 此參數若未給，則系統預設依照您發出此 request 的 IP address 來決定本 API 進行搜尋時的中心點。但請注意 : **此方式將增加額外的系統動作，可能因網路而造成搜尋速度相當幅度的延遲。因此，強烈建議您一律給 `location=<緯度>,<經度>` 參數**
        - **radius** : 
            - 選擇性參數，指定以上述 `location` 為中心，以本參數 `radius` 指定方圓半徑之距離作為搜索範圍 (最大為 50.000 公里)。若未給，則由台灣圖霸系統自動智慧判斷
    - **(Migration 指南) 與 Google Maps 的 Place Autocomplete API 相容性**
        1. 除以上 **參數** 章節所描述的參數外，其他 [Google Maps Place Autocomplete API 的參數](https://developers.google.com/places/web-service/autocomplete#place_autocomplete_requests) 包括 `sessiontoken`, `offset`, `language`, `types`, `components`, `strictbounds` 也均 ignore (因此，同樣地，您可自行決定要刪除或留著)
- **Request Message Body**: None.
- **Response**
    - Status code : **200** OK
        - 表示成功完成您的 request
        - **Response Message Body**:
            - type: text/json
            
            ```json-doc
            {
               "html_attributions" : [                 // 您必須向使用者顯示的本 API 所屬的圖資版權資訊
                   "台灣圖霸", 
                   "研鼎智能", 
                   "PAPAGO!"
               ],
               "predictions" : [
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
                     "tel" : <String>,                 // 本筆搜尋結果的電話號碼 (例如 "02-87921567")
                     "city" : "<String>,               // 本筆搜尋結果所屬的城市 (例如 "台北市")
                     "town" : "<String>,               // 本筆搜尋結果所屬的行政區 (例如 "內湖區")
                     "type" : <String>,                // 本筆搜尋結果的類型，可為 : "景點", "地址", 或 "道路"
                     "chain" : <String>,               // 本筆搜尋結果若屬於某連鎖機構, 則此欄位為該機構名稱 (例如 "研勤集團")
                     "branch" : <String>,              // 本筆搜尋結果若屬於某連鎖機構, 則此欄位為該分店名稱 (例如 "台中辦公室")
                     "cat" : <String>,                 // 本筆搜尋結果於台灣圖霸系統內所歸屬的景點類型 (例如 "公司行號")
                     "distance" : float,               // 本筆搜尋結果與輸入的中心點座標間的直線距離, 單位為 km
                 },
                 ... (more results) ...
               ],
               "status" : <String>  // Status Code
            }
            ```
            - 上述每一筆搜尋結果內的各欄位, 若無值, 仍一律回傳, 但帶空值
            - **predictions** 陣列帶回本次的預測結果；而且，為一口氣回足所有的查詢結果。讓您無須分數次查詢。本 API 的查詢，最多將一次回應多達 10 筆資料
            - **(Migration 指南) 與 Google Maps 的 Find Place API 相容性**
                1. 本 API 的 response 與 [Google Maps Place Autocomplete API 的 response](https://developers.google.com/places/web-service/autocomplete#place_autocomplete_responses) 內 `predictions[i]` 內的欄位截然不同 (包括 `description`, `matched_substrings`, `reference`, `terms`, `types`, `structured_formatting` 等欄位)。因此，本 API 的 migration 功夫可能會稍大些
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
    - `https://api.map8.zone/place/autocomplete/json?key=<您的 key>&input=明美&location=25.06102,121.58790`
    
        ```json-doc
        {
            "html_attributions" : [
                "台灣圖霸", 
                "研鼎智能", 
                "PAPAGO!"
            ],
            "results" : [
              {
                "formatted_address" : "台北市內湖區南京東路六段北側",
                "geometry" : {
                    "location" : {
                        "lat" : 25.060834,
                        "lng" : 121.587175
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "明美公園",
                "tel" : "",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "景點",
                "chain" : "", 
                "branch" : "",
                "cat" : "公園",
                "distance" : 0.075, 
              }, 
              {
                "formatted_address" : "",
                "geometry" : {
                    "location" : {
                        "lat" : 25.028364,
                        "lng" : 121.587175
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "YouBike微笑單車明美公園",
                "tel" : "",
                "city" : "台北市", 
                "town" : "內湖區", 
                "type" : "景點",
                "chain" : "YouBike微笑單車", 
                "branch" : "明美公園",
                "cat" : "租車公司",
                "distance" : 0.252, 
              },
              {
                "formatted_address" : "台北市松山區新東街28號之1",
                "geometry" : {
                    "location" : {
                        "lat" : 25.059238,
                        "lng" : 121.566031
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "明美",
                "tel" : "02-27628939",
                "city" : "台北市", 
                "town" : "松山區", 
                "type" : "景點",
                "chain" : "", 
                "branch" : "",
                "cat" : "購物商場",
                "distance" : 2.21, 
              },
              {
                "formatted_address" : "台北市信義區松德路40號",
                "geometry" : {
                    "location" : {
                        "lat" : 25.038958,
                        "lng" : 121.576684
                    },
                },
                "id" : "R625AYXZ38324999QP10236182",
                "place_id" : "R625AYXZ38324999QP10236182",
                "name" : "明美精品名店",
                "tel" : "02-27584348",
                "city" : "台北市", 
                "town" : "信義區", 
                "type" : "景點",
                "chain" : "", 
                "branch" : "",
                "cat" : "購物商場",
                "distance" : 2.7, 
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

