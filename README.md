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
-----------------------下面未做
chmod +w /usr/local/mysql
chown -R mysql:mysql /usr/local/mysql
------------------------

四、下载MySQL源码包并解压
wget https://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.38.tar.gz
tar -zxvf mysql-5.6.38.tar.gz  
cd mysql-5.6.38 

五、编译安装MySQL
从mysql5.5起，mysql源码安装开始使用cmake了，设置源码编译配置脚本。
设置编译参数
cmake . -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/data/mysqldb -DSYSCONFDIR=/etc -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITH_PERFSCHEMA_STORAGE_ENGINE=1 -DWITHOUT_EXAMPLE_STORAGE_ENGINE=1 -DWITHOUT_FEDERATED_STORAGE_ENGINE=1 -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS=all -DENABLED_LOCAL_INFILE=1 -DWITH_READLINE=1 -DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock -DMYSQL_TCP_PORT=3306 -DCOMPILATION_COMMENT="lq-edition" -DENABLE_DTRACE=1 -DWITH_SSL=yes -DWITH_DEBUG=1

-------------------------------------------参考
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DMYSQL_DATADIR=/usr/local/mysql/data/ \
-DMYSQL_UNIX_ADDR=/usr/local/mysql/data/mysql.sock \
-DMYSQL_USER=mysql \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_EXTRA_CHARSETS:STRING=utf8,gbk \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_MEMORY_STORAGE_ENGINE=1 \
-DWITH_READLINE=1 \
-DENABLED_LOCAL_INFILE=0 \
-DENABLE_DTRACE=1 \
-DMYSQL_TCP_PORT=3306

cmake  -DDEFAULT_CHARSET=utf8  #默认字符集
-DDEFAULT_COLLATION=utf8_general_ci
-DENABLE_DOWNLOADS=true    #是否支持下载文件
-DENABLE_DTRACE=true     #开启跟踪调试
-DENABLE_GCOV=true
-DENABLED_PROFILING=true     #记录发送到服务器的语句
-DMYSQL_TCP_PORT=3306    #指定端口
-DWITH_ASAN=true    #开启地址池清理
-DWITH_EMBEDDED_SERVER=true #开启libmysqld 动态库
-DWITH_EXTRA_CHARSETS=all    #支持所有字符集
-DWITH_LIBEDIT=true
-DWITH_READLINE=true     #使用捆绑的readline库
-DWITH_SSL=yes   #支持ssl协议连接
-DWITH_ZLIB=system     #支持zlib 也就是bzip2
-DWITH_BUNDLED_MEMCACHED=ON  #支持mencache 缓存
-DWITH_INNOBASE_STORAGE_ENGINE=1   #innodb存储引擎
-DMYSQL_DATADIR=/usr/local/mysql/data  #数据库存放目录


---------------------------------------------

make  
make install

 yum install –y openssl openssl-devel ncurses ncurses-devel
 有报错时，cmake有个文件我们需要删除，删除当前目录下CMakeCache.txt文件并重新编译
 rm -rf CMakeCache.txt （删除可能会报错）
 
 错误1：
make[2]: /usr/bin/dtrace：命令未找到
make[2]: *** [include/probes_mysql_dtrace.h] 错误 127
make[1]: *** [CMakeFiles/gen_dtrace_header.dir/all] 错误 2
make: *** [all] 错误 2
#解决方法
yum install dtrace systemtap-sdt-devel -y

错误2：
/bin/sh: /usr/bin/bison: 没有那个文件或目录
make[2]: *** [sql/sql_yacc.cc] 错误 127
make[1]: *** [sql/CMakeFiles/GenServerSource.dir/all] 错误 2
make: *** [all] 错误 2
#解决方法：
yum install bison-devel  bison -y


127未解决
Generating include/probes_mysql_dtrace.h, include/probes_mysql_nodtrace.h
make[2]: DTRACE-NOTFOUND: Command not found
make[2]: *** [include/probes_mysql_dtrace.h] Error 127
make[1]: *** [CMakeFiles/gen_dtrace_header.dir/all] Error 2
-------------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------------

一：卸载旧版本
rpm -qa | grep mysql
rpm -e mysql #普通删除模式 
rpm -e --nodeps xxx（xxx为刚才的显示的列表） # 强力删除模式，如果使用上面命令删除时，提示有依赖的其它文件，则用该命令可以对其进行强力删除
rm /etc/my.cnf #删除/etc/my.cnf 
 

二：安装编译代码需要的包
yum -y install make gcc-c++ cmake bison-devel ncurses-devel
 

三：创建mysql用户（但是不能使用mysql账号登陆系统） 
groupadd mysql 
useradd -s /sbin/nologin -g mysql mysql
 
四：下载MySQL 源码
cd /usr/local/src
wget -c http://cdn.mysql.com//Downloads/MySQL-5.6/mysql-5.6.33.tar.gz
 

五：安装
复制代码
tar zxvf mysql-5.6.33.tar.gz
cd mysql-5.6.33/
cmake
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql
-DMYSQL_DATADIR=/data/mysql/data
-DSYSCONFDIR=/etc
-DWITH_MYISAM_STORAGE_ENGINE=1
-DWITH_INNOBASE_STORAGE_ENGINE=1
-DWITH_MEMORY_STORAGE_ENGINE=1
-DWITH_READLINE=1
-DMYSQL_UNIX_ADDR=/var/lib/mysql/mysql.sock
-DMYSQL_TCP_PORT=3306
-DENABLED_LOCAL_INFILE=1
-DWITH_PARTITION_STORAGE_ENGINE=1
-DEXTRA_CHARSETS=all
-DDEFAULT_CHARSET=utf8
-DDEFAULT_COLLATION=utf8_general_ci
复制代码
编译并安装
make && make install
 

六：配置MySQL
1、修改/usr/local/mysql权限
chown -R mysql:mysql /usr/local/mysql
 
2、执行初始化配置脚本
进入安装路径
 cd /usr/local/mysql
执行初始化配置脚本，创建系统自带的数据库和表
scripts/mysql_install_db --basedir=/usr/local/mysql --datadir=/data/mysql/data --user=mysql
注：在启动MySQL服务时，会按照一定次序搜索my.cnf，先在/etc目录下找，找不到则会搜索"$basedir/my.cnf"，在本例中就是 /usr/local/mysql/my.cnf，这是新版MySQL的配置文件的默认位置！
注意：在CentOS 6.4版操作系统的最小安装完成后，在/etc目录下会存在一个my.cnf，需要将此文件更名为其他的名字，如：/etc/my.cnf.bak，否则，该文件会干扰源码安装的MySQL的正确配置，造成无法启动。在使用"yum update"更新系统后，需要检查下/etc目录下是否会多出一个my.cnf，如果多出，将它重命名成别的。否则，MySQL将使用这个配置文件启动，可能造成无法正常启动等问题。
 
3、启动MySQL
添加服务，拷贝服务脚本到init.d目录，并设置开机启动
cp support-files/mysql.server /etc/init.d/mysql
chkconfig mysql on
service mysql start
 
4、配置用户
MySQL启动成功后，root默认没有密码，需要设置root密码，设置之前，需要先设置PATH，要不不能直接调用mysql
修改/etc/profile文件，在文件末尾添加
export PATH=/usr/local/mysql/bin:$PATH
关闭文件，运行下面的命令，让配置立即生效
source /etc/profile
现在可以在终端内直接输入mysql进入mysql的命令行了
mysql -uroot
执行下面的命令修改root密码 
mysql> SET PASSWORD = PASSWORD('123456');
若要设置root用户可以远程访问，执行
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
使授权立即生效
FLUSH PRIVILEGES;
红色的password为远程访问时，root用户的密码，可以和本地不同。
 
5、配置防火墙
默认防火墙的3306端口默认没有开启，若要远程访问，需要开启这个端口
vim /etc/sysconfig/iptables
在“-A INPUT –m state --state NEW –m tcp –p –dport 22 –j ACCEPT”，下添加：
-A INPUT -m state --state NEW -m tcp -p -dport 3306 -j ACCEPT
然后保存，并关闭该文件，在终端内运行下面的命令，刷新防火墙配置：
service iptables restart
配置完毕！






