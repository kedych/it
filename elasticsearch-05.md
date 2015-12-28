# ElasticSearch - 改變日誌設定

標準的日誌紀錄設定已符合絕大多數的用途，改變或調整日誌層級(log level)主要在檢查bugs或了解在錯誤組態或奇怪的plugin狀態時候的異常排除特別有用。詳細日誌(verbose log)可以用在像Elasticsearch社群回報問題的時候使用。

如果我們需要自行除錯Elasticsearch或者改變日誌層級，就需要改變config/logging.yml之中的參數。

在Elasticsearch安裝目錄下的config目錄，有一個logging.yml，此檔案包含所有的工作日誌設定。

調整日誌設定步驟如下：

1. 忽略所有ElasticSearch有的日誌種類，例如想要採取root level的日誌方式，打開logging.yml找到原本的：


    es.logger.level: INFO

    
要改成root level的話，要改成：

    es.logger.level: DEBUG

    
2. 接著透過命令列執行ElasticSearch，就可以看到很多垃圾文字(詳細的訊息)，大概長這樣：



    [2015-12-28 18:01:50,543][DEBUG][cluster.service          ] [node1] processing [gateway_initial_state_recovery]: execute
    [2015-12-28 18:01:50,545][DEBUG][cluster.service          ] [node1] processing [gateway_initial_state_recovery]: took 1ms no change in cluster_state
    [2015-12-28 18:01:50,554][DEBUG][http.netty               ] [node1] Bound http to address {203.145.207.213:9200}
    [2015-12-28 18:01:50,555][DEBUG][http.netty               ] [node1] Bound http to address {[2001:e10:1440:ffff:20c:29ff:fe67:db53]:9200}
    [2015-12-28 18:01:50,559][DEBUG][http.netty               ] [node1] Bound http to address {[fe80::20c:29ff:fe67:db53]:9200}
    [2015-12-28 18:01:50,560][INFO ][http                     ] [node1] publish_address {203.145.207.213:9200}, bound_addresses {203.145.207.213:9200}, {[2001:e10:1440:ffff:20c:29ff:fe67:db53]:9200}, {[fe80::20c:29ff:fe67:db53]:9200}
    [2015-12-28 18:01:50,560][INFO ][node                     ] [node1] started
    [2015-12-28 18:01:50,563][DEBUG][cluster.service          ] [node1] processing [local-gateway-elected-state]: execute
    [2015-12-28 18:01:50,564][DEBUG][cluster.service          ] [node1] cluster state updated, version [2], source [local-gateway-elected-state]
    [2015-12-28 18:01:50,564][DEBUG][cluster.service          ] [node1] publishing cluster state version 2
    [2015-12-28 18:01:50,564][DEBUG][cluster.service          ] [node1] set local cluster state to version 2
    [2015-12-28 18:01:50,585][INFO ][gateway                  ] [node1] recovered [0] indices into cluster_state
    [2015-12-28 18:01:50,585][DEBUG][cluster.service          ] [node1] processing [local-gateway-elected-state]: took 21ms done applying updated cluster_state (version: 2, uuid: j0qgVGxNQZ-3oIw53f17bg)
