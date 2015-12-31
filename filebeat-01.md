# Filebeat - Installation

以前要傳送日誌到logstash是使用logstash forwarder，現在都已經改用新的Filebeat。



## Filebeat on Windows

我們先試著在Windows 7上使用Filebeat傳送日誌到我們建立的logstash，再傳送到ElasticSearch叢集。

Windows版本可以從這裡下載:
https://download.elastic.co/beats/filebeat/filebeat-1.0.1-windows.zip

解壓縮以後可以看到好幾個檔案，就先放到C:

* filebeat.exe
* filebeat.template.json
* filebeat.yml
* install-service-filebeat.ps1
* uninstall-service-filebeat.ps1


首先，在Windows的powershell，將filebeat安裝成服務。
第一次使用的時候，會有簽章問題而無法執行，所以先在powershell中，不限制

    Set-ExecutionPolicy Unrestricted
    [Y]

第二，執行安裝filebeat服務的ps，一樣在powershll中執行

    PS > cd 'C:\Program Files\Filebeat'
    PS C:\Program Files\Filebeat> .\install-service-filebeat.ps1

接著編輯 filebeat.yml

