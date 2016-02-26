# Filter

##grok
把非結構化的log資料做分析
ex: 
log為55.3.244.1 GET /index.html 15824 0.043
pattern為%{IP:client} %{WORD:method} %{URIPATHPARAM:request} %{NUMBER:bytes} %{NUMBER:duration}
結果是:
{
"client": [
"55.3.244.1"
],
"method": [
"GET"
],
"request": [
"/index.html"
],
"bytes": [
"15824"
],
"duration": [
"0.043"
]
}

##date
date為事處理時間的函式，可以轉換日誌記錄中的時間字串

日誌記錄中的時間字串，變成LogStash::Timestamp 對象，然後轉存到 @timestamp 字段裡。