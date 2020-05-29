

<div class="WordSection1">

<p class="MsoNormal"><span class="SpellE"><b style=""><span style="font-size: 22pt; line-height: 107%;">Zabbix</span></b></span><b style=""><span style="font-size: 22pt; line-height: 107%;"> Documentation <o:p></o:p></span></b></p>

<p class="MsoNormal">Main manual: <span class="MsoHyperlink"><a href="https://www.zabbix.com/documentation/current/manual">https://www.zabbix.com/documentation/current/manual</a></span></p>

<p class="MsoNormal">Community site: You can find most public share templates in
here.<span style="">&nbsp; </span><span class="MsoHyperlink"><a href="https://share.zabbix.com/">https://share.zabbix.com/</a></span>. But most
of them doesn’t work well. You need spend time to debug XML.<span style="">&nbsp; </span>I have created one specific for DELL NS
model. </p>

<p class="MsoNormal"><o:p>&nbsp;</o:p></p>

<p class="MsoNormal"><b style=""><span style="font-size: 22pt; line-height: 107%;">Installation step:<o:p></o:p></span></b></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">setenforce</span></span> 0</p>

<p class="MsoNormal" style="margin-left: 0.5in;"># <span class="GramE">Modified</span>
secure <span class="SpellE">linux</span> file and change from enforce to disable</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="GramE">vi</span> /<span class="SpellE">etc</span>/<span class="SpellE">selinux</span>/<span class="SpellE">config</span></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">timedatectl</span></span> set-<span class="SpellE">timezone</span>
"America/<span class="SpellE">Los_Angeles</span>" </p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">dnf</span></span> -y install @<span class="SpellE">httpd</span></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">systemctl</span></span> enable --now <span class="SpellE">httpd</span></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">systemctl</span></span> status <span class="SpellE">httpd</span></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">dnf</span></span> module -y install php:7.2</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">dnf</span></span> -y install <span class="SpellE">php</span> <span class="SpellE">php</span>-pear <span class="SpellE">php-cgi</span> <span class="SpellE">php</span>-common <span class="SpellE">php-mbstring</span> <span class="SpellE">php-snmp</span> <span class="SpellE">php-gd</span> <span class="SpellE">php</span>-xml <span class="SpellE">php-mysqlnd</span> <span class="SpellE">php-gettext</span> <span class="SpellE">php-bcmath</span> <span class="SpellE">php-json</span> <span class="SpellE">php-ldap</span></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">php</span></span> -v</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">systemctl</span></span> enable --now <span class="SpellE">php</span>-fpm</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">sed</span></span> -<span class="SpellE">i</span> "s/^;<span class="SpellE">date.timezone</span> =$/<span class="SpellE">date.timezone</span> =
\"America\/<span class="SpellE">Los_Angeles</span>\"/" /<span class="SpellE">etc</span>/php.ini</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">systemctl</span></span> restart <span class="SpellE">httpd</span> <span class="SpellE">php</span>-fpm</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">dnf</span></span> -y update</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">dnf</span></span> module install <span class="SpellE">mariadb</span></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">systemctl</span></span> enable --now <span class="SpellE">mariadb</span></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE">mysql_secure_installation</span>
</p>

<p class="MsoNormal" style="margin-left: 0.5in;">#<span class="SpellE">mariadb</span>
root password: <span class="SpellE">The@third@floor</span></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">mysql</span></span> -u root -p</p>

<p class="MsoNormal" style="margin-left: 0.5in;">CREATE DATABASE <span class="SpellE">zabbix</span>;</p>

<p class="MsoNormal" style="margin-left: 0.5in;">GRANT ALL PRIVILEGES ON zabbix.* TO
<span class="SpellE">zabbix</span>@'localhost' IDENTIFIED BY '<span class="SpellE">The@third@floor</span>';</p>

<p class="MsoNormal" style="margin-left: 0.5in;">FLUSH PRIVILEGES;</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="GramE">vi</span> /<span class="SpellE">etc</span>/<span class="SpellE">zabbix</span>/<span class="SpellE">zabbix_server.conf</span></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE">DBName</span>=<span class="SpellE">zabbix</span></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE">DBUser</span>=<span class="SpellE">zabbix</span></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE">DBPassword</span>=<span class="SpellE">The@third@floor</span></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><o:p>&nbsp;</o:p></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">systemctl</span></span> enable --now <span class="SpellE">zabbix</span>-server
<span class="SpellE">zabbix</span>-agent</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="GramE">vi</span> /<span class="SpellE">etc</span>/php.ini</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE">memory_limit</span>
128M</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE">upload_max_filesize</span>
8M</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE">post_max_size</span>
16M</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE">max_execution_time</span>
300</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE">max_input_time</span>
300</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE">max_input_vars</span>
10000</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="GramE">firewall-<span class="SpellE">cmd</span></span> --add-service=http --permanent</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="GramE">firewall-<span class="SpellE">cmd</span></span> --add-port={10051,10050}/<span class="SpellE">tcp</span>
--permanent</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="GramE">firewall-<span class="SpellE">cmd</span></span> --add-port={161,162}/<span class="SpellE">tcp</span>
--permanent</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="GramE">firewall-<span class="SpellE">cmd</span></span> --add-port={161,162}/<span class="SpellE">udp</span>
--permanent</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="GramE">firewall-<span class="SpellE">cmd</span></span> --reload</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">systemctl</span></span> restart <span class="SpellE">httpd</span> <span class="SpellE">php</span>-fpm</p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span class="SpellE"><span class="GramE">zcat</span></span> /<span class="SpellE">usr</span>/share/doc/<span class="SpellE">zabbix</span>-server-<span class="SpellE">mysql</span>*/create.sql.gz
| <span class="SpellE">mysql</span> -<span class="SpellE">uzabbix</span> -p <span class="SpellE">zabbix</span></p>

<p class="MsoNormal"><span style="">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span class="SpellE"><span class="GramE">systemctl</span></span> restart <span class="SpellE">zabbix</span>-server</p>

<p class="MsoNormal"><o:p>&nbsp;</o:p></p>

<p class="MsoNormal"><b style=""><span style="font-size: 22pt; line-height: 107%;">Key configuration files and log files:
<o:p></o:p></span></b></p>

<p class="MsoNormal" style="margin-left: 0.5in;">/<span class="SpellE">etc</span>/<span class="SpellE">zabbix</span>/<span class="SpellE">zabbix_server.conf</span></p>

<p class="MsoNormal" style="margin-left: 0.5in;"><span style="">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span class="SpellE">DBName</span>=<span class="SpellE">zabbix</span></p>

<p class="MsoNormal" style="margin-left: 0.5in; text-indent: 0.5in;"><span class="SpellE">DBUser</span>=<span class="SpellE">zabbix</span></p>

<p class="MsoNormal" style="margin-left: 0.5in; text-indent: 0.5in;"><span class="SpellE">DBPassword</span>=<span class="SpellE">The@third@floor</span></p>

<p class="MsoNormal" style="margin-left: 0.5in; text-indent: 0.5in;"><span class="SpellE">CacheSize</span>=4096M</p>

<p class="MsoNormal" style="margin-left: 0.5in; text-indent: 0.5in;"><span class="SpellE">HistoryCacheSize</span>=1024M</p>

<p class="MsoNormal" style="margin-left: 0.5in; text-indent: 0.5in;"><span class="SpellE">TrendCacheSize</span>=1024M</p>

<p class="MsoNormal" style="margin-left: 0.5in; text-indent: 0.5in;"><span class="SpellE">ValueCacheSize</span>=4096M</p>

<p class="MsoNormal" style="text-indent: 0.5in;">/<span class="SpellE">var</span>/log/<span class="SpellE">zabbix</span>/zabbix_server.log</p>

<p class="MsoNormal" style="text-indent: 0.5in;"><o:p>&nbsp;</o:p></p>

<p class="MsoNormal"><b style=""><span style="font-size: 22pt; line-height: 107%;">Command list for troubleshooting:<o:p></o:p></span></b></p>

<p class="MsoNormal"><span style="">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span class="SpellE"><span class="GramE">dnf</span></span> install net-<span class="SpellE">snmp</span>-<span class="SpellE">utils</span></p>

<p class="MsoNormal"><span style="">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span># Use
to test snmpv3 service in device</p>

<p class="MsoNormal" style="text-indent: 0.5in;"><span class="SpellE"><span class="GramE">snmpwalk</span></span> -v3 -u <span class="SpellE">Zabbix</span>-user
-l <span class="SpellE">AuthPriv</span> -a SHA -A 3uHcUKNKYQuwW6K3 -x AES -X
cRfUbbKFJA794aHL 10.10.0.253</p>

<p class="MsoNormal" style="text-indent: 0.5in;"># <span class="GramE">If</span> you
reconfigure the same device in <span class="SpellE">Zabbix</span> server, you
need do the following </p>

<p class="MsoNormal" style="text-indent: 0.5in;"><span class="SpellE">zabbix_server</span>
-R <span class="SpellE">snmp_cache_reload</span> </p>

<p class="MsoNormal" style="text-indent: 0.5in;"><span class="SpellE"><span class="GramE">systemctl</span></span> restart <span class="SpellE">zabbix-server.service</span></p>

<p class="MsoNormal"><o:p>&nbsp;</o:p></p>

<p class="MsoNormal"><b style=""><span style="font-size: 22pt; line-height: 107%;">Existing incident – <o:p></o:p></span></b></p>

<p class="MsoNormal"><span class="SpellE">Isilon</span> SNMP service will turn red
around one day.<span style="">&nbsp; </span>There is no more
monitoring date after SNMP service turn. The quick fix is run ‘<span class="SpellE">zabbix_server</span> -R <span class="SpellE">snmp_cache_reload</span>’
and <span class="SpellE">Isilon</span> SNMP service will turn back green.<span style="">&nbsp; </span></p>

<p class="MsoNormal">Here is my resolution in below. I use CRON to schedule run ‘<span class="SpellE">zabbix_server</span> -R <span class="SpellE">snmp_cache_reload</span>’
in every six hours. </p>

<p class="MsoNormal">#install <span class="SpellE">crontabs</span></p>

<p class="MsoNormal"><span class="SpellE"><span class="GramE">dnf</span></span>
install <span class="SpellE">crontabs</span></p>

<p class="MsoNormal">#start and enable service</p>

<p class="MsoNormal"><span class="SpellE"><span class="GramE">systemctl</span></span>
enable <span class="SpellE">crond</span> –now</p>

<p class="MsoNormal">#edit <span class="SpellE">crond</span> service</p>

<p class="MsoNormal"><span class="SpellE"><span class="GramE">crontab</span></span> –e</p>

<p class="MsoNormal">0 */6 * * * /<span class="SpellE">usr</span>/<span class="SpellE">sbin</span>/<span class="SpellE">zabbix_server</span> -R <span class="SpellE">snmp_cache_reload</span></p>

<p class="MsoNormal">#check <span class="SpellE">cron</span> log</p>

<p class="MsoNormal"><span class="GramE">grep</span> CRON /<span class="SpellE">var</span>/log/<span class="SpellE">cron</span></p>

<p class="MsoNormal">May 26 12:00:01 ttf-lax-mon04 <span class="GramE">CROND[</span>10737]:
(root) CMD (/<span class="SpellE">usr</span>/<span class="SpellE">sbin</span>/<span class="SpellE">zabbix_server</span> -R <span class="SpellE">snmp_cache_reload</span>)</p>

<p class="MsoNormal">May 26 12:00:01 ttf-lax-mon04 <span class="GramE">CROND[</span>10724]:
(root) CMDOUT (<span class="SpellE">zabbix_server</span> [10737]: command sent
successfully)<span style="">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></p>

<p class="MsoNormal"><span style="">&nbsp;</span></p>

<p class="MsoNormal" style="text-indent: 0.5in;"><o:p>&nbsp;</o:p></p>

</div>




