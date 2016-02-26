# Filter
以下針對幾個常用的做解說:
* Grok
* Geoip
* Mutate
* Date
* Multiline
* Syslog_pri


##Grok
把非結構化的log資料做解析，拆解出對應的log欄位，例如:

log資料為
```
55.3.244.1 GET /index.html 15824 0.043
```
所以我們可以再grok內容寫到match的message中，有哪些欄位做對應
```
    filter {
        grok { 
            match => { 
                "message" => %{IP:client} %{WORD:method} %{URIPATHPARAM:request} %{NUMBER:bytes} %{NUMBER:duration} } }
    }
```
  
結果是:
```
    {
        "client": ["55.3.244.1"],
        "method": ["GET"],
        "request": ["/index.html"],
        "bytes": ["15824"],
        "duration": ["0.043"]
    }
```
Note:

可以用Grok Debugger來除錯(http://grokdebug.herokuapp.com/)

教學網站:https://www.youtube.com/watch?v=YIKm6WUgFTY

##Date
date為事處理時間的函式，可以轉換日誌記錄中的時間字串

日誌記錄中的時間字串，變成LogStash::Timestamp 對象，然後轉存到 @timestamp 字段裡。