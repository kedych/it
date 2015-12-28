# ElasticSearch - 改變日誌設定

標準的日誌紀錄設定已符合絕大多數的用途，改變或調整日誌層級(log level)主要在檢查bugs或了解在錯誤組態或奇怪的plugin狀態時候的異常排除特別有用。詳細日誌(verbose log)可以用在像Elasticsearch社群回報問題的時候使用。

如果我們需要自行除錯Elasticsearch或者改變日誌層級，就需要改變config/logging.yml之中的參數。

