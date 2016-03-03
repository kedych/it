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

有個很重要的觀念，當Elasticsearch回傳搜尋結果的時候，就已經完成搜尋request回應的工作，同時不保留任何server-side端的資源，也不會對搜尋的結果有任何的暫存、註記、記錄等，屬於stateless的形式。這點跟很多其他資料庫的使用上有明顯的不同，例如在SQL之中，搜尋結果可以繼續拆分搜尋子集合並進行近一步的工作 (諸如joint、fetch等)，如果有需要進行類似工作，則需要使用stateful的server-side cursor。


## 查詢語言簡介 (Introducing the Query Language)

Elasticsearch提供一個JSON風格的特定領域專用語言，讓我們用來執行查詢，[關於Query DSL可參考本網頁](https://www.elastic.co/guide/en/elasticsearch/reference/2.1/query-dsl.html) 此查詢語言包含範圍相當全面，或許乍看很好理解，但最好的方法還是透過範例來實際操作。
上個範例，執行以下搜尋：

    {
    "query": { "match_all": {} }
    }

詳細拆解上述搜尋語句，查詢(*"query"*)部分顯示了我們查詢的定義(Query definition)為何，查詢定義內容為 *match_all*，就是我們想要執行的搜尋。而*match_all*查詢，是最簡單的，代表要查詢索引之中所有的documents。


此外，關於*query*參數，可以傳遞其他參數來影響(調整)搜尋結果。例如，以下執行全部搜尋(*match_all*)，配合*size*的使用，只回傳一筆document：

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": { "match_all": {} },
        "size": 1
    }'

一般搜尋中，如果*size*沒有特別指定，預設就是10，顯示10筆搜尋結果。
接下來的範例也是搜尋全部符合(*match_all*)，但是回傳的結果我們想要第11到20筆。

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": { "match_all": {} },
        "from": 10,
        "size": 10
    }'

使用了*from*和*size*參數，告訴Elasticsearch，我們想要看的結果是第幾筆開始的多少筆。要注意的是如同多數程式語言，所謂的__第一筆__是從__0__開始計算，這兩個參數在我們需要製造分頁的時候非常方便。如果*from*沒有指定，預設就是從__第一筆__(也就是__0__)開始回傳。


小提醒:

* *size*沒有特別指定，預設就是10
* *from*沒有指定，預設就是__第一筆__(也就是__0__)

接下來的範例，除了使用*matsh_all*之外，再依照用戶的帳戶餘額進行遞減排序，也就是在request body之中，加入*sort*參數，告訴Elasticsearch要排序的欄位，以及排序的方式。

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": { "match_all": {} },
        "sort": { "balance": { "order": "desc" } }
    }'

## 執行搜尋 (Executing Searches)

上一章節介紹簡單的搜尋參數，接著來挖掘更多有關查詢描述語言(Query DSL)。先看看搜尋回傳結果，可以看到會帶所有欄位。也就是在 *_source* 欄位沒有進行任何描述時的情況。如果我們不需要所有的欄位，可以在搜尋的request body中進行需要的欄位描述，例如指需要回傳source中的帳號(*account_number*)與餘額(*balance*)，可以透過以下搜尋語法，在*_source*之中指定：

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": { "match_all": {} },
        "_source": ["account_number", "balance"]
    }'

上述搜尋語法減少*_source*欄位中的內容，搜尋結果一樣回傳*_source*欄位，但裡面只帶有*account number*跟*balance*的結果。如果有用過SQL，可以發現概念跟SQL中的

    SELECT accrount number, balance FROM ...
    
是很類似的概念。接著來看搜尋的部分，先前使用了 *match_all*來查詢所有的document，然後並不是每次的查詢只是列出所有的document，所以，可以使用*match*設定欄位符合的搜尋條件。

這個範例回傳帳戶號碼為20的：

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": { "match": { "account_number": 20 } }
    }'

此範例搜尋地址欄位含有mill的：

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": { "match": { "address": "mill" } }
    }'

此範例搜尋地址欄位含有mill或lane的，注意直接空白就是OR運算的意思：

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": { "match": { "address": "mill lane" } }
    }'

如果需要搜尋的項目是包含"mill lane"完整片語(phrase)，就需要改用*match_phrase*來下條件：

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": { "match_phrase": { "address": "mill lane" } }
    }'

前面介紹簡單的欄位選擇與比對，可以使用*match*和*match_phrase*組合出想要搜尋的條件。在使用上還是不那麼全面，可以再配合布林查詢(*bool*ean query)，*bool*可以設定需要的邏輯來結合小搜尋到大搜尋之中。

透過以下*bool*搜尋，結合兩個*match*查詢，因使用的布林條件是*must*，其中兩個match條件是必須同時滿足(同時成立、同時為true)，也就是會回傳地址同時含有mill和lane的結果：

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": {
            "bool": {
                "must": [
                    { "match": { "address": "mill" } },
                    { "match": { "address": "lane" } }
                ]
            }
        }
    }'

如果要使用*bool*搜尋，但布林條件只要用OR，則需要怎麼調整呢？上述是的AND運算使用了*must*作為布林條件，要OR的話則使用*should*，也就是__應該__符合其中之一的*match*條件即會回傳，範例如下：

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": {
            "bool": {
                "should": [
                    { "match": { "address": "mill" } },
                    { "match": { "address": "lane" } }
                ]
            }
        }
    }'

前面介紹在*bool*查詢如何使用AND(*must*)與OR(*should*)的使用方式，聰明的讀者應想到還有一個布林運算NOT，在Elasticsearch之中，要使用NOT就是使用*must_not*即可，從前面的操作範例來看，要改成地址欄位不要有mill也不要有lane存在的查詢方式如下：

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": {
            "bool": {
                "must_not": [
                    { "match": { "address": "mill" } },
                    { "match": { "address": "lane" } }
                ]
            }
        }
    }'

目前說明了*bool*能搭配的用法，對應到AND、OR、NOT就是*must*、*should*、 *must_not*，這些布林運算是可以依照搜尋條件進行組合搭配，完成比較複雜的搜尋，透過範例尋找40歲但是不住在ID(一個州的簡寫)的人：

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": {
            "bool": {
                "must": [
                    { "match": { "age": "40" } }
                ],
                "must_not": [
                    { "match": { "state": "ID" } }
                ]
            }
        }
    }'

## 執行過濾 (Executing Filters)

在之前的搜尋API(Search API)章節，搜尋結果回傳的*_score*被暫時跳過，其實*_score*代表的意義就是此筆document與搜尋條件相符程度的量化數值，*_score*數值越高表示此document與搜尋條件的相符、相關性越高；反之亦然。

但搜尋並沒不是時時都需要計算score，通常在有使用過濾條件(filtering)的document集合才有必要計算score。Elasticsearch會偵測這些情況並且自動最佳化搜尋執行，避免去計算不重或者沒有用到的score。

上一節提及的布林(*bool*)查詢，也支援*filter*的用法，讓搜尋結果可以被其它條件限制、過濾，而不因此影響相依性score的計算，也就是說，*score*計算只在搜尋時進行，搜尋之後針對結果的過濾(filtering)不會去改變此數值。

接著介紹一個filter－*range*，可以用在數值欄位的範圍過濾與選定。在此搜尋中結合*bool*，找尋索引中的所有欄位(*must*、*match_all*)，並使用*filter*針對結果進行範圍(*range*)過濾，要過濾的欄位是*balance*，要找出大於等於(*gte* greater than or equal)20000也小於(*lte* less than or equal)30000的結果。

    curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
    {
        "query": {
            "bool": {
                "must": { "match_all": {} },
                "filter": {
                    "range": {
                        "balance": {
                            "gte": 20000,
                            "lte": 30000
                        }
                    }
                }
            }
        }
    }'

## 執行聚合 (Executing Aggregations)

## 小結

## 參考文獻

https://www.elastic.co/guide/en/elasticsearch/reference/2.1/_executing_searches.html