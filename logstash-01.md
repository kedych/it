# Logstash - Installation

取得Logstash，請確認要使用的版本號：

    sudo wget https://download.elastic.co/logstash/logstash/logstash-2.1.1.tar.gz
    
解壓縮檔案

    sudo tar zxvf logstash-2.1.1.tar.gz
    
    
切換到/etc/openssl/tls/目錄，製作憑證與金鑰

    sudo openssl req -subj '/CN=es1.niucloud.niu.edu.tw' -x509 -days 3650 -batch -nodes -newkey rsa:2048 -keyout private/logstash-forwarder.key -out certs/logstash-forwarder.crt
