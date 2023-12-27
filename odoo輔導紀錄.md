<table>
    <tr>
        <td>輔導方案</td>
    </tr>
</table>
#### 買斷方案說明
1. ODOO16快速入門方案-第一年含維護-12萬
   + 範疇
     + 銷售
     + 採購
     + 庫存
     + 財務
   + 模組包
     + 第三方模組安裝與翻譯
     + 台灣報表結構設定與庫存報表
     + 微功能調整
   + 服務內容
     + 8小時顧問基本資料設定/開帳說明
     + 8小時的線上顧問諮詢時數
     + 8小時的報表客製時數
     + 16小時的客服時數
     + 網路課程(含進階)
     + ODOO16使用手冊(完整版)
   + 協助安裝與建置環境，不含伺服器、網域、SSL憑證等費用。
   + 每年維護費用為總價的20%-2萬4。
     + 16小時的客服服務。
   + 服務時數加購2000/小時。

#### 租賃方案說明
1. ODOO16雲端進銷存租賃方案-每月1500
   + 每一個機器限制3名使用者，增加使用者500/U。
   + 服務內容
     + 首次購買送4小時客服時數。
     + 每月2小時客服時數。
     + ERP網路課程。
     + ERP使用手冊。
   + 預付3個月/4500;6個月/9000
   + 預付12個月/18000,9折優惠16200
   + 加值模組
     + 財務 300/月
     + 製造 300/月
   + 可增購顧問與客製時數2000/小時。


<table>
    <tr>
        <td>輔導紀錄</td>
    </tr>
</table>

#### 銷售內控
1. 訂單預測
2. 訂單報價是否過低、到期日是否有維護
3. 客戶信用額度是否異常
4. 訂單交期是否異常
5. 訂單折扣是否合理
6. 訂單變更流程
7. 出貨單是否轉應收
8. 發票按時申報、序時序號
9. 售後服務的處理
10. 預收訂金開發發票
11. 明細帳與總帳是否一致
12. 銷退原因維護
13. 收款如期收回
14. 銷售差異分析
15. 

#### 現況訪談
1. 參觀工廠
2. 確認現況的製造流程
3. 確認現況的財務流程
4. 需求與產出資料
   +  提供一組製程料號與BOM資料
   +  提供相關憑證與報表
   +  第一版差異文件與現訪評估表

#### 教育訓練與操作(差異評估)
1. 教育訓練根據現訪的內容備課並依據SOP進行說明
2. 操作練習著重使用者的操作熟悉度。
3. 產出文件
   +  客戶已簽的SOP
   +  基本資料預計時程表
   +  第二版差異文件

#### 基本資料檢核與開帳說明
1. 根據時程表，顧問協助當天完成80%的進度。
2. 基本資料的匯入測試(料號、BOM、倉庫)
3. 產出文件
   +  開帳說明文件

#### 上機模擬與程式驗收
1. 根據基本資料資料庫進行模擬
2. 匯入庫存開帳演練
3. 實際案例二到三筆
4. 匯入財務開帳演練

#### 開帳與上線駐廠
1. 再次檢查事項
   +  委外流程(料號、BOM表、位置)
   +  單位、轉換率
   +  BOM表的靈活
   +  製造/人工費用科目與大類
   +  料號的設定(採購否、銷售否、單位、成本)
   +  盤點庫存調整的對象科目:9999
   +  財務匯率的維護
   +  如果有用內部序號，要注意品名一樣時的匯入方式。
   +  預估的製造費用/人工費用的成本單價維護。
4. 確認客戶盤點時間
5. 開帳流程範例
   +  29早上提供盤點數字、29下午財務計算成本
   +  30早上匯入料號(含成本單價)、30下午匯入批號、庫存數量；並進行財務開帳。
6. 

#### 14版變更流程
1. 調整後更改，會變更後面的數量。

#### 結帳流程
1. 成本核對要注意小數位差儘量調到正負相同，因庫存報告現況會用成本欄位計算。
2. 庫存計價可以的話要調整為進位。


#### 預計改善清單
1. 美金匯率的轉換:https://apps.odoo.com/apps/modules/12.0/currency_exchange_rate_inverted/
2. 帳齡現在是用到期日計算。

#### 指令紀錄
1. 更新BOM :update mrp_bom set consumption = 'flexible'
2. 更新單位:
  >  
    update product_template set uom_id=23,uom_po_id=23 where uom_id=20
    update mrp_bom set product_uom_id=38 where product_uom_id=22
    update mrp_bom_line set product_uom_id=23 where product_uom_id=20
3. 更新盤點:
  >  
    -- update account_move set state='draft' where name like 'STJ%'
    -- update account_move set name='/' where name like 'STJ%'
    delete from account_move where name like 'STJ%';
    delete from account_move_line where move_name like 'STJ%';
    delete from stock_move;
    delete from stock_move_line;
    delete from stock_valuation_layer;
    delete from stock_quant;
    update stock_inventory set state = 'confirm'
