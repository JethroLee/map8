# 台灣圖霸 | Map8 Platform 
# Application Programming Interface Specification

歡迎使用 **![](images/logo.png) 台灣圖霸 | Map8 Platform** 地圖平台


## Notation
1. 左右鍵符號 (大於、小於符號, 也就是 `<` `>`) 所描述的是一個變數 (variable) 的 formal parameter 形式。本文件底下若提及參數部分，使用到此表示法時，請讀者將之代換成實際的內容 (也就是代換為 actual parameter，並且，不留下 `<` `>` 符號)。例如, `https://api.map8.zone/find?keyword=<關鍵詞>` 若實際關鍵詞為 taiwan，則實際呼叫時，應代換為 `https://api.map8.zone/find?keyword=taiwan`。
2. 位於 **API** 欄位內格式的 `<參數>`，為標準的 URL 之 query string 格式編碼 (i.e., `name=value` 以 URL `%` 編碼, 並以 `&` 連接)
3. 除非另有指定，否則，地理經緯度座標 (lat 或 latitude 均指經度，lng 或 longitude 均指緯度) 以 WGS84 / EPSG:3857 為地理座標系統

## Version
- v0.4_2019-05-25


## Routes > Directions API SPEC
功能 : 路徑規劃 API

### Directions API
- 路徑規劃, 提供起點跟終點兩個點的座標值, 依照 map8 的圖資與演算法來產生連接此兩點的路徑

- **API** :
    ```
    https://api.map8.zone/route/car/{起始點座標}.json
    ```
- **HTTP Method** : 
    - **GET**
- **Synopsis**
    - **<參數>**
        - **起始點座標**
            - 格式為 ```<緯度>,<經度>;<緯度>,<經度>.json```

- **Request Message Body**: None.
- **Response**
    - Status code : **200** OK
        - 表示成功完成您的 request
    - `路徑`
        - **Response Message Body**:
            - type: application/json
            
            ```json-doc
            {
              "code": "Ok",
              "waypoints": [
                {
                  "hint": "string",
                  "name": "",
                  "location": [
                    [
                      121.546828,
                      25.057806
                    ]
                  ],
                  "distance": 110.769
                }
              ],
              "routes": [
                {
                  "legs": [
                    {
                      "distance": 0,
                      "duration": 0,
                      "summary": "string",
                      "weight": 0,
                      "steps": [
                        {
                          "driving_side": "string",
                          "distance": 0,
                          "geometry": "string",
                          "duration": 0,
                          "weight": 0,
                          "name": "string",
                          "mode": "string",
                          "maneuver": [
                            {
                              "bearing_after": 0,
                              "bearing_before": 0,
                              "type": "string",
                              "location": [
                                [
                                  121.546828,
                                  25.057806
                                ]
                              ]
                            }
                          ],
                          "intersections": [
                            {
                              "out": 0,
                              "entry": [
                                [
                                  "true",
                                  "true"
                                ]
                              ],
                              "location": [
                                [
                                  121.546828,
                                  25.057806
                                ]
                              ],
                              "bearings": [
                                [
                                  121,
                                  272
                                ]
                              ]
                            }
                          ]
                        }
                      ]
                    }
                  ],
                  "weight_name": "string",
                  "geometry": "string",
                  "weight": 32,
                  "distance": 228.4,
                  "duration": 32.6
                }
              ]
            }
            ```
            - 以下分別敘述回傳的結構的各個欄位的資訊所代表之意義
            ```
                code : Ok //Request could be processed as expected
            ```
    
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
- Example
    


## 共同

### HTTP Status Code
以上 API，可能回傳的 HTTP status code 如后 : 
- 400 Bad Request : 表示您的 requset 解析有誤。通常是給入的參數多了或少了，或是格式有錯誤，或必要參數卻沒給，等等
- 401 Authorization Required : 表示您未給定您的 key，或是您給的 key 並非有效。請跟我們聯絡
- 503 Service Unavailable : 表示您的 request 已經超出與我們約定的 QoS (服務品質) 等級。通常過一會兒 (QoS 上限解除) 再重發一次即可成功。如果持續發生，請跟我們聯絡

## 附註
- 當然，載在 URL 的 query string 的參數部分無順序性


<br/><br/>

----

<p align="center">
<img src="https://raw.githubusercontent.com/GO-LiFE/map8/master/images/logo_96x96.png" /> <br/> https://map8.zone
</p>
