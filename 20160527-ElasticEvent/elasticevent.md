# Elastic Event
2016-06 資訊管理部 張凱迪

##一、前言
Elasticsearch 為全世界最火熱即時搜索和分析資料引擎領導者
May 27, 2016　|　華山文創・台北

![Banner 1](20160527-ElasticEvent00.jpg)
![Banner 2](20160527-ElasticEvent01.jpg)

這麼強悍有霸氣的標語，能喊出來的應該沒幾個，而我相信Elastic真真切切有資格稱為即時搜尋和分析資料引擎的領導者。去年底因一些機緣，開啟了Elasticsearch、Logstash、Kibana的學習與使用過程，也開始關注ELK發展，上週某天忽然發現Elastic有場研討會在台北，速速點選報名，希望能有更伸更多的瞭解。

以下是些現場照片跟簡單的個人紀錄與心得分享，Elastic Stack能作的事情很多，繼續向前走，多學習多探索。

###二、議程摘要
以下是研討會議程，資料截自研討會官網：

####Elastic 市場策略及業務發展
主講人：何致偉　Elastic 亞太區銷售總監

####Elastic Stack + X-Pack
說明並窺視即將上市 Xpack，針對企業完整從日誌或檔案取得至分析呈現一鍵操作和管理，為企業創造最佳效率。

主講人：曾勇　Elastic 技術顧問

####HTC宏達電Use Case
HTC 宏達電將現身說法，分享行動裝置線上軟體更新、裝置管理、行為分析及研發生產自動化資訊架構與維運。

主講人：周鉦琪　HTC 資深副理

####整合Hadoop及Elasticsearch，加速大數據應用
大數據平台組件是很複雜且多元的，以 Hadoop 為 Data Lake 的中心，看看 ElasticStack 如何串接及應用。

主講人：陳進賢　歐立威技術協理

####ELK-統一資訊(PIC)日常系統維運的好幫手
統一資訊(PIC)如何運用 ELK於內部應用系統及對外營運系統的管理工作

主講人：劉祖弘　統一資訊平台技術 TEAM 工程師

####Log Analytics在金融與IOT的成功案例
分享銀行業如何利用 Log Analytics 成功收集銀行重要資訊，帶動銀行業務。以及在金融業機器如何藉由 Log Analytics 升級成高效智能的金融物聯網服務。

主講人：陳文裕　數位無限軟體總經理

###三、與會過程
只貼議程太敷衍了，議程中有很大部分是不同領域的專家，有一塊主講Hadoop結合Elasticsearch的應用，不同領域和專業的內容，不甚瞭解也無法全吸收，盡量看有記得什麼，中間穿插照片，請大家簡單配合心得服用囉！

(p.s. 不是我手殘發抖就是hTC M9的拍照不行，該換支手機了)


####Elastic 市場策略及業務發展

![](20160527-ElasticEvent.jpg)
亞太地區銷售總監何致偉介紹主要使用Elasticsearch的公司關鍵評語作為開場，用Elastic Stack架構的人其實很多，以下是ㄧ些知名企業使用的簡單評語，這些知名企業與短評如下：

![Facebook](20160527-ElasticEvent6.jpg)
1. Facebook - 一天處理超過6000萬個查詢，舉凡在Facebook的白色搜尋欄位中，找尋的項目都是透過Elasticsearch來進行的。

![theguardian](20160527-ElasticEvent7.jpg)
2. theguardian - 一天處理超過4000萬個文件，在全球的Web資訊進行即時分析。

![WikiMedia](20160527-ElasticEvent8.jpg)
3. WikiMedia - 提供靈活彈性的搜尋。

![verizon](20160527-ElasticEvent9.jpg)
4. verizon - 即時日誌與分析索引超過5兆筆記錄，用來完成極重要的應用。

![TomTom](20160527-ElasticEvent10.jpg)
5. TomTom - 有了Elastic Stack之後，Tomtom能夠處理監控每秒超過6500的訊息，獲得更深層維運訊息，來調整分散在全球的應用、網站跟伺服器。

![Mozilla](20160527-ElasticEvent11.jpg)
6. Mozilla - 每天使用ELK即時索引、搜尋、分析超過3億筆事件，保護網路、服務、和系統免於安全威脅。

![NASA](20160527-ElasticEvent12.jpg)
7. NASA - 有跟火星的設備在通訊，延遲30分鐘的即時分析(因為傳回來就要30分鐘)，用在最佳化太空任務。

![eBay](20160527-ElasticEvent13.jpg)
8. eBay - 透過Elasticsearch的幫忙，在全球的14個事業體服務2.5億位不同的使用者，購買5千萬個商品，處理每秒產生的15萬個查詢。

![E*TRADE](20160527-ElasticEvent14.jpg)
9. E\*TRADE - 使用Elastic產品和解決方案，幫助消費者改善投資。(照片糊了，請見諒)

![Tango](20160527-ElasticEvent15.jpg)
10. Tango - 透過ELK即時監控日誌與管理效能，確保全球的2.5億使用者有良好的通訊品質。

![](20160527-ElasticEvent16.jpg)
總結以上，整體Elastic Stack的應用領域包含：安全、日誌分析、分析、搜尋等不同領域。

![](20160527-ElasticEvent17.jpg)
整個產品線以前大家俗稱為ELK，現在要正名為Elastic Stack囉！而除了開放原始碼的Kibana、Elasticsearch、Logstash、Beats之外，加強ELK Stack的X-Pack，也就是需要付費訂閱的外掛套件，包含了Security by Shield、Alerting by Watch、Monitoring by Marvel、Graph by [???]。除此之外，還有提供雲端服務，也就是Elastic Cloud，直接訂閱服務就不用自己架設備跟服務，就整套可以拿來用囉！

####Introduction to Elastic Stack and X-Pack
![](20160527-ElasticEvent19.jpg)
主講人：曾勇　Elastic 技術顧問

第一個議程接著由Elastic亞太區技術顧問曾勇(Medcl)主講，介紹整個Elastic Stack和X-Pack，照片裡還是亞太地區銷售總監何致偉。

![](20160527-ElasticEvent23.jpg)
曾顧問提到是第一次來台灣，另外想說，M9拍照真的很糟糕XD

![](20160527-ElasticEvent25.jpg)
非常活躍的開發者，先前在資策會參與Elasticsearch的課程，就有發覺到Elasticsearch RTF，原來作者就是曾勇！有興趣的朋友可以到曾勇的Github看看 http://github.com/medcl

![](20160527-ElasticEvent26.jpg)
我不是故意拍成這樣的，我沒有要牽拖是手機的問題...

![](20160527-ElasticEvent27.jpg)
先介紹ELK各個產品的Logo與變革。

![](20160527-ElasticEvent28.jpg)
還有以前常常讓人疑惑的ELK到底版本之間要怎麼搭配，因為不同的Elasticsearch、Logstash、Kibana是有相依性問題，不能隨意混搭的。

![](20160527-ElasticEvent29.jpg)
以前有一張Release Bonanza表格，可以看到該如何搭配。

![](20160527-ElasticEvent30.jpg)
Elastic公司自己也知道這問題，所以他們到了5.0版本以後，就統一版本號，未來的Release版本都會使用相同的版本號碼進行發佈，就不會有哪個E搭哪個L或者K的問題了。

![](20160527-ElasticEvent32.jpg)
曾勇展示了Elastic 5.0的一個新功能，就是建立關聯分析圖，細節可能等使用過才知道。

![](20160527-ElasticEvent34.jpg)
有花一些時間介紹Beats，也就是用來安裝在主機上蒐集資料傳送到Elastic Stack的小agent，以前都是用Logstash或者Logstash Forwarder，但是這樣環境就需要JAVA會有點肥大，所以他們開發了Beats，也就是適用於不同環境的輕量化資料代理轉發程式。

![](20160527-ElasticEvent35.jpg)
在beats之中有一個特別的產品叫做Packetbeat，直接安裝在伺服器上面，即時解析應用層的協定，目前支援的應用程式有：

* http
* MySQL
* PostgresSQL
* Redis
* Thrift-RPC
* MangoDB
* DNS
* Memcache
等，實用性相當高，在Elastic官方網站也有Live Demo，可以直接連過去參考 http://demo.elastic.co 。

![](20160527-ElasticEvent37.jpg)
Elastic Stack的告警機制，預先定義好規則，讓程式來通知你。也提到一條好萊塢守則　- Don't call us, we will call you!

![](20160527-ElasticEvent38.jpg)
前面提到的新功能，Elastic Stack關聯繪圖，結合搜尋跟繪圖的技術，提供簡單的graph-walking API，讓使用者能夠透過完整的Elasticsearch搜尋建立關聯性圖形。

![](20160527-ElasticEvent39.jpg)
這類技巧跟方法可以用在關聯性辨別、詐騙偵測、商品推薦等。


####Get visibility form our application & user
![](20160527-ElasticEvent41.jpg)
主講人：周鉦琪　HTC 資深副理

![](20160527-ElasticEvent43.jpg)
M9碰到主管可能比較正常，這是周副理。

![](20160527-ElasticEvent45.jpg)
先點出軟體開發的挑戰，包含了：
1. 我們看不到使用者覺得這個應用如何
2. 我們看不到應用的深層細節
3. 缺乏一個集中、方便來追蹤議題的方法

![](20160527-ElasticEvent46.jpg)
如何照顧好使用者經驗？因為我們平常無法得知使用者對我們的應用程式感覺如何。所以要想辦法從操作和行為上去挖掘內部的資訊。那麼，應用程式有什麼隱含的資訊呢？各項功能的重要性、優先程度如何？使用者從登入到登出的狀態，使用者花了多少時間才把事情做完？所有使用的歷程如何統計的。

![](20160527-ElasticEvent47.jpg)
看得更精細，才有機會主動預防問題發生。怎麼知道應用程式的健康狀態有沒有變差呢？要知道功能執行總數、功能執行時間的歷史記錄，自動擴大規模的群組節點，CPU負載或Java GC頻率增加等等。

![](20160527-ElasticEvent51.jpg)
快速除錯的挑戰。由於以前缺乏一個集中、方便來追蹤議題的方法，我們必須要使用ssh一台一台登入，然後再慢慢使用tail去追蹤服務或伺服器的日誌，而日誌又散落在不同的伺服器和不同的分割區，管理起來或者遇到要除錯的時候，都會是很讓人困擾的問題。

在雲端時代呢？因為主機和服務會auto-scaling或者存放在docker，需要的時候才啟動才會跑，不需要的時候可能就關掉或回收，根本無從除錯，這些無法管理或不易管理的日誌，有時候就會是造成問題的根因，所以一定要有辦法才迎戰雲端新時代。

![](20160527-ElasticEvent52.jpg)
上述議題沒解決之前，為什麼要這麼辛苦地繼續監看呢？因為，需要花很大的努力才能解決上述三個議題，而要解決議題又需要一個可以處理這麼大資料量的架構，然後如何針對個別的應用程式產出客製化的報表，都是問題。而在一個企業或組織來說，解決上述問題的重要性和優先性，往往沒有處理使用者或老闆新的要求來得高。

所以，想要解決上述議題，最理想的方法就是可以簡單做到，然後不要花太多的力氣。（這當然，誰不希望這樣）

![](20160527-ElasticEvent53.jpg)
所以HTC也是透過Elastic Stack再加上docker的應用，來解決上述議題，並且用來收容應用程式記數(application counter)、軟體日誌(Software Logs)跟環境日誌(Environment Logs)。

![](20160527-ElasticEvent54.jpg)
這是HTC的實做流程，大致是三個階段，參考圖就好。

![](20160527-ElasticEvent56.jpg)
副理有提到，軟體日誌最重要的就是要能夠清楚知道：什麼人，在什麼時間，對什麼東西，做什麼事情，做了多久，把握這個原則，透過這些因素設計資料欄位(在Elasticsearch中就是Type要定義好的意思)。

![](20160527-ElasticEvent57.jpg)
最後提到HTC導入Elastic Stack的好處，減少跟每個應用程式owner整合的功，因位都講好要看的東西是哪些了，也比較簡單能夠達到未來的分析和稽核，最重要的是，有個集中管理進入點，能夠很方便的檢查所有的應用程式日誌。

![](20160527-ElasticEvent59.jpg)
中場點心時間，後面議程待續。

####整合Hadoop及Elasticsearch，加速大數據應用。
![](20160527-ElasticEvent61.jpg)
主講人：陳進賢　歐立威技術協理
![](20160527-ElasticEvent63.jpg)
吃完點心以後，由Alex說明ES整合Hadoop的應用。在Elastic的產品線之中，有一塊是ES-Hadoop，也就是做為串連Elasticsearch跟Hadoop的中介專案。

![](20160527-ElasticEvent64.jpg)
先說明現代資料架構(Modern Data Architecture, MDA)以及Hadoop生態系在MDA中扮演的角色，主要串起了各式不同的資料來源(現有的CRM, ERP, Clickstream, Logs以及新興的Sensor, Sentiment, Geolocation, Unstructured)、資料系統以及應用(Business Analytics, Custom Applications, Package Applications)，並且提供在資料系統以及應用程式之間進行資料維運管理與開發的能力和彈性。

![](20160527-ElasticEvent65.jpg)
接著是提到Data Warehouse如何進行最佳化，現有的環境之中，企業資料倉儲(Enterprise Data Warehouse)可能有以下問題:

1.有些時候都在處理低價值的工作流程
2.現有探索或需要分析的時候，舊資料已經搬移、封存或找不到
3.原始資料常被捨棄、找不到了。

所以在現狀下，可能有50%的資源在處理維運、30%的資源花在處理ETL(Extract-Transform-Load，建置或更新資料倉儲內容，對於所需之數據進行資料擷取、轉換、載入的過程)，剩下少少的20%才能專注用在分析上。

如果透過Hadoop來擴增:

1.有機會讓企業資料倉儲從低價值工作中解放
2.需要分析或探索的時候，保留完整的原始和歷史資料，不用花一堆時間在ETL
3.載入後再探勘資料的價值，因為整個schema可以在讀取時決定

![](20160527-ElasticEvent66.jpg)
該如何透過Hadoop解放新形態的資料價值呢?

1.sentiment - 立刻得知消費者對我們品牌與產品的感受
2.clickstream - 截取和分析網站訪客的資料軌跡，並且用來最佳化網站
3.sensor/machine - 自動從遠端的感測器和機器探勘串流資料的特徵
4.grographic - 分析地理資訊，幫助維運者瞭解更多事情
5.server logs - 研究伺服器日誌來診斷流程問題與失敗，並預防安全事件
6.unstructured (text, video, picture, etc...) - 從成千上萬的檔案、網頁、文件等找出特徵

![](20160527-ElasticEvent67.jpg)
YARN Unlocks the Data Lake Vision

目標要做倒一個地方儲存所有的資料，可以是用多種方法對資料進行處理戶動。
在第一代的Hadoop，主要就是底層儲存在HDFS，需要運算和處理的部分交給MapReduce進行。到了第二代的Hadoop，底層一樣是HDFS，最上層則是各種不同的新應用，包含傳統的Hadoop(MapReduce)、靈活的資料處理(Hive, Pig)、線上資料處理(Hbase, Accumuio)、串流處理(Storm)等，在上層不同的新應用與底層的HDFS之間，則需要透過一個有效的叢集資源管理與分片服務(稱為YARN)，來把上層跟底層進行兩好的串接。

![](20160527-ElasticEvent68.jpg)
ES在現代資料架構中扮演的角色，則是可以基於MDA的資料來源與資料系統之上，進行各式各樣的應用。

![](20160527-ElasticEvent69.jpg)
Hadoop+Elastic的目標是想要帶來資料的價值，也就是有資料存在的地方，就能夠找到資料的價值。
* Hadoop是一個生態系，我們則是生存在生態系裡面的公民。
* seamlessly把資料移入移出Hadoop
* 在hadoop資料中解放幾乎即時的搜尋
* Hadoop分散式跟版本agnostic

![](20160527-ElasticEvent71.jpg)
結合ES跟Hadoop的使用案例:

* 共用架構(Common Architectures)
 * 進入兩個平台的資料流(Hadoop, Elasticsearch)
 * 平台之間的回饋
* Strengths
 * Elasticsearch 即時分析與回答(answers)
 * Hadoop 深層、客制、批次為主的分析
* Example Use Cases
 * 日誌分析
 * 廣告投放目標與個人化  
 * 增強網站搜尋和推薦引擎能力
 * 
![](20160527-ElasticEvent72.jpg)
* 一個服務商就可以完成搜尋的所有需求，從資料截取到視覺化分析
* 在Hadoop資料中執行即時搜尋案例，包含了全文檢索、地理資訊、透過聚合與矩陣達到暫存資料的近即時分析
* 在不斷回饋的框架中，可以將資料在兩個平台之間無縫搬移
* 快速成長與各種企業特色的套用，例如安全、告警、監控
* 免費而且簡單的在Hadopp裡進行即時視覺化跟跟資料瀏覽
 * Kibana 是很簡單的選擇
 * Hadoop 生態系選項過熱了，沒能總是即時分析，而且可能開始有一些商業費用的支出。
 
 ![](20160527-ElasticEvent73.jpg)
 ELK-日常系統維運的好幫手
 統一資訊(PIC)如何運用ELK於內部應用系統及對外營運系統的管理工作
 統一資訊(PIC) 系統支援及技術研發部 劉祖弘 Gary Liu
 
 ![](20160527-ElasticEvent74.jpg)
 M9的拍照，就不多說了，看下一張有講者自己簡介。
 
 ![](20160527-ElasticEvent75.jpg)
 ![](20160527-ElasticEvent76.jpg)
 還有統一集團的版圖，相當的龐大。
 
 ![](20160527-ElasticEvent77.jpg)
 透過故事的例子，把工作的情境帶出來給大家，Case 1: 先說有一天Gary違反AP Log管理規範。說都沒有按照要求把完整的Log寫在檔案，還問說能不能把Log寫在DB，James就說怕影響效能，要Gary
 
 ![](20160527-ElasticEvent78.jpg)
 Case 2: 碰到問題的時候，以前都需要透過很多shell command進行log查找，這應該是網管人基本技能
 
 ![](20160527-ElasticEvent79.jpg)
 所以就列出一些常用的命令囉!
 
 ![](20160527-ElasticEvent80.jpg)
 Gary就開始做他的筆記...
 
 ![](20160527-ElasticEvent81.jpg)
 Case 3: 網站全面異常，要查要看的好多，趕快開一堆ssh連線開始查log, debug...
 
 ![](20160527-ElasticEvent82.jpg)
 順便分派了任務，要查的主機實在太多了，那就交給你查吧!
 
 ![](20160527-ElasticEvent84.jpg)
 屋漏，當然一定要連夜雨的阿！不然要幹嘛，再加碼Apache Log、DB Log、網管Log，就通通交給你幫我連絡囉！所以大家都嚇死了~
 
 ![](20160527-ElasticEvent85.jpg)
 所以這時候我們是不是要趕快調單位，哈哈。
 
 ![](20160527-ElasticEvent86.jpg)
 當然不是，所以某一天Ruby看到了這個畫面...
 
 ![](20160527-ElasticEvent87.jpg)
 主角ELK出現了
 
 ![](20160527-ElasticEvent89.jpg)
 統一資訊的ELK架構大概是這樣，可能是比較早期開始建置，有Kafka Cluster跟Storm Cluster，在報系內我們很單純，就是純ELK架構，負載平衡給HAProxy做、高可用則是給Keepalive來完成。
 
 ![](20160527-ElasticEvent90.jpg)
 所以有了ELK，配合Kibana就不用開一堆機器ssh到處看log了。
 
 ![](20160527-ElasticEvent91.jpg)
 Gary是在2014左右知道ELK，然後嚐試實做書中的ELK架構，可以看到前端是Logstash Agent(也就是Logstash Forwarder, 各種beats的前身)，給Redis List之後送到Storm Topology，接著才是進入Elasticsearch與Kibana。
 
 ![](20160527-ElasticEvent92.jpg)
 接著說明導入ELK前後的差異。
 
 ![](20160527-ElasticEvent93.jpg)
 後續的計畫也可以參考
 
 ![](20160527-ElasticEvent94.jpg)
 再來是廣編特輯，說明他們研究的資料探勘發現的現象。
 
 ![](20160527-ElasticEvent95.jpg)
 以及Data Mining的經驗。
 
 ![](20160527-ElasticEvent96.jpg)
 LogLoop在金融與IoT成功案例
 主講人：陳文裕　數位無限軟體總經理
 
 ![](20160527-ElasticEvent97.jpg)
 最後一個議程是Wenyu來跟大家分享他們以ELK為核心，開發整合出來的LogLoop解決方案。
 
 ![](20160527-ElasticEvent101.jpg)
 首先提到從2016年到2020年，預估的資料量的成長又會翻好幾倍，隨著工業4.0、IoT與智慧城市的發展、連網裝置的盛行，資料的來源越來越多引出了軟體正在吞噬這個世界的觀點。
 
 ![](20160527-ElasticEvent103.jpg)
 因為軟體與各式應用很多，底層架構技術方法到中間管理儲存到上層的分析應用，通通都有不同的做法，造成了在資料分析上架構複雜、功能不一、無法互通(要通的成本代價很大)、數量龐大等問題，這些問題一直存在現實中，並且隨著時間的推移漸漸惡化，因為東西越來越多了。
 
 ![](20160527-ElasticEvent104.jpg)
數位軟體無線推出了CloudFusion，不管是哪個雲端服務供應商(IBM、Amazon、Openstack等)，在Public Cloud或是Private Cloud，能有一個統一的混合雲管理平台

![](20160527-ElasticEvent105.jpg)
透過單一介面管理各種雲端環境，一致化整合異質雲特性，把服務落地並提供高效能的大數據日誌分析(這邊才是重點)

![](20160527-ElasticEvent106.jpg)
解決方案從內到外的概念上可以分成CloudFusion、AppBalloon跟LogLoop

![](20160527-ElasticEvent107.jpg)
LogLoop內部與核心是Elasticsearch，開發了幾個角色：分別是LogLoop Gate、LogLoop Analyzer、Present、LogLoop　Repository，最左邊一樣是各式各樣的Log來源。LogLoop的特色與能力是：1分鐘至少分析50萬筆日誌事件、可以無限延伸(scale out)、視覺化分析等。這特性其實跟Elastic Stack中的角色相同。

![](20160527-ElasticEvent110.jpg)
這邊是流程和架構圖，由左至右主要有三大塊，分別是Source、Process and Store、Read，等於從各種不同的外部資料來源，可能是OpenData、Bank branches、Population、Company reg-changes等，這邊在說明中使用的案例是，銀行的鈔票辨識系統，整合IoT跟數據分析的技術，蒐集所有分行的鈔票辨識系統，即時分析的數據，提高偽鈔偵測與追蹤的能力，類似做到偽鈔的區域聯防，追蹤偽鈔什麼時候出現、出現的地點和資訊等，這些大量的運算資訊整合了Hadoop、Google Maps、Tableau等，即時呈現資訊。

![](20160527-ElasticEvent111.jpg)
最後一個例子則是智慧電動單車，腳踏車上都有一個智慧感測原件，可以想像成行車電腦，會定時回傳車況、零件耗磨狀況等，以及車子常去的地方，這些資訊通通會彙整整到集中的平台進行分析。

![](20160527-ElasticEvent113.jpg)
蒐集這些數據進行分析，是為了提供更精確深層的客戶體驗，因為電動單車的電池、行車電腦等元件，屬於比較貴重、備料成本高的組件，維修店面不願意也不適合背太多的庫存，所以透過這些分析，店家可以知道，特定範圍內有多少車，每台車的車況、零件狀況如何等，雖然這台可能不是A店家出貨的，但是只要是這個品牌的經銷商，都可以查詢到鄰近的車子狀況，車主也可以同時知道附近哪裡有維修店家，串起顧客與維修店家之間的聯繫以及更好的消費體驗和感受。

###結語
以上，大概是本次Elastic Event的與會經過，在Elastic的世界裡，可以深深的感受到他降低了資料蒐集、搜尋分析的門檻，整體叢集設計也帶來很大的彈性，雖然也花了很多時間摸索，從以前的scale up思維到邁向分散式的scale out(是指從OpenSource建構的、能自己掌握的部分為分散式，個人覺得自己構建的Elastic Stack還不能滿足雲端環境的特性要求，所以不稱他為雲端)，資料快速轉換成視覺化與儀表的特性，賦與搜尋新的定義與生命。

也可以感覺到Elastic社群的活力，相信這會是很值得投入的一塊，至少，在資管部現有的環境裡面，把Log收好收滿，再盡可能的找出有意義的分析或者設計告警等。更希望自己做好以後，能夠應用Elastic Stack到報系其他會有各種數據的關係企業，例如分析com的內容(可能看看GA之外還有什麼?)、電子商務消費分析、客戶關係服務CRM想做的事情等，有沒有什麼是我們能夠使得上力的地方，讓我們能走得更高更遠，更有價值。