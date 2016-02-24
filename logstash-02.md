# Logstash - Getting Data

安裝完成以後，要知道如何使用Logstash進行資料蒐集。

簡單來說，Logstash蒐集處理資料分三個階段，分別為:

* Input - 告訴Logstash蒐集資料的來源
* Filter - 對資料進行中間過程的處理
* Output - 輸出資料

整個Logstash具有相當的歷史和多種變化方式，在此先以主要使用的內容進行說明。

設定Logstash組態檔, 配置在

    /etc/logstash.conf

整個組態檔案大略有三個區塊，對應到蒐集處理資料的三大階段。

## Input
告訴Logstash蒐集資料的來源，我們使用的情境，Input來源有file (讀取特定路徑下的檔案)以及TCP Connection送入的資訊(例如rsyslog傳來的資料)。在第一階段

## Filter

中間的處理是最需要花時間規劃的地方，正確的欄位與資料識別有注於後續使用Elasticsearch和Kibana進行分析。
Filter之中，先使用

## Output
情境就直接送到Elasticsearch，先不做太大的著墨， 未來應可嘗試轉向自己的配置。


## 啟動Logstash

我們建立的服務需要長期執行，透過以下的方式進行:

    nohup command &> /dev/null

## Logstash範例 - IIS5蒐集日誌

