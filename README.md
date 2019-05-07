# 台灣圖霸 | Map8 Platform
歡迎使用 **![](images/logo.png) 台灣圖霸 | Map8 Platform** 地圖平台

- 歡迎試用我們的 **台灣圖霸 (Map8 Platform) 地圖平台 API**!! [請點此申請試用](https://docs.google.com/forms/d/1BMN0cnmROBvtfU1JAxk-2sR9KcZdViHMNFtsyTR12l8)~
- 然後也歡迎您到我們的 [官方 API Explorer](https://www.map8.zone/api-explorer) 試用看看~ :smile:

有任何技術疑難，歡迎您發到 [issues 專區](/../../issues)~ 

或是有其他任何疑問，也都歡迎您 [跟我們聯絡](http://www.goyourlife.com/zh-TW/map-contact/) 喔!!!

<br/>

## 台灣圖霸 (Map8 Platform) 地圖平台 API 功能，包括 : 

## Maps 類
1. **[Maps Static API](./api/maps/maps-static-api.md)**
    - 製作顯示地圖的圖檔，讓您在網站或其他任何素材中嵌入靜態地圖
    - [Map8 Platform API Explorer](https://www.map8.zone/api-explorer/#/%5BMaps%5D%20Static%20API)

2. **[Maps Embed API](./api/maps/maps-embed-api.md)**
    - 不需要任何程式碼，用最簡單的方式 (網址傳遞參數)，就可以在您的網站嵌入動態的互動式地圖

3. **[Maps Javascript API](https://www.map8.zone/vector/index)**
    - 為您的網站添加互動式地圖。擁有自己的地圖內容與圖樣


## Places 類
提供全台灣超過 **650 萬筆** `門牌地址`、**100 萬筆** `道路`、**300 萬筆** `景點 POI` 之豐富圖資的搜尋功能。

每月圖資更新，每天路調，專業製圖，品質極佳 (不似有些坊間電子地圖，景點雖多，但品質良莠不齊，令人擔心)。

一次的 response 即將全部欄位資料送回給您。讓您的工程師 coding 起來輕鬆，維護起來簡單，更新功能快速，更棒的是，您不必擔心被剝好幾次皮。

極佳的運算能力，瞬間即回應您的 request，滿足您對 QoS (服務品質) 的嚴格要求。

此外，跟 Google Maps 不一樣的地方是，我們直接在結果內回傳距離給您!!!

Places 提供底下 API's : 

1. **[Places API](./api/places/places-api.md)**
    - **Place Search** : 搜尋地圖圖資, 包括 : 
        - **[Find Place API](./api/places/places-api.md#find-place-api)**
            - 給定關鍵字，搜尋景點
        - **[Nearby Search API](./api/places/places-api.md#nearby-search-api)**
            - 給定座標，搜尋周遭
        - **[Text Search API](./api/places/places-api.md#text-search-api)**
            - 給定任意的關鍵字 (請把這個當作搜尋引擎, 關鍵字以空白分隔, 例如 "加油站 台中" 這樣即可得到滿意的結果)
    - **[Place Autocomplete API](./api/places/places-api.md#place-autocomplete-api)**
        - 依據您給定的搜尋關鍵字，將推測的可能清單回覆給您 (通常運用在需要極佳使用體驗, 逐字逼近搜尋目標物的場景上)
    - [Map8 Platform API Explorer](https://www.map8.zone/api-explorer/#/%5BPlaces%5D%20Find%20Place%20API)
    
2. **[Geocoding API](./api/places/geocoding-api.md)**
    - [地址定位](./api/places/geocoding-api.md#geocoding-api) : geocoding, 也就是將 `地址 / 門牌` 轉為地理座標 `經緯度` 
    - [反地址定位](./api/places/geocoding-api.md#reverse-geocoding-api) : reverse geocoding, 也就是將地理座標 `經緯度` 轉為 `地址 / 門牌`
    - [Map8 Platform API Explorer](https://www.map8.zone/api-explorer/#/%5BPlaces%5D%20Geocoding%20API)

3. **Places Library, Maps Javascript API**
    - 為您的網站添加互動式地圖，並加入台灣圖霸的圖資搜尋功能 (以 javascript library 的形式提供您簡單的開發應用介面)


## Routes 類
- Directions API
    - 路徑規劃功能

<br/>

## 優點
- 輕量化的向量圖磚，檔案小，傳輸速度快
- `嵌入了圖資` 內，使用者體驗會感覺與底圖是 `一體` 的 (而不若傳統透過 GeoJSON / KML 感覺與底圖是分開的，而且難以支撐大規模資料)
- `高度互動` (interactive)、 `可共享` 的 `視覺化` (sharable visualizations) 分析呈現
- `任選區域` 檢視，zoom-in，獲得 `更高解析度` 而且 `不失真`
- 資料視覺化 (data visualization)、高度互動 (highly interactive)、可共享 (shareable) 的 solution

    > 現代 BI (Business Intelligence) 的需求，是遠遠超過於傳統分析報表僅止於圖 (graphs) 與表 (charts) 的。您想要鑑別哪些投保物件恰位於風暴路徑上嗎? 或是想比對建置成本與人行流量以最佳化展店地點嗎? 想規劃運送路徑以避開交通並降低油耗? 
    - `台灣圖霸` 正是這些問題的最佳解答。而且，視覺化 (visual) 且互動 (interactive)

#### `台灣圖霸` https://map8.zone 提供您基於地圖的資料分析視覺化、高度互動、可共用的極佳使用體驗!!!

<br/>

## 特色
### 地圖資料豐富
資料庫內含 PAPAGO! 導航所使用之地圖資料，包含導航路網，興趣點及門牌店家。 透過 API 展示、應用各項資訊。

### 極佳相容性
支援使用者添加自有圖片或其他網路地圖服務，亦可透過藉由資料格式支援、提供資料繪製、輸出等方法讓資訊呈現更加多元。

### 功能強大
提供坐標轉換、量測、定位、查詢、路徑規劃、使用者位置等功能，讓各種應用開發更加便利。

### 支援向量圖磚
資料量小，大幅減少資料更新的時間。可動態改變地圖外觀、疊加 3D 建物。任意視角、360 度旋轉、縮放平移更加流暢不失真。
<br/><sub>* 與網格圖磚相比</sub>

### 客製服務
可為您路調，製作屬於您的特定區域圖資。除了為您量身訂製專有圖層或是套疊您的自有圖層，更可自由決定全受管或是自行管理服務主機，一切隨您！自有圖資自己管，維護在私有網路內，自有圖資與數據資安更有保障。

### 彈性方案
可選擇按月、按季度、按年的租賃方案或依據使用流量進行計費。

<br/><br/>

----

<p align="center">
<a href="https://map8.zone"><img src="https://raw.githubusercontent.com/GO-LiFE/map8/master/images/logo_96x96.png" /></a> <br/> https://map8.zone
</p>
