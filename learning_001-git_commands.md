# Git常用命令


git cat-file -p [hash ID or master]

顯示tree or blob物件

git hash-object [filename]

顯示[filename]內容經hash演算後的數值

git init

初始當下目錄為git工作目錄

git add .

加入所有檔案到git index

git add app/*

加入app/下所有檔案到git index

git add *.txt

加入所有.txt檔案到git index

git status

顯示目前git狀態

git status -s

顯示目前git狀態 精簡

git commit

所有變更合併到git版本 會開啟notepad提示輸入註解

git commit -m "版本紀錄的說明文字"

所有變更合併到git版本  -m 為註解

git log

顯示全部git commit日誌

git log -10

顯示最近10筆git commit日誌

git rm '*.txt'

從index移除所有.txt檔案、實體檔案也會移除

git rm 'app/*.html'

從index移除所有app下所有.html檔案、實體檔案也會移除

git rm --cached [filename]

從index移除所有[filename] 實體檔案保留

git mv 'oldname' 'newname'

更改檔案名稱

git reset

重設git工作目錄的

git reset --hard

重設git index也把工作目錄還原到目前最新版

重設git工作目錄 index與所有檔案

git checkout master 'filename'

從master取回[filename]檔案 (如果把[filename]改壞了)

git add
git mv
git rm
git status
git commit
git ls-files

