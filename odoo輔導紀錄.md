<table>
    <tr>
        <td>輔導紀錄</td>
    </tr>
</table>

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

#### 結帳流程
1. memo


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
