# 搜尋引擎與日誌分析

##基本概念 Basic Concept

###名詞與注意

|Term|說明|
|-|-|
|Cluster|整體儲存運算架構 用名稱來識別 預設名稱為elasticsearch|
|Node|叢集中的節點|
|Index| 文件蒐集的集合，名稱全部要小寫，在叢集架構中，index要定義多少就定義多少。|
|Type|一個index可以定義一個以上的Type。Type是index易識別語意的邏輯分類、分割。|
|Document|可以被index的最小資訊單位，以JSON格式存在。|
|Shards |大型index可以被切割成很多塊，稱為Shards。建立index的時候，可以定義想要的shards數量，每個shard可以被想成叢集任一節點的獨立index。|
|Replicas|雲端環境中，節點失效是可預期的事情，所以把Shards製作一份或多份的複製就是replicas。|

Sharding理由：
* 可以依照內容的量進行平行分割/擴展。
* 可以分散與平行化處理，增加效能與吞吐量。

Replicas理由：
* 在shard/node失效的時候，提供高可用。
* 可以scale out搜尋效能與吞吐量，特別是可以在所有的replicas進行平行搜尋的時候。

總結，index可以被切分成多個shards，index也可以被複製0份或多份。一但使用replica，每個index都會有primary shards(原始replicat來源shard)與replica shards(被複製的primary shard)。

### Reference
* [Basic Concept](https://www.elastic.co/guide/en/elasticsearch/reference/current/_basic_concepts.html)

