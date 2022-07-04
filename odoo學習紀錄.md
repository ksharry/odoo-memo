## Harry研究原生-銷售
1. 銷售確認段產生出貨單(stock/stock_move.py的_action_confirm寫入picking後寫入stock_move,1151,1158)
   + stock.move與stock.move.line與stock.picking寫入
     + config.py的module_procurement_jit設定picking_no_auto_reserve為Ture-odoo14無直接相關
     + sale_management/sale_order.py的action_confirm呼叫_action_confirm(
     + sale/sale.py的_action_confirm呼叫_action_launch_stock_rule
     + sale_stock.py的_action_launch_stock_rule呼叫self.env['procurement.group'].run(procurements)
     + stock_rule.py的run決定走_run_pull
     + ***stock_rule.py的_run_pull寫入stock_move，接著走moves._action_confirm()
     + stock_move.py的moves._action_confirm()會去呼叫_assign_picking(
     + stock_move.py的_assign_picking呼叫Picking.create(
     + ***stock_picking.py的create寫入stock.picking
       +  先取pick.type取得序號
       +  _autoconfirm_picking 立即移轉的才啟用
       +  新增追蹤者
     +  回到stock_move.py的_action_done呼叫moves._action_confirm()會去呼叫_action_assign
     +  ***stock_move.py的_action_assign寫入stock_move_line
2. 創建應收:
   + Wizard:action_view_sale_advance_payment_inv
   + sale_make_invoice_advance.py的create_invoices呼叫sale_orders._create_invoices(final=self.deduct_down_payments)
   + sale.py的_create_invoices呼叫self.env['account.move'].sudo().with_context(default_move_type='out_invoice').create(invoice_vals_list)
     + account_account.py的create呼叫res_ids = super(AccountGroup, self).create(vals_list)
     + account_move.py的create呼叫self._move_autocomplete_invoice_lines_create(vals_list)
     + account_move.py的_move_autocomplete_invoice_lines_create呼叫new_vals_list.append(move._move_autocomplete_invoice_lines_values())產生稅金與應收，並檢查是否平衡
     + account_move.py的_move_autocomplete_invoice_lines_values呼叫self._recompute_dynamic_lines(recompute_all_taxes=True)
       + 計算稅金:account_move.py的_recompute_dynamic_lines呼叫_compute_base_line_taxescreate寫入account_move_line
       + 計算cash:account_move.py的_recompute_dynamic_lines呼叫_recompute_cash_rounding_lines()
       + 計算付款:account_move.py的_recompute_dynamic_lines呼叫_recompute_payment_terms_lines寫入account_move_line
   + 處理退貨:action_switch_invoice_into_refund_credit_note
   + 回到sale_make_invoice_advance，因為open_invoice=1，所以關閉銷售，開啟應收畫面action_view_invoice

3. 銷售出貨
   + 取消預留(do_unreserve):
     + stock_picking.py的do_unreserve呼叫_do_unreserve
     + stock_move.py的_do_unreserve
       + 更新stock_move_line的product_uom_qty為0
       + 刪除stock_move_line的資料
   + 檢查可用性(action_assign):
     + stock_picking.py的action_assign呼叫_action_assign
     + _action_assign產生stock_move_line  
     + account_move的forecast_availability是計算欄位，建議名稱要更改預測可用數量
   + 驗證流程(stock_account/stock_move.py的251,273,280)
     + Quant寫入
       + stock/wizard/Stock_immediate_transfer.py的process呼叫pickings_to_validate.with_context(skip_immediate=True).button_validate()
       + sale_stock/stock.py的_action_done呼叫super()._action_done()
       + stock/stock_picking.py的button_validate呼叫pickings_to_backorder.with_context(cancel_backorder=False)._action_done()
       + stock_account/stock_move.py的_action_done呼叫super(StockMove, self)._action_done(cancel_backorder=cancel_backorder)
       + stock/stock_move.py的_action_done呼叫moves_todo.mapped('move_line_ids').sorted()._action_done()
       + *stock/stock_move_line.py的_action_done呼叫Quant._update_reserved_quantity(
       + stock/stock_quant.py的_update_reserved_quantity呼叫quant.reserved_quantity -= max_quantity_on_quant
       + field.py寫入__set__寫入records.write({self.name: write_value})
       + stock/stock_quant.py的write呼叫super(StockQuant, self).write(vals)
       + moodels.py寫入更新
     + SVL寫入
       + stock/wizard/Stock_immediate_transfer.py的process呼叫pickings_to_validate.with_context(skip_immediate=True).button_validate()
       + stock/stock_picking.py的button_validate呼叫pickings_to_backorder.with_context(cancel_backorder=False)._action_done()
       + sale_stock/stock.py的_action_done呼叫super()._action_done()
       + stock/stock_picking.py的_action_done呼叫todo_moves._action_done(cancel_backorder=self.env.context.get('cancel_backorder'))
       + stock_account/stock.move.py的_action_done呼叫_create_out_svl
       + stock_account/stock.move.py的_create_out_svl呼叫self.env['stock.valuation.layer'].sudo().create(svl_vals_list)寫入SVL
       + base/ir_field.py的create進行新增
     + 計價分錄寫入
       + stock/wizard/Stock_immediate_transfer.py的process呼叫pickings_to_validate.with_context(skip_immediate=True).button_validate()
       + stock/stock_picking.py的button_validate呼叫pickings_to_backorder.with_context(cancel_backorder=False)._action_done()
       + sale_stock/stock.py的_action_done呼叫super()._action_done()
       + stock/stock_picking.py的_action_done呼叫todo_moves._action_done(cancel_backorder=self.env.context.get('cancel_backorder'))
       + stock_account/stock_move.py的_action_done呼叫svl.stock_move_id._account_entry_move(svl.quantity, svl.description, svl.id, svl.value)
       + stock_account/stock_move.py的_account_entry_move呼叫self.with_company(company_from)._create_account_move_line(acc_valuation, acc_dest, journal_id, qty, description, svl_id, cost)
       + stock_account/stock_move.py的_create_account_move_line呼叫AccountMove.sudo().create(寫入分路明細(_prepare_account_move_line)

3. 應收
   + 驗證(action_post)
     + 大綱(stock_account/account_move.py的_post48,55行)
       + _onchange_invoice_date(會改變重新計算匯率與稅金
       + _check_balanced是用SQL去判斷
       + stock_account/account_move.py的_post呼叫self.env['account.move.line'].create(self._stock_account_prepare_anglo_saxon_out_lines_vals())-COGS
         + stock_account/product.py的get_product_accounts呼叫get_product_accounts取得目錄的所有ID
         + account/product.py
           + INCOME:收入
           + EXPENSE:成本
           + STOCK_INCOME:應付暫估
           + STOCK_OUTCOME:待出貨
           + STOCK_VALUATION
           + 看起來是用標準價格進行給值
       + stock_account/account_move.py的_post呼叫_stock_account_anglo_saxon_reconcile_valuation呼叫product_account_moves.reconcile()
       + account/account.move.py的reconcile呼叫self.env['account.partial.reconcile'].create(sorted_lines._prepare_reconciliation_partials())產生資料，寫A8內的資料
       + account/account.move.py的reconcile呼叫self.env['account.full.reconcile'].create({-寫哪一個A8
   + 重製草稿(button_draft)
     + account_edi/account_move.py的.remove_move_reconcile()移除
     + account_full_reconcile.py的moves_to_reverse._reverse_moves移除
     + stock_account/account_move.py的button_draft呼叫filtered(lambda line: line.is_anglo_saxon_line).unlink()移除account_move
   + 付款(action_register_payment)
     + Call wizard AccountPaymentRegister.py的action_create_payments
     + _create_payments呼叫_get_batches()設定:keyvalue(付款)與line(會計明細)
     + _create_payment_vals_from_wizard()整理付款資訊
     + self.env['account.payment'].create(payment_vals_list)寫入
       + account_payment.py呼叫super().create(vals_list)
       + account.move.py寫入account_move
       + account.move.py寫入account_move_line
     + payment/account.payment.py
       + 進行s2s
       + 呼叫原生的會計過帳
       + 再次進行調節
   + 增加(invoice_outstanding_credits_debits_widget)
     + account/account_payment_field.js的_onOutstandingCreditAssign呼叫js_assign_outstanding_line
     + account_edi/account_move.py的reconcile呼叫super().reconcile()
       + account/account_move.py的reconcile呼叫_create_tax_cash_basis_moves(
         + account/account_partial_reconcile.py呼叫moves._post(soft=False)
           + sale/account_invoice.py
           + stock_account/account.move.py
           + account_edi/account.move.py
           + account/account.move.py寫入過帳
       + account/account_move.py的reconcile呼叫呼叫_create_exchange_difference_move(-計算匯差
       + account/account_move.py的reconcile呼叫self.env['account.full.reconcile'].create(-寫入調節
     + account_edi/account_move.py的_update_payments_edi_documents更新EDI
     + 回到會計畫面顯示(_compute_payments_widget_to_reconcile_info)

## Harry寫服務模組3
#### [cookbook網址](https://alanhou.org/odoo-14-cms-website-development/)
1.  寫snippet
    + 寫view，樣板格式，website.snippets
    + 寫位置expr="//div[@id='snippet_structure']
    + 原生的snippet位置:website/view/snippet/s_text_block.xml
2.  寫動態snippet
    + snippet寫xml樣板，
    + 註冊組建與選項
    + 寫js，/static/src/js/snippets.js，動態渲染
    + snippet添加JS文件website.assets_frontend

## Harry測試ODOO15
#### [沈弘哲odoo15網址](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/15.0)
1. 視訊 chrome://flags/#unsafely-treat-insecure-origin-as-secure
2. 協同編輯
3. HTML Editor
4. Group By Many2many
5. Project Sharing - backend view
6. command palette
7. Website Builder, Configuration Wizards, New Jinja mail templates
8. Scheduled Actions Triggers
9. Profiling and Product Images，[profiling使用說明](https://github.com/jlfwong/speedscope/blob/main/README-zh_CN.md)

## Harry寫服務模組紀錄2
#### [沈弘哲網址](https://github.com/twtrubiks/odoo-demo-addons-tutorial)
1. 寫class繼承問題紀錄
   + depend 要用底線(e_service)
   + class名稱要改
   + view的繼承ID是模組.畫面(要加上模組)
2. 寫prototype繼承紀錄
   + 使用在mail_thread,depend mail
3. 寫delegation繼承紀錄
   + user與partner;產品與產品變體
   + 應用在建立明細時，自動產生主檔
   + 所以子合約不適合，因為會自動產生主合約。
   + 想不到應用，什麼情會需要建立小的大的也要自動建立。
4. 加acton步驟
   + 在data寫xml(ir.actions.server),並指定model與函式
   + py寫函式內容
   + 後台的action與service actiond可以查詢
   + 空的 recordset 行為也像是 singleton
5. 加排程動作
   + 在data寫xml(ir.cron)，並指定model與函式
   + py寫函釋內容
   + 後台的安排的動作可以查看寫好的排程
6. 加編號
   + 在data下寫xml(e_service_sequence_id)
   + py檔寫功能，在寫入時更新create
   + 後台序號可以查看
7. 加活動
   + 在data下寫xml(mail.activity.type)
   + PY程式寫繼承、功能，VIEW寫活動畫面
   + 後台活動類型可以查看
   + 多寫User_id，遇到未知的權限錯誤，查看是不是table寫錯與更新模組。
8. 新增Wizard
   + TransientModel會寫入檔案並定期刪除
   + 寫wizard下py與xml
   + 第一種:寫按鈕觸發wizard.xml的action觸發view並傳值，使用context="{'default_partner_id': partner_id}"，接著py檔透過default_get接收後進行預設
   + 第二種:在wizard的按鈕透過view預設再進行傳值到py。field name="context">{'default_test_pass_data': 'hello 123'}
   + 第三種:寫py按鈕透過呼叫另一個功能傳值
9. 新增Report
   + AbstractModel沒有Table
   + report_action() 會去 call _get_report_values()
   + odoo External ID not found in the system 是因為manifest沒有加
   + 要記得模組+action，不然會出現ValueError: not enough values to unpack (expected 2, got 1)
   + 架構
     + 寫一個列印的Wizard取得條件值，寫一個XML點選下載XML，透過PY傳值。
     + 寫一個樣板，並用record紀錄樣板的action按鈕
10. 新增Config 
    + 新增py繼承，並寫get與set
    + 寫xml
    + 寫預設還是要透過LIB取值
11. 時區
    + 資料庫是原始時間，透過時區轉換進行呈現，odoo會自動轉。
12.  barcode(無掃描槍，PASS)
13.  hierarchy撰寫(聯繫人)
14.  fields_view_getPASS
15.  多公司測試PASS
16.  testing測試
     + python C:/odoo/odoo-14.0/odoo-bin -i e_service -d dsc -c C:/odoo/odoo-14.0/odoo.conf --test-enable
     + 要有限制跟欄位
17.  orm快取-增加性能，使用裝飾器直接存取。
18.  Raw SQL 有Tuple跟dic格式，跳過ORM。
19.  pivot用SQL寫另一個VIEW去串
20.  image_mixin與binary(不需要其他的尺寸)
21.  [0,0 創造  [4,0 連接 [6,0 連結多筆(IDS)]]]
22.  odoo13以上只要view上面有加<field name="active" invisible="1"/>，就有歸檔功能
23.  odoo14以上有search panel只能多對一，可以調整參數
24.  domain 
     + 波蘭表示式,[domain文件](https://www.odoo.com/documentation/12.0/developer/howtos/backend.html#domains)
     + 從後面往回看
25.  index
     + [文件](https://docs.postgresql.tw/the-sql-language/performance-tips/using-explain)
     + 指令:explain select id,state from e_service
     + Seq Scan
26.  ORM write差異:flush()可看出僅寫入一次
27.  寫controllers
     + db_name = dsc
     + http指令
       + http://localhost:8069/get_e_service/type1
       + console指令
         + import requests
         + r = requests.get('http://localhost:8069/get_e_service/type1')
         + r.text
         + r.json()
     + json指令
       + curl -X POST -H "Content-Type: application/json" -d "{}" http://localhost:8069/get_e_service/type2 
28.  透過 AbstractModel 擴充 Model
     + 同一種東西，不同應用。
29.  session_redis
     + 統一管理，速度較快
30.  Odoo 15 中的 LISTEN/NOTIFY 運作原理
     + /longpolling/poll 這個 URL 觸發 LISTEN
       + odoo15/addons/bus/controllers/main.py
       + odoo15/addons/bus/models/bus.py
     + 觸發 NOTIFY
       + odoo15/addons/mail/controllers/discuss.py
       + odoo15/addons/mail/models/mail_thread.py
       + odoo15/addons/mail/models/mail_channel.py
       + odoo15/addons/bus/models/bus.py
31.  odoo15 Restfulapi
     + Restfulapi設定
       + GET /api/users/ 取得全部 res_users.
       + GET /api/users/<string:pk> 取得特定 res_users.
       + POST /api/users/ 新增 res_users.
       + PATCH /api/users/<string:pk> 修改特定 res_users 資料.
     + 取得
       + ODOO要先取得session id,web/session/authenticate/
       + http://localhost:8069/api/users/
32.  寫controller
     + 新增controllers，寫路徑與傳值
     + 寫view用網頁，引入網頁外框<t t-call="website.layout">


## Harry開發課程紀錄(齊揚)
#### 課程紀錄
1. 6/7-odoo開發環境介紹
   + Pycharm 環境介紹說明
   + odoo template 配置
   + odoo config 介紹
   + postgres db config 
   + odoo MVC 架構介紹
 
2. 6/10-odoo Model 介紹
   + Model 定義介紹
   + function,compute,default 應用
   + module init & module manifest 介紹解說
   + odoo 開發設計框架詳細解說 Generic Structure
 
3. 6/14-odoo ORM API(上)
   + datatype 詳細解說(Char,Int,Date,Datetime,Boolean,Binary 一般欄位)
   + datatype 詳細解說(M2o,O2m,M2m 複合式欄位)
   + datatype compute 類型欄位設計及應用

4. 6/17-odoo ORM API(下)
   + Odoo shell script 應用練習
   + odoo domain,filter,sort 練習
   + Record Set,Tuple,List,field 介紹
   + 教學案例介紹(簡易文管模型)
 
5. 6/21-odoo security介紹
   + 講義O2M,M2M:
     + (0,_,{‘field’:value})    新建一條記錄並將其與之關聯
     + (1,id,{‘field’:value})   更新已關聯記錄的值
     + (2,id,_) 移除關聯並刪除id關聯的記錄
     + (3,id,_) 移除關聯但不刪除id關聯的記錄,通常使用來刪除many2many欄位關聯值
     + (4,id,_) 關聯已存在記錄,僅適用  many-to-many 欄位
     + (5,_,_)  刪除所有關聯,但不刪除關聯記錄
     + (6,_,[ids]) 替換已關聯記錄清單為此處清單
   + O2M使用
     + 0 :　新增子表
     + 1,id :　更新子表
     + 2,id :　刪除子表
     + 3,id :　刪除子表
     + 5 :　刪除所有子表
   + model insert/write/update/delete security 配置介紹
   + model security groups 配置規劃
   + model security rule配置規劃
  
6. 6/24 odoo inherit 介紹
   + odoo model inherit 規劃應用
   + odoo view inherit 規劃應用
 
7. 6/28 odoo 自行案例設計規劃
   + 規劃設計構思
   + 使用介面構思
   + 表格欄位設計
   + 邏輯功能運用
   + 測試及偵錯訓練
 
8. 7/1-odoo report 介紹
   + Qweb report 介紹
   + py3o template(Build Fish Report) 設計應用

## Harry寫報表紀錄
#### 銷售報表
1. QWEB紀錄
   + t-call是呼叫樣板
   + t-att是屬性
2. 調整客戶靠左
   + report_saleorder_document
   + external_layout_standard
   + address_layout 調整t-att-class="colclass"為class="col-6"，使用視圖尋找外部ID
3. 調整銷售報表表身
   + 清空欄位資料
   + 新增表格框度 th width="15%"
4. 調整銷售報表新增一個Action(寫XML即可)
   + 先寫action對應語言樣板report_saleorder
   + 寫語言樣板對應客製的不印稅樣板report_saleorder_document
   + 後台要將Menu加上去下拉選單才會有
   + 翻譯會是空的
   + 如何進化用同一樣板進行
#### 庫存報表
1. 修改出貨單品名
   + 新增一個繼承
   + stock.report_delivery_document
   + 調整product_id.name
   + 如果出貨為done，樣板:stock_report_delivery_aggregated_move_lines
   + 上面的改function:_get_aggregated_product_quantities
2. 寫簽收欄位
   + 使用web.external_layout_standard調整新增頁尾，並空一行
#### 應收報表
1. 修改出貨單品名

## Harry寫服務模組紀錄
#### [沈弘哲網址](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1)
1. name,model都要用.；id用底線
2. 注意IDS的名稱對應。
3. tree editable="bottom"
4. d2 = datetime.datetime(2022, 4, 13, 8, 0, 0)
5. tree string="no_create_tree" create="0" delete="false" edit="1" editable="top">
6. [Advanced Views](https://www.odoo.com/documentation/12.0/developer/howtos/backend.html#advanced-views)
7. tree default_order="name"
8. odoo14很像預設ondelete='restrict'
9. python C:/odoo/odoo-14.0/odoo-bin shell -d dsc -c C:/odoo/odoo-14.0/odoo.conf
10. self.env['e.service.b'].search([('service_id.name', '=', 'A0001')])
11. SELECT "e_service_b".id FROM "e_service_b" LEFT JOIN "e_service" AS "e_service_b__service_id" ON ("e_service_b"."service_id"
 = "e_service_b__service_id"."id") WHERE ("e_service_b__service_id"."name" = 'A0001') ORDER BY  "e_service_b"."id"
e.service.b(1, 7, 8, 9)
12. 反灰 attrs="{'readonly': [('state', '=', 'available')]}"
13. browse()主要根據記錄id 返回記錄 多個，采用列表形式。單個時可以直接傳入
14. self.env['e.service'].with_user(2).browse(1).note，沒測出來權限

#### 1-18都為紙本memo,19以後調整git紀錄
15. Youtube Tutorial - odoo 手把手教學 - model _rec_name 說明 - part18
    + 有寫rec_name指定要取代name的欄位。

16. Youtube Tutorial - odoo 手把手教學 - copy override 說明 - part19
    + 要了解原生copy邏輯在使用會比較好，避免異常。

17. Youtube Tutorial - odoo 手把手教學 - move position 說明 - part20
    + 繼承交換兩個欄位的位置
    + 前面加一個ID，此ID位置為MOVE
18. odoo 手把手教學 - ir.actions.act_url 說明 - part21
19. Youtube Tutorial - odoo 手把手教學 - Smart Button 說明 - part22
    + 新增計數功能
20. Youtube Tutorial - odoo 手把手教學 - options create_edit 說明 - part23
    + 欄位的options功能
      + options="{'no_quick_create': True}
      + options="{'no_create_edit': True}
      + options="{'no_create': True}
      + options="{'no_open': True}
 
21. Youtube Tutorial - odoo 手把手教學 - PostgreSQL ondelete cascade 說明 - part24
  + ondelete是postgres的功能依附
  + 參數說明
    + ondelete='set null'
    + ondelete='cascade'
    + ondelete='restrict'

22. Youtube Tutorial - odoo14 手把手教學 - auto_join 說明 - part25
    + auto_join預設是false，使用sub查詢
    + odoo12使用inner join ;  odoo14改用left join
    + 補充[odoo 教學 - 如何透過 log_level 了解 ORM RAW SQL](https://www.youtube.com/watch?v=sZWFGf23gWc)
      +  orm指令對應的raw sql
      +  log_level = debug_sql      

23. Youtube Tutorial - odoo 手把手教學 - view parent 說明 - part26
    + o2m,m2o時，要抓單頭資料做限制時使用
    + 說明:
      + 語法:<field name="tag_ids" widget="many2many_tags" attrs="{'readonly': [('parent.name', '=', 'test-readonly')]}"/>
      + 要搭配<tree editable="top">使用才會生效 -->直接編輯才生效
 
24. Youtube Tutorial - odoo 手把手教學 - domain 搭配 fields 的三種用法 - part27
    + 第一種 - 直接在 model 中的 fileds 定義  employee_id = fields.Many2one('hr.employee', string="Employee", required=True,domain=[('active', '=', True)] )
    + 第二種 - 直接在 view 中的 fileds 定義  <field name="employee_id" domain="[('user_id', '=', user_id)]"/>
    + 第三種 - 透過 onchange 的方法增加 domain result = dict() , result['domain'] = {'employee_id': [('user_id', '=', self.user_id.id)]

25. Youtube Tutorial - odoo 手把手教學 - form_view_ref 以及 tree_view_ref 說明 - part28
    + 一個M2O跳出的編輯畫面，如果有兩個畫面，取後面的為主。
    + 語法:<field name="sheet_id" context="{'form_view_ref':'demo_expense_tutorial_v1.view_form_demo_expense_sheet_tutorial'}"/>
 
26. Youtube Tutorial - odoo 手把手教學 - groups 搭配 fields 用法 - part30
    + 使用model定義群組比較嚴謹，使用view相對就是隱藏而已
    + module語法:groups='demo_expense_tutorial_v1.demo_expense_tutorial_group_manager'
    + view語法:<field name="tag_ids" widget="many2many_tags" groups="demo_expense_tutorial_v1.demo_expense_tutorial_group_manager"/>
 
27. Youtube Tutorial - odoo 手把手教學 - ACID transactions 說明 - part31
    + [Transaction教學2](https://www.youtube.com/watch?v=P67IfMK4Y5g)
    + Atomicity，原子性就是一筆錯就全部ROLLBACK
    + Consistency，交易結束後狀態要一致
    + Isolation 隔離性，資料庫允許多個並發交易，但會先鎖定
    + Durability 持久性
 
28. Youtube Tutorial - odoo 手把手教學 - 特殊 groups 應用說明 - part32
    + base.group_no_one 開發者模式才可以看得到



## Harry測試紀錄
1. O2M 表頭對表身(一對多)，單頭
2. M2O 表頭對員工(多對一)，單身

