# 更換Windows 7 MAK為KMS版本

## 前情提要

因為裝了零售版本的Windows，需要改成大量授權啟動，但是又不想要重灌（浪費時間），只要把安裝的金鑰更換即可。

## 做法

開啟

    slmgr.vbs -ipk  FJ82H-XT6CR-J8D7P-XQJJ2-GPDD4

搞定，然後就用合法的KMS啟動，打完收工。


## 延伸

其他的錯誤代碼解決方案

### 0xC004C003錯誤代碼
slmgr.vbs -ipk  FJ82H-XT6CR-J8D7P-XQJJ2-GPDD4
  -ipk : 安裝產品金鑰(取代現有的金鑰)

   enter按下去後你的產品識別碼就被重新設定囉~
   這時候再重新執行KMS進行認證

### 0xC004C020錯誤代碼
slmgr.vbs -ipk FJ82H-XT6CR-J8D7P-XQJJ2-GPDD4
slmgr.vbs -ato

完成後，請重新開機進行KMS認證。

以上，打完收工。