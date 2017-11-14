# LAMP-安装

Mysql安装

一、编译安装MySQL前的准备工作
安装编译源码所需的工具和库
yum install gcc gcc-c++ cmake ncurses-devel perl

二、设置MySQL用户和组
新增mysql用户组
groupadd mysql
新增mysql用户
useradd -r -g mysql mysql 

三、新建MySQL所需要的目录
新建mysql安装目录
mkdir -p /usr/local/mysql  
新建mysql数据库数据文件目录
mkdir -p /data/mysqldb 

四、下载MySQL源码包并解压
wget https://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.38.tar.gz
tar -zxvf mysql-5.6.38.tar.gz  
cd mysql-5.6.38 

五、编译安装MySQL
从mysql5.5起，mysql源码安装开始使用cmake了，设置源码编译配置脚本。
设置编译参数
cmake . -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/data/mysqldb -DSYSCONFDIR=/etc -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITH_PERFSCHEMA_STORAGE_ENGINE=1 -DWITHOUT_EXAMPLE_STORAGE_ENGINE=1 -DWITHOUT_FEDERATED_STORAGE_ENGINE=1 -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS=all -DENABLED_LOCAL_INFILE=1 -DWITH_READLINE=1 -DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock -DMYSQL_TCP_PORT=3306 -DCOMPILATION_COMMENT="lq-edition" -DENABLE_DTRACE=1 -DWITH_SSL=yes -DWITH_DEBUG=1



