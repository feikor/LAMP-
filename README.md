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



