# SATA模擬IDE安裝Windows以後改為AHCI

## 前情提要

雖然已經出到Windows 10，但Windows 7應該可以再戰幾年。以前也看到有人說在安裝Windows 7前要準備好AHCI驅動程式，安裝的時候載入，才能找到硬碟。原本電腦使用SATA，最一開始裝的時候主機版的SATA設定為 [Compatible IDE]，因更換為SSD時去檢查主機版才發現，直接改成[AHCI]的結果就是一直開不了機。

## 做法

微軟已經做了一個安裝檔，只要在 [Compatible IDE]的情況下安裝好該修正檔，應是驅動程式，不用等到開進入Windows 進行硬體裝置搜尋才安裝驅動，所以裝好就可以直接到BIOS改成AHCI，就順利進入Windows，打完收工。

下載點

