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