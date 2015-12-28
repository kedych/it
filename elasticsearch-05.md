# ElasticSearch - 改變日誌設定

標準的日誌紀錄設定已符合絕大多數的用途，改變或調整日誌層級(log level)主要在檢查bugs或了解在錯誤組態或奇怪的plugin狀態時候的異常排除特別有用。詳細日誌(verbose log)可以用在像Elasticsearch社群回報問題的時候使用。

如果我們需要自行除錯Elasticsearch或者改變日誌層級，就需要改變config/logging.yml之中的參數。

在Elasticsearch安裝目錄下的config目錄，有一個logging.yml，此檔案包含所有的工作日誌設定。

調整日誌設定步驟如下：

1. 忽略所有ElasticSearch有的日誌種類，例如想要採取root level的日誌方式，打開logging.yml找到原本的：


    rootLogger: INFO, console, file
    
要改成root level的話，要改成：

    
    rootLogger: DEBUG, console, file
    
2. 接著透過命令列執行ElasticSearch，就可以看到很多垃圾文字(詳細的訊息)，大概長這樣：


    […][INFO ][node1] [ES_cluster] version[2.1.1],pid[12343], build[1c22558/2015-12-28T14:58:15Z]
    […][INFO ][node1] [ES_cluster] initializing …
