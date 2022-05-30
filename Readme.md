# Lecture 14: IoT Flask Web (github, vs code)
## Team20's Homework #5 (branch: step4)

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