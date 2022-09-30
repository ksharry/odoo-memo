# bf報表
1. windows要改設定裡面為ods，排除pdf的錯誤。
2. [bf影片](https://www.youtube.com/watch?v=JbCn0uspnvc&list=PL6I8Iehrw1KZdLfEa4zSnf2tmVOjy2puF&index=2)

# pycharm的git設定
1. 先git bash 用git clone
2. 設定pycharm git
3. 設定pycharm的編譯器與路徑
4. 

# odooapps上架
1. [教學網址](https://www.youtube.com/watch?v=rryr9ILToT0&t=1608s)
2. github新增repostory
3. git clone 網址
4. cd 目錄
5. git checkout -b 13.0
6. git config user.name ksharry
7. git config user.email ksharry1025@gmail.com
8. 整理模組確認是否可用
9. git add -A
10. git commit -M "[ADD] XXX"
11. git push --set-upstream origin 13.0
12. 到ODOO帳號進行設定
13. 如果要金額就打上金額與幣別就可以

# ODOO 相關
1. odoo13以前是backbone.js

## ODOO Mick曾 VSCODE用DOCKer設定方式
1. /usr/bin/python3 -m debugpy --listen 0.0.0.0:4000 /usr/bin/odoo -c /etc/odoo/odoo.conf
2. debug 你要先加 python 套件 debugpy
3. https://dev.to/kerbrose/how-to-remote-debugging-odoo-docker-images-python-based-framework-4o2h

## ODOO OWASP TOP10-[網址](https://www.odoo.com/zh_TW/security#owasp)
0. 定義網路安全(開放式Web應用程式安全專案（OWASP）是一個線上社群，在Web應用安全領域提供免費的文章，方法，文件，工具和技術)
1. Injection Flaw-[網址](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/589498/)
   + select id from users where username = '' or 1=1–  and password = '123'
2. Cross Site Scripting (XSS)-竊取用戶的 cookie-[網址](https://www.gss.com.tw/images/stories/epaper_GSS_security/pdf/epaper_gss_security_0067.pdf)
   + 使用者點擊特製的連結稱為 Reflected Attack
   + 網頁中已植入惡意的語法稱為 Persistent Attack
3. Cross Site Request Forgery (CSRF)跨站請求偽造-[網址](https://tech-blog.cymetrics.io/posts/jo/zerobased-cross-site-request-forgery/)
   + 預防:增加所有敏感動作的驗證方式，例如：金流、提交個資 等…多加一道驗證碼的機制
4. Malicious File Execution - 透過漏洞安裝惡意程式Drive-by Download -[網址](https://ithelp.ithome.com.tw/articles/10000037)
5. Insecure Direct Object Reference - 透過路徑取得密碼 - [網址](https://blog.xuite.net/tolarku/blog/302302400-%E5%8E%9F%E4%BE%86%E9%80%99%E9%BA%BC%E7%B0%A1%E5%96%AE%E5%B0%B1......+-+Insecure+Direct+Object+Reference)
6. Insecure Cryptographic Storage - 加密AES,SHA256 - [網址](https://ithelp.ithome.com.tw/articles/10222412)
7. Insecure Communications - https - 網址同上
   + [odoo安全清單](https://www.odoo.com/documentation/14.0/administration/install/deploy.html#security)
8. Failure to Restrict URL Access - 用戶都須透過數據訪問驗證才能存取，例如訂單驗證 -網址同上

## ODOO 網址紀錄
1. [odoo設定對應不同網址對應PORT](https://www.odoo.com/zh_TW/forum/bang-zhu-1/odoo-14-multi-site-multi-company-same-server-shared-database-not-forwarding-traffic-to-the-correct-domain-182679#answer-182686)
2. [nginx設定對應不同網址對應PORT](https://www.serverlab.ca/tutorials/linux/web-servers-linux/how-to-configure-multiple-domains-with-nginx-on-ubuntu/)

#### 紀錄:
1. 清除模組:app_odoo_customize
2. odoosleep:
   + pip install interval
   + pip install ecpay_invoice3

#### bf安裝
1. sudo su - odoo -s /bin/bash
2. sudo apt-get install unoconv
3. pip3 install py3o.template

#### 搬動addons指令
1. sudo cp -a /odoo/custom/git/addons/* /odoo/custom/addons/.
2. sudo chown -R odoo: /odoo/custom/addons
3. sudo chmod 755 -R /odoo/custom/addons

#### github使用方式(win10)
1. 參考https://progressbar.tw/posts/3
2. https://git-scm.com/downloads
3. 打開Git Bash
4. ssh-keygen -t rsa -C 'Your email address'
5. ssh-add ~/.ssh/id_rsa  or   ssh-agent bash
6. C:\Users\Harry\.ssh
7. 到github的設定，新增SSH的金鑰。
8. 上傳程式碼，先前要先去新增repositories。
  > 
    echo "# googlesheets" >> README.md
    cd /odoo/custom/git
    git init
    git add README.md
    git add .
    git remote -v
    git pull origin main
    git commit -m "first commit"
    git branch -M main
    git remote add origin git@github.com:ksharry/googlesheets.git
    git push -u origin main
7. 如果要隱藏，就增加.gitignore，如有檔案操作如下
  > 
    git rm --cached xxxx(檔名)
    git clean -fX
    重新push
 7. git更新odoo指令
  > 
    git pull
    ./odoo-bin -d [資料庫名稱] -u base

#### 無計價處理方式
1. 應付
   + 複製一筆近期的計價
   + 更新stock_valuation的account_id
   + 更新account_move_line 的 move_id (如果填入的單號-料號正確，很像會自己填入)
2. 應收
   + 因為應收確認要對應，因此要新增account_full_reconcile做對應
   + 對應account_move_line

#### 沃X-出貨保留問題
1. 更新stock_move_line的數量
2. 更新stock_quant的預留數量
3. Can't reserve products for lot-訂單
4. It is not possible to unreserve more products of %s than you have in stock-出貨
5. 調撥數量異常(調整product_qty) select * from stock_move_line where reference='WH/MISC8/00027'

#### 城X-價差問題
1. 更新account_move_line的purchase_id

#### 應收查看
1. order_move_line
   + qty_delivered > 0的表示有出貨
   + qty_invoiced > 0 表示有轉應收
   + 要查看應收與訂單關聯(invoice_lines)如下
     > select * from sale_order_line_invoice_rel where invoice_line_id = 15696
2. account_move_line
   + 銷貨收入可以抓的exclude_from_invoice_tab = false and 科目是411開頭
   + 應收帳款可以抓residual>0的
   + 單收入與應收可以排除is_anglo_saxon_line是空白的
   + 應收本幣抓balance，原幣抓price_total

2. stock_move有存訂單明細的ID
   + 關聯出貨與應收如下:(需安裝第三方:stock_picking_invoice_link)
     > select * from stock_move_invoice_line_rel where move_id in ('19379', '17272');
3. 跨資料庫更新
   + 第一步
     > CREATE EXTENSION dblink;
   + 第二步
     > INSERT INTO account_incoterms(id, name, code, active, create_uid,create_date, write_uid, write_date) SELECT id, name, code, active, create_uid,create_date, write_uid, write_date FROM dblink('dbname=abcd host=localhost port=5432 user=odoo password=odoo', 'select id, name, code, active, create_uid,create_date, write_uid, write_date from account_incoterms') AS fields(id integer, name character varying, code character varying, active boolean, create_uid integer,create_date timestamp without time zone, write_uid integer, write_date timestamp without time zone);

select setval('account_incoterms_id_seq', (select max(id)+1 from account_incoterms), false);

#### 更改LOGO位置
1. /home/wkc/odoo-dev/odoo/addons/web/static/src/img

#### 腳本匯入
1. sudo su - odoo -s /bin/bash
2. 編輯檔案路徑
3. python3 odoo-bin shell -c /etc/odoo-server.conf  -d dsc_211228 --no-http
4. exec(open('/odoo/odoo-server/test_shell.py').read())



#### wkf沒裝成功
1. sudo wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.trusty_amd64.deb
2. sudo gdebi --n `basename https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.trusty_amd64.deb`
3. 如有出錯執行下面程序
   +  sudo add-apt-repository ppa:linuxuprising/libpng12
   +  sudo apt update
   +  sudo apt install libpng12-0
4. sudo ln -s /usr/local/bin/wkhtmltopdf /usr/bin
5. sudo ln -s /usr/local/bin/wkhtmltoimage /usr/bin
6. 

#### 設定問題
1.庫存計價->編輯動作->增加消耗品:[('product_id.type', 'in', ('product','consu'))]

#### odoo問題
1. odoo一直會斷線要更新要設定worker

#### Yun666
1. 搜尋NGINX，調整為TRUE跟調整website_name
2. ngix設定網址:/etc/nginx/sites-available/odoo
3. 確認nginx狀態 sudo systemctl status nginx
4. 重啟sudo systemctl start nginx
5. sudo certbot --nginx -d ottsleep.com --noninteractive --agree-tos --email harry.chang@dahsheng.com --redirect
6. SSL安裝:https://peterli.website/%E5%A6%82%E4%BD%95%E5%9C%A8ubuntu-16-04%E4%B8%8A%E5%AE%89%E8%A3%9Dcertbot%E7%94%A2%E7%94%9F%E6%9C%89%E6%95%88%E7%9A%84ssl%E6%86%91%E8%AD%89/
7. 設定檔案/etc/nginx/nginx.conf
8. 移除:cd /etc/letsencrypt/renewal/
9. sudo certbot delete --cert-name support.reverie.com.tw

#### NGINX相關
1. sudo add-apt-repository ppa:certbot/certbot -y && sudo apt-get update -y
2. sudo apt-get install python3-certbot-nginx -y
3. sudo certbot --nginx -d ottsleep.com -d www.ottsleep.com --noninteractive --agree-tos --email harry.chang@dahsheng.com --redirect
4. sudo certbot --nginx -d gosufu.com -d www.gosufu.com --noninteractive --agree-tos --email harry.chang@dahsheng.com --redirect
5. sudo certbot --nginx -d support.reverie.com.tw -d support-test.reverie.com.tw --noninteractive --agree-tos --email harry.chang@dahsheng.com --redirect
6. sudo service nginx reload
7. sudo systemctl start nginx
8. sudo systemctl stop nginx
9. sudo certbot revoke --cert-path /etc/letsencrypt/archive/ottsleep.com/cert1.pem
10. sudo certbot delete --cert-name ottsleep.com
11. sudo apt-get --purge remove nginx 
12. sudo apt-get --purge remove nginx 
13. sudo apt-get autoremove 
14. sudo apt-get --purge remove nginx 
15. sudo apt-get --purge remove nginx-common 


#### vm安裝問題
1. vm tool 安裝 用虛擬光碟下載到目錄後,使用sudo perl vmware-indatll.pl進行安裝
2. 如果要複製可以複製vmdk,vmx兩個檔案

#### pycharm複製環境
1. setting先設定一個ven14環境
2. 下載odoo14
3. 右上角設定啟動參數與右下角改變環境
4. pip install requirements,忽略以下
   +  feedparser==5.2.1
   +  python-stdnum==1.8
6. 啟動環境並新增資料庫

sudo reboot

    
#### github使用方式(ubuntu)
1. sudo apt-get install git
2. 產生金鑰ssh-keygen -t rsa -b 4096 -C "your_email@example.com" ;  確認指令:eval "$(ssh-agent -s)"
3. 新增金鑰ssh-add ~/.ssh/id_rsa
4. 複製金鑰，vi ~/.ssh/id_rsa.pub
5. 到github的設定，新增SSH的金鑰。
6. linux設定  git config --global user.name “ksharry”  ;  git config --global user.email w461059@hotmail.com
7. 連線到  ssh -T git@github.com   成功訊息:Hi github! You've successfully authenticated
8. 上傳程式碼，先前要先去新增repositories。
 >
    git init 
    git pull #獲取新版本
    git status #獲取需要上傳的檔案 
    git add . # .表示全新增， git add README.md 表示只新增說明檔案
    git commit -m "add new files" # a commit
    git push
    git remote add origin git@github.com:ksharry/odoo14-cookbook    #新增遠端數據庫
    git push -u origin master     #參數 -u 等同於 --set-upstream，設定 upstream 可以使分支開始追蹤指定的遠端分支   
    git checkout main
    
    git branch newbranch      #新增分支
    git checkout newbranch    #切換工作分支
    git merge newbranch       #合併
    git branch -D newbranch   #刪除

<table>
    <tr>
        <td>元植課程</td>
    </tr>
</table>

#### 紀錄 
1. python3.7.9 使用管理著，自定選python37路徑。1.勾選第一個，最後DISABLE
2. postgresql  使用10以上版本。
3. ODOO GITHUB下載
4. 主機重開指令順序 (這個超重要, 之前沒有照這個順序下指令重開機, 結果資料庫直接毀損):
  > 
    sudo service odoo-server stop
    sudo service postgresql stop
    sudo reboot

#### 開發紀錄 (多注意點和底線)
1. bbb新增承租紀錄:
    + 增加py的one2many
    + 新增py,xml,menuitem,security,的程式
    + 新增manafest,init
2. bbb新增客戶:
    + 新增py繼承，menuitem，xml對應尋找位置
    + 新增manafest,init
    
    
    
    
<table>
    <tr>
        <td>odoo14安裝</td>
    </tr>
</table>

#### 紀錄 
1. 環境vmware 
2. 啟動指令 https://www.candidroot.com/blog/our-candidroot-blog-1/
/how-to-install-odoo-14-on-ubuntu-20-04-lts-67
  > 
    sudo su - odoo -s /bin/bash
    cd /odoo/odoo-server
    ./odoo-bin -c /etc/odoo-server.conf

  >
    wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
    sudo apt install -y -f ./google-chrome-stable_current_amd64.deb

<table>
    <tr>
        <td>ubuntu啟動指令</td>
    </tr>
</table>

#### 紀錄 
1. Postgresql使用 
  + systemctl status postgresql.service ,查看 PostgreSQL 服務狀態
  + sudo -i -u postgres   ,  pql  登入
  + https://www.itread01.com/content/1546698734.html

2. 列舉資料庫：\l
3. 選擇資料庫：\c 資料庫名
4. 檢視該某個庫中的所有表：\dt
5. 切換資料庫：\c interface
6. 檢視某個庫中的某個表結構：\d 表名
7. 檢視某個庫中某個表的記錄：select * from apps limit 1;
7. 顯示字符集：\encoding
8. 退出psgl：\q
  
#### 紀錄 
0. 工具安裝
    + sudo apt install vim
    + pgadmin4
       
       > sudo vim /etc/apt/sources.list.d/pgdg.list
       
       > deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main
       
       > sudo wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
       
       > sudo apt update
       
       > sudo apt install pgadmin4

    +copyq
     
      > sudo apt install software-properties-common
         sudo add-apt-repository ppa:hluk/copyq
         sudo apt update
         sudo apt install copyq
         
    + x11vnc
    
       >apt-get install x11vnc
        sudo x11vnc -storepasswd [your_password] /etc/x11vnc.pass
        x11vnc

1. docker 安裝https://docs.docker.com/install/linux/docker-ce/ubuntu/

2. postgresql安裝  
    > 進入docke postgresql   :  docker ps ; docker exec -it 49054d415576 bash   :   volum路徑:/var/lib/docker/vloume/xxxxxxx or odooxxxx
    
    > 遠端pgadmin 安裝後設定,odoo/postgresql/odoo/odoo  ip設定即可。
    
    > 勿用--調整使用docker-compose (docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo --name db postgres:10.0)

2.wkhtmltopdf安裝
    
    > wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.xenial_amd64.deb
    
    > sudo apt --fix-broken install,sudo dpkg -i libpng12-0_1.2.54-1ubuntu1_amd64.deb
    
    > sudo dpkg -i dpkg -i wkhtmltox_0.12.5-1.xenial_amd64.deb
    
3. odoo安裝
    >git clone https://github.com/odoo/odoo.git
    
    >sudo apt-get install python3-venv ; python3 -m venv odoo13
    
    

   > pip3 install wheel
   
   > sudo apt-get install python3 python-dev python3-dev \
     build-essential libssl-dev libffi-dev \
     libxml2-dev libxslt1-dev zlib1g-dev \
     python-pip
     
   > sudo apt-get install libsasl2-dev python-dev libldap2-dev libssl-dev
   
   > pip3 install -r requirements.txt
   
 4.設定odoo環境
    
    >odoo.conf
       [options]
       addons_path = /home/twtrubiks/work/odoo13/addons
       data_dir = /home/twtrubiks/Downloads/odoo-git/odoo-data
       db_host = localhost

5. 路徑 要有odoo-bin:/home/dsc/odoo
    > source odoo13/bin/activate 
    
    > cd odoo
    
    > docker-compose up -d
    
    > python3 odoo-bin -w odoo -r odoo -c /home/dsc/odoo/config/odoo.conf

6 登入網址：
   > http://0.0.0.0:8069/
   
7.database網址：
  >  http://0.0.0.0:8069/web/database/manager

8. 複製語法python odoo-bin scaffold aa_sale1 addons

9. 語法：卡8069 netstat -lp --inet  , kill -9 pid

10.ssh連線開放
  >  sudo apt-get install ssh
   
  >  sudo nano /etc/ssh/sshd_config  開放Port 22  , AllowUsers dsc
  
  >  sudo /etc/init.d/ssh  restart
  
  >  sudo gedit /etc/hosts.allow   ALL: 192.168.3.*
  
  >  ssh –X dsx@192.168.x.x
   
11. XSHELL 上傳:sudo apt-get install lrzsz 

12. 直接在linux安裝指令 :python3 odoo-bin -i demo_expense_tutorial_v1 -d odoo -c /home/dsc/odoo/config/odoo.conf

13. 

14. jupyter 64位元出現sqlite3問題，https://www.sqlite.org/download.html ，下載Precompiled Binaries for Windows對應的位元數放入DLLs檔案即可。

15. 永續成本，公司編輯視圖 
  > 
    <group string="Cost Accounting" groups="account.group_account_user"> 
     <field name="anglo_saxon_accounting"/> 
    </group>
    
16. odoo14調整連線pgadmin /etc/postgres/12/main 
  + 調整pg_hba.conf 0.0.0.0/0  # IPv4 local connections:   host    all             all             0.0.0.0/0               md5
  + 調整pg_hba.conf 0.0.0.0/0  # replication privilege.    host    replication     all             0.0.0.0/0               md5
  + host    all             all             114.35.75.63/32            md5
  + 
  + 開放postgresql.conf    listen_addresses = '*'
  + 重啟  sudo /etc/init.d/postgresql restart
  > 
    ps aux  | grep 'postgres *-D'
    service postgresql status
    port sudo ufw status verbose
    sudo -u postgres psql 
    ALTER USER postgres WITH PASSWORD 'postgres';
    sudo -i -u postgres

16. odoo14調整port
  + sudo ufw enable  啟用
  + sudo apt-get install ufw     安裝
  + sudo ufw default allow/deny   開啟
  + sudo ufw status numbered    查看狀態
  + sudo ufw status verbose     查看狀態
  + sudo ufw allow 80/tcp / sudo ufw allow 443/tcp


