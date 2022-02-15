---
title: centos7安装Nginx
tags: 环境安装与配置
layout: article

---





**记录一下centos7 安装nginx的过程**

<!--more-->

---



# 一、配置 EPEL源

```shell
sudo yum install -y epel-release
sudo yum -y update
```

# 二、安装Nginx

```shell
sudo yum install -y nginx
```

安装成功后，默认的网站目录为： /usr/share/nginx/html

默认的配置文件为：/etc/nginx/nginx.conf

自定义配置文件目录为: /etc/nginx/conf.d/

# 三、开启端口80和443

查看防火墙状态

```shell
systemctl status firewalld.service
# 打开
systemctl start firewalld.service
```

如果你的服务器打开了防火墙，你需要运行下面的命令，打开80和443端口。

```shell
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
# 自己想要打开的端口
firewall-cmd --zone=public --add-port=9999/tcp --permanent
# 重新加载配置
sudo firewall-cmd --reload
```

如果你的服务器是阿里云ECS，你还可以通过控制台安全组，打开80和443端口，或者其他自定义端口。

# 四、操作Nginx

```shell
1.启动 Nginx
systemctl start nginx

2.停止Nginx
systemctl stop nginx


3.重启Nginx
systemctl restart nginx


4.查看Nginx状态
systemctl status nginx


5.启用开机启动Nginx
systemctl enable nginx

6.禁用开机启动Nginx
systemctl disable nginx
```

# 五、放网页

```
cd /usr/share/nginx/html
```

将网页复制放入即可(index.html 是默认主页名称)

如果想有子页面，再html文件夹下新建文件夹即可（如：la(la.html))

```
URL http://ip/la/la.html
```

如果想修改端口

```
vi /etc/nginx/nginx.conf
```

```shell
  server {
        listen       8888;     #将80修改为自己想要的端口，例如8888
        listen       [::]:8888;#将80修改为自己想要的端口，例如8888
        server_name  _;
        root         /usr/share/nginx/html;


        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }
```

```
URL http://ip:8888/la/la.html
```

记得将端口在防火墙中打开！

# 六、配置多端口多网页

```
cd /etc/nginx/conf.d
```

创建file1.conf(文件名随意，但必须是conf后缀)，下面为文件内容

```
vi file1.conf
```

```shell
server {
   listen       8887;	    #自定义端口
   listen       [::]:8887;  #自定义端口
   server_name  _;
   root         /code/dist1;#自定义资源目录

   index index.html index.htm;
   location / {
   	try_files $uri $uri/ /index.html;
   }
}
```

创建file2.conf(文件名随意，但必须是conf后缀)，下面为文件内容

```
vi file2.conf
```

```shell
server {
   listen       8889;	    #自定义端口
   listen       [::]:8889;  #自定义端口
   server_name  _;
   root         /code/dist1;#自定义资源目录

   index index.html index.htm;
   location / {
   	try_files $uri $uri/ /index.html;
   }
}
```

```shell
firewall-cmd --zone=public --add-port=8887/tcp --permanent
firewall-cmd --zone=public --add-port=8889/tcp --permanent
# 重启
sudo firewall-cmd --reload && systemctl restart nginx
```

```
URL 
http://ip:8887
http://ip:8889
```



# 参考文章

[Centos 7下安装配置Nginx](https://developer.aliyun.com/article/699966)

[Nginx 不同端口配置多个项目](https://blog.csdn.net/shenxianhui1995/article/details/103870923)