# Day 2

CCR D2

Review 
Splunk
CSIRT 人 流程 事情

#Session 1
##ASA and SourceFire Firewall, 
##NGFW, NGIPS

需要具備的能力 而非特定產品
以ASA & SourceFire為例子
這些設備能給你什麼樣的可見度

不能預防發生 但是要假設已經發生但是我們沒有看到 沒有看到就不知道 我們要說一旦安全事件發生 導致沒有可見度 在發生的時候 和發生之後 做了什麼事情
要知道scope影響有多大

page 4點出各個不同階段需要具備的能力和功能 是一個新的安全模型
Advanced Malware Protection 在after階段

###ASA Overview
NG下一代防火牆的意義 跟現在最大的區別 在於 "應用程式" (多看了payload)的感知 包含了web應用 IDentity以及各式各樣的應用, 把ID跟application結合做策略

Cisco Talos 思科的安全部門 蒐集攻擊資訊 IPS規則更新等 ex: ASA有C&C server list

對flow或connection的偵測與控制

防火牆部署模式
Routed Mode
Transparent Mode - 類似switch的應用
Multi-context - 虛擬化之後的應用

未來也要支援 Global ACL (聽說是為了吸引某家廠商的客戶)

connection table
at ASA, use 
  show conn
 
###下一代防火牆如果連接數很多需要大量記憶體, 
因為要紀錄連接狀態

另一塊是NAT , Xlate table stores active NAP mappings
靜態NAT 比較省記憶體
動態NAT 比較花費記憶體跟更多資源 因為要紀錄多對一的狀態

###Layer 7 inspection
ASA也可以針對各種應用和規則進行配置(ex: SMTP附加檔案, HTTP傳檔..)

###用正規表示做URI&host match
regex BAD_URI ".*verybadscript.*"
regex BAD_HOST "verybadsite\.com"

class-map type inspect http match-all BLOCK_URL
match request uri regex BAD_URI
match request header host regex BAD_HOST

policy-map global_policy
 class inspection_default
   inspect http URL_POLICY
service-policy global_policy global   

如果沒做好 可能會吃很多的CPU使用量或者不小心阻擋全部流量... 所以為了達成目的怎麼選擇工具就是需要自己評斷

###Threat Detection
很多NGFW都有, 但是大家都沒什麼在用...

封包如果被丟掉, 但後續有沒有繼續folow up, 例如drop rate忽然很高, 是不是要去細看原因?
當drop rate達到一個數量的時候發出SNMP告警.

如果有人做port scans 也要可以封鎖並且告警, 但是打開偵測scan activatiy會很吃CPU跟記憶體, 對防火牆來說是一個很大的負擔, 平常如果防火牆很忙碌, 不要開比較好.... Orz 


###NetFlow Secure Event Loggin (NSEL)
NetFlow=網路流量的帳單

是從Flow Change狀態開始紀錄

syslog vs flow看要收哪個 收兩個或收一個都可 依照自己的狀況判斷

###Inline Packet Capture
防火牆應該都要有的功能 直接擷取封包 

##SourceFire (NGIPS)
基於snort開發的硬體, 有以下capability:
snort - 偵測snort rule - 雲端:規則更新
FireSIGHT- 探索event, 包含hosts, user, os, services, vulnerabilityes等, 偵測設備版本與弱點特徵符合會告警, 雲端:弱點更新 作業系統定義
AppID - 傳統client server架構的事件, web apps等, 雲端:應用app定義 偵測
Files - 文件類型與檔案傳輸做處理, 雲端:malware cloud lookups (AMP), sandbox, Trajectories
L2/L3 -  connection logs, flows, 雲端:安全智慧IP聲譽, URL分類與更新等.

###Advanced Malware Protection, AMP
http://www.cisco.com/c/en/us/products/security/advanced-malware-protection/index.html
類似malware的virus total, 由Cisco主導, 已知文件會告訴你分數, 如果是新的malware, 會送到sandbox (AMP Threat Grid)針對行為進行判斷給分.

檔案會穿過的地方都可以考慮使用AMP - AMP everywhere

###Context through FireSIGHT
###indicators Of Compromise, IOC
各種活動與單獨的事件組成綜合評分 判斷是否有危害的可能




