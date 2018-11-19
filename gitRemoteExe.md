# Git remote 練習

## 提要

1.  先安裝好 Git
1.  先註冊一個 Github 帳號
1.  練習會用到的關鍵字
    1.  git add
        ```
        git add 檔案名稱
        ```
    1.  git commit
        ```
        git commit -m "說明字串"
        ```
    1.  git branch
        ```
        git branch 分支名稱
        ```
    1.  git check
        ```
        git check 分支名稱
        git check -b 分支名稱
        ```
    1.  git tag
        ```
        git tag tag版本號碼
        git tag -d tag版本號碼
        ```
    1.  git stash
        ```
        git stash save -u "儲存的訊息"
        git stash pop
        ```
    1.  git pull
    1.  git push
        ```
        git push
        git push origin tag版本號碼
        git push --delete origin tag版本號碼
        ```
    1.  git clone
        ```
        git clone 連結
        ```

## 開始前準備

1.  先建一個專案資料夾，名稱隨意。以下用`GitRemoteExe`當例子。
1.  在`GitRemoteExe`裡面建立 3 個子資料夾分別是`AA`、`BB`、`CC`。
1.  分別在`AA`、`BB`、`CC`裡面開啟終端機，模擬三位不同使用者。
    - 可以在資料夾裡按右鍵，選取`Git Bash Here`。
    - 也可開啟終端機，進到指定資料夾。

## 故事前情提要

> **AA** 與 **BB** 共同開發 **XX** 專案，一開始由 **AA** 建立專案，完成一部份後，**BB** 開始進入開發。爾後 **CC** 加入，完成後 push 到遠端分支。

## 故事開始

### AA Terminal

> **AA** 建立專案，完成了`Hello.java`，並且標註為`1.0`版。

```
git init 'XX'
```

> 建立 **XX** 專案

```
cd XX
```

```
touch Hello.java
```

> linux 指令，會建立一個`Hello.java`檔案。<br>windows 沒有類似指令，必須另外建立一個檔案。

```
echo 'Good Morning;' > Hello.java
```

```
git add Hello.java
```

> 將`Hello.java`從`Working Directory`區`add`到`Staging Area`區。

```
git commit -m "AA: 首次提交, 完成 Hello功能"
```

> 將`Hello.java`從`Staging Area`區`commit`到`Repository`區，必須加上`-m`來命名這次的`commit`。

```
git tag 1.0
```

> 給予這次`commit`一個`tag`，`tag`通常使用數字。

```
git log --graph --oneline
```

> `git log`檢查`commit`歷史，`--graph`會產生???，`--oneline`會把複雜資訊整理為一行。

### Github 建立專案

> 先登入 Github 網站，並建立一個新的專案，命名為 **XX**。

### AA Terminal

```
git remote add origin Github給的url
git push -u origin master
```

> 將檔案`push`到 Github 的 **XX** 專案。<br><br>注意！如果使用`SSH`，必須先在 Github 上面建立`SSH Key`。否則無法`push`。<br><br>`SSH Key`建立請參考`SSHKey.md`。

```
git push origin 1.0
```

> 將剛才建立的`tag 1.0` `push`到 **XX** 專案裡。

### BB Terminal

> **BB** 加入，著手開發`Idiot.java`。

```
git clone Github給的url
```

> **BB** 將 **XX** 專案複製到 **BB** 的資料夾。

```
cd XX
```

> 進入 **XX** 專案資料夾。

```
git checkout -b develop
```

> `checkout -b develop`可以切換並建立名為`develop`的分支。<br>另可以使用
>
> ```
> git branch develop
> git checkout develop
> ```
>
> 也能達到相同結果。

```
touch Idiot.java
```

```
echo '5987' > Idiot.java
```

```
git add Idiot.java
git commit -m "BB: 完成 Idiot.java"
```

> `add`並`commit` `Idiot.java`檔案。

```
git branch -vv
```

> 查看目前分支的情況。

```
git checkout master
```

> 切換到`master`分支。

```
git merge develop
```

> 在`master`分支裡，將`develop`分支`merge`到`master`分支。

```
git pull
```

> 記得`push`之前先做`pull`的動作。

```
git tag 2.0
```

> 給予剛剛的`commit`一個`tag`，命名為`2.0`。

```
git log --graph --oneline
```

> 查看`commit`資訊。

```
git push
```

> 將檔案`push`到 **XX** 專案。

```
git push origin 2.0
```

> 將`tag 2.0` `push`到 **XX** 專案裡。<br><br>這邊要注意，開始有時間軸的問題。<br>**BB** push 後，**AA** 同時在開發其他東西了，而且不知道 **BB** 作了啥。此時開始出現分支。

### AA Terminal

> **AA** 同時著手開發`Wahaha.py`，完成一部份後，`push`到遠端後，等待 **CC** 來接手。

```
git checkout -b feature
```

> 建立 feature 分支。

```
touch Wahaha.py
```

> linux 指令，建立`Wahaha.py`

```
echo 'I am happy, wahahahaa' > Wahaha.py
git add Wahaha.py
git commit -m "AA: Wahaha.py開發到一半, 需要CC協助開發"
```

```
ls
```

> linux 指令，列出資料夾中的檔案。<br>
> windows 相似的指令為`dir`。

```
git push --set-upstream origin feature
```

> 將本地`feature` `push`到遠端儲存庫。<br><br>`--set-upstream origin`可以建立本地分支與遠端儲存庫的對應關係。<br>如果直接`push feature`會無法成功，原因出在沒有設定本地分支與遠端儲存庫之間的對應。

### CC Terminal

> **CC** 加入開發，接手 **AA** 的`Wahaha.py`。

```
git clone Github給的url
cd XX
```

> 加入 **XX** 專案。

```
git checkout -b feature
```

> 建立`feature`分支。

```
git branch -vv
```

> 查看分支資訊

```
ls
```

```
git log --graph --oneline
```

> 檢查`commit`資訊。

```
git branch --set-upstream-to=origin/feature feature
git pull
```

> 上面的語法是手動設定本地分支去追蹤遠端分支，建議使用。<br><br>`git pull origin feature`是自動設定本地分支去追蹤遠端分支，可以達到相同效果，但不建議使用。<br><br>直接用`git pull`會失敗，因為`feature`分支並沒有追蹤遠端的`origin/feature`分支。

```
git log --graph --oneline
```

```
echo '白癡' >> Wahaha.py
git add Wahaha.py
git commit -m "CC: 完成AA寫到一半的 Wahaha.py"
git log --graph --oneline
```

```
git checkout master
git log --graph --oneline
```

```
git merge feature
git log --graph --oneline
```

```
git pull
git tag 3.0
git log --graph --oneline
git push
git push origin 3.0
```

### BB Terminal

> **BB** 新增了`Powerful.java`，但發現之前寫的`Idiot.java`有錯。

```
git checkout develop
```

```
touch Powerful.java
echo '我好厲害' > Powerful.java
ls
```

```
git log --graph --oneline
```

```
git stash save -u "之前的Idiot.java有錯，先把現在的東西暫存起來。"
```

> 利用`stash`暫時儲存分支內修改的內容。`save -u "可寫上儲存的註解，方便辨識"`。

```
ls
```

```
git checkout master
```

> `stash`後，切換到`master`分支。

```
git checkout -b bugFix
```

> 建立並切換到`bugFix`分支，用來修改`Idiot.java`的錯誤。

```
echo 'I am NOT idiot!!!!' > Idiot.java
echo '除了我以外, 通通都是白癡' >> Idiot.java
```

```
git add Idiot.java
git commit -m "BB: 修改 Idiot.java的錯誤"
git checkout master
git merge bugFix
```

> 修改好後，進行`commit`，並切換回`master`分支，將`bugFix`分支`merge`到`master`分支。

```
git pull
git push
git tag 2.1
git push origin 2.1
git log --graph --oneline
```

> 給予`tag`並`push`到遠端。

> 此時 **BB** 發現`tag`版本錯亂了，想修改`tag`版本。

```
git tag -d 2.1
```

> 刪除本地`tag 2.1`。

```
git push --delete origin 2.1
```

> 刪除遠端`tag 2.1`。

```
git tag 3.1
git push origin 3.1
```

> 給新的`tag`。

```
git log --graph --oneline
```

### CC Terminal

```
cat Idiot.java
```

> `cat`是 linux 指令，可以檢查檔案裡面寫的內容。目前 **CC** 看到的是還沒被修改的`Idiot.java`內容(`tag 2.0`)。

```
git pull
```

> **CC** 把檔案重新`pull`到資料夾中。

```
cat Idiot.java
```

> **CC** 再次查看檔案內容，會發現 **BB** 把檔案修改了。

```
git log --graph --oneline
git push
```

> **CC** 檢查完再把檔案`push`到遠端。

### BB Terminal

> **BB** 改完錯誤後, 繼續開發`Powerful.java`。

```
ls
```

> 查看目前檔案清單。

```
git checkout develop
git stash pop
```

> 切換到`develop`分支。<br>回到前一次`stash save`。

```
ls
```

> 查看`stash pop`後的檔案清單。

```
echo '誰都贏不過我' >> Powerful.java
echo '哼' >> Powerful.java
```

```
git add Powerful.java
git commit -m "BB: 林北完成了Powerful.java"
```

> 完成後進行`commit`。

```
git checkout master
git merge develop
```

> 將`develop`分支`merge`到`master`分支。

```
git log --graph --oneline
```

```
git pull
git log --graph --oneline
```

> 要`push`前的好習慣，先進行`pull`。

```
git tag 4.0
git push
git push origin 4.0
git log --graph --oneline
```

> 進行`push`。

### AA Terminal

> **AA** 打算查看目前專案情況。

```
git checkout master
git pull
git log --graph --oneline
```
