---
title: 从0到100：zabbix及其支持环境的完整安装教程
date: 2016-08-08 02:45:19
tags: [zabbix, LAMP]
comments: true
---

版本信息：
Ubuntu15.10
Apache2.4.12
php5.6.11（zabbix3.0要求php版本至少5.4以上）
Mysql5.6.31
zabbix3.0

前言：本教程包括了ubuntu上LAMP(Linux+Apache+Mysql+Php)环境的搭建以及zabbix安装。因为我们最终是要通过外部计算机访问我们的服务器的，所以我希望你可以先运行一下“ifconfig -a”语句来查看以下自己的IP地址，以方便之后测试服务器。文中将以“IPAddr”来代替你的IP地址，阅读时请注意。
这是博主虚拟机上的IP地址：
<!-- more-->
![虚拟机上的IP地址](/assets/images/zabbix_2.jpg)

## 0.预安装
后边会用到的软件，装一下即可：
`sudo apt-get install vim -y`

## 1.Apache安装
在命令行运行下列语句下载apache：
`sudo apt-get install apache2 -y`
启动apache服务：
`sudo /etc/init.d/apache2 start`
看到下列语句说明启动成功：
![[OK]Starting apache2 (via systemctl):apache2.service](/assets/images/zabbix_1.jpg)
从其他PC上打开浏览器，输入*http://IPAddr*，打开页面，如果显示如下，则表示Apache安装成功。
![Apache默认页面](/assets/images/zabbix_3.jpg)

## 2.安装Mysql
在命令行运行下列语句下载mysql：
`sudo apt-get install mysql-server -y`
安装的时候会弹出窗口让你设置root帐户的初始密码，根据个人喜好设置一个即可。
同样的，安装完了我们也要启动一下mysql的服务：
`sudo /etc/init.d/mysql start`
看到下列语句说明启动成功：
![[OK]Starting mysql (via systemctl):mysql.service](/assets/images/zabbix_5.jpg)

## 3.安装php5
在命令行输入下列语句下载php5：
`sudo apt-get install php5 -y`
接着安装phpmyadmin：
`sudo apt-get install phpmyadmin -y`
安装的过程中根据提示，选择apache2，dbconfig-common那里选择YES，再输入系统root的密码和数据库root的密码即可。版本不同，顺序可能不大一样，总之问什么答什么就对了。
顺便改写以下/var/www目录的权限，方便以后编辑网站文件：
`sudo chmod 777 /var/www`
创建phpmyadmin的链接：
`sudo ln -s /usr/share/phpmyadmin /var/www/html/`
修改一下php5的配置，打开配置文件：
`sudo vim /etc/php5/apache2/php.ini`
加入红框中的语句：
![extension=mysqli.d](/assets/images/zabbix_6.jpg)
保存退出。
现在在其他的PC上打开浏览器，输入*http://IPAddr/phpmyadmin*，显示以下页面表示配置成功：
![phpmyadmin登录页面](/assets/images/zabbix_7.jpg)
## 4.安装配置zabbix server
###4.1 下载deb：
```bash
cd ~
wget http://repo.zabbix.com/zabbix/3.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.0-1+trusty_all.deb
dpkg -i zabbix-release_3.0-1+trusty_all.deb
apt-get update
```
###4.2 安装服务器端
运行下列语句：
`sudo apt-get install zabbix-server-mysql zabbix-frontend-php -y`
安装完成之后试着启动一下zabbix服务,出现下列语句即为成功：
![[OK]Starting zabbix_server (via systemctl):zabbix_server.service](/assets/images/zabbix_8.jpg)

###4.3 配置zabbix_server.conf
打开配置文件：
`sudo vim /etc/zabbix/zabbix_server.conf`
把对应项的值改为如下(没有的自己在对应位置加上即可)：

 - DBHost=localhost
 - DBName=zabbix
 - DBUser=zabbix
 - DBPassword=zabbix

###4.4 配置mysql
```sql
mysql -u root -p
(输入你的数据库root密码)
mysql> create database zabbix character set utf8 collate utf8_bin;
mysql> grant all privileges on zabbix.* to zabbix@localhost identified by 'zabbix';
mysql> flush privileges;
mysql> \q
cd /usr/share/doc/zabbix-server-mysql
zcat create.sql.gz | mysql -u root -p zabbix
（输入你的数据库root密码，点击回车后稍微等一会儿）
sudo cp -r /usr/share/zabbix /var/www/html/zabbix
/etc/init.d/zabbix-server restart
```
最后出现下列语句即为成功：
![[OK]Starting zabbix_server (via systemctl):zabbix_server.service](/assets/images/zabbix_8.jpg)
###4.5 配置php
编辑php的配置文件：
`sudo vim /etc/php5/apache2/php.ini`
把对应项的值改为如下(没有的自己在对应位置加上即可)：

 - post_max_size = 16M
 - max_execution_time = 300
 - max_input_time = 300
 - date.timezone = "Asia/Shanghai"

改完之后重启apache2：
`/etc/init.d/apache2 restart`

## 5.进入zabbix
在另外一台PC上打开浏览器，在地址栏输入：
*http://IPAddr/zabbix*
显示以下页面：
![zabbix欢迎页面](/assets/images/zabbix_9.jpg)
点击右下角的Next step进入Check of pre-requisites页面：
![Check of pre-requisites页面](/assets/images/zabbix_10.jpg)
这个页面是检测服务器配置是否合格的页面，必须全部为OK才可以点击Next step进入Configure DB connection页面。
![Configure DB connection页面](/assets/images/zabbix_11.jpg)
其中password为zabbix（我们刚刚配置数据库时设置的）。
接下来的Zabbix server details和Pre-installation summary两个页面无脑点Next step即可。
显示如下页面我们就可以点击Finish了。
![Congratulations](/assets/images/zabbix_12.jpg)
点击Finish之后出现zabbix server的登录页面，这里Username为Admin，Password为zabbix，最后点击Sign in，大功告成~
![登录页面](/assets/images/zabbix_13.jpg)
![zabbix管理页面](/assets/images/zabbix_14.jpg)