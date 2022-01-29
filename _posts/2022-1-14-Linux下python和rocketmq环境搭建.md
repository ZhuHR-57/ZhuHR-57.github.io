---
title: Linux下python和rocketmq环境搭建
tags: 环境安装与配置
layout: article
---





**记录一下linux安装python和python的环境管理工具以及配置rocketmq的过程**

<!--more-->

---



# centos7

## 安装Python3.8.6

```shell
# 安装系统依赖
yum install openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel gcc gcc-c++  openssl-devel libffi-devel python-devel mariadb-devel
# 进入tmp目录(这个随意)
cd /tmp
# 下载
wget https://www.python.org/ftp/python/3.8.6/Python-3.8.6.tgz
# 解压
tar -xzvf Python-3.8.6.tgz -C  /tmp
cd /tmp/Python-3.8.6
# 编译到/usr/local目录下
./configure --prefix=/usr/local
# 安装
make&&make install
# 软连接
ln -s /usr/local/bin/python3.8 /usr/bin/python3
ln -s /usr/local/bin/pip3.8 /usr/bin/pip3
```

## 安装virtualenvwrapper

```shell
# 下载安装
yum install python-setuptools python-devel && pip3 install virtualenvwrapper

# 编辑.bashrc文件 - vim ~/.bashrc 最后添加如下内容后保存退出
VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
# 更新环境变量
source  ~/.bashrc
# 新建虚拟环境
mkvirtualenv -p python3 python xxx
```

##  安装librocketmq

```shell
# 进入虚拟环境后
pip install rocketmq-client-python

#下面步骤很重要!!!!!!
ln -s /usr/local/lib/librocketmq.so /usr/lib
sudo ldconfig

# 配置防火墙
firewall-cmd --zone=public --add-port=10911/tcp --permanent 
firewall-cmd --zone=public --add-port=10912/tcp --permanent 
sudo firewall-cmd --reload

# 下载librocketmq安装包
wget https://github.com/apache/rocketmq-client-cpp/releases/download/2.2.0/rocketmq-client-cpp-2.2.0-centos7.x86_64.rpm
# 安装
sudo rpm -ivh rocketmq-client-cpp-2.2.0-centos7.x86_64.rpm
```

## 配置yum

安装python3改完软链接以后发现yum命令报错了，yum是依赖python2.7的，你把python改成了3.8了，所以报错了。

```bash
[root@localhost Python-3.8.12]# yum
  File "/usr/bin/yum", line 30
    except KeyboardInterrupt, e:
                            ^
SyntaxError: invalid syntax
```

可以修改yum里对python2的依赖即可。虽然安装了python3但是系统里python2依旧还在系统里，可以通过python2来指定用python2.7的命令，首先来看下python2的命令在哪里

```bash
[root@localhost ~]# which python2
/usr/bin/python2
```

可以cd到/usr/bin目录下 通过ls -alh|grep python查看python命令的详细情况。

```bash
[root@localhost bin]# ls -alh|grep python
```

修改对开头python的依赖，修改成python2或python2.7都可以。

```bash
vi /usr/libexec/urlgrabber-ext-down 
```

修改对开头python的依赖，修改成python2或python2.7都可以。

```bash
vi /usr/bin/yum
```

修改完这两个文件后，再敲yum命令就不会报错了。



# Ubuntu20.04

**Ubuntu20.04自带python3.8**

## 安装pip3

```shell
apt install pip3
```

## 安装virtualenvwrapper

```shell
# 下载安装包
yum install python-setuptools python-devel
# 安装
pip3 install virtualenvwrapper

# 编辑.bashrc文件 - vim ~/.bashrc 最后添加如下内容后保存退出
VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
# 更新环境变量
source  ~/.bashrc
# 新建虚拟环境
mkvirtualenv -p python3 python xxx
```

##  安装librocketmq

```shell
# 进入虚拟环境后
pip install rocketmq-client-python

#下面步骤很重要!!!!!!
ln -s /usr/local/lib/librocketmq.so /usr/lib
sudo ldconfig

# 配置防火墙
firewall-cmd --zone=public --add-port=10911/tcp --permanent 
firewall-cmd --zone=public --add-port=10912/tcp --permanent 
sudo firewall-cmd --reload

# 下载librocketmq安装包
wget https://github.com/apache/rocketmq-client-cpp/releases/download/2.0.0/rocketmq-client-cpp-2.0.0.amd64.deb
# 安装
sudo dpkg -i rocketmq-client-cpp-2.0.0.amd64.deb
```



# 参考文章

[CentOS7下安装python3.8 ](https://www.cnblogs.com/xiejava/p/15541899.html)