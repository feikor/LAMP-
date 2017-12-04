# LAMP-安装

Mysql安装（见mysql安装一文）

Apache安装
yum -y install autoconf automake bison perl-devel 
yum -y install make gcc gcc-c++ cmake bison-devel ncurses ncurses-devel perl openssl openssl-devel(mysql时装了)

安装apr
cd /usr/local/src/
wget http://oss.aliyuncs.com/aliyunecs/onekey/apache/apr-1.5.0.tar.gz
tar zxvf apr-1.5.0.tar.gz
cd apr-1.5.0
./configure --prefix=/usr/local/apr
make && make install

安装apr-util
cd /usr/local/src/
wget http://oss.aliyuncs.com/aliyunecs/onekey/apache/apr-util-1.5.3.tar.gz
tar zxvf apr-util-1.5.3.tar.gz 
cd apr-util-1.5.3
./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr
make && make install

安装pcre
cd /usr/local/src/
wget http://zy-res.oss-cn-hangzhou.aliyuncs.com/pcre/pcre-8.38.tar.gz 
tar zxvf pcre-8.38.tar.gz
cd pcre-8.38
./configure --prefix=/usr/local/pcre
make && make install


编译安装Apache

cd /usr/local/src/

wget http://mirrors.sohu.com/apache/httpd-2.4.28.tar.gz
tar zxvf httpd-2.4.28.tar.gz
cd httpd-2.4.28

wget http://zy-res.oss-cn-hangzhou.aliyuncs.com/apache/httpd-2.4.23.tar.gz 
tar zxvf httpd-2.4.23.tar.gz
cd httpd-2.4.23
./configure 
--prefix=/usr/local/apache 
--sysconfdir=/etc/httpd 
--enable-so 
--enable-cgi 
--enable-rewrite 
--with-zlib 
--with-pcre=/usr/local/pcre 
--with-apr=/usr/local/apr 
--with-apr-util=/usr/local/apr-util 
--enable-mods-shared=most 
--enable-mpms-shared=all 
--with-mpm=event

./configure --prefix=/usr/local/apache --sysconfdir=/etc/httpd --enable-so --enable-cgi --enable-rewrite --with-zlib --with-pcre=/usr/local/pcre --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util --enable-mods-shared=most --enable-mpms-shared=all --with-mpm=event


make && make install

cp -rf /usr/local/apache/bin/apachectl /etc/init.d/httpd

vi /etc/init.d/httpd
在开头第一行的#!/bin/sh下行加上
#chkconfig:345 85 15
#description:Start and stops the Apache HTTP Server.
以上两句必须添加，否则会提示“httpd服务不支持”；第一行3个数字参数意义分别为：哪些Linux级别需要启动httpd(3,4,5)；启动序号(85)；关闭序号(15)。

cd 改权限和开机自启
cd /etc/init.d/
chmod 755 httpd
chkconfig --add httpd


Php的安装
wget http://mirrors.sohu.com/php/php-5.6.32.tar.gz
tar zxvf php-5.6.32.tar.gz
cd php-5.6.32

yum install -y libxml2 libxml2-devel libmcrypt libmcrypt-devel zlib zlib-devel pcre pcre-devel libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers



./configure -prefix=/usr/local/php -with-apxs2=/usr/local/apache/bin/apxs -with-config-file-path=/usr/local/php/etc.with-libxml-dir=/usr/include/libxml2 -with-mmsql=mysqlnd -with-mysqli=mysqlnd -with-pdo-mysql=mysqlnd -disable-fileinfo -enable-mbstring -enable-ftp -enable-gd-native-ttf -enable-cli -enable-sockets.enableeexif -enable-zip -enable-soap -enable-pcntl -with-gd -with-jpeg-dir -with-png-dir -with-openssl-dir -with-openssl -with-pear -with-freetype-dir -with-zlib -with-mcrypt -with-xmlrpc -with-zlib-dir -with-bz2 -with-curl

configure: WARNING: unrecognized options: --with-mmsql, --enable-sockets.enableeexif


make && make install

cp -rf /usr/local/src/php-5.6.32/php.ini-production /usr/local/php/etc/php.ini
配置apache
vi /usr/local/apache/conf/httpd.conf
php-cli和php-common

vi /etc/httpd/httpd.conf

在#LoadModule rewrite_module modules/mod_rewrite.so后面加下面内容
LoadModule php5_module  modules/libphp5.so

找到 AddType application/x-gzip.gz.tgz，在下面添加一行
AddType application/x-httpd-php .php

<IfModule dir_module>
    DirectoryIndex index.html index.php
</IfModule>
Warning: phpinfo(): It is not safe to rely on the system's timezone settings. You are *required* to use the date.timezone setting or the date_default_timezone_set() function.

找到date.timezone，修改为 date.timezone = PRC，后保存。

PHP Loaded Configuration File None 
phpinfo()输出 loaded configuration file none
解决方法可在apache配置文件中增加 
PHPIniDir “The path to your php.ini”
比如：PHPIniDir "/usr/local/php/etc/php.ini"
重启apache。 
确保PHPIniDir在loadModule php5_module之前 
vi /etc/httpd/httpd.conf

----------------------------------------------------------------------------
查看系统时间和配置
$ date -R
$ cat /etc/sysconfig/clock
$ tzselect
tzselect命令只告诉你选择的时区的写法，并不会生效。所以现在它还不是东8区北京时间。你可以在.profile、.bash_profile或者/etc/profile中设置正确的TZ环境变量并导出。 例如在.bash_profile里面设置
TZ='Asia/Shanghai'; export TZ
source一下
$ source ~/.bash_profile
---------------------------------------------------------------------------






