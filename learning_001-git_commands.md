# Git簡易觀念

## 檔案狀態週期
* untracked (未追蹤，未加入 Git 儲存庫的檔案)
* unmodified (未修改，檔案第一次被加入，或檔案內容與 HEAD 指標內容一致狀態、也可以說是commit後未修改的狀態)
* modified (已修改，檔案已經被編輯過，或是檔案內容與 HEAD 指標內容不一致的狀態，而且還沒被標示成可以commit、與not staged同意思)
* staged (等待被commit，代表下次執行 git commit 會將這些檔案全部送入版本庫)


## 常用命令


####git cat-file -p [hash ID or master]

顯示tree or blob物件

####git hash-object [filename]

顯示[filename]內容經hash演算後的數值

####git init

初始當下目錄為git工作目錄

####git add .

加入所有檔案到git index

####git add app/*

加入app/下所有檔案到git index

####git add *.txt

加入所有.txt檔案到git index

####git add -u

只把「更新」或「刪除」的檔案變更寫入index。

####git status

顯示目前最新版與index之間差異

####git status -s

顯示目前git狀態 精簡

####git commit

所有變更合併到git版本 會開啟notepad提示輸入註解

####git commit -m "版本紀錄的說明文字"

所有變更合併到git版本  -m 為註解

####git log

顯示全部git commit日誌

####git log -10

顯示最近10筆git commit日誌

####git log --pretty=oneline

把git log用一行輸出

####git log --pretty=oneline abbrev-commit

把git log用一行輸出，並只輸出部分絕對名稱

####git rm '*.txt'

從index移除所有.txt檔案、實體檔案也會移除

####git rm 'app/*.html'

從index移除所有app下所有.html檔案、實體檔案也會移除

####git rm --cached [filename]

從index移除所有[filename] 實體檔案保留

####git mv 'oldname' 'newname'

更改檔案名稱

####git reset

重設git工作目錄的index

####git reset --hard

重設git index也把工作目錄還原到目前最新版

####git checkout master [filename]

從master取回[filename]檔案 (如果把[filename]改壞了)

####git ls-files

列出所有已經儲存在index中的檔案路徑

####git diff
=> 工作目錄 vs 索引

####git diff HEAD
=> 工作目錄 vs HEAD

####git diff --cached HEAD
=> 索引     vs HEAD

####git diff --cached
=> 索引     vs HEAD

####git diff --staged
同上

####git diff HEAD^ HEAD
=> HEAD^   vs HEAD

####git cat-file -p [object_id]
顯示特定[object_id]物件資料

###git merge [other_branchname]
把[other_branchname]合併到現在的版本

####git checkout -b [new_branchname]
建立並切換working directory到[new_branchname]

###git reflog
顯示版本動作與註解(merge, commit, checkout...)

####git branch -d [branchname]
刪除[branchname]分支(要已經merge過才能使用-d刪除)

####git branch -D [branchname]
刪除[branchname]分支(還沒被merge的分支使用 -D刪除)

####git branch [branchname] [SHA1]
撿回絕對名稱[SHA1]並命名為[branchname]分支

####git reset --hard ORIG_HEAD
回覆到上一版

git status
git ls-files -u
git diff [filepath]

#參考資料

* [30 天精通 Git 版本控管](https://github.com/doggy8088/Learn-Git-in-30-days)
* [Echo's Note](http://bdottn.github.io/tags/Git/)