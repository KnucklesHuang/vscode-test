本文同時發佈於 
[[VSCode] 在 Visual Studio Code 使用 Git 版本控制 - KnucklesNote板 - Disp BBS](https://disp.cc/b/KnucklesNote/gD4Z) 

在 Visual Studio Code 使用 Git 版本控制，安裝環境 Windows 10

以下說明會以 VS Code 中文化的界面來操作 Git 的功能，再附上等同於 Git 的指令。
會說明以下主題：
- [安裝 Git for Windows](#安裝-git-for-windows)
- [初始化存放庫 (Repository)](#初始化存放庫-repository)
- [提交變更的檔案 (Commit)](#提交變更的檔案-commit)
- [復原上個提交 (Reset)](#復原上個提交-reset)
- [擱置變更 (Stash)](#擱置變更-stash)
- [忽略檔案 (gitignore)](#忽略檔案-gitignore)
- [推送到 GitHub (push)](#推送到-github-push)
- [推送到自訂主機的存放庫](#推送到自訂主機的存放庫)
- [建立分支 (Branch)](#建立分支-branch)
- [安裝 Git Graph 模組](#安裝-git-graph-模組)
- [安裝 Git History 模組](#安裝-git-history-模組)
- [安裝 GitLens 模組](#安裝-gitlens-模組)


# 安裝 Git for Windows

執行 VS Code 後，開啟一個專案資料夾，例如 vscode-test  
點左邊的「原始檔控制」(Ctrl+Shift+G)  
![](https://i4.disp.cc/u/23/a08ab9d.png)

點「下載適用於 Windows 的 Git」會連到 https://git-scm.com/  
點「Downloads」，點「Download for Windows」  
![](https://i4.disp.cc/u/23/a814c68.png)

點「Click here to download」
![](https://i4.disp.cc/u/23/f111c8f.png)
選項都照預設值點 Next 就好

安裝好 Git 後，在開始功能表裡可以看到有 Git CMD 和 Git Bash，其中 Git CMD 是給習慣用 Windows CMD 指令用的，Git Bash 是給習慣用 Linux Bash 指令用的

Git Bash 開啟的預設路徑是使用者的家目錄，想改成專案目錄的話  
在開始功能表對 Git Bash 點右鍵/「更多」/「開啟檔案位置」
![](https://i4.disp.cc/u/23/5cb2706.png)
在總案總管對 Git Bash 的捷徑點右鍵/「內容」  
在捷徑頁的「目標」，刪除「--cd-to-home」  
「開始位置」設為自己的專案目錄
![](https://i4.disp.cc/u/23/68e1361.png)
之後開啟 Git Bash 就會預設路徑為專案目錄了

執行 Git Bash，設定 Git 用戶的名稱和E-mail
```
$  git config --global user.name "Knuckles"
$  git config --global user.email "knuckles@xxxx.xxx"
```
之後提交時會記錄提交者為這個使用者

檢查設定結果
```
$ git config --list
user.name=Knuckles
user.email=knuckles@xxxx.xxx
```

# 初始化存放庫 (Repository)

回到 VS Code，在原始檔控制選「將存放庫初始化」  
等於在專案目錄裡執行指令 `$ git init`  
![](https://i4.disp.cc/u/23/0c6b56e.png)

會在專案目錄中產生一個隱藏的 .git 資料夾，用來做 Git 存放庫 (Repository)  
而專案目錄中其他的檔案與資料夾就叫工作目錄 (Working directory)

在原始檔控制頁，可以看到預設的分支名稱為 main  
![](https://i4.disp.cc/u/23/f166339.png)

若要加入已經開啟的專案，點「⋯」/「複製」  
等於指令 `$ git clone {存放庫的.git網址}`  
![](https://i4.disp.cc/u/23/829d1f6.png)  
接著輸入存放庫的 .git 連結，或是點「從GitHub複製」  
![](https://i4.disp.cc/u/23/03ae398.png)

若是新開啟的專案的話，新增一個 test.php 檔  
原始檔控制的「變更」裡會出現一個新增的檔案  
![](https://i4.disp.cc/u/23/ded9341.png)  
檔名會顯示為綠色，後面被標了個 U，  
代表是新增的檔案，尚未被 Git 追蹤 (Untracked)

此時點「✔ 提交」的話會說沒有暫存變更(Staged changes)的檔案，是否要將所有變更的檔案都設為暫存變更並提交，點取消
![](https://i4.disp.cc/u/23/2363549.png)


# 提交變更的檔案 (Commit)

想要提交變更的檔案，可以點旁邊的加號「✚」  
![](https://i4.disp.cc/u/23/badcea5.png)  
或是對檔案按右鍵選「暫存變更」(Stage Changes)  
![](https://i4.disp.cc/u/23/03f8591.png)  
等於指令 `$ git add test.php`

可以看到 test.php 被移動到了「暫存的變更」區 (Staged Changes)  
檔案後面的狀態也從 U (Untracked) 變成 A (Added)  
![](https://i4.disp.cc/u/23/8fcbd0d.png)  
後悔的話也可以點旁邊的減號「─」取消暫存變更  
等於指令 `$ git reset test.php`

要查看檔案變更狀態與暫存的變更，也可以用指令 `$ git status`

接著要將暫存的變更提交，在上方的訊息輸入提交的說明，例如「建立新檔案」  
點「✔ 提交」(Commit)  
![](https://i4.disp.cc/u/23/b1f8439.png)  
等於指令 `$ git commit -m "建立新檔案 test"`  
若前面沒有設定 Git 使用者的話，這邊會跳出錯誤訊息

沒有輸入提交說明的話，會跳出編輯器，在開頭加上說明後存檔關閉即可  
下面每行開頭為 # 的註解會被忽略，未輸入說明的話會取消提交
![](https://i4.disp.cc/u/23/03a2d79.png)

成功提交後，回到檔案總管查看 test.php，檔名變成白色了  
點開左下角的「時間表」，可以看到有一個提交記錄了  
![](https://i4.disp.cc/u/23/90b69f2.png)

提交後再修改程式，修改的地方在左邊會有不同的顏色標記  
檔名會顯示為橘色，狀態會被標記為 M (Modified)  
在原始檔控制會看到被加入到「變更」的檔案  
![](https://i4.disp.cc/u/23/1e627cd.png)

點暫存變更後，輸入提交說明，點「✔ 提交」  
![](https://i4.disp.cc/u/23/9535116.png)

在時間表可以看到已記錄了第二個提交  
![](https://i4.disp.cc/u/23/65c2d2c.png)  
要查看提交記錄，也可以用指令 `$ git log`

如果想要再補充一些更新，但不想再新增一個提交，或是只是想更改提交說明的話  
提交時可改用下拉選單的「認可(修改)」英文是 Commit (Amend)   
或是點存放庫選單的「⋯」/「提交」/「認可(修改)」  
![](https://i4.disp.cc/u/23/6d466ee.png)  
等於指令 `$ git commit --amend`  
會將此次修改併入上一次的提交


# 復原上個提交 (Reset)

要是想直接取消上一次的提交，點存放庫右邊的選單「⋯」/「提交」/「複原上個提交」  
英文是 Commit / Undo Last Commit  
![](https://i4.disp.cc/u/23/71f47fe.png)  
要回到前幾次的提交就再復原幾次即可

存放庫選單的功能也可以用 Ctrl+Shift+P 輸入 git commit 後選擇想要的指令  
![](https://i4.disp.cc/u/23/2169560.png)

例如新增了一行程式並提交，用來測試看看復原上個提交  
![](https://i4.disp.cc/u/23/6309cb4.png)

執行「復原上個提交」後，可以看到剛剛的提交取消了，但新增的程式還在，程式被放回到「暫存的變更」區
![](https://i4.disp.cc/u/23/ead5ca6.png)

點存放庫選單「⋯」/「顯示 Git 輸出」，可以看到實際上是執行了
`$ git reset --soft HEAD~`

使用 reset 重置目前的版本到指定的提交記錄  
其中 --soft 代表不回復工作目錄的檔案，也不回復暫存的變更  
HEAD~ 代表目前分支最新版本的上一個提交記錄  

若是「回復上個提交」後再點一下「取消所有的暫存變更」  
就相當於執行了 `$ git reset HEAD~`  
![](https://i4.disp.cc/u/23/b0ab0c4.png)

若是「回復上個提交」、「取消所有的暫存變更」，再點「捨棄所有變更」  
就相當於執行了 `$ git reset --hard HEAD~`  
![](https://i4.disp.cc/u/23/414610e.png)  
代表完全回復到上次提交時的狀態，工作目錄變更的程式碼都會回復


# 擱置變更 (Stash)

如果想要把目前已變更的檔案全部先存起來放著不處理，可以使用擱置變更(Stash)  
例如在程式加了一行註解，所以有一個變更的檔案
![](https://i4.disp.cc/u/23/39f29f6.png)

點選存放庫選單「⋯」/「隱藏」/「擱置變更(Stash)」，英文是 Stash / Stash
![](https://i4.disp.cc/u/23/288e407.png)
接著輸入 Stash 的說明，例如「加上註解」  
![](https://i4.disp.cc/u/23/d171282.png)  
等於指令 `$ git stash push -m 加上註解`

會看到 test.php 沒有在變更的檔案裡了，檔案裡加上的註解也消失了
![](https://i4.disp.cc/u/23/87123d0.png)

想要把未追蹤(標記 U )的檔案也隱藏的話，要選擇 「⋯」/「隱藏」/「擱置變更(包含未被追蹤的檔案)」，但要注意這樣檔案會從工作目錄移除

想要回復隱藏的變更，選「⋯」/「隱藏」/「套用擱置… (Apply Stash)」，  
選擇要套用的 Stash 即可  
![](https://i4.disp.cc/u/23/2b026e9.png)  
等於指令 `$ git stash apply stash@{0}`

要套用並刪除這個 Stash 的話，選「取回擱置…(Pop Stash)」  
`$ git stash pop stash@{0}`

# 忽略檔案 (gitignore)
若有檔案不想加入 Git 追蹤，例如專案的設定值、含有隱密資料的檔案  
可以按右鍵選「新增到 .gitignore」  
![](https://i4.disp.cc/u/23/f337007.png)

會在專案目錄新增一個 .gitignore 檔案，並加入要忽略的檔案  
若要忽略整個資料夾的內容，例如 .vscode，要修改 .gitignore 檔案，  
加入 .vscode/*


# 推送到 GitHub (push)

要讓程式可以與其他人共同開放的話，可以使用發佈(Public)上傳到 GitHub

當所有變更的檔案都提交後，提交按鈕就會變成「發佈 Branch」  
或是在原始檔控制的分支 main 旁點發佈的圖示  
![](https://i4.disp.cc/u/23/2aede32.png)  
出現需要 GitHub 登入權限的話，輸入 GitHub 帳號密碼登入

輸入要在 GitHub 上建立的存放庫名稱，選擇要私人還是公開  
現在免付費帳號也可以使用私人存放庫了  
![](https://i4.disp.cc/u/23/32b4575.png)  
選好後就會自動在 GitHub 建好新的存放庫，不用自己登入建立

然後會推送(Push)本機分支 main 至遠端分支 origin/main，並設定了追蹤，等於以下指令   
`$ git remote add origin https://github.com/KnucklesHuang/vscode-test.git `  
新增一個遠端分支，其中 orgin 為預設的遠端分支名稱  
`$ git push -u origin main`  
推送分支 main 到遠端 origin  
其中 -u 等於 --set-upstream 設定分支開始追蹤指定的遠端分支

發佈成功後，分支名稱旁的圖示就會變成「同步處理」  
![](https://i4.disp.cc/u/23/fd2f472.png)  
此時已設定分支 main 要追蹤遠端分支 orgin/main  
可以直接使用「推送」「提取」，不需要選擇遠端

登入 GitHub 可以看到上傳好的程式
![](https://i4.disp.cc/u/23/060c4ed.png)
點「Add a README」在 GitHub 新增一個說明檔
編輯完說明後，點「Commit changes」
![](https://i4.disp.cc/u/23/9d57ed4.png)

在 Commit message 輸入提交的說明後點「Commit changes」
![](https://i4.disp.cc/u/23/8054569.png)
這樣會在 GitHub 這邊也建立了一次提交

要把 GitHub 存放庫的提交提取(Pull)回來並合併至本機的分支 main  
可以點分支 main 旁的同步處理圖示，或是選擇「⋯」/「提取、推送」/「同步處理」  
同步處理就是先提取再推送，等於指令 `$ git pull 、$ git push`  
![](https://i4.disp.cc/u/23/fd2f472.png)

同步處理成功的話，在檔案總管就可以看到新增的 README.md 檔
![](https://i4.disp.cc/u/23/eebc2e5.png)

若提取時出現錯誤訊息：There is no tracking information for the current branch.
Please specify which branch you want to merge with.  
代表目前分支沒有追蹤的遠端分支，改為使用「提取自...」就可以指定遠端分支，再使用發佈分支即可追蹤遠端分支


# 推送到自訂主機的存放庫

如果不想使用 GitHub 當遠端存放庫的話，也可以使用自己的主機

使用 Git Bash 切換到存放專案的目錄，例如  
`$ cd /e/wamp/www`  
產生一個給遠端主機用的存放庫 vscode-test.git  
`$ git clone --bare vscode-test vscode-test.git`  
然後將 vscode-test.git 資料夾傳送至遠端主機  
例如放在 my-host\.com 的 /home/knuckles/git/

在 VS Code 新增一個遠端存放庫  
在存放庫選單，選擇「⋯」/「遠端」/「新增遠端存放庫」  
或是 Ctrl+Shift+P，搜尋「add remote」  
![](https://i4.disp.cc/u/23/9a11098.png)

輸入遠端存放庫的 URL: 例如 ssh://knuckles@my-host.com:1234/home/knuckles/git/vscode-test.git  
其中 knuckles 為登入的帳號，my-host為自己的主機位址，1234 為 ssh 的 port
![](https://i4.disp.cc/u/23/d7b5694.png)

為這個新增的遠端存放庫取個名稱，例如「my-host」  
![](https://i4.disp.cc/u/23/8df6723.png)  
等於指令 `$ git remote add my-host {遠端存放庫的URL} `

要把本機的分支 main 推送(push)到遠端存放庫  
可以點存放庫選單「⋯」/「提取、推送」/「推送至…」  
或是用 Ctrl+Shift+P，搜尋「push to」然後選取要推送到遠端 my-host  
![](https://i4.disp.cc/u/23/7da495a.png)  
等於指令 `$ git push my-host main`

要把遠端存放庫的提交提取(Pull)回來並合併至本機的分支 main  
可以點存放庫選單「⋯」/「提取、推送」/「從…提取」  
或是用 Ctrl+Shift+P，搜尋「pull from」然後選取要從遠端分支 my-host/main 提取  
![](https://i4.disp.cc/u/23/505d698.png)  
等於指令 `$ git pull my-host main`

要讓本機分支改為追蹤遠端分支 my-host/main 的話，可以使用指令  
`$ git branch -u my-host/main`  
-u 等於 --set-upstream 設定分支開始追蹤指定的遠端分支  
更改追蹤後，就可以直接點提取、推送或同步處理了


# 建立分支 (Branch)

想要開發一個新功能，但又不想在開發過程影響到本來的程式，這時就可以開一個新的分支，等功能寫好後再合併回原本的分支

在存放庫選單，點「⋯」/「分支」/「建立分支...」
![](https://i4.disp.cc/u/23/cdf1def.png)  
輸入分支名稱  
![](https://i4.disp.cc/u/23/bb8f7cc.png)  
等於指令 `$ git checkout -b test1`  
其中 checkout 是移動到分支，加上 -b 代表建立分支後移動過去，也可以使用  
`$ git branch test1 `  
`$ git checkout test1`  
先建立好分支 test1，再移動過去

在存放庫可以看到目前分支變成 test1 了，在程式加上修改後提交
![](https://i4.disp.cc/u/23/4f24803.png)

想回到分支 main 時，點存放庫的分支名稱，或點選單「⋯」/「簽出至…」
![](https://i4.disp.cc/u/23/46ca2ab.png)  
選擇分支 main

![](https://i4.disp.cc/u/23/9811734.png)  
等於指令 `$ git checkout main`

回到分支 main 後，可以發現在分支新增的程式不見了，  
我們再這邊新增其他的程式後提交  
![](https://i4.disp.cc/u/23/7094fc7.png)

此時想要將分支 test1 的修改合併過來的話，  
在分支 main 的存放庫選單點「⋯」/「分支」/「合併分支…」  
![](https://i4.disp.cc/u/23/13e0956.png)  
選擇要合併的分支 test1  
![](https://i4.disp.cc/u/23/d2d30f7.png)  
等於指令 `$ git merge test1`

此時顯示檔案 test.php 發生了1個衝突，檔案狀態被標記為「1, !」  
可選擇要用分支 main 的版本，還是分支 test1 的版本，  
或是兩個都要的話選擇「接受兩者變更」  
![](https://i4.disp.cc/u/23/c5d66f6.png)

接受兩者變更後，分別在兩個分支寫的程式都加了上來，檔案狀態為 !  
在原始檔控制的「合併變更」區點 ✚ 暫存變更  
![](https://i4.disp.cc/u/23/57b04a5.png)

提交的說明已經自動寫好了，點「✔ 提交」  
![](https://i4.disp.cc/u/23/099c10e.png)


# 安裝 Git Graph 模組

![](https://i4.disp.cc/u/23/795107d.png)

在原始檔控制存放庫，點 Git Graph 圖示，可顯示圖型化的分支記錄  
![](https://i4.disp.cc/u/23/db0ed83.png)
點右邊的齒輪圖示 Repository Settings，可以設定 Git 使用者的 Name 和 Email  
也可以設定遠端存放庫  
![](https://i4.disp.cc/u/23/5772362.png)


# 安裝 Git History 模組

![](https://i4.disp.cc/u/23/b989ef2.png)

檔案的右上角按鈕，或是檔案的右鍵選單，  
會多一個「Git: View File History (Alt+H)」，會列出檔案提交記錄
![](https://i4.disp.cc/u/23/bc3dbf9.png)

點原始檔存放庫旁邊的圖示，也可以開啟目前分支的提交記錄
![](https://i4.disp.cc/u/23/691cfbe.png)


# 安裝 GitLens 模組

![](https://i4.disp.cc/u/23/aaf0654.png)

在原始檔控制加入更多的資料檢視  
![](https://i4.disp.cc/u/23/0a142cf.png)
![](https://i4.disp.cc/u/23/10a3479.png)  
想自訂 Git 使用者圖示的話，可以到 https://gravatar.com/ 用 E-mail 登入設定

檔案的上方會顯示編輯的使用者，游標所在的行會顯示提交資訊 (Inline Blame)
![](https://i4.disp.cc/u/23/cb49639.png)

點編輯器上面向左的圖示，或是對檔案按右鍵選「Open Changes」/「Open Changes with Previous Revision」
![](https://i4.disp.cc/u/23/dd79add.png)
可快速查看每次提交更改的地方，再點一次圖示
![](https://i4.disp.cc/u/23/f618271.png)
可切換到更上一次更改的地方
![](https://i4.disp.cc/u/23/088b018.png)
圖示最多只能顯示8個，可以按右鍵選擇要使用的圖示
![](https://i4.disp.cc/u/23/71c2e43.png)

點存放庫旁的圖示可顯示分支的提交記錄
![](https://i4.disp.cc/u/23/76f6e11.png)
預設是顯示在下方的面版，想改成顯示在編輯器的話，在設定搜尋
「gitlens graph layout」，把「panel」改成「editor」
![](https://i4.disp.cc/u/23/df20e8c.png)


參考:  
VSCode 官網文件 https://code.visualstudio.com/docs/sourcecontrol/overview
https://hackmd.io/@howhow/git_with_vscode