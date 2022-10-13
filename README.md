# zabbix3 Zabbix 5.0 Configuration in RHEL 8 (CentOS 8):
***
Chapters:
0:00 Channel Intro
0:15 Zabbix Overview & Details
1:10 Zabbix 5 Pre-requesties
1:35 Making SELinux Permissive
2:00 Installing Required Packages For Zabbix
3:30 Installing MariaDB Packages & Starting Service
4:20 Reset Root Password For Database
5:35 Creating Database
6:15 Importing Initial Schema & Data
7:18 Enabling Strict Mode
7:45 Enter Zabbix Password in DB File
8:48 Starting & Enabling Zabbix Services
9:14 Adding Firewall Rules
10:10 Setting Timezone in Configuration File
10:58 Restart & Enable Apache Services
10:20 Configuring Zabbix Frontend

***
Please check pinned comment for steps & details.
***
setenforce 0 && sed -i 's/^SELINUX=.*/SELINUX=permissive/g' /etc/selinux/config
getenforce 
clear
rpm -Uvh https://repo.zabbix.com/zabbix/5.0/rh...
clear
dnf clean all
dnf -y install zabbix-server-mysql zabbix-web-mysql zabbix-apache-conf zabbix-agent
clear
dnf -y install mariadb-server && systemctl start mariadb && systemctl enable mariadb
clear
mysql_secure_installation
clear
mysql -uroot -p'rootDBpass' -e "create database zabbix character set utf8 collate utf8_bin;"
mysql -uroot -p'rootDBpass' -e "grant all privileges on zabbix.* to zabbix@localhost identified by 'rootDBpass';"
clear
mysql -uroot -p'rootDBpass' zabbix -e "set global innodb_strict_mode='OFF';"
zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p'rootDBpass' zabbix
mysql -uroot -p'rootDBpass' zabbix -e "set global innodb_strict_mode='ON';"
clear
vim /etc/zabbix/zabbix_server.conf
systemctl restart zabbix-server zabbix-agent
systemctl enable zabbix-server zabbix-agent
clear
firewall-cmd --add-service={http,https} --permanent
firewall-cmd --add-port={10051/tcp,10050/tcp} --permanent
firewall-cmd --reload
clear
firewall-cmd --list-all
clear
vim /etc/php-fpm.d/zabbix.conf
systemctl restart httpd php-fpm
systemctl enable httpd php-fpm

***
Thanks for watching the video. If it helped you then, please do like & share it with others as well. Feel free to post your queries & suggestions in the comment box, we will be happy to answer your queries. If you like our hard work then please do subscribe to our channel & turn on the bell notification to get the latest notifications of our video.
***
