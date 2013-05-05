重構
----

關於重構
=======

### 重構時機

* 完成某部份功能後，新增或修改某功能前

### 必要條件

* 一定要寫好對應的測試
* 一定要瞭解程式碼
* 一定要把重構時間加入時程裡

### 注意事項

* 要讓共事的伙伴也能看懂重構後的程式碼
* 落落長的系統難以重構時，就重 GO 吧...

關於測試
=======

### 常用技巧

* 程式儘量以帶入參數的方式來注入不易測試的對象，例如 DAO 、 Mailer 等
* 利用 Mock Data Object 來做測試
* 善用 IDE 的測試機制及快捷鍵

### 怎麼重構

1. 從現有版本分支
2. 決定要實做的方向
3. 小步前進
4. 測試
5. 完成一次小重構後，提交至版本控制系統
6. 繼續步驟 2 ~ 5
7. 整個功能完成後，合併回主幹

範例：批次訂單狀態更新程式
======================

### 背景

* 某電子商務系統 Web 平台後端自動化系統

### 功能說明

* 金流及物流服務的排程程式，會將相關資訊寫入 佇列資料表中
* 本程式會依照佇列資料表中的資訊，批次更新訂 單狀態
* 訂單狀態:已付款、已開發票、已出貨、已結案
* 訂單狀態更新後，要觸發對應的動作，例如寄信
* 完成後的佇列要刪除，避免再次觸發

基礎重構
=======

### 起手式 – Extract Method

1. 將相關的數行程式碼複製到一個新方法裡
2. 不是屬於方法的變數，就當做參數傳入
3. 將原來的程式碼註解起來，改為呼叫新的方法，測試
4. 將註解起來的程式碼刪除，測試

### 將 DAO (SQL) 封裝至 Model 中

1. 建立一個抽象 Model 類別，並將 DAO 改為外部可帶入的類別屬性
2. 建立訂單子 Model 類別
3. 在主程式中，將原來使用 DAO 的部份，改用 Model 類別，測試

### 注意

* 因為通常我們會使用 ORM 來取代 DAO ，因此這個重構的重點在於要讓 Model 類別是可以被測試的

### 將全域常數改為類別常數
1. 在訂單 Model 類別建立表示狀態的類別常數
2. 在原本有使用到全域常數利用編輯器取代功能，改成類
別常數，測試
3. 移除原本用 define 定義的常數，測試

其他該注意的事
============

* 見好就收，別花 80% 的力氣來得到 20% 的效益
* 不要硬套模式的形，能更清楚表達意圖的程式碼才是最有效的
* 別忘了把分支合併回主幹
* 讓伙伴知道你重構了些什麼；重構時可以採用 Pair Programming ，重構後可以用文件或註解
* 重構後如果伙伴看不懂，要嘛就放棄這次重構，要嘛就放棄這個伙伴 (誤)
* 讓你的伙伴跟著你一起成長!