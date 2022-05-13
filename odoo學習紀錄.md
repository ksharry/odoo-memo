## Harry寫全新紀錄
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

## Harry測試紀錄
1. O2M 表頭對表身(一對多)，僅表身有關連值
2. M2O 表頭對員工(多對一)，紀錄ID

## 沈弘哲
#### 1-18都為紙本memo,19以後調整git紀錄
* [Youtube Tutorial - odoo 手把手教學 - Many2one - part1](https://youtu.be/vb_Z8KCI-wk) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---many2one---part1)

* [Youtube Tutorial - odoo 手把手教學 - Many2many - part2](https://youtu.be/QeZfJqTGP-w) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---many2many---part2)

* [Youtube Tutorial - odoo 手把手教學 - One2many - part3](https://youtu.be/WiLdXP781N0) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---one2many---part3)

* [Youtube Tutorial - odoo 手把手教學 - One2many Editable Bottom and Top - part3-1](https://youtu.be/HJcBAFXQYVc) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---one2many-editable-bottom-and-top---part3-1)

* [Youtube Tutorial - odoo 手把手教學 - Search Filters - part4](https://youtu.be/zcWMs16p9Xw) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---search-filters---part4)

* [Youtube Tutorial - odoo 手把手教學 - 說明 noupdate 以及 domain_force - part5](https://youtu.be/twn6zz3OeRs) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---%E8%AA%AA%E6%98%8E-noupdate-%E4%BB%A5%E5%8F%8A-domain_force---part5)

* [Youtube Tutorial - odoo 手把手教學 - 如何透過 button 呼叫 view, form - part6](https://youtu.be/URxuH2HG44Q) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---%E5%A6%82%E4%BD%95%E9%80%8F%E9%81%8E-button-%E5%91%BC%E5%8F%AB-view-form---part6)

* [Youtube Tutorial - odoo 手把手教學 - 說明 name_get 和 _name_search - part7](https://youtu.be/g-dclCkwY5c) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---%E8%AA%AA%E6%98%8E-name_get-%E5%92%8C-_name_search---part7)

* [Youtube Tutorial - odoo 手把手教學 - 使用 python 增加取代 One2many M2X record - part8](https://youtu.be/GBCGS2znnT8) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---%E4%BD%BF%E7%94%A8-python-%E5%A2%9E%E5%8A%A0%E5%8F%96%E4%BB%A3-one2many-m2x-record---part8)

* [Youtube Tutorial - odoo 手把手教學 - tree create delete edit False - part9](https://youtu.be/0fpA89QcYZM) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---tree-create-delete-edit-false---part9)

* [Youtube Tutorial - odoo 手把手教學 - 同一個 model 使用不同的 view_ids - part10](https://youtu.be/YltcAu9OZhc) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---%E5%90%8C%E4%B8%80%E5%80%8B-model-%E4%BD%BF%E7%94%A8%E4%B8%8D%E5%90%8C%E7%9A%84-view_ids---part10)

* [Youtube Tutorial - odoo 手把手教學 - widget 介紹 handle 和 many2onebutton - part11](https://youtu.be/zb5fSEtEo_g) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---widget-%E4%BB%8B%E7%B4%B9-handle-%E5%92%8C-many2onebutton---part11)

* [Youtube Tutorial - odoo 手把手教學 - view 搭配 context - part12](https://youtu.be/c-nzbAuaH9I) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---view-%E6%90%AD%E9%85%8D-context---part12)

* [Youtube Tutorial - odoo 手把手教學 - view 搭配 active_test context - part13](https://youtu.be/RR9ycgky444) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---view-%E6%90%AD%E9%85%8D-active_test-context---part13)

* [Youtube Tutorial - odoo 手把手教學 - view 搭配 domain - part14](https://youtu.be/Rh-rmXIHTZo) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---view-%E6%90%AD%E9%85%8D-domain---part14)

* [Youtube Tutorial - odoo 手把手教學 - 如何看到當下 view 繼承頁面 - part15](https://youtu.be/Vs6ScbYuZNs) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---%E5%A6%82%E4%BD%95%E7%9C%8B%E5%88%B0%E7%95%B6%E4%B8%8B-view-%E7%B9%BC%E6%89%BF%E9%A0%81%E9%9D%A2---part15)

* [Youtube Tutorial - odoo 手把手教學 - odoo rainbow - part16](https://youtu.be/g4vywRLklE0) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---odoo-rainbow---part16)

* [Youtube Tutorial - odoo 手把手教學 - tree decoration - part17](https://youtu.be/tJdw6IEb8UQ) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---tree-decoration---part17)

* [Youtube Tutorial - odoo 手把手教學 - model _rec_name 說明 - part18](https://youtu.be/JtcSnbHNjAU) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---model-_rec_name-%E8%AA%AA%E6%98%8E---part18)
  + 有寫rec_name指定要取代name的欄位。

* [Youtube Tutorial - odoo 手把手教學 - copy override 說明 - part19](https://youtu.be/VDnIFb7e7wM) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---copy-override-%E8%AA%AA%E6%98%8E---part19)
  + copy預設為True，可以自動複製其他資料
  + 要了解原生copy邏輯在使用會比較好，避免異常。

* [Youtube Tutorial - odoo 手把手教學 - move position 說明 - part20](https://youtu.be/l-bFOqTYgTA) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---move-position-%E8%AA%AA%E6%98%8E---part20)
  + 繼承交換兩個欄位的位置
  + 前面加一個ID，此ID位置為MOVE

* odoo 手把手教學 - ir.actions.act_url 說明 - part21 - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---iractionsact_url-%E8%AA%AA%E6%98%8E---part21)

* [Youtube Tutorial - odoo 手把手教學 - Smart Button 說明 - part22](https://youtu.be/fsZK1KRgnF0) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---smart-button-%E8%AA%AA%E6%98%8E---part22)
  + 新增計數功能

* [Youtube Tutorial - odoo 手把手教學 - options create_edit 說明 - part23](https://youtu.be/GdPKllI7quI) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---options-create_edit-%E8%AA%AA%E6%98%8E---part23)
  + 欄位的options功能
    + options="{'no_quick_create': True}
    + options="{'no_create_edit': True}
    + options="{'no_create': True}
    + options="{'no_open': True}
 
* [Youtube Tutorial - odoo 手把手教學 - PostgreSQL ondelete cascade 說明 - part24](https://youtu.be/OTh5R2LrwJE) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---postgresql-ondelete-cascade-%E8%AA%AA%E6%98%8E---part24)
  + ondelete是postgres的功能依附
  + 參數說明
    + ondelete='set null'
    + ondelete='cascade'
    + ondelete='restrict'

* [Youtube Tutorial - odoo14 手把手教學 - auto_join 說明 - part25](https://youtu.be/OOlPZETkYKw) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/14.0/demo_expense_tutorial_v1#odoo14-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---auto_join-%E8%AA%AA%E6%98%8E)
  + auto_join預設是false，使用sub查詢
  + odoo12使用inner join ;  odoo14改用left join
  + 補充[odoo 教學 - 如何透過 log_level 了解 ORM RAW SQL](https://www.youtube.com/watch?v=sZWFGf23gWc)
    +  orm指令對應的raw sql
    +  log_level = debug_sql      

* [Youtube Tutorial - odoo 手把手教學 - view parent 說明 - part26](https://youtu.be/i_hG4s_YJN0) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---view-parent-%E8%AA%AA%E6%98%8E---part26)
  + o2m,m2o時，要抓單頭資料做限制時使用
  + 說明:
    + 語法:<field name="tag_ids" widget="many2many_tags" attrs="{'readonly': [('parent.name', '=', 'test-readonly')]}"/>
    + 要搭配<tree editable="top">使用才會生效 -->直接編輯才生效
 
* [Youtube Tutorial - odoo 手把手教學 - domain 搭配 fields 的三種用法 - part27](https://youtu.be/ZUNRoWxVWAE) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---domain-%E6%90%AD%E9%85%8D-fields-%E7%9A%84%E4%B8%89%E7%A8%AE%E7%94%A8%E6%B3%95---part27)
  + 第一種 - 直接在 model 中的 fileds 定義  employee_id = fields.Many2one('hr.employee', string="Employee", required=True,domain=[('active', '=', True)] )
  + 第二種 - 直接在 view 中的 fileds 定義  <field name="employee_id" domain="[('user_id', '=', user_id)]"/>
  + 第三種 - 透過 onchange 的方法增加 domain result = dict() , result['domain'] = {'employee_id': [('user_id', '=', self.user_id.id)]

* [Youtube Tutorial - odoo 手把手教學 - form_view_ref 以及 tree_view_ref 說明 - part28](https://youtu.be/_YkrOp3ytlQ) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---form_view_ref-%E4%BB%A5%E5%8F%8A-tree_view_ref-%E8%AA%AA%E6%98%8E---part28)
  + 一個M2O跳出的編輯畫面，如果有兩個畫面，取後面的為主。
  + 語法:<field name="sheet_id" context="{'form_view_ref':'demo_expense_tutorial_v1.view_form_demo_expense_sheet_tutorial'}"/>

* odoo 手把手教學 - Message Post 教學 - part29 - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---message-post-%E6%95%99%E5%AD%B8---part29)

* [Youtube Tutorial - odoo 手把手教學 - groups 搭配 fields 用法 - part30](https://youtu.be/JyNyg7iHar0) - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---groups-%E6%90%AD%E9%85%8D-fields-%E7%94%A8%E6%B3%95---part30)
  + 使用model定義群組比較嚴謹，使用view相對就是隱藏而已
  + module語法:groups='demo_expense_tutorial_v1.demo_expense_tutorial_group_manager'
  + view語法:<field name="tag_ids" widget="many2many_tags" groups="demo_expense_tutorial_v1.demo_expense_tutorial_group_manager"/>
 
* [(等待新增)Youtube Tutorial - odoo 手把手教學 - ACID transactions 說明 - part31]() - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---acid-transactions-%E8%AA%AA%E6%98%8E---part31)

* [(等待新增)Youtube Tutorial - odoo 手把手教學 - 特殊 groups 應用說明 - part32]() - [文章快速連結](https://github.com/twtrubiks/odoo-demo-addons-tutorial/tree/master/demo_expense_tutorial_v1#odoo-%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E5%AD%B8---%E7%89%B9%E6%AE%8A-groups-%E6%87%89%E7%94%A8%E8%AA%AA%E6%98%8E---part32)
