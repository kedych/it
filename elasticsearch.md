# 搜尋引擎與日誌分析

##名詞與注意

|Term|說明|
|-|-|
|Cluster|整體儲存運算架構 用名稱來識別 預設名稱為elasticsearch|
|Node|叢集中的節點|
|Index| 文件蒐集的集合，名稱全部要小寫，在叢集架構中，index要定義多少就定義多少。|
|Type|一個index可以定義一個以上的Type。Type是index易識別語意的邏輯分類、分割。|
|Document|可以被index的最小資訊單位，以JSON格式存在。|
|Shards & Replicas|大型index可以被切割成很多塊，稱為Shards。建立index的時候，可以定義想要的shards數量，每個shard可以被想成叢集任一節點的獨立index。|


Sharding的理由：
* 可以依照內容的量進行平行分割/擴展。
* 可以分散與平行化處理，增加效能與吞吐量。

## Reference
* [Basic Concept](https://www.elastic.co/guide/en/elasticsearch/reference/current/_basic_concepts.html)

