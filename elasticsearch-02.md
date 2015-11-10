# ElasticSearch - Installation


##Get OracleJDK

http://docs.oracle.com/javase/8/docs/technotes/guides/install/linux_jdk.html#BJFJHFDD

###移除舊的JDK
rpm -qa | grep jdk
yum remove openjdk....


###下載jdk-8u60-linux-x64.rpm

###安裝
rpm -ivh jdk-8u60-linux-x64.rpm

確認安裝成功
java -version
應該要看到

    java version "1.8.0_60"
    Java(TM) SE Runtime Environment (build 1.8.0_60-b27)
    Java HotSpot(TM) 64-Bit Server VM (build 25.60-b23, mixed mode)

###export $JAVA_HOME with JDK/JRE
編輯/etc/profile & $home/.bash_profile

加入以下

    export JAVA_HOME="/usr/java/latest"

存檔離開

###取得ElasticSearch
使用以下命令

    wget https://download.elasticsearch.org/elasticsearch/release/org/elasticsearch/distribution/tar/elasticsearch/2.0.0/elasticsearch-2.0.0.tar.gz

解壓縮

    tar -zxvf elasticsearch-2.0.0.tar.gz

切目錄

    cd elasticsearch-2.0.0/bin/

啟動elasticsearch (單節點)
    ./elasticsearch

但我們要有Cluster 所以改用以下命令

    ./elasticsearch --cluster.name my_cluster_name --node.name my_node_name

    ./elasticsearch --cluster.name [我的叢集名稱] --node.name [我的節點名稱]

本環境設定叢集名稱為 es_cluster (elasticsearch cluster)
節點分別為node1, node2, node3, 在三台機器分別的啟動命令為

./elasticsearch --cluster.name es_cluster --node.name node1

./elasticsearch --cluster.name es_cluster --node.name node2

./elasticsearch --cluster.name es_cluster --node.name node3

第一次跑失敗 因為elasticsearch不建議使用root執行

所以移動elasticsearch目錄到/opt/下面
再換一般使用者身分執行

還有權限問題 建議一開始下載elasticsearch使用一般使用者

應該就可以正常執行了

#叢集健康狀態

Elasticsearch使用REST API，可以得之叢集狀態與進行管理

預設通訊埠是9200 使用curl對本機9200發request即可
命令：
curl 'localhost:9200/_cat/health?v'

應該會得到類似的回應：

    [kedy@es1 ~]$ curl 'localhost:9200/_cat/health?v'
    epoch      timestamp cluster   status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shar ds_percent
    1446545469 18:11:09  escluster green           1         1      0   0    0    0        0             0                  -                 100.0%

##cluster狀態
命令：

    curl 'localhost:9200/_cat/nodes?v'


回應：

    [kedy@es1 ~]$ curl 'localhost:9200/_cat/nodes?v'

    host      ip        heap.percent ram.percent load node.role master name
    127.0.0.1 127.0.0.1            2          77 0.00 d         *      node1

##列出所有索引(List All Indices)
命令：

    curl 'localhost:9200/_cat/indices?v'

回應：

    health status index pri rep docs.count docs.deleted store.size pri.store.size

因為我們剛建立，所以目前沒有任何索引。

##建立index
先來建立第一個index, 叫消費者"customer", 然後再試試看列出index會是什麼樣子.

命令：

    curl -XPUT 'localhost:9200/customer?pretty'
    curl 'localhost:9200/_cat/indices?v'

結果：

    [kedy@es1 ~]$ curl -XPUT 'localhost:9200/customer?pretty'
    {
        "acknowledged" : true
    }

123    

    [kedy@es1 ~]$ curl 'localhost:9200/_cat/indices?v'
    health status index    pri rep docs.count docs.deleted store.size pri.store.size
    yellow open   customer   5   1          0            0       254b           254b

從第二個命令的回應畫面, 可以看到目前有哪些index存在, 還有相關資訊, 例如多少個primary shards跟幾份replica, 還有包含多少document在理面.

在列出index的時候, 可以發現狀態的指示是yellow而非green, 這是因為replicas還沒被分配. 發生的原因是因為Elasticsearch預設只幫indexr建立一個replica, 所以這時候就只有一個replica, 除非有叢集有其它node加入, 　只要有新的node加入, 然後replica被分配到第二個節點的時候, index的健康狀態就會是green.

#索引與查詢文件(Index and Query a Document)

階下來開始放一些東西到消費者(customer)的索引(index)吧!為了要索引一個文件, 我們必須告訴Elasticsearch使用哪一種型態的索引.

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

Reference data:
https://www.elastic.co/guide/en/elasticsearch/reference/2.0/_modifying_your_data.html
