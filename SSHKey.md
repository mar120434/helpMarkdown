# SSH Key 建立

要在 Github 上 push 專案前需要建立 SSH Key。<br>
一台電腦對應一個 SSH Key。

## 步驟

<ol>
<li>
在專案資料夾按右鍵，點選 Git Bash Here。會開啟 git 的小黑窗。
<li>
輸入 windows 指令

```
dir -a
```

或 linux 指令

```
ls -a
```

來確認有沒有 .ssh 這個資料夾

<li>
輸入

```
cd .ssh
```

進到 .ssh 資料夾中，再輸入

```
ls
```

會有一個 know_hosts 檔案

<li>
輸入

```
ssh-keygen -b 數字 -C "E-mail"
```

##### 數字可以是 1024、2048、4096

##### E-mail 例如 123@123

之後連按 3 下 Enter -> 會確認加密方式，預設為 rsa 加密<br>
最後產生 SSH Key

<li>
輸入

```
ls
```

發現除了 know_hosts 外，多了 id_rsa 和 id_rsa.pub

<li>
輸入

```
notepad id_rsa.pub
```

打開 id_rsa.pub，複製內容，最後一行會有 E-mail 部分。

<li>
<ol>
<li>
進入 Github 網站登入，在 Settings 中的側邊欄找到 SSH and GPG keys
<li>
點選New SSH Key。
<li>
將 id_rsa.pub 最後面的 E-mail 貼到 Title；將剩下內容貼到 Key。完成 SSH Key 建置。
</li>
</ol>
</li>
</ol>
<hr>
完成 SSH Key 建立後才能將專案 push 到 Github 上。
