# ElasticSearch - 管理對應

* Using explicit mapping creation 使用詳盡對應
* Mapping base types 對應基礎形態
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


###碰到索引建立，但shard沒有分配
在操作過程中，碰到建立index後，shard沒有分配的問題，導致叢集錯誤，狀態變成red，此時可以把index刪除，先判斷刪除後的叢急狀態是否為green，再重建index，接著看是否有正確分配shard和replica，以及叢集狀態是否一樣為正常(green)狀態。

###放入文件(put a document)

使用cURL直接對叢集操作，放入一個document，內含兩個欄位，分別是姓名(name)和年紀(age)，採用JSON格式進行資料填寫：

在操作的叢集，URL後方分別為

* index
* type
* id

使用斜線(/)符號進行區分，接著使用 -d 參數，放入欄位與資料。

    [kedy@es1 ~]$ curl -XPUT http://es1:9200/test/mytype/1 -d '{"name":"kedy", "age":"31"}'
    
接著從提示字元可以看到結果：
    
    {"_index":"test","_type":"mytype","_id":"1","_version":2,"_shards":{"total":2,"successful":2,"failed":0},"created":true}

結果顯示操作的的index、type、id、_version、shard等，可以判斷操作是否成功，最後一個created則說明這筆document是否被建立(true)或更新(false)。


###顯示對應

為了知道一個type內的各項mapping，能夠過cURL得知，使用命令如下，因前面我們沒有特別指定mapping，因此欄位類型就是ElasticSearch自動mapping的結果：

    curl -XGET http://es1:9200/test/mytype/_mapping?pretty=true
    
直接在 主機URL/index/url/type 後面加上 _mapping即可顯示結果，而pretty參數是為了讓人們方便閱讀，使用pretty參數得到的結果如下，會是巢狀的JSON物件：

    {
        "test" : {
            "mappings" : {
                "mytype" : {
                    "properties" : {
                        "age" : {
                            "type" : "string"
                        },
                        "name" : {
                            "type" : "string"
                        }
                    }
                }
            }
        }
    }

如果不下pretty=true或將pretty=false顯示結果如下：

    {"test":{"mappings":{"mytype":{"properties":{"age":{"type":"string"},"name":{"type":"string"}}}}}}
  
    
###小結
前面執行了建立索引(create an index)、放入文件(put a document)、顯示對應等工作，在文件索引的階段中，ElasticSearch會檢查該type是否存在，如果不存在，就會依照該欄位的type動態建立適當的類別。

ElasticSearch會讀取所有對應欄位的預設特徵(properties)並且開始處理：
* 如果欄位已經存在對應中，然後欄位值也是有效的(就是有符合正確的type)，那ElasticSearch就不會改變目前的mapping。
* 如果欄位已經存在對應中，但是欄位值跟型態對應不符、是不同型態，那麼type inference enging就會更改或升級欄位type，例如從int改成long的形態。而如果type根本不相容，就會造成例外(exception)接著索引程序就會失敗囉。
* 最後是如果欄位不存在的話，就會自動偵測欄位形態，接著就會更新到一個新欄位的對應中。
* 

每個文件(document)的索引都會使用UID作為唯一的識別，會儲存在該document一個特別的欄位，名稱為 _uid，這個值會自動使用 _id計算得知。而 _id這個值則是在索引的時候被提供，如果 _id不存在的話，ElasticSearch就會自動指派一個數值。

當建立或修改一個對應形態(mapping type)的時候，ElasticSearch會自動傳輸相關對應或改變到所有的叢集節點中，接著所有包含該特定型態的shard都會處理到相對應的更改。


##Mapping base types 對應基礎類別
使用詳盡對應(explicit mapping)可以讓我們很快速地在沒有資料庫結構或沒有特定schema的狀況下建立資料，並且不用擔心怎麼選擇或給予適合的欄位型態。因此，為了比較好的搜尋結果與效能，