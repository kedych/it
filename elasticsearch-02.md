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

    wget https://download.elasticsearch.org/elasticsearch/release/org/elasticsearch/distribution/tar/elasticsearch/2.0.0/elasticsearch-2.1.1.tar.gz

解壓縮

    tar -zxvf elasticsearch-2.1.1.tar.gz

切目錄

    cd elasticsearch-2.1.1/bin/

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

