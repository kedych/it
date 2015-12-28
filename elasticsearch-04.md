# ElasticSearch - 組態與叢集

## 組態檔案
主要設定檔有兩個，分別是：

* config/elasticsearch.yml
* config/logging.yml

顧名思義，就是主要的設定檔案與日誌設定檔案。

小提示：為了避免每次ElasticSearch版本升級都要重新設定組態檔，把檔案放在ElasticSearch目錄之外是不錯的選擇。

在主要的設定檔elasticsearch.yml之中，有一些重要的參數：
* path.work 指定路徑 ElasticSearch 存放暫存檔。
* path.log 指定日誌放置路徑，整體日誌配置是由logging.yml處理
* path.plugins讓你配置外掛路徑。 

用來控制索引shard數量的參數是：
* index.number_of_shards

而控制複本(replica)數量的參數是:
* index.number_of_replicas

## Linux下的參數設定

1. /etc/security/limits.conf
增加限制參數

    elasticsearch - nofile 299999
    elasticsearch - memlock unlimited


2. 控制記憶體swapping


    vim elasticsearch.yml:
    bootstrap.mlockall: true

The standard limit of file descriptors (the maximum number of open files for a user) is typically
1,024. When you store a lot of records in several indices, you run out of file descriptors very
quickly, so your ElasticSearch server becomes unresponsive and your indices might become
corrupted, leading to a loss of data. If you change the limit to a very high number, your
ElasticSearch server doesn't hit the maximum number of open files.
The other settings for memory restriction in ElasticSearch prevent memory swapping and
give a performance boost in a production environment. This is required because during
indexing and searching ElasticSearch creates and destroys a lot of objects in the memory.
This large number of create/destroy actions fragments the memory, reducing performance:
the memory becomes full of holes, and when the system needs to allocate more memory,
it suffers an overhead to find compacted memory. If you don't set bootstrap.mlockall:
true, then ElasticSearch dumps the memory onto a disk and defragments it back in the
memory, which freezes the system. With this setting, the defragmentation step is done in
the memory itself, providing a huge performance boost.

如果沒設定這個參數， ElasticSearch在dump記憶體資訊到硬碟的時候，可能會造成系統凍結(freeze)，而影響效能。   

3.固定記憶體使用量

    ES_MIN_MEM and ES_MAX_MEM


##設定不同類別的節點
因為ElasticSearch原本就設計在雲端環境，可以在大量的紀錄(record)中進行快速的搜尋和資料處理，當環境具有大量紀錄時，我們會需要整體ElasticSearch具備高可用與良好的效能，此時，就需要在叢集中加入更多的節點。

ElasticSearch讓管理者能彈性配置不同類型的節點，一樣使用

    config/elasticsearch.yml
    
進行節點的設置，例如：
1.是否要讓節點能擔任master node。
    
    node.master: true
        
在雲端環境中master node就是仲裁者，會負責shard管理，確認叢集情況、以及做為每個索引行為的主要控制者，本參數預設值是true。

有一個建議的master node數量，就是整個叢集節點數量除以二再加一

    Number of Master node = (Number of nodes)/2 + 1

    
2.是否要讓節點儲存資料

    node.data: true
    
node.data決定是否要讓節點儲存資料，本參數預設值也是true，這個節點就會在所引與搜尋資料的時候，加入與分擔工作。


上述兩個參數的組合，可以組合出四種不同的結點類型。

|node.master|node.data|節點描述|
|-|-|-|
|true|true|此為預設值，一個結點可以擔任master node也可以儲存資料。|
|false|true|本節點永遠不做為master node，可以當作叢集中工作的一部分，類似workhouse|
|true|false|本節點只擔任master node而不儲存資料，擔任叢集中協調者、仲裁者的角色。|
|false|false|不存資料也不當master node，只當做搜尋的負載平衡節點， 例如從其他結點截取資料、聚合結果(aggregate result)之類的工作。|

最常使用的節點類型參數是node.master=true與node.data=true，但如果有非常大規模的叢集或特殊需求，就可以自行決定節點類型進行差異化配置，來達到比較好的搜尋與聚合結果。

## 運算行為
ElasticSearch為了確保各種運作(包含 在index/mapping/object行為)的安全性，內部有一些規則，定義好如何執行運算，運算大致有：

1. Cluster/index operations: 所有的叢集/索引 的主動寫入預設是被鎖住的，首先會先套用在master node，之後才會套用到其他的secondary nodes。
1. Document operations: 所有的寫入動作也是鎖住的，除了在single hit shard之外。讀取運算會在所有的shard replicas之間平衡。

## 叢急組態

雖然文件寫得很神, 只要在啟動的時候給予叢集名稱(cluster.name)與結點名稱(node.name),,在AWS上, ElasticSearch搜尋節點的時候, multicast也無法使用, 自己實測環境也沒有看到有封包出來,還是手動設定組態比較妥當...

如果要強制JAVA使用IPv4環境:
export _JAVA_OPTIONS="-Djava.net.preferIPv4Stack=true"

編輯組態檔案
    
    vim /opt/elasticsearch-2.1.1/config/elasticsearch.yml


master node:

    cluster.name: "es_c"
    node.name: "node1"
    node.master: true
    node.data: false
    discovery.zen.ping.timeout: 10s
    discovery.zen.ping.multicast.enabled: false
    discovery.zen.ping.unicast.hosts: ["masternode.niucloud.niu.edu.tw"]
    network.host: _ens32_

data node:

    cluster.name: "es_c"
    node.name: "node2"
    node.master: true
    node.data: false
    discovery.zen.ping.timeout: 10s
    discovery.zen.ping.multicast.enabled: false
    discovery.zen.ping.unicast.hosts: ["masternode.niucloud.niu.edu.tw"]
    network.host: _ens32_
    
如此, 啟動之後可以看到順利加入cluster的訊息


    [kedy@es1 bin]$ ./elasticsearch
    Picked up _JAVA_OPTIONS: -Djava.net.preferIPv4Stack=true
    [2015-12-22 16:23:10,223][INFO ][node                       ] [node1] version[2.1.1], pid[15696], build[40e2c53/2015-12-15T13:05:55Z]
    [2015-12-22 16:23:10,224][INFO ][node                     ] [node1] initializing ...
    [2015-12-22 16:23:10,346][INFO ][plugins                  ] [node1] loaded [], sites [head]
    [2015-12-22 16:23:10,368][INFO ][env                      ] [node1] using [1] data paths, mounts [[/ (rootfs)]], net usable_space [44.9gb], net total_space [49.9gb], spins? [unknown], types [rootfs]
    [2015-12-22 16:23:12,128][INFO ][node                     ] [node1] initialized
    [2015-12-22 16:23:12,128][INFO ][node                     ] [node1] starting ...
    [2015-12-22 16:23:12,190][INFO ][transport                ] [node1] publish_address {203.145.207.213:9300}, bound_addresses {203.145.207.213:9300}
    [2015-12-22 16:23:12,200][INFO ][discovery                ] [node1] es_c/8fea1CyuR4yn5GWZwkrAPA
    [2015-12-22 16:23:22,238][INFO ][cluster.service          ] [node1] new_master {node1}{8fea1CyuR4yn5GWZwkrAPA}{203.145.207.213}{203.145.207.213:9300}{data=false, master=true}, reason: zen-disco-join(elected_as_master, [0] joins received)
    [2015-12-22 16:23:22,254][INFO ][http                     ] [node1] publish_address {203.145.207.213:9200}, bound_addresses {203.145.207.213:9200}
    [2015-12-22 16:23:22,254][INFO ][node                     ] [node1] started
    [2015-12-22 16:23:22,299][INFO ][gateway                  ] [node1] recovered [0] indices into cluster_state
    [2015-12-22 16:23:25,060][INFO ][cluster.service          ] [node1] added {{node2}{fyXz_OPURPeyatd6AI5qUA}{203.145.207.214}{203.145.207.214:9300}{data=false, master=false},}, reason: zen-disco-join(join from node[{node2}{fyXz_OPURPeyatd6AI5qUA}{203.145.207.214}{203.145.207.214:9300}{data=false, master=false}])
    [2015-12-22 16:23:27,266][INFO ][cluster.service          ] [node1] added {{node3}{PC8jRy67SXu_P2zWezRUEQ}{203.145.207.215}{203.145.207.215:9300}{data=false, master=false},}, reason: zen-disco-join(join from node[{node3}{PC8jRy67SXu_P2zWezRUEQ}{203.145.207.215}{203.145.207.215:9300}{data=false, master=false}])
 

##安裝plugin
ElasticSearch有一個特色是可以安裝各式各樣的plugin，不同的外掛能透過不同的方法延伸ElasticSearch的特色和功能。在ElasticSearch之中，大致分成兩類不同的外掛：

### Site plugins
在進入點(entry points)進行靜態內容服務的外掛。主要用途為建立管理應用程式，用來進行叢集的監控和管理。常見的site plugin有

* ElasticSearch head (http://mobz.github.io/elasticsearch-head/)
* Elastic HQ (http://www.elastichq.org/)
* Bigdesk (http://bigdesk.org

### Native plugins
這些是包含應用程式碼的 .jar 檔案，內容有：

* Rivers (plugins that allow you to import data from DBMS or other sources)
* Scripting Language Engines (JavaScript, Python, Scala, and Ruby)
* Custom analyzers, tokenizers, and scoring
* REST entry points
* Supporting new protocols (Thrift, memcache, and so on)
* Supporting new storages (Hadoop)

##如何安裝plugin

在bin/目錄下有plugin的執行檔，用來管理ElasticSearch的外掛，先以ElasticSearch header為範例：

###ElasticSearch Header Plugin
上面提到的外掛有很多種，剛好建立了叢集，可以安裝ElasticSearch的外掛, 透過web介面直接觀看叢集狀態


    $bin/plugin --install mobz/elasticsearch-head

裝好之後透過

    http://hostname:9200/_plugin/head/

就可以看到叢集資訊畫面
    