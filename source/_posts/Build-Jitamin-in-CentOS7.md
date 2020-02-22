---
title: Build Jitamin in CentOS7
date: 2018-07-19 22:59:30
tags:
  - Tools
---
环境:CentOS7
* **安装相关依赖**：
    * 安装apache: yum install httpd
    * 安装php 7:
        * yum -y install epel-release
        * rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
        * yum install php70w
        * 查看PHP是否安装成功: php -v
    * 安装Composer:
        * curl -sS https://getcomposer.org/installer | php
        * mv composer.phar /usr/local/bin/composer
        * 检查composer是否安装成功: composer
    * 安装Mysql:
        * wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
        * yum -y install mysql57-community-release-el7-10.noarch.rpm
        * yum -y install mysql-community-server
* **安装Jitamin**:
    * cd <安装目录>如：cd /usr/local
    * 下载jitamin: git clone https://github.com/jitamin/jitamin.git jitamin
    * cd jitamin/
    * cp .env.example .env
    * 配置.env文件:
        * 修改DB_HOSTNAME,DB_USERNAME等数据库属性
    * 安装相关依赖:composer install -o --no-dev
        * <font color=#FF4500>**可能会遇到一些problem**</font>:
            * 缺失PHP-gd: yum install php70w-gd
            * 缺失PHP-mbstring: yum install php70w-mbstring
            * 缺失PHP-dom: yum install php70w-dom
    * 创建数据库表: vendor/bin/phinx migrate
    * 安装初始数据: vendor/bin/phinx seed:run
    * 确保bootstrap/cache和storage目录可写:
        * chmod -R 0777 bootstrap/cache
        * chmod -R 0777 storage
    * 同步cache:
        * php artisan config:cache
        * php artisan route:cache
* **配置web服务器**:
    * 主要就是配置apache的配置文件
    * 利用yum安装的apache默认配置文件存放于: /etc/httpd/conf/httpd.conf
    * 我们要做的就是在/etc/httpd/conf.d目录下新建jitamin.conf内容如下:
```
    <VirtualHost *:80>
    DocumentRoot "/usr/local/jitamin/public"
    DirectoryIndex index.php
    <Directory "/usr/local/jitamin/public">
        AllowOverride all
        Options -Indexes +FollowSymLinks +ExecCGI
        AllowOverride All
        Order allow,deny
        Allow from all
        Require all granted
    </Directory>
    </VirtualHost>
```
     * 最后重启apache即可: service httpd restart

* <font color=#FF4500>**可能会遇到的问题**</font>
    * 在创建数据库表和插入数据库数据的时候可能会遇到xxx pdo xxxx相关的问题
        * 解决方法
            * yum install php70w-pdo
            * yum install php70w-mysql
    * 这个时候访问xxx.xxx.xxx.xxx(主机地址)可能会出现500 internal error的问题，查看log会发现是显示PHP无法连接上mysql
        * 解决方法
            * grant授权，本机配置时是直接grant all到所有的机器上
    * 继续访问还是会出现500的error
        * 解决方法
            * 最后发现是selinux的问题
            * setenforce 0
            * setsebool httpd_can_network_connect_db=1
