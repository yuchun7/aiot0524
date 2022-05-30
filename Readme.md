# Lecture 14: IoT Flask Web (github, vs code)
## Team20's Homework #5 (branch: step2)

### Step 2: Simple Flask

#### VS code 使用方法：
![](https://i.imgur.com/ALkLD15.jpg)

你可以利用vs code將修改過後的檔案上傳至github。步驟如下：
1. 將修改過後的檔案 ctrl+s 儲存
2. 按下左側第三個按鍵切換到 git 模式
3. 點選 `...` 的icon
4. 選擇提交→全部提交

:point_right: 運用 vs code點選的方式比起輸入`git add`、`git commit` 、`git push`的方式更為方便。

上傳資料的紀錄可以在 github 的 history 中看到。
![](https://i.imgur.com/ZCq9MDL.png)

#### 如何切換 branch?

![](https://i.imgur.com/N0rGs6Z.jpg)
透過 vs code 建立分支，步驟如下：
1. 按下左側第三個按鍵切換到 git 模式
2. 點選 `...` 的icon
3. 選擇分支並建立分支
4. 輸入您的分支名稱

所有東西其實都放在 .git 這個資料夾裡，他是一個隱藏的倉庫，當簽出至branch時，隱藏的倉庫會把隱藏的branch的資料拿出來給你看
![](https://i.imgur.com/fjOCv3H.gif)

#### 建立簡易Flask

1. 新增一個檔案 app.py
2. 新增一個資料夾名稱為 static
3. 撰寫程式碼

<font color='#4169e1'>@route</font> 在 Flask 當中便是我們用於註冊網址的函式。\
<font color='#4169e1'>@route(<font color='#ff6347'>' / '</font>)</font> 便是我們註冊的網址，網址直接就是我們寫好程式的「根目錄」。
