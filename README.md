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









