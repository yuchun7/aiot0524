# Lecture 14: IoT Flask Web (github, vs code)
## Team20's Homework #5 (branch: step5)

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

    ![](https://i.imgur.com/1Ljc8rL.jpg)


