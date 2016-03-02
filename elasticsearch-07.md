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


這時候指定id之document裡面的name欄位，就會從"kedy chang"被替換成"joe chen" 再仔細觀察回應，也可以看到版本編號有異動：

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

##更新Documents (Updating Documents)

為了要能夠索引跟取代documdents，我們必須要能夠更新documents。記得， 在Elasticsearch之中，取代不是真的直接更新，而是刪除舊的documents， 再重新索引新document進去.

以下範例顯示如何更新已存在且id為1的document之中的姓名欄位, 改成"Joe Chen"

    curl -XPOST 'localhost:9200/customer/external/1/_update?pretty' -d '
    {
        "doc": { "name": "Joe Chen" }
    }'

以下範例顯示如何更新已存在且id為1的document之中的姓名欄位為"Joe Chen"， 同時也增加年紀欄位

    curl -XPOST 'localhost:9200/customer/external/1/_update?pretty' -d '
    {
        "doc": { "name": "Joe Chen", "age": 30 }
    }'

Update也可以使用簡單的script執行。注意，動態script(如下範例)，預設script的支援關閉的，詳系可以參考 [scripting docs](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/modules-scripting.html)。下面範例是將年紀增加5:

    curl -XPOST 'localhost:9200/customer/external/1/_update?pretty' -d '
    {
        "script" : "ctx._source.age += 5"
    }'
    
上述範例中，ctx._source 參考文件還在等待Elastic官方更新。在此文件撰寫的同時，Elasticsearch的更新功能一次只能針對一個document進行，未來Elasticsearch也許會提供針對多document的query condition的能力，類似SQL的UPDATE-WHERE敘述一樣。

## 刪除Documents (Deleting Documents)

在Elasticsearch中刪除Document還滿直覺的，以下範例顯示刪除index為消費者(customer)中類型(type)為external且ID為2的document。

    curl -XDELETE 'localhost:9200/customer/external/2?pretty'
    
如果需要大量刪除符合特定query的document，可以使用名稱為 delete-by-query 的plugin。

備註:此功能似乎被停用，待確認。

## 批次處理 (Batch Processing)

