# Logstash - Installation

取得Logstash，請確認要使用的版本號：

    sudo wget https://download.elastic.co/logstash/logstash/logstash-2.1.1.tar.gz
    
解壓縮檔案

    sudo tar zxvf logstash-2.1.1.tar.gz
    
    
切換到/etc/openssl/tls/目錄，製作憑證與金鑰

    sudo openssl req -subj '/CN=es1' -x509 -days 3650 -batch -nodes -newkey rsa:2048 -keyout private/logstash-forwarder.key -out certs/logstash-forwarder.crt

建立Logstash組態檔案

    input {
        lumberjack {
            port => 5000
            type => "logs"
            ssl_certificate => "/etc/pki/tls/certs/logstash-forwarder.crt"
            ssl_key => "/etc/pki/tls/private/logstash-forwarder.key"
            }
    }


    filter {
        if [type] == "syslog" {
                grok {
                    match => { "message" => "%{SYSLOGTIMESTAMP:syslog_timestamp} %{SYSLOGHOST:syslog_hostname} %{DATA:syslog_program}(?:\[%{POSINT:syslog_pid}\])?: %{GREEDYDATA:syslog_message}" }
                    add_field => [ "received_at",  "%{@timestamp}" ]
                    add_field => [ "received_from", "%{host}" ]
                }
            syslog_pri { }
        date {
            match => [ "syslog_timestamp", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
        }
    }
}

output {
  elasticsearch { host => localhost }
  stdout { codec => rubydebug }
}