# Homework 5

## Lecture 14: IoT Flask Web (github, vs code)

### Step 3 :





# Lecture 14: IoT Flask Web (github, vs code)
## Team20's Homework #5 (branch: step3)

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