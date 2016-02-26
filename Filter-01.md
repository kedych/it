# Filter
以下針對幾個常用的做解說:
* Grok
* Date
* Geoip
* Mutate
* Multiline
* Syslog_pri


##Grok
把非結構化的log資料做解析，拆解出對應的log欄位，例如:

log資料為
```
55.3.244.1 GET /index.html 15824 0.043
```
所以我們可以在grok內容寫到match的message中，有哪些欄位做對應
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
date為事處理時間的函式，可以轉換日誌記錄中的時間字串。
日誌記錄中的時間字串，變成LogStash::Timestamp 對象，然後轉存到 @timestamp 欄位中。

```
ex:
   
    filter {
        grok {
            match => ["message", "%{HTTPDATE:logdate}"]
        }
        date {
            match => ["logdate", "dd/MMM/yyyy:HH:mm:ss Z"]
        }
    }
```

##Geoip
是ip address歸類查詢庫，根據ip address對應地域訊息(包含國家、省分和經緯度等)。
在一般情況下:

```
    filter {
        geoip {
            source => "message"
        }
    }
```
結果可能為
```
    {
        "message" => "183.60.92.253",
        "@version" => "1",
        "@timestamp" => "2014-08-07T10:32:55.610Z",
        "host" => "raochenlindeMacBook-Air.local",
        "geoip" => {
            "ip" => "183.60.92.253",
            "country_code2" => "CN",
            "country_code3" => "CHN",
            "country_name" => "China",
            "continent_code" => "AS",
            "region_name" => "30",
            "city_name" => "Guangzhou",
            "latitude" => 23.11670000000001,
            "longitude" => 113.25,
            "timezone" => "Asia/Chongqing",
            "real_region_name" => "Guangdong",
            "location" => [
                [0] 113.25,
                [1] 23.11670000000001
            ]
        }
    }
```
也能夠自己選擇想要顯示的欄位如下:
```
    filter {
        geoip {
            fields => ["city_name", "continent_code", "country_code2", "country_code3", "country_name", "dma_code", "ip", "latitude", "longitude", "postal_code", "region_name", "timezone"]
        }
    }
```
