---
layout: post
title: Elasticsearch 筆記
published: true
date: 2016-09-11 04:24
tags:
  - Elasticsearch
categories: []
comments: true

---
### 前言

最近工作上使用的資料庫主要以 Elasticsearch 為主。而 Elasticsearch 跟傳統的關聯式資料庫有諸多的不同之處。初期在開發的時候並不是那麼快的上手。所以記錄下該如何使用 Elasticsearch 與如何在官方文件中找到自己需要的功能。

### 1. 準備工作

#### 1.1 你需要知道的名詞

一開始我對名詞的對應並不是特別的重視，隨著實戰上的需求，我開始需要查找 API 的時候，發現文件有點不知道從何看起。後來隨著使用的功能越來越多。必須對 Elasticsearch 有更深一層的了解，於是花了大約兩天左右的時間把文件重要的部份大略的看過一次。這樣的過程讓我理解了哪些功能可以在哪些地方找到，也是我寫下這篇筆記的動機，

`index` 對應關聯式資料庫中的 `database`。
`type` 對應關聯式資料庫中的 `table`。
`docuement` 對應的是關聯式資料庫中的一筆資料。
`mapping` 對應關聯式資料庫中的 `schema`。

#### 1.2 文件導覽

常用的文件分為兩部分 `definitive guide` 與 `referenece` 。為了直接對應官方文件這邊就直接用英文名詞。

基本的資料操作和一些基本的用法可以在 [The Definitive Guide](https://www.elastic.co/guide/en/elasticsearch/guide/current/getting-started.html) 中找到，Definitive Guide 整體比較偏向 `如何達成你要做的事情`。包括以下幾種：

- 文件的新增刪除
- 如何做全文搜索
- 如何搜尋

而 API 的詳細分類和用法的範例則可以在 [Elasticsearch Reference](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/index.html) 中找到。比較常用的有：

- Document API - 單筆資料的操作在這邊查找
- Search API - Elasticsearch 支援的搜尋方法非常多種，Search API 是非常重要的一環。了解 search api 後可以搭配 `update_by_query` 或 `delete_by_query` 等等的 api 來做更新或刪除的動作
- Cat API - 可以獲得系統目前的資訊
- Indice API - 用來管理 `index` 的 api。這邊我一開始看到疑惑了一下。過了一會兒才聯想到 elasticsearch 的 index 等於關聯式資料庫的 db。所以其實每個 api 開的名稱都是很有條理和整齊度的
- Aggregations - aggregation 對應關聯式資料庫的 `GROUP`，做分組後取得結果。
- Query DSL - 如果需要自定義搜尋，elasticsearch 有提供query DSL 讓你可以高度彈性的組出想要的搜尋。

另外 Elasticsearch Ruby 的 API 位置在[這邊](http://www.rubydoc.info/gems/elasticsearch-api/Elasticsearch/API/Actions)。因為文件的連結沒有在官方 github 中明顯的寫出來，一開始讓我疑惑了一下。


#### 1.3 可以用哪些方法存取 Elasticsearch

跟 Elasticsearch 的溝通只要透過 HTTP Request 就可以存取。這也是雖然 Elasticsearch 是以 Java 作為底層，但是卻可以跟大多數其他程式語言製作系統輕鬆串接重要原因。所以操作上只要你可以發出 HTTP Request 就可以跟 Elasticsearch 溝通。而因為安全性的關係，Elasticsearch 常常架設在內網。需要在遠端存取的時候透過VPN 會是比較安全的作法。

我比較常用的方法有以下幾種：


1. 直接在終端機下使用 curl 取得結果，指令由官網複製下來修改成自己的格式。好處是這樣做可以排除很多不確定因素，例如gem是否有bug等等的問題。壞處是取得的結果並不好看。如果要好看的話需要自行處理。
1. 使用 Restclient 將 curl 指令包起來發 HTTP Reqeust 給 Elasticsearch。跟 Elasticsearch-ruby 比較起來，這是單純的 HTTP Request，比較不容易有 Gem 中的 Bug。如果在終端機中確定指令可用，可以直接用 Restclient打造。直接使用`system call`也是這邊的替代方案之一。
1. 使用外掛(plugin)存取，好處是方便存取，看到的結果也會是整理過的。缺點是要架設需要花一段時間先把環境整理好。
1. 使用 [Elasticsearch-ruby](http://www.rubydoc.info/gems/elasticsearch-api/Elasticsearch/API/Actions)存取 Elasticsearch 。好處是搜尋出來的結果已經經過包裝。缺點是需要大量新增或更新資料的時候因為過度包裝速度會比較慢。一開始使用的時候會擔心有些API不是官方的最新版本。實際測試的心得是大部分都是可行的。如果得不到預期的結果再使用 curl 來下原始的指令。到這邊可以發現，你需要對 elasticsearch的行為有一定的認識，不然你是無法好好的使用他。


### 2. mapping

#### 2.1 Schema Free 更要嚴謹的定義資料庫

`mapping` 即關聯式資料庫中的 `schema`。但是 Elasticsearch 有著 `schema free` 的特性。即如果你想存入的欄位的型別與 `mapping`的型別不同時你仍然可以存入。還未定義過的欄位也可以直接存入資料庫。

有著 `schema free` 的特性更需要注重資料的格式與資料欄位的設計，如果不當的使用很可能讓資料庫中資料變成一堆難以整理的垃圾。

#### 2.2 特別需要注意的 String 型態

Elasticsearch 的 String 比較特別。如果不做特別設定的話，Elasticsearch 會預設處理方式為 `full text`，也就是會幫你的 string 做切字的動作。如果要把整個 string 欄位視為一個 `keyword` ，則需要在 mapping 的時候加上 `index: "not_analyzed"`。
```json
PUT my_index
{
  "mappings": {
    "my_type": {
      "properties": {
        "full_name": {
          "type":  "string"
        },
        "status": {
          "type":  "string",
          "index": "not_analyzed"
        }
      }
    }
  }
}
```

相關連結:

[String datatype | Elasticsearch Reference [2.4] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/string.html)
[Field datatypes | Elasticsearch Reference [2.4] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/mapping-types.html)

#### 2.3 keyword 、 full text 與 analyzer

`keyword`的行為跟關聯式資料庫的欄位行為比較像。除了用字串比對的方式以外沒什麼其他的搜尋方法。而`full text` 則會通過 `analyzer` 進行分析，你可以使用系統提供的 `analyzer`。系統提供的 analyzer 只能切英文字。如果要切中文可以用一套叫做 `elasticsearch-analysis-ik`的開源解決方案來處理。或是自行撰寫也是一個選項。


#### 2.4 查詢 mapping 的語法

在終端機中使用 curl

```
curl -XGET 'http://localhost:9200/spider/_mapping/tkecw'
```

使用`RestClient`

```rb
res = RestClient.get "http://localhost:9200/spider/_mapping/tkecw"
JSON.parse res.body
```

相關連結：

[Get Field Mapping | Elasticsearch Reference [2.4] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-get-field-mapping.html)

### 參考資料

[Getting Started | Elasticsearch: The Definitive Guide [2.x] | Elastic](https://www.elastic.co/guide/en/elasticsearch/guide/current/getting-started.html)
[Module: Elasticsearch::API::Actions — Documentation for elasticsearch-api (2.0.0)](http://www.rubydoc.info/gems/elasticsearch-api/Elasticsearch/API/Actions)
[Field datatypes | Elasticsearch Reference [2.4] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/mapping-types.html)




未完待續...