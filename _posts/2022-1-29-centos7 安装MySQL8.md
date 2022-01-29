---
title: centos7安装MySQL8
tags: 环境安装与配置
layout: article
---





**记录一下centos7 安装mysql8的过程**

<!--more-->

---





# 1. 卸载MariaDB 和 Mysql

MariaDB是MySQL的一个分支，主要由开源社区维护。

- CentOS 7及以上版本已经不再使用MySQL数据库，而是使用MariaDB数据库；
- 如果直接安装MySQL，会和MariaDB的文件冲突；

因此，需要先卸载自带的MariaDB，再安装MySQL。

1. **查看版本**：`rpm -qa|grep mariadb`
2. **卸载**：`rpm -e --nodeps 文件名`
3. **检查是否卸载干净**：`rpm -qa|grep mariadb`

1. 查看MySQL信息 `rpm -qa|grep mysql`
   - 如果没有输出，说明当前系统没有安装MySQL
2. 如果不是想要的版本，则**卸载**：`rpm -e --nodeps mysql文件名`

# 2. 安装MySQL

## 下载安装

```shell
# 安装mysql服务
sudo yum install mysql-community-server
# 修改源的配置
cd /etc/yum.repos.d
vi mysql-community.repo
# 将[mysql80-community]下gpgcheck=1 -> gpgcheck=0
```

```shell
sudo yum install -y https://repo.mysql.com//mysql80-community-release-el7-3.noarch.rpm
sudo yum install mysql-community-server
```

```shell
# 使用以下命令启动MySQL服务器：
systemctl start mysqld
# 检查MySQL服务器的状态：
systemctl status mysqld
```

## 初始化

```shell
# 查看密码
sudo grep 'temporary password' /var/log/mysqld.log
# 执行下面命令登录 输入上一步骤输出的密码
mysql -uroot -p
# 进入mysql后 更改超级用户帐户的root密码
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'YourNewPassword';
# 退出
exit;
```

```shell
#以root用户登录到服务器，输入新密码：
mysql -u root -p
Enter password: (enter root password here)
```

## 配置远程连接

```shell
# 登陆Mysql
mysql -u root -p
# 选择 mysql 数据库
use mysql;
# 创建新用户
CREATE USER 'root'@'%' IDENTIFIED BY 'NewPassword@123';
# navicat只支持以前的"mysql_native_password"
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'NewPassword@123';
# 设置该账户可以远程登陆
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';
# 刷新权限
flush privileges;
```

```shell
# 打开防火墙
firewall-cmd --zone=public --add-port=3306/tcp --permanent 
# 重载防火墙
firewall-cmd --reload
```



# 3. 忘记密码

```shell
# 修改配置文件免密码登录mysql
vim /etc/my.cnf 
#  在 [mysqld]最后加上如下语句 并保持退出文件
skip-grant-tables  
# 重启mysql服务
service mysqld restart 
```

```shell
# 免密码登录到mysql上
mysql -u root -p   
# password直接回车
```

```shell
# 选择mysql数据库
use mysql;
# 查看root相关用户信息 
# host: 允许用户登录的ip‘位置’%表示可以远程；
# user:当前数据库的用户名
select host, user, authentication_string, plugin from user;
# 把密码设置为空
update user set authentication_string='' where user='root';  
# 退出
exit;
```

```shell
# 删除/etc/my.cnf文件最后的 skip-grant-tables 
vim /etc/my.cnf 
#  在 [mysqld]最后删除如下语句 并保持退出文件
skip-grant-tables  
# 重启mysql服务
service mysqld restart 
```

```shell
# 免密码登录到mysql上
mysql -u root -p   
# password直接回车
```

```shell
# 不使用远程连接
ALTER user 'root'@'localhost' IDENTIFIED BY 'NewPassword@123'  
# 使用远程连接
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'NewPassword@123';
```



# 参看文章

[centos7 安装MySql8.0](https://juejin.cn/post/6919849319189184520)

[CentOs7中Mysql8.0设置远程连接](https://blog.csdn.net/weixin_43818197/article/details/104783858)

[Centos7重置Mysql 8.0.1 root 密码](https://www.cnblogs.com/jjg0519/p/9034713.html)