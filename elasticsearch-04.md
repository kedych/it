# ElasticSearch - 叢集組態


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

date node:

    cluster.name: "es_c"
    node.name: "node2"
    node.master: true
    node.data: false
    discovery.zen.ping.timeout: 10s
    discovery.zen.ping.multicast.enabled: false
    discovery.zen.ping.unicast.hosts: ["masternode.niucloud.niu.edu.tw"]
    network.host: _ens32_
    
如此, 啟動
