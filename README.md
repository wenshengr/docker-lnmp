# 使用 Docker 部署 LNMP 开发环境


### 项目简介
docker-lnmp 是基于 docker-compose 开发的运行在 Docker 上的 LNMP 开发环境，包含 PHP、MySQL、Redis、MongoDB、Memcache等镜像，满足您的基础学习、开发和测试需求。


### 包含镜像
********************************************************
* 
* docker-lnmp 包含以下镜像，每种镜像的版本都配置在.env文件中：
* 
* LNMP: nginx-1.18 + php-7.3 + mysql-5.7
* MongoDB: 4.2.8-bionic
* Redis: 5.0.0
* Memcache: 1.6.6
* 
* PHP_VERSION: php-fpm 版本
* NGINX_VERSION: nginx 版本
* MYSQL_VERSION: mysql 版本
* REDIS_VERSION: redis 版本
* MONGO_VERSION: mongodb 版本
* MEMCACHE_VERSION: memcache 版本
* 
********************************************************

其中：
#### Nginx 基于 nginx:1.18-alpine

#### PHP基于 php:7.3-fpm-alpine, 其中包含以下扩展
```
amqp
bcmath
Core
ctype
curl
date
dom
fileinfo
filter
ftp
gd
hash
iconv
json
libxml
mbstring
memcached
mongodb
mysqli
mysqlnd
openssl
pcntl
pcre
PDO
pdo_mysql
pdo_sqlite
Phar
posix
readline
redis
Reflection
SeasLog
session
SimpleXML
sockets
sodium
SPL
sqlite3
standard
swoole
tokenizer
xml
xmlreader
xmlwriter
yaconf
yaf
yaml
Zend OPcache
zip
zlib
```

#### MySQL 是 mysql:5.7 版本

#### Redis 是  redis:5.0-alpine 版本

#### MongoDB 是 mongo:4.2.8-bionic 版本

#### Memcache 是 memcached:1.6.6-alpine 版本


## 下载使用

Docker LNMP 默认将同级目录映射到 php-fpm 容器的工作目录，在项目的同级目录下载 Docker LNMP：
```
$ git clone https://github.com/wenshengr/docker-lnmp.git
```

进入 docker-lnmp 目录，生成配置文件 `.env`

```
cd docker-lnmp
```
项目根目录：`.env` 文件中 `WEB_ROOT_PATH`

日志文件根目录： `.env` 文件中 `LOG_PATH`

### 启动服务

在 docker-lnmp 目录，启动服务，命令如下：
``` 
$ docker-compose up -d
Creating lnmp-mongo     ... done
Creating lnmp-php-pfm   ... done
Creating lnmp-memcached ... done
Creating lnmp-nginx     ... done
Creating lnmp-mysql     ... done
```

nginx 默认会启动 php-fpm 和 mysql 服务，如需启动其它服务请手动添加，可选服务有 mongo、redis、memcached。

##### 测试服务是否启动成功
启动成功后，在 docker-lnmp 的www目录新建 phpinfo.php 文件，浏览器访问 `http://localhost:380/phpinfo.php`，则可看到 phpinfo() 相关信息。


### 关闭服务

在 docker-lnmp 目录，关闭服务，命令如下：
``` 
	docker-compose down
```


### 重新构建服务

如修改 dockerfile 文件，需重新构建服务，如重新构建 nginx 命令如下：
```
	docker-compose build nginx
```

建议先关闭服务，构建完成再重启服务。


### 虚拟主机

Nginx 虚拟主机相关配置请参考如下文档，配置完需重启服务，完整配置请参考 `nignx/conf.d/default.conf` 文件：

# Nginx 配置虚拟主机


## 添加域名映射

Mac、Ubuntu等宿主机 /etc/hosts 文件中添加域名映射，配置如下：
Windows 宿主机 C:/Windows/System32/drivers/etc/hosts 文件中添加域名映射，配置如下：
```
127.0.0.1 test.local
```


## 添加 server 配置

目录 docker-lnmp/nginx/conf.d 下新增 default.conf 文件，配置如下：
```
server {
    listen 80;
    server_name  test.local;
    root /var/www/test;

    index index.php  index.html index.htm;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /var/www;
    }

    location ~ \.php$ {
        fastcgi_pass   php-fpm:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
}
```


## 3. 重启服务

参照基本命令。
