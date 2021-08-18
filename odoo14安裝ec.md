STEP1.1：Odoo環境架設

sudo wget https://raw.githubusercontent.com/Yenthe666/InstallScript/14.0/odoo_install.sh

sudo chmod +x odoo_install.sh

sudo ./odoo_install.sh

sudo apt-get install ttf-wqy-zenhei -y

sudo apt-get install ttf-wqy-microhei -y

sudo reboot


STEP1.2：Odoo環境架設

sudo nano /etc/odoo-server.conf

dbfilter = ^%d$

proxy_mode = True

sudo service odoo-server restart


STEP2：Apache2安裝

sudo apt-get update

sudo apt-get install apache2 -y

sudo a2enmod proxy proxy_http

sudo service apache2 restart


STEP4.1：Free SSL 安裝

sudo apt-get update

sudo apt-get install software-properties-common

sudo add-apt-repository universe

sudo add-apt-repository ppa:certbot/certbot

sudo apt-get update

sudo apt-get install python-certbot-apache 


STEP4.2：Free SSL 安裝

sudo certbot --apache
odoo14ecclass01.odooecclass.cloudns.asia


STEP4.3：Free SSL 安裝

sudo nano /etc/apache2/sites-available/000-default-le-ssl.conf

     ProxyPreserveHost On
     ProxyPass / http://127.0.0.1:8069/ retry=0
     ProxyPassReverse / http://127.0.0.1:8069/

sudo service apache2 restart

sudo reboot
