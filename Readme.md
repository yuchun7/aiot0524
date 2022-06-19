# Homework #5.02 (to huanchen1107/aiot_hw5)

## Lecture 14: IoT Flask Web (github, vs code)

### Step 1 : Development Environment Setup
1. Please install vs code, register github, install git for windows
2. (check-point 1) github create a new repository (aiot0524)
3. go to vs code clone this repository (choose new branch) 
4. vs code 安裝 python extension 
5. pip install flask, pandas, sklearn 
  * 快捷鍵 ctrl+shift+p ===> package manager 叫出 (git clone....)
  * 快捷鍵 ctrl+' ==> 叫出終端機 
6. (check-point 2) 為了要upload local file to github from local要終端機 C:> 設定下面 (不設定 branch default ='main')
   * C:> git config --global user.name "Huan Chen"
   * C:> git config --global user.email huanchen1107@gmail.com
7. C:> git remote add origin https://github.com/huanchen1107/aiot0524.git

if you want to change \
git remote add origin https://github.com/huanchen1107/aiot0524.git \
https://github.com/yuchun7/aiot0524.git \
git branch -M main 
git push -u origin main

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

### Step 3: Use Data to Draw Highlight

1. 這個步驟使用到了XAMPP這個資料庫系統，我們首先登入資料庫，設定兩個新的使用者帳戶（帳號均為test123，密碼均為test123），一個是任何主機（％），一個是localhost，並分別給他們全部的權限。
2. 接著，我們新建aiotdb這個資料庫，並匯入老師提供的sensors資料表（在branch: step3中的db資料夾內）。這個資料表的內容如下圖，共有六個欄位，分別是"id", "time", "value", "temp", "humi" 及 "status"。
![](https://i.imgur.com/p6JXC2U.png)

3. 資料庫建立完成後，使用vs code來編輯app.py及index.html兩個檔案使Github能夠留存History。
4. 接著，來到了這個步驟的主要目的：藉由資料表裡的value及status數據畫出Highchart。利用app.py中的SQL code，如下，就能夠取出資料表內全部的數值，再將這些取到的數值傳送至index.html，將time數據當作X軸，value數據當作Y軸，就能畫出Highchart了。另外，status=1時，數值的點為綠色，status=0時，數值的點為紅色。
```sql
SELECT * FROM sensors
```

![](https://i.imgur.com/nn3n0eS.png)


### Step 4: Set random and Update Data

1. 在Step 3中，已經能夠取出資料庫裡的資料，並將time數據當作X軸，value數據當作Y軸，繪製成Highchart。在這個步驟中，我們的目的是破壞value和status兩者間的相關性，使之後的Step 5能夠以AI模型來預測出新的value對應到的新的status，並變更Highchart上每個value數值的顏色。
2. 藉由以下的SQL程式碼，我們可以將value的值隨機設定成0~1000的數值，並update至sensors資料庫，與此同時，time的數值也會跟著更新。
```sql
update sensors set value = RAND()*1000 where true
```
3. 一樣藉由下方的SELECT程式碼，將更新後的所有數值都取出，並傳送至indexNoAI.html，以新的time數據當作X軸，新的value數據當作Y軸，畫出了新的Highchart。
```sql
SELECT * FROM sensors
```
4. Step 4與Step 3畫出的Highchart的差別在於，由於Step 4是由打亂的value畫出的，value和status的相關性已經被打亂，所以綠色和紅色的點不再像Step 3一樣整齊。若是想要得到與value正確相應的status值，以畫出整齊的Highchart，則需藉由Step 5的模型來達成。

![](https://i.imgur.com/sgHkTuu.gif)

### step5：加入預先訓練的模型

> 在 step4 中，我們將 sensor 資料表內每筆資料的 value 欄位的值，更改為一隨機值，因而造成資料的 value 值與其 status 值無法正確匹配的情況，在此步驟中我們將加入預先訓練好的 AI model 來解決這個現象。
>
1. 在 app.py 加入新的 route 以及 function，用來回傳 AI model 預測的結果給 indexAI.html 顯示。
    ```code=python
    @app.route("/getPredict")
    def getPredict():
        #==== step 1: setup variable ===========
        myserver ="localhost"
        myuser="test123"
        mypassword="test123"
        mydb="aiotdb"
        debug =0
        from  pandas import DataFrame as df
        import pandas as pd   #引用套件並縮寫為pd
        import numpy as np
    ```

2. 在 function 中加入以下這段程式碼，來載入我們預先訓練好的 logistic regression 模型（model 壓縮檔在 model 資料夾內）。

    ```code=python
    import pickle
    import gzip
    with gzip.open('./model/myModel.pgz', 'r') as f:
        model = pickle.load(f)
    ```
    
3. 使用下面的程式碼取得接下來要給模型預測的 sensor 資料表中的資料。
    ```code=python
    import pymysql.cursors
    #db = mysql.connector.connect(host="140.120.15.45",user="toto321", passwd="12345678", db="lightdb")
    #conn = mysql.connector.connect(host=myserver,user=myuser, passwd=mypassword, db=mydb)
    conn = pymysql.connect(host=myserver,user=myuser, passwd=mypassword, db=mydb)
    
    c = conn.cursor()
    if debug:
        input("pause.. conn.cursor() ok.......")
    
    #====== 執行 MySQL 查詢指令 ======#
    c.execute("SELECT * FROM sensors")
    
    #====== 取回所有查詢結果 ======#
    results = c.fetchall()
    print(type(results))
    print(results[:10])
    if debug:
        input("pause ....select ok..........")
    
    test_df = df(list(results),columns=['id','time','value','temp','humi','status'])
    
    print(test_df.head(10))
    ```
5. 取得資料後，就可以透過以下程式碼，使用剛剛載入的 model 預測 status，並將 dataframe status 欄位的值更改為預測出來的值。
    ```code=python
    testX=test_df['value'].values.reshape(-1,1)
    testY=model.predict(testX)
    test_df['status']=testY
    ```
5. 最後用預測出的 status 值更新 sensor 資料表，並 return 整個 dataframe 回 indexAI.html，就可以畫出如下圖所示經由 AI model 整理過後的 Highchart。

    ![](https://i.imgur.com/rpe0uQS.png)

