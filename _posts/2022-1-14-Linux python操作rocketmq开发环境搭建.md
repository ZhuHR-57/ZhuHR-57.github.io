---
title: Linux python操作rocketmq开发环境搭建
tags: mq
layout: article
---

# Linux python操作rocketmq开发环境搭建



## 安装Python

### Centos7

安装系统依赖

```
yum install openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel gcc gcc-c++  openssl-devel libffi-devel python-devel mariadb-devel
```

安装python3.8

```
wget https://www.python.org/ftp/python/3.8.6/Python-3.8.6.tgz
tar -xzvf Python-3.8.6.tgz -C  /tmp
cd  /tmp/Python-3.8.6
2. 把Python3.8安装到 /usr/local 目录

./configure --prefix=/usr/local
make
make altinstall

3. 更改/usr/bin/python链接

ln -s /usr/local/bin/python3.8 /usr/bin/python3
ln -s /usr/local/bin/pip3.8 /usr/bin/pip3
```

安装virtualenvwrapper

```
yum install python-setuptools python-devel
pip3 install virtualenvwrapper

编辑.bashrc文件 - vim ~/.bashrc 最后添加如下内容
VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh

退出后 执行 source  ~/.bashrc

新建虚拟环境
mkvirtualenv -p python3 python mxshop_srv
```

 安装依赖包

```
进入虚拟环境后
pip install rocketmq-client-python

#下面步骤很重要!!!!!!
ln -s /usr/local/lib/librocketmq.so /usr/lib
sudo ldconfig
```

配置防火墙

```
firewall-cmd --zone=public --add-port=10911/tcp --permanent 
firewall-cmd --zone=public --add-port=10912/tcp --permanent 
sudo firewall-cmd --reload
```

安装librocketmq

```
wget https://github.com/apache/rocketmq-client-cpp/releases/download/2.2.0/rocketmq-client-cpp-2.2.0-centos7.x86_64.rpm
sudo rpm -ivh rocketmq-client-cpp-2.2.0-centos7.x86_64.rpm
```



### 2.Ubuntu20.04

**Ubuntu20.04自带python3.8**

安装pip3

```
apt install pip3
```

安装virtualenvwrapper

```
yum install python-setuptools python-devel
pip3 install virtualenvwrapper

编辑.bashrc文件 - vim ~/.bashrc 最后添加如下内容
VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh

退出后 执行 source  ~/.bashrc

新建虚拟环境
mkvirtualenv -p python3 python mxshop_srv
```

 安装依赖包

```
进入虚拟环境后
pip install rocketmq-client-python

#下面步骤很重要!!!!!!
ln -s /usr/local/lib/librocketmq.so /usr/lib
sudo ldconfig
```

配置防火墙

```
firewall-cmd --zone=public --add-port=10911/tcp --permanent 
firewall-cmd --zone=public --add-port=10912/tcp --permanent 
sudo firewall-cmd --reload
```

安装librocketmq

```
wget https://github.com/apache/rocketmq-client-cpp/releases/download/2.0.0/rocketmq-client-cpp-2.0.0.amd64.deb
sudo dpkg -i rocketmq-client-cpp-2.0.0.amd64.deb
```

