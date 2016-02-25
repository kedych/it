#使用初探

##基於REST API的操作


#資料進入流程
當一筆record進入ElasticSearch的時候，目的shard會基於已下兩種方式選擇:

1. record ID 如果沒有指定ID，就會自動產生一個
1. 如有定義 routing 或 parent 參數，就會依照這些參數的hash值來選擇正確的shard。

##建立index
先來建立第一個index, 叫消費者"customer", 然後再試試看列出index會是什麼樣子.

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

從第二個命令的回應畫面, 可以看到目前有哪些index存在, 還有相關資訊, 例如多少個primary shards跟幾份replica, 還有包含多少document在理面.

在列出index的時候, 可以發現狀態的指示是yellow而非green, 這是因為replicas還沒被分配. 發生的原因是因為Elasticsearch預設只幫indexr建立一個replica, 所以這時候就只有一個replica, 除非有叢集有其它node加入, 　只要有新的node加入, 然後replica被分配到第二個節點的時候, index的健康狀態就會是green.



#索引與查詢文件(Index and Query a Document)

所有存放在ElasticSearch中的紀錄都是JSON物件。接下來開始放一些東西到消費者(customer)的索引(index)吧!為了要索引一個文件, 我們必須告訴Elasticsearch使用哪一種型態的索引.

索引一個簡單的消費者文件到到消費者索引中, 型態為"external", ID是1:

範例中的JSON document: {"name":"kedy chang"}

命令為：

    curl -XPUT 'localhost:9200/customer/external/1?pretty' -d '{ "name": "kedy chang"     }'
    

回應為：

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

從Elasticsearch的回應可以知道, 一個新的 customer document已經被成功建立在 customer index 而且型態為 external.　這個document也會有一個內部的id (此回應之範例為1), 這個內部id也是我們在建立index的時候指定的.

有一點很重要的是ElasticSearch並不要求說在所引文件之前一定要先建立index. 如果之前沒有index存在的話, Elasticsearch會自動建立一個消費者index.


剛剛已經完成建立document, 現在我們來探索剛才建立的document, 一樣使用curl, 使用XGET存取elasticresearch的REST API.

命令：

    curl -XGET 'localhost:9200/customer/external/1?pretty'

回應為：

    {
        "_index" : "customer",
        "_type" : "external",
        "_id" : "1",
        "_version" : 1,
        "found" : true, "_source":{ "name": "kedy chang"}
    }


沒有什麼特別的欄位，"found"，從這邊可以看到一個要求的"id"欄位1跟一個"_source"，也就是我們之前建索引的完整JSON document。

#刪除一個索引(Delete an index)

測試看看怎麼砍掉剛剛建立的index

命令：

    curl -XDELETE 'localhost:9200/customer?pretty'
    curl 'localhost:9200/_cat/indices?v'
    
回應：


    {
    "acknowledged" : true
    }

    [kedy@es1 ~]$ curl -XGET 'localhost:9200/_cat/indices?v'
    health status index    pri rep docs.count docs.deleted store.size pri.store.size
    yellow open   kedytest   5   1          0            0       780b           780b

就只剩下剛剛另外建立的kedytest index, 在前述命令指定刪除的customer index已經不見了


這時候我們複習剛剛的建立index跟document還有刪除的命令，使用的命令如下：

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