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

