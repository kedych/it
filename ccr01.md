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

實用資訊:

* ISC DNSDB 反解隨機產生的hostname
* Virustotal

簡報有CSIRT response流程的例子


###SIEM ovew and introduction

Wei Wang
Network Consultant Engineer

###What is a SIEM


###OpenSOC
Big data platform
  hadoop
  storm
  kafka
  elasticsearch
Cisco開發整合以上工具 可以看看OpenSOC 是一個open source project

###conceptual architecture
1. 首先蒐集secutiry event (Log, syslog, raw network steam)
2. 解析、格式、加值賦予意義(ex: MAC查是誰)
3. 應用+分析

OpenSOC是Cisco自己內部在用, 後來open出來

###Splunk
原本Cisco想買splunk, 但是後來沒下文, 現在要買就很難買得起了...

###部署考慮
####data input
Logging level / verbosity 看要收到多少層級(Information or other? debug太多錢了..xD)
Cisco的firewall log level只收到level 5 (warning), 反正就依照狀況再去調...
####index tier
raw log vs cooked data

####indexers
###admin 

###SANS Top 6 categories of Critical Log information
一個SOC應該要注意的六類重大日誌資訊, 包含
1. 授權認證(authentication and authorization)
2. 系統與日期改變(system and date changes)
3. 網路調整(network activity)
4. 資源存取(resource access)
5. 惡意程式活動(malware activity)
6. 失效與重大錯誤(Failure and Critical Errors)
簡單來說就是不該進來的人做了不該做的事情, 以上這六大類可以發現約九成以上的網路安全問題

####Splunk CIM (common information model)
####我們要看什麼？透過歷史紀錄建立baseline, 之後很好查abnormal


###整合
###如何開始觀察
其實完全看自己 沒有一個標準 自己找方法 沒有一定
open mind, anything is possible

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
