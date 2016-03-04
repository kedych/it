# 應用分析

例如Log訊息為:
```
2016/2/4 �U�� 05:01:40 08F0 PACKET 0000000002F70FE0 UDP Rcv 120.101.0.1 ef80 Q [1000 NOERROR] A (3)es2(8)niucloud(3)niu(3)edu(2)tw(0)
```
透過
拆解欄位
```
%{DATA:date} %{GREEDYDATA:A1} %{TIME:time} %{WORD:A2} %{WORD:type} %{WORD:A3} %{WORD:protocal} %{WORD:A4} %{IP:client} %{WORD:A5} %{WORD:A6} %{SYSLOG5424SD:A7} %{WORD:A8} %{GREEDYDATA:A9}
```