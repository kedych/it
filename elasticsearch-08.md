# 資料初探 (Exploring Your Data)

## 簡易資料集 (Sample Dataset)

研讀與操作先前的官方手冊後，對Elasicsearch操作應該有些簡單的概念，接著來試試更真實的資料集合。官方網站文件中提及，有整理了一份
JSON隔式的虛擬消費者銀行帳戶資訊，資料架構如下：

    {
        "account_number": 0,
        "balance": 16623,
        "firstname": "Bradshaw",
        "lastname": "Mckenzie",
        "age": 29,
        "gender": "F",
        "address": "244 Columbus Place",
        "employer": "Euron",
        "email": "bradshawmckenzie@euron.com",
        "city": "Hobucken",
        "state": "CO"
    }

備註：上述資料與欄位係透過 http://www.json-generator.com/ 產生，如有雷同純屬巧合。

## 載入簡易資料集 (Loading the Sample Dataset)

官方文件用的範例帳戶資料可以從以下兩個地方下載：

https://github.com/bly2k/files/blob/master/accounts.zip?raw=true

把檔案解壓縮後會取得accounts.json，將之放到Elasticsearch主機，接著使用curl命令，將檔案傳入Elasticsearch，可以看到index為bank、type為account，並使用bulk:

    curl -XPOST 'localhost:9200/bank/account/_bulk?pretty' --data-binary "@accounts.json"
    
輸入後可以看到如下的輸出，回應應是201或200。

    {
    "index" : {
        "_index" : "bank",
        "_type" : "account",
        "_id" : "990",
        "_version" : 4,
        "_shards" : {
            "total" : 2,
            "successful" : 2,
            "failed" : 0
        },
        "status" : 200
        }
    }
    
再使用 _cat API列出目前的index

    curl 'localhost:9200/_cat/indices?v'
    
看到回應如下

    health status index                 pri rep docs.count docs.deleted store.size pri.store.size
    green  open   bank                    5   1       1000            0    905.8kb        448.8kb

依序看到index的健康 (health)、狀態 (status)、名稱 (index)、primary shard、replica、文件數量(docs.count)、刪除文件數 (docs.deleted)、儲存空間大小 (store.size)、主要儲存空間大小 (pri.store.size)

可以看到將 accounts.json 以 --data-binary 形式透過 bulk API載入後，共有1000筆type為account的document在bank index之中。

## 搜尋API (The Search API)

在ES之中有兩種搜尋的方法，一種是把搜尋參數透過REST request URI傳遞，直接把參數變成URI的一部分，另一種是把搜尋參數透過REST request body傳遞，把搜尋參數以JSON document形式傳入。

以REST request body型式的搜尋，表示方式比較簡單具體、也比較好以JSON格式構建出較具閱讀性的搜尋語法。

範例示範一個REST request URI的搜尋方式，其餘部分則以requet body method為主。

    curl 'localhost:9200/bank/_search?q=*&pretty'
    
上述範例表示搜尋索引名稱為bank的內容，查詢參數(q=*)為所有(星號)，表示回傳所有bank index中的document，以下是眾多搜尋結果中的一筆。


    {
    "took" : 39,
    "timed_out" : false,
    "_shards" : {
        "total" : 5,
        "successful" : 5,
        "failed" : 0
    },
    "hits" : {
        "total" : 1000,
        "max_score" : 1.0,
        "hits" : [{
         "_index" : "bank",
        "_type" : "account",
        "_id" : "208",
        "_score" : 1.0,
        "_source":{"account_number":208,"balance":40760,"firstname":"Garcia","lastname":"Hess","age":26,"gender":"F","address":"810 Nostrand Avenue","employer":"Quiltigen","email":"garciahess@quiltigen.com","city":"Brooktrails","state":"GA"}
    }

回傳結果的前端可以看到一些關於本搜尋回傳的結果，

* took – 執行搜尋花多少時間，單位為毫秒。
* timed_out – 搜尋有沒有超過最大容忍時間(timeout)。
* _shards – 搜尋的shards數量，通常是成功/失效的shards數量總合。
* hits – 搜尋結果
* hits.total – 符合搜尋條件的document數量。
* hits.hits – 實際搜尋結果陣列，預設顯示前10個 documents。
* _score - 之後補充。
* max_score - 之後補充。

第二個範例使用REST request body方式進行搜尋，也找出全部的內容。

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": { "match_all": {} }
    }'

可以看到跟request URI的差別，在於URI之中帶搜尋條件的q=*參數，改成了附帶JSON query給_search API，得到的結果如下：

    {
    "took" : 2035,
    "timed_out" : false,
    "_shards" : {
        "total" : 5,
        "successful" : 5,
        "failed" : 0
    },
    "hits" : {
        "total" : 1000,
        "max_score" : 1.0,
        "hits" : [ {
            "_index" : "bank",
            "_type" : "account",
            "_id" : "25",
            "_score" : 1.0,
            "_source":{"account_number":25,"balance":40540,"firstname":"Virginia","lastname":"Ayala","age":39,"gender":"F","address":"171 Putnam Avenue","employer":"Filodyne","email":"virginiaayala@filodyne.com","city":"Nicholson","state":"PA"}
    }, 

可以看到搜尋結果其實是一致的，差別只在於搜尋條件的傳遞方式，在於使用URI傳遞還是使用JSON query的格式傳遞。

有個很重要的觀念，當Elasticsearch回傳搜尋結果的時候，就已經完成搜尋request回應的工作，同時不保留任何server-side端的資源，

It is important to understand that once you get your search results back, Elasticsearch is completely done with the request and does not maintain any kind of server-side resources or open cursors into your results. This is in stark contrast to many other platforms such as SQL wherein you may initially get a partial subset of your query results up-front and then you have to continuously go back to the server if you want to fetch (or page through) the rest of the results using some kind of stateful server-side cursor.

## 查詢語言簡介 (Introducing the Query Language)

## 執行搜尋 (Executing Searches)

## 執行過濾 (Executing Filters)

## 執行聚合 (Executing Aggregations)

## 小結