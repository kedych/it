# ElasticSearch - 管理對應

* Using explicit mapping creation 使用詳盡對應
* Mapping base types 對應基礎類別
* Mapping arrays 對應陣列
* Mapping an object 對應物件
* Mapping a document 對應文件
* Using dynamic templates in document mapping 對應文件的動態範本
* Managing nested objects 對應巢狀物件
* Managing a child document 對應子文件
* Adding a field with multiple mappings 增加具多對應的欄位
* Mapping a geo point field 對應地理資訊點的欄位
* Mapping a geo shape field 對應地理資訊區塊欄位
* Mapping an IP field 對應網路位址(IP)欄位
* Mapping an attachment field 對應附件欄位
* Adding metadata to a mapping 加入對應的metadata
* Specifying a different analyzer 特定差異化分析
* Mapping a completion suggester 對應完成建議(有點怪, 再想想)


##簡介
對應(mapping)在ElasticSearch中是很重要的觀念，它定義了搜尋引擎如何處理文件(document)。

在搜尋引擎中，有兩個主要的運作：

* 索引(Indexing) -　這是在Index中接收文件(document)和儲存/索引/處理的行為。
* 搜尋(Searching) - 這是從索引中探尋資料的行為。

上述兩種運作非常緊集的結合，也就是說，在所引發生的錯誤的話，可能連帶會影響搜尋的結果，導致結果不是預期的或是有錯過什麼重要的內容。

ElasticSearch在索引/類別層級，有一個詳盡明確的對應，在索引的時候，如果沒有提供預設的對應(mapping)，一個預設的對應就會從文件組成欄位的結構來猜測並建立。接著，新的對應(mapping)就會自動傳遞到所有的叢集節點。

預設對應種類(default type mapping)都有一個自動感測(sensible)的預設值，如果需要改變mapping狀態或客製化各種其他不同觀念的索引(儲存、忽略、結束等)，就需要提供一個新的對應定義。

接著學習如何操作各種不同類型的對應(mapping)方式。


## Using explicit mapping creation 使用詳盡對應
如果把索引當做SQL的資料庫，那麼對應(mapping)就像是資料庫中的表格定義或資料表架構(schema)。

ElasticSearch能夠瞭解我們想要索引文件的結構，並且能自動建立詳盡的對應定義(explicit mapping creation)。

學習explicit mapping時，要準備一個可用的ElasticSearch叢集，還有基礎的JSON知識，接著就可以繼續做囉！

###建立索引(create an index)

如同前面使用初探介紹，使用cURL直接對叢集操作：

    [kedy@es1 ~]$ curl -XPUT http://es1:9200/test
    
接著從提示字元可以看到結果：

    {"acknowledged":true}

###放入一份文件(put a document)

使用cURL直接對叢集操作：

###碰到索引建立，但shard沒有分配
在操作過程中，碰到從急　