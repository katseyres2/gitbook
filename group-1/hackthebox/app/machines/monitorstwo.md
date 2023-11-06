---
cover: ../../../../.gitbook/assets/MonitorsTwo.png
coverY: 256.77866666666665
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# MonitorsTwo

```bash
kali@kali> apt update
kali@kali> apt upgrade -y
kali@kali> apt install software-properties-common -y
kali@kali> apt install nginx -y
# debian 11
kali@kali> apt install -y curl vim acl composer fping git graphviz imagemagick mariadb-client mariadb-server mtr-tiny nginx-full python3-memcache python3-mysqldb snmp snmpd whois php-snmp rrdtool librrds-perl
# debian 10
kali@kali> apt install -y curl vim acl composer fping git graphviz imagemagick mariadb-client mariadb-server mtr-tiny nginx-full python-memcache python-mysqldb snmp snmpd whois php-snmp rrdtool librrds-perl

kali@kali> apt -y install php php-common
kali@kali> apt -y install php-cli php-fpm php-json php-pdo php-mysql php-zip php-gd  php-mbstring php-curl php-xml php-pear php-bcmath php-gmp php-ldap

kali@kali> systemctl enable mysql
kali@kali> systemctl restart mysql
kali@kali> mysql -u root -p

mysql> CREATE DATABASE cacti;
mysql> CREATE USER 'cactiuser'@'localhost' IDENTIFIED BY 'SafePassWord'; ## Make it strong
mysql> GRANT ALL PRIVILEGES ON cacti.* TO 'cactiuser'@'localhost';
mysql> FLUSH PRIVILEGES;
mysql> EXIT

mysql -u root -p mysql < /usr/share/mysql/mysql_test_data_timezone.sql
mysql -u root -p

GRANT SELECT ON mysql.time_zone_name TO cactiuser@localhost;
ALTER DATABASE cacti CHARACTER SET = 'utf8mb4'  COLLATE = 'utf8mb4_unicode_ci';
flush privileges;
exit
```

{% code title="/etc/mysql/mariadb.conf.d/50-server.cnf-server.cnf" %}
```
[mysqld]
collation-server = utf8mb4_unicode_ci
character-set-server  = utf8mb4
max_heap_table_size = 128M
tmp_table_size = 64M
join_buffer_size = 64M
innodb_file_format = Barracuda
innodb_large_prefix = 1
innodb_buffer_pool_size = 1GB
innodb_buffer_pool_instances = 10
innodb_flush_log_at_timeout = 3
innodb_read_io_threads = 32
innodb_write_io_threads = 16
innodb_io_capacity = 5000
innodb_io_capacity_max = 10000
```
{% endcode %}

```bash
systemctl restart mysql
```

{% code title="/etc/php/*/fpm/php.ini" %}
```ini
date.timezone = Africa/Nairobi ## Input your Time zone
max_execution_time = 300       ## Increase max_execution_time
memory_limit = 512M            ## Increase memory limit
```
{% endcode %}

{% code title="/etc/php/*/fpm/pool.d/www.conf" %}
```
listen = /run/php/php-fpm.sock
```
{% endcode %}

```bash
systemctl restart php*-fpm.service
rm /etc/nginx/sites-enabled/default
```

{% code title="/etc/nginx/conf.d/cacticonfig.conf" %}
```nginx
server {
 listen      80;
 server_name cacti.example.com;
 root        /var/www/html;
 index       index.php;
 access_log  /var/log/nginx/cacti_access.log;
 error_log   /var/log/nginx/cacti_error.log;
 charset utf-8;
 gzip on;
 gzip_types text/css application/javascript text/javascript application/x-javascript image/svg+xml text/plain text/xsd text/xsl text/xml image/x-icon;
 location / {
   try_files $uri $uri/ /index.php?$query_string;
  }
  location /api/v0 {
   try_files $uri $uri/ /api_v0.php?$query_string;
  }
  location ~ .php {
   include fastcgi.conf;
   fastcgi_split_path_info ^(.+.php)(/.+)$;
   fastcgi_pass unix:/run/php/php-fpm.sock;
  }
  location ~ /.ht {
   deny all;
  }
 }
```
{% endcode %}

```bash
systemctl restart nginx
wget https://files.cacti.net/cacti/linux/cacti-1.2.22.tar.gz --no-check-certificate
tar -zxvf cacti-1.2.22.tar.gz

mv cacti-1.2.22 /var/www/html/
mv /var/www/html/cacti-1.2.22/ /var/www/html/cacti
chown -R www-data:www-data /var/www/html/
mysql -u root -p cacti < /var/www/html/cacti/cacti.sql
```

{% code title="/var/www/html/cacti/include/config.php" %}
```
$database_type = "mysql";
$database_default = "cacti";
$database_hostname = "localhost";
$database_username = "cactiuser";
$database_password = "SafePassWord"; 
$database_port = "3306";
$database_ssl = false;
```
{% endcode %}

```bash
nginx  -t
systemctl restart nginx
```

{% code title="/etc/cron.d/cacti" %}
```
*/5 * * * * www-data php /var/www/html/cacti/poller.php > /dev/null 2>&1
```
{% endcode %}







{% embed url="https://raw.githubusercontent.com/ariyaadinatha/cacti-cve-2022-46169-exploit/main/cacti.py" %}
