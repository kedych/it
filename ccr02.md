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