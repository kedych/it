#使用初探

##基於REST API的操作

Elasticsearch安裝好以後，要知道如何跟叢集溝通。使用的方法為REST API，透過API可以完成以下操作：

* 確認叢集、節點、索引的狀態與取得統計資料。
* 管理叢集、節點、索引與Metadata。
* 對索引執行資料的建立、讀取、更新、刪除，以及在索引之間執行搜尋運算。
* 執行進階搜尋運算，例如分頁、排序、過濾、程式化、聚合甚至更多其他不同的運算。

##資料進入流程
當一筆record進入ElasticSearch的時候，目的shard會基於以下兩種方式選擇:

1. record ID 如果沒有指定ID，就會自動產生一個
1. 如有定義 routing 或 parent 參數，就會依照這些參數的hash值來選擇正確的shard。

##建立index
參考官方文件的範例，先建立第一個index，叫消費者(customer)，接著試著列出index的結果：

命令1：

    curl -XPUT 'localhost:9200/customer?pretty'

結果：

    [kedy@es1 ~]$ curl -XPUT 'localhost:9200/customer?pretty'
    {
        "acknowledged" : true
    }
    
命令2：

    curl 'localhost:9200/_cat/indices?v'

結果：

    [kedy@es1 ~]$ curl 'localhost:9200/_cat/indices?v'
    health status index    pri rep docs.count docs.deleted store.size pri.store.size
    yellow open   customer   5   1          0            0       254b           254b

結果會以JSON回傳，為便於視覺讀取，在命令1結尾多增加 ?pretty 參數，讓結果在呈現上會是具有程式排版風格的呈現。而命令2則是在命令結尾增加 ?v 讓結果呈現標題列。

從第二個命令的回應畫面，可以看到目前有哪些index存在，還有相關資訊, 例如多少個primary shards跟幾份replica，還有包含多少document在理面。

列出index的時候，可以發現叢集健康狀態的指示是Yellow而非Green, 這是因為replicas還沒被分配。發生的原因是因為Elasticsearch預設只幫indexr建立一個replica，所以這時候叢集之中就只有一個replica存在，除非有叢集有其它node加入，只要有新的node加入，然後replica被分配到第二個節點的時候，index的健康狀態就會是green。

狀態為Green代表所有功能都是正常運作的。Yellow代表所有資料都有，但可能有些replica並沒有正常配置，但此時叢集功能還是正常，包含資料的新增修改刪除與搜尋等都可以。如果狀態為Red，表示有些資料不知道怎麼了，可能有問題，此時也會影響搜尋與運算結果。

要注意的是，就算叢集狀態在Red，有些功能可能還是可以運作的，只是會影響最終的執行結果，因此要盡快排除此狀態。


#索引與查詢文件(Index and Query a Document)

要跟ElasticSearch溝通是透過REST API將語法或文件以JSON物件形式進行交換。

接下來開始放點東西到Elasticsearch叢集之中！參考官方文件對消費者(customer)的索引(index)進行操作，要索引一個文件，必須告訴Elasticsearch要對index中的哪個型態(type)進行操作。

索引一個簡單的消費者文件到名為消費者的索引中，要操作的型態(Type)為"external", ID指定為1:

範例的JSON document: {"name":"kedy chang"}

命令為：

    curl -XPUT 'localhost:9200/customer/external/1?pretty' -d '{ "name": "kedy chang" }'
    

正確執行後，可以看到回應為：

    {
        "_index" : "customer",
        "_type" : "external"
        "_id" : "1",
        "_version" : 1,
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "created" : true
    }

從Elasticsearch回應可以得知一個型態(type)為external的document已經成功建立在消費者(customer)索引(index)中。這個document也會有內部的唯一識別(_id)，此範例回應之唯一識別(_id)等於1, 這個內部id也是我們在建立index的時候指定的.

有一點重要的是ElasticSearch並不要求在處理文件(document)前要先建立index，也就是說如果之前index不存在，Elasticsearch會自動建立一個命令提到的index，再執行命令中的操作。

剛剛已經完成建一個document, 接著來探索剛建立的document, 一樣使用curl命令, 透過XGET存取Elasticresearch的REST API.

命令：

    curl -XGET 'localhost:9200/customer/external/1?pretty'

回應：

    {
        "_index" : "customer",
        "_type" : "external",
        "_id" : "1",
        "_version" : 1,
        "found" : true, "_source":{ "name": "kedy chang"}
    }


沒有太特別的欄位，從回應可以看到，使用的索引(_index)、類型(_type)與唯一識別(_id)，而"found"欄位，顯示命令送出的結果為"true"，代表有順利取得資料，"_source"，也就是我們前一步驟中建立的完整JSON document。

#刪除一個索引(Delete an index)

前面知道如何新增索引(index)、建立文件(document)、取回文件(document)，接著測試如何刪除一個索引(index)。

也是使用CURL透過REST API對叢集進行操作，帶的參數是 XDELETE，如下：

命令：

    curl -XDELETE 'localhost:9200/customer?pretty'
    
回應：


    {
    "acknowledged" : true
    }

刪除完畢以後列出索引(index)。

命令：

    curl 'localhost:9200/_cat/indices?v'

回應：

    [kedy@es1 ~]$ curl -XGET 'localhost:9200/_cat/indices?v'
    health status index    pri rep docs.count docs.deleted store.size pri.store.size
    yellow open   kedytest   5   1          0            0       780b           780b

就只剩下剛剛另外建立的kedytest index, 在前述命令指定刪除的customer index已經不見了


這時候複習剛剛的建立index跟document還有刪除的命令，使用的命令如下：

建立"customer" index

    curl -XPUT 'localhost:9200/customer'


建立id 1的document, 欄位為name: kedy chang, type為external

    curl -XPUT 'localhost:9200/customer/external/1' -d '
    {
    "name": "kedy chang"
    }'
    
瀏覽index為customer, type為external, id為1的document
    
    curl 'localhost:9200/customer/external/1'
    
刪除customer　index

    curl -XDELETE 'localhost:9200/customer'
    
最後歸納出來，簡單的操作模式為：

    curl -X<REST Verb> <Node>:<Port>/<Index>/<Type>/<ID>



#修改資料(Modifying Your Data)

##索引與取代documents

以下命令是插入一個document

    curl -XPUT 'localhost:9200/customer/external/1?pretty' -d '
    {
        "name": "kedy chang"
    }'

接著我們指定相同的id, 再插入一次

    curl -XPUT 'localhost:9200/customer/external/1?pretty' -d '
    {
        "name": "joe chen"
    }'


這時候指定id之document裡面的name欄位, 就會從"kedy chang"被替換成"joe chen"
再仔細觀察回應, 也可以看到版本編號有異動：

    [kedy@es1 ~]$ curl -XGET 'localhost:9200/customer/external/1?pretty'
    {
        "_index" : "customer",
        "_type" : "external",
        "_id" : "1",
        "_version" : 4,
        "found" : true,
        "_source":{ "name": "joe chen"}
    }


上面是我們更新同樣id的document, 如果我們更改了id, 

        curl -XPUT 'localhost:9200/customer/external/2?pretty' -d '
    {
        "name": "joe chen"
    }
    
Elasticsearch就會新建立一個document

上面的例子我們都有指定id, 如果不指定的話, 要改用 -XPOST, 如此一來, Elasticsearch就會隨機產生一個ID.

例如:

    curl -XPOST 'localhost:9200/customer/external?pretty' -d '
    {
        "name": "Fish Chang"
    }'

記得這種情況要使用的方法是 -XPOST 而不是 -XPUT

#更新Documents
為了要能夠索引跟取代documdents, 我們必須要能能夠更新documents 記得, 在Elasticsearch之中, 取代不是真的直接更新, 而是刪除舊的documents, 再重新索引新document進去.

以下範例顯示如何更新已存在且id為1的document之中的姓名欄位, 改成"Joe Chen"

    curl -XPOST 'localhost:9200/customer/external/1/_update?pretty' -d '
    {
        "doc": { "name": "Joe Chen" }
    }'

以下範例顯示如何更新已存在且id為1的document之中的姓名欄位為"Joe Chen", 同時也增加年紀欄位

    curl -XPOST 'localhost:9200/customer/external/1/_update?pretty' -d '
    {
        "doc": { "name": "Joe Chen", "age": 30 }
    }'

Update也可以使用簡單的script執行. 注意, 動態的script(如下範例)預設是將script的支援關閉的. 詳系可以參考 [scripting docs](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/modules-scripting.html). 下面範例是將年紀增加5:

    curl -XPOST 'localhost:9200/customer/external/1/_update?pretty' -d '
    {
        "script" : "ctx._source.age += 5"
    }'
    
上述範例中, ctx._source 參考文件還在等待Elastic官方更新. 在此文件撰寫的同時,  Elasticsearch的更新功能一次只能針對一個document進行, 未來Elasticsearch也許會提供針對多document的query condition的能力, 類似SQL的UPDATE-WHERE敘述一樣.