---
title: Linux LNMP服务器搭建教程
date: 2016-09-21 01:30:14
tags:
    - Linux
    - LNMP
categories: 服务器
---
# 安装NGINX
## 下载nginx源码包
1.官网下载地址：http://nginx.org/en/download.html

2.通过wget|apt-get：wget http://nginx.org/download/nginx-1.13.0.tar.gz

3.[点我下载](http://nginx.org/download/nginx-1.13.0.tar.gz)

## 解压安装包并安装

```
tar -xzf nginx-1.13.0.tar.gz

cd nginx-1.13.0

yum -y install pcre-devel openssl openssl-devel

./configure - -prefix=/usr/local/nginx 

make && make install
```

可以参看这篇文章：http://www.linuxidc.com/Linux/2016-03/129303.htm  
执行完后会得到这样表示安装成功
![](media/14955282000741/14955289656087.jpg)
这里提示安装路径为:/usr/local/nginx
这时候访问网站是没有任何效果的因为服务还没有启动：
## 启动NGINX

```
nginx -c /usr/local/nginx/conf/nginx.conf
```

## 停止NGINX
通过进程

> ```
查看进程     ：   ps -ef | grep nginx
快速停止Nginx：   kill -QUIT 主进程号
            或
强制停止Nginx：   kill -TERM 主进程号
```

## 重启NGINX
如果更改了配置就要重启Nginx，要先关闭Nginx再打开？不是的，可以向Nginx 发送信号，平滑重启。
平滑重启命令：

```
kill -HUP 住进称号或进程号文件路径
```

或者使用

```
/usr/nginx/sbin/nginx -s reload
```

##nginx只能访问主页其他页面404问题

```
location / {
  root   D:/WWW/Lifes/public;
  index  index.html index.htm index.php;

  try_files $uri $uri/ /index.php?$query_string;
  if (!-e $request_filename){  
      rewrite ^/(.*) /index.php last;  
  } 
}
```

----------------------------------华丽的分割线-----------------------------------

**⚠️注意**，修改了配置文件后最好先检查一下修改过的配置文件是否正 确，以免重启后Nginx出现错误影响服务器稳定运行。判断Nginx配置是否正确命令如下：

```
nginx -t -c /usr/nginx/conf/nginx.conf
```

或者

```
/usr/nginx/sbin/nginx -t
```

# 安装PHP
## 下载php
[下载php url在php官网中复制]
`wget url`

[安装编译器 依赖工具]
`yum install  gcc gcc-c++ libxml2-devel`	
下载PHP 选择相对于的版本
[PHP官网下载](http://www.php.net/downloads.php)
注意：Linux 下载失败是下载的URL被加密了需要修改下URL方法如下：
1.先将文件加入下载列表
2.打开下载列表找到对应PHP文件-》右击拷贝地址将它粘贴到浏览器地址栏中不要回车，你会看到类似这样的链接
`http://124.205.69.131/files/2088000005848DB0/cn2.php.net/distributions/php-7.0.19.tar.gz`  
将URL中的 `124.205.69.131/files/2088000005848DB0/` 删除掉，拷贝URL 即可在 「Linux」 中使用 「wget」 下载了。


[php文件夹下编译php "--enable-fpm"：nginx需要它解析php]
./configure --prefix=/usr/local/php7 --enable-fpm 

> --with-mysqli=/usr/local/mysql/bin/mysql_config  指定mysqli位置
  --with-mysql=/usr/local/mysql/  指定mysql位置
  --with-config-file-path=/usr/local/php/etc/  指定配置文件目录



[编译&安装]
->make && make install	

拷贝文件：
1.进入 `/usr/local/php/etc/` 目录

```
cp php-fpm.conf.default php-fpm.conf
cd php-fpm.d
cp www.conf.default www.conf
cd /usr/local/php/sbin
./php-fpm
```

全局PHP ：
export PATH=$PATH:/usr/local/php7/bin   
## 安装php
## 启动
## 停止
## 重启
/usr/local/php/sbin/php-fpm &
## 扩展问题
`/php/bin/phpize` 报错
![](media/14955282000741/14966714379954.jpg)

```
wget http://ftp.gnu.org/gnu/m4/m4-1.4.9.tar.gz
tar -zvxf m4-1.4.9.tar.gz
cd m4-1.4.9/
./configure && make && make install

cd ../

wget http://ftp.gnu.org/gnu/autoconf/autoconf-2.62.tar.gz
tar -zvxf autoconf-2.62.tar.gz
cd autoconf-2.62/
./configure && make && make install
```
### 安装步骤
1.进入PHP源码包「etc」 目录下 进入相应的扩展目录

```
/usr/local/php/bin/phpize   #这里为你自己的phpize路径，如果找不到，使用whereis phpize查找

#执行后，发现错误 无法找到config.m4 ，config0.m4就是config.m4。直接重命名

mv config0.m4 config.m4

/usr/local/php/bin/phpize

./configure --with-openssl --with-php-config=/usr/local/php/bin/php-config

make

make install
```
安装完成后，会返回一个 .so 文件（openssl.so）的目录。在此目录下把 openssl.so 文件拷贝到你在php.ini 中指定的 extension_dir 下（在php.ini文件中查找：extension_dir =），我这里的目录是 
`/usr/local/php/lib/php/extensions`
编辑php.ini文件，在文件添加

```
extension=openssl.so
```

重启Apache/Nginx即可

# mysql的安装
首先使用yum或apt-get下载依赖管理：

```
yum -y install gcc gcc-c++ ncurses-devel cmake
```
⚠️注意：MySQL从5.5开始，使用cmake 进行编译设置；因此，我们还要安装cmake编译工具

装MySQL需要依赖 Boost  的C++扩展，而且只能是 1.59.0 版本
Boost 下载地址： http://www.boost.org/users/history  ；
选择1.59.0版本下载，在编译是填写相应参数，指定Boost源码位置即可,我这里放装到了 /usr/local/ 下；

```
wget https://jaist.dl.sourceforge.net/project/boost/boost/1.59.0/boost_1_59_0.tar.gz

boost_1_59_0.tar.gz

tar xzvf boost_1_59_0.tar.gz      # 解压扩展

mv boost_1_59_0 /usr/local/boost     # 将扩展源码剪切到/usr/local下
```
## 下载mysql
前往[mysql官网](https://dev.mysql.com/downloads/mysql/)下载linux通用版本源码包。或 [点我下载mysql-5.7.18源码版本](https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.18.tar.gz)

## 安装mysql

```
wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.18.tar.gz

tar xzvf mysql-5.7.18.tar.gz

cd mysql-5.7.18     # 切换目录到刚解压的文件夹中

cmake  -DCMAKE_INSTALL_PREFIX=/usr/local/mysql  -DMYSQL_DATADIR=/usr/local/mysql/data  -DMYSQL_UNIX_ADDR=/tmp/mysql.sock  -DDEFAULT_CHARSET=utf8  -DDEFAULT_COLLATION=utf8_general_ci -DMYSQL_TCP_PORT=3306 -DWITH_BOOST=/usr/local/boost

make && make install.  #此过程很漫长，请耐心等待
```
⚠️注意：这里的 -DWITH_BOOST 要改成你的路径否则会出错 我这里路径是 /usr/local/boost

> **cmake 参数解释**：
-DCMAKE_INSTALL_PREFIX： 指定安装路径
-DMYSQL_DATADIR ： 指定数据存放路径
-DMYSQL_UNIX_ADDR ：指定套间字路径
-DDEFAULT_CHARSET ： 设置字符集
-DDEFAULT_COLLATION ： 设置字符校验集
-DWITH_BOOST ： 指定Boost扩展源码路径  
-DMYSQL_USER=mysql ： 指定mysql运行用户 
-DMYSQL_TCP_PORT=3306 ：指定mysql端口
-DWITH_INNOBASE_STORAGE_ENGINE=1 ： 安装innodb存储引擎
-DWITH_MEMORY_STORAGE_ENGINE=1 ： 安装memory存储引擎
-DWITH_READLINE=1 ： 支持readline库
-DENABLED_LOCAL_INFILE=1 ： 启用加载本地数据

mysql安装成功后查看/etc/my.cnf 文件是否存在，如果没有的话执行以下命令有的话则跳过。

```
cp /usr/local/mysql/support-files/my-default.cnf /etc/my.cnf   #复制配置文件
```

修改配置文件 /etc/my.cnf  修改内容如下

```
basedir = /usr/local/mysql/         # 这里是mysql的安装路径

datadir = /usr/local/mysql/data     # 这里是 mysql的数据存放路径

socket = /tmp/mysql.sock            # 这个我忘了
```


添加mysql用户和创建数据存放目录，并修改权限

```
groupadd  mysql

useradd -r -g mysql -s /bin/false mysql  # 创建不可登录用户

mkdir -p /usr/local/mysql/data

cd /usr/local/mysql  #切换至安装目录

bin/mysqld --defaults-file=/etc/my.cnf --initialize --user=mysql   # 初始化数据库

chown -R mysql:mysql /usr/local/mysql # 修改权限
```

>初始化数据库命令参数解释：
--defaults-file ： 制定MySQL配置文件路径
--initialize ： 初始化随机密码，注意，初始化的密码是一个过期密码，登录后需要修改密码
--user： 指定账户
上一个命令执行完之后，会在命令提示符的最后给出随机密码，
一定记住 将此密码记录下来

到这里mysql 安装完毕接下来让我们启动mysql吧！

```
cd /usr/local/mysql  #切换到mysql安装目录

support-files/mysql.server start  # 启动mysql 服务器

bin/mysql -u root -p    

Enter password：   # 输入刚刚的随机密码链接数据库
```

进入数据库必须修改密码否侧无法使用

```
ALTER USER 'root'@'localhost' IDENTIFIED BY  'NewPassWord';
```

⚠️注意：这里修改密码必须且只能使用「ALTER」 命令修改

## 启动
```
cd /usr/local/mysql/support-files
cp mysql.server /etc/init.d/mysqld  #添加mysql启动命令
cd /usr/local/mysql/bin
vim /root/.bash_profile #将mysql添加到系统的环境变量里
PATH=$PATH:$HOME/bin:/usr/local/lnmp/mysql/bin
source /root/.bash_profile  #刷新环境变量文件
echo $PATH                  #查看mysql添加到环境变量
which mysql        #测试
/usr/local/lnmp/mysql/bin/mysql
```

## 停止
## 重启

# NGINX全局设置

```
vi /etc/init.d/nginx  (输入下面的代码)

#!/bin/bash
# nginx Startup script for the Nginx HTTP Server
# it is v.0.0.2 version.
# chkconfig: - 85 15
# description: Nginx is a high-performance web and proxy server.
#              It has a lot of features, but it's not for everyone.
# processname: nginx
# pidfile: /var/run/nginx.pid
# config: /usr/local/nginx/conf/nginx.conf

#nginx程序路径
nginxd=/usr/local/nginx/sbin/nginx

#nginx配置文件路径
nginx_config=/usr/local/nginx/conf/nginx.conf

nginx pid文件的路径，可以在nginx的配置文件中找到
nginx_pid=/var/run/nginx.pid
RETVAL=0
prog="nginx"
# Source function library.
. /etc/rc.d/init.d/functions
# Source networking configuration.
. /etc/sysconfig/network
# Check that networking is up.
[ ${NETWORKING} = "no" ] && exit 0
[ -x $nginxd ] || exit 0
# Start nginx daemons functions.
start() {
if [ -e $nginx_pid ];then
   echo "nginx already running...."
   exit 1
fi
   echo -n $"Starting $prog: "
   daemon $nginxd -c ${nginx_config}
   RETVAL=$?
   echo
   [ $RETVAL = 0 ] && touch /var/lock/subsys/nginx
   return $RETVAL
}
# Stop nginx daemons functions.
stop() {
        echo -n $"Stopping $prog: "
        killproc $nginxd
        RETVAL=$?
        echo
        [ $RETVAL = 0 ] && rm -f /var/lock/subsys/nginx /var/run/nginx.pid
}
# reload nginx service functions.
reload() {
    echo -n $"Reloading $prog: "
    #kill -HUP `cat ${nginx_pid}`
    killproc $nginxd -HUP
    RETVAL=$?
    echo
}
# See how we were called.
case "$1" in
start)
        start
        ;;
stop)
        stop
        ;;
reload)
        reload
        ;;
restart)
        stop
        start
        ;;
status)
        status $prog
        RETVAL=$?
        ;;
*)
        echo $"Usage: $prog {start|stop|restart|reload|status|help}"
        exit 1
esac
exit $RETVAL
```

同样的修改了nginx的配置文件nginx.conf，也可以使用上面的命令重新加载新的配置文件并运行，可以将此命令加入到rc.local文件中，这样开机的时候nginx就默认启动了

在 rc.local 加入一行如下代码： 

```
vi /etc/rc.local 

/etc/init.d/nginx start
```

保存并退出，下次重启会生效。

## 将nginx添加为系统服务

```
chkconfig --add nginx

chkconfig nginx on
```

对于其他服务也同样适用，比如Mysql,php-fpm等等

# Laravel 安装
下载composer [官网下载地址](https://getcomposer.org/download/)

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

php -r "if (hash_file('SHA384', 'composer-setup.php') === '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be6190fa0355160742ab2e1c88d40d5be660b410') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```

```
php composer-setup.php

php -r "unlink('composer-setup.php');"
```

上述命令执行完毕后当前目录会有一个名为 「composer.par」 文件

给文件可执行权限并剪切到 `/usr/local/bin/` 目录下重命名为 「composer」

```
chmod +x composer.pre
mv composer.pre /usr/local/bin/composer
```

现在可以全局运行composr 命令 ，如果不行则运行一下命令；

```
vim /etc/profile

export PATH="$PATH:/usr/local/bin/"
```

`Esc    :wq` 保存


## laravel安装
>首先，使用 Composer 下载 Laravel 安装包：

```
composer global require "laravel/installer"
```

注意：下载速度慢可以使用下面镜像地址 打开命令行窗口（windows用户）或控制台（Linux、Mac 用户）并执行如下命令：
`composer config -g repo.packagist composer https://packagist.phpcomposer.com`

>将 `~/.composer/vendor/bin` 路径加到 PATH,只有这样系统才能找到 `laravel` 的执行文件,安装完成，就可以使用 `laravel new` 命令在指定目录创建一个新的 Laravel 项目.


```
vim  ~/.bashrc

alias laravel='~/.composer/vendor/bin/laravel'  

source ~/.bashrc

chmod -R +x  /root/.composer/vendor/laravel/    #增加Laravel执行权限
```

开始你的Laravel的旅程吧～。

>执行遇到`/usr/bin/env: php: No such file or directory` 怎么办？
主要是php安装文件不在 `/usr/local/bin` 。安装在 `/usr/local/php`中

找到php的可执行文件，`/usr/local/php5513/bin/php`


`ln -s /usr/local/php5513/bin/php /usr/local/bin/php`


把可执行文件连接过去就可以了。

⚠️注意：下载laravel项目访问时候会出现空白页面是因为「bootstrap」 和 「storage」需要读写权限 

```
chmod -R 777 bootstrap/

chmod -R 777 storage/
```


