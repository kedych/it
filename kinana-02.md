# 基本搜尋

by Fish Chang

##一、全文搜尋
kedy
"kedy-ch"

##二、字段
field:value
filed:"value"
ex: message:9200
message:"TCP-9200"

##三、匹配
?匹配單一字
匹配0到多個字
ex: ke?y
ky
note:?*不能做第一個字母使用

##四、正則表示式(不太懂)
mesg:/mes{2}ages?/

##五、模糊搜尋
~:單字後加上~
ex: kedy~

##六、近似搜尋(不太懂)
在短語後面加上〜
“select where”~3選擇表示select 和where 中間隔著3個單詞以內

##七、範圍搜尋(不太懂)
數值和時間類型的字段可以對某一範圍進行查詢
length:[100 TO 200]
date:{"now-6h" TO "now"}
[ ] :表示端點值包含在範圍內
{ } :表示端點值不包含在範圍內

##八、邏輯操作
AND
OR
+:搜尋結果必須包含此項
-:搜尋結果不能含有此項
ex: +apache -jakarta test
(必須存在apache ，不能有jakarta，而test可有可無)

##九、分組
(jakarta OR apache) AND jakarta

##十、字段分組(不太懂)
title:(+return +"pink panther")

##十一、轉義特殊符號(不太懂)
+ - && || ! () {} [] ^" ~ * ? : \
以上符號當作值搜尋時須透過\間隔


##參考

https://segmentfault.com/a/1190000002972420