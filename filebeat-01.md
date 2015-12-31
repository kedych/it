# Filebeat - Installation

以前要傳送日誌到logstash是使用logstash forwarder，現在都已經改用新的Filebeat。

我們先試著在Windows 7上使用Filebeat傳送日誌到我們建立的logstash，再傳送到ElasticSearch叢集。

Windows版本可以從這裡下載:
https://download.elastic.co/beats/filebeat/filebeat-1.0.1-windows.zip

解壓縮以後可以看到好幾個檔案，就先放到C:

* filebeat.exe
* filebeat.template.json
* filebeat.yml
* install-service-filebeat.ps1
* uninstall-service-filebeat.ps1