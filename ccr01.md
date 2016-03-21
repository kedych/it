# Cisco Cyber Range
Speaker: Steve Lawford
Network Consulting Engineer

11 years
technoqal support
強調安全的重要性
第三天會有一些技巧

攻防演練 實際模擬 很多Lab要做

#agenda
每天4 session
#D1
##cyber range architecture and demo
介紹cyber range平台 三年前建置CCR 理由是 很多人有很多產品 有很多裝置 但是只有警告 但是不知道怎麼用 不知道怎麼正確使用 我們有很多log但是部知道怎麼看怎麼用 不知道什麼重要什麼不重要
所以透過這個平台有大量裝置讓大家能有觀念建立

透過資安認知 讓攻擊很難進來 進來卻很快被發現
攻擊不只在外部 因為BYOD 無法知道裝置是否安全有木馬 還有資料外洩問題 這是我們要面對網路資安變化
malware 社交工程等 我們必須要用更有效率的方法發現這些問題
我們必須要確保最小損壞 假設大家都已經被hack了

網路中藥有很多監控的裝置才能看到 可能的威脅 訂出正常異常的baseline

CCR平台在雪梨

CCR service pageages
三到五天的workshop
可以租用平台


CCR平台中攻擊的工具是open source工具跟商業產品混合使用

平台介紹完以後有Live demo...
ASA Web Email
更多內部攻擊細節演練 在五天的workshop會有更多介紹

Splunk有App: Cisco Security Suite (有很多領域的log分析)

分析的資料可能從SIEM來也有可能是各種設備

使用者netflow的監控或許可以考慮進行

從client/server上下傳比例的flow判斷是否可能有data loss情況

看到使用者的流量 內容 來做更多的分析組合和判斷

##CSIRT best practice introduction

介紹如何運作
範例事件
  惡意程式下載
  MAC木馬
  
用網頁廣告插入攻擊  malwaretizing (marlware+advertisement)
新興的攻擊方式 不是XSS

Netfolw for incident detection -> at 端口

Email URL check, attachment MD5 check

Lancope - Host quick view

監控site reputation連線狀況

減輕症狀

* 防火牆
其實是最少用的 因為ACL已經寫好 人家也攻進來了 所以沒差 而且從操作跟流程來說 防火牆在可操作性來說最低

* BGP blackhole
最自動化的阻擋方式 直接把該IP traffic資訊丟掉 <- is being to used most.

* DNS RPZ sinkhole

簡報有看到的命令

   ps auxww | grep -i zUser

ISC DNSDB 反解隨機產生的hostname

##SIEM ovew and introduction
splunk?
##分組 測試平台 工具介紹 攻防演練混合攻擊

#D2
##Firewall
##5 attack cased
##web security
##Email security

#D3
##cyber threat defense with ISE
##wireless security
##mixed attack
##competition between teams
