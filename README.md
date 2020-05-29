# zabbix

Zabbix Documentation 
Main manual: https://www.zabbix.com/documentation/current/manual
Community site: You can find most public share templates in here.  https://share.zabbix.com/. But most of them doesn’t work well. You need spend time to debug XML.  I have created one specific for DELL NS model. 

Installation step:
setenforce 0
# Modified secure linux file and change from enforce to disable
vi /etc/selinux/config
timedatectl set-timezone "America/Los_Angeles" 
dnf -y install @httpd
systemctl enable --now httpd
systemctl status httpd
dnf module -y install php:7.2
dnf -y install php php-pear php-cgi php-common php-mbstring php-snmp php-gd php-xml php-mysqlnd php-gettext php-bcmath php-json php-ldap
php -v
systemctl enable --now php-fpm
sed -i "s/^;date.timezone =$/date.timezone = \"America\/Los_Angeles\"/" /etc/php.ini
systemctl restart httpd php-fpm
dnf -y update
dnf module install mariadb
systemctl enable --now mariadb
mysql_secure_installation 
#mariadb root password: The@third@floor
mysql -u root -p
CREATE DATABASE zabbix;
GRANT ALL PRIVILEGES ON zabbix.* TO zabbix@'localhost' IDENTIFIED BY 'The@third@floor';
FLUSH PRIVILEGES;
vi /etc/zabbix/zabbix_server.conf
DBName=zabbix
DBUser=zabbix
DBPassword=The@third@floor

systemctl enable --now zabbix-server zabbix-agent
vi /etc/php.ini
memory_limit 128M
upload_max_filesize 8M
post_max_size 16M
max_execution_time 300
max_input_time 300
max_input_vars 10000
firewall-cmd --add-service=http --permanent
firewall-cmd --add-port={10051,10050}/tcp --permanent
firewall-cmd --add-port={161,162}/tcp --permanent
firewall-cmd --add-port={161,162}/udp --permanent
firewall-cmd --reload
systemctl restart httpd php-fpm
zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix
	systemctl restart zabbix-server

Key configuration files and log files: 
/etc/zabbix/zabbix_server.conf
	DBName=zabbix
DBUser=zabbix
DBPassword=The@third@floor
CacheSize=4096M
HistoryCacheSize=1024M
TrendCacheSize=1024M
ValueCacheSize=4096M
/var/log/zabbix/zabbix_server.log

Command list for troubleshooting:
	dnf install net-snmp-utils
	# Use to test snmpv3 service in device
snmpwalk -v3 -u Zabbix-user -l AuthPriv -a SHA -A 3uHcUKNKYQuwW6K3 -x AES -X cRfUbbKFJA794aHL 10.10.0.253
# If you reconfigure the same device in Zabbix server, you need do the following 
zabbix_server -R snmp_cache_reload 
systemctl restart zabbix-server.service

Existing incident – 
Isilon SNMP service will turn red around one day.  There is no more monitoring date after SNMP service turn. The quick fix is run ‘zabbix_server -R snmp_cache_reload’ and Isilon SNMP service will turn back green.  
Here is my resolution in below. I use CRON to schedule run ‘zabbix_server -R snmp_cache_reload’ in every six hours. 
#install crontabs
dnf install crontabs
#start and enable service
systemctl enable crond –now
#edit crond service
crontab –e
0 */6 * * * /usr/sbin/zabbix_server -R snmp_cache_reload
#check cron log
grep CRON /var/log/cron
May 26 12:00:01 ttf-lax-mon04 CROND[10737]: (root) CMD (/usr/sbin/zabbix_server -R snmp_cache_reload)
May 26 12:00:01 ttf-lax-mon04 CROND[10724]: (root) CMDOUT (zabbix_server [10737]: command sent successfully)	
 

