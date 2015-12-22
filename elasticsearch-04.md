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