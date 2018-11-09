## 配置LNMP

:link: https://secure.phabricator.com/book/phabricator/article/installation_guide/

## 下载依赖

:link:  https://secure.phabricator.com/book/phabricator/article/installation_guide/

```bash
# pick some install directory
$ cd somewhere / # $ cd /home/ubuntu/software/phacility/
comewhere / $ git clone https://github.com/phacility/libphutil.git 
somewhere / $ git clone https://github.com/phacility/arcanist.git 
somewhere / $ git clone https://github.com/phacility/phabricator.git
```

## 配置Nginx + PHP

:link: [Nginx+php-fpm](https://www.c2newlive.com/nginxphp-fpm%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/)

```nginx
server {
  listen 80;
  # phabricator.example.com 193.112.69.100
  server_name phabricator.thoughts.vip;
  # /path/to/phabricator/webroot
  root        /home/ubuntu/software/phacility/phabricator/webroot;

  location / {
    index index.php;
    rewrite ^/(.*)$ /index.php?__path__=/$1 last;
  }

  location /index.php {
    # TCP
    fastcgi_pass   localhost:9000;
    # unix socket
    # fastcgi_pass unix:/run/php/php7.2-fpm.sock;
    
    fastcgi_index   index.php;

    #required if PHP was built with --enable-force-cgi-redirect
    fastcgi_param  REDIRECT_STATUS    200;

    #variables to make the $_SERVER populate in PHP
    fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
    fastcgi_param  QUERY_STRING       $query_string;
    fastcgi_param  REQUEST_METHOD     $request_method;
    fastcgi_param  CONTENT_TYPE       $content_type;
    fastcgi_param  CONTENT_LENGTH     $content_length;

    fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;

    fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
    fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;

    fastcgi_param  REMOTE_ADDR        $remote_addr;
  }
}
```

```bash
# 查看端口是否正确监听，这里目标端口是9000
service php7.2-fpm start
netstat -ltnp
# 若没有，开始配置PHP-FPM
# 目标配置文件
sudo vim /etc/php/7.2/fpm/pool.d/www.conf
```

:link: [php-fpm.conf 配置官方指南](http://php.net/manual/zh/install.fpm.configuration.php)

:link: [php-fpm的配置和优化](https://www.zybuluo.com/phper/note/89081)

```bash
listen.owner = www-data
listen.group = www-data
# 注释掉下面这行,启用TCP；反之，开启即为启用UNIX SOCKET
# listen.mode = 0660
# listen.mode 是unix socket设置选项，如果使用tcp方式访问，这里注释即可。

# 应用unix socket
# listen = /run/php/php7.2-fpm.sock
# 应用TCP
  listen = 9000
```

```bash
# 配置完成，重启服务
sudo service php7.2-fpm restart
# 查看进程
netstat -ltnp
```

## 配置MYSQL

:link: [MYSQL文档](http://www.runoob.com/mysql/mysql-tutorial.html)

```bash
# 当MySQL工作时，您需要将Phabricator模式加载到其中
phabricator/ $ ./bin/storage upgrade
# 如果使用非特权用户连接到数据库，则可能必须覆盖默认用户，以便可以使用root或其他管理员用户应用架构更改
phabricator/ $ ./bin/storage upgrade --user <user> --password <password>
# 通过传递`--force`标志来避免脚本问题的提示
phabricator/ $ ./bin/storage upgrade --force
```

```
# mysql
host: localhost
user: root
password: ******
```

> 注意：更新Phabricator时，请再次运行`存储升级`以应用任何新更新。

## 配置Phabricator

### 注册Github Auth Application

OAuth App ID【Client ID】：8f3e6528a58966b271bf

OAuth App Secret 【Client Secret】：ecb747b40cc5f2f1b5e9578f16c0585c43ad994b

OAuth App Notes ：Github auth application.

### 注册Google+ Auth Application

```json
{"web":{"client_id":"1032950197363-nfc859912u5mpsdkfl13ei04dcotgrpp.apps.googleusercontent.com","project_id":"phabricator-220206","auth_uri":"https://accounts.google.com/o/oauth2/auth","token_uri":"https://www.googleapis.com/oauth2/v3/token","auth_provider_x509_cert_url":"https://www.googleapis.com/oauth2/v1/certs","client_secret":"5MSRUY-bFlJFhY5tr4KumE-n","redirect_uris":["http://phabricator.thoughts.vip/oauth/google/login/"]}}
```



### 启动守护进程【Daemons】

:link: [Managing Daemons with phd](https://secure.phabricator.com/book/phabricator/article/managing_daemons/)

```bash
phabricator/ $ ./bin/phd start
# phd help 查看帮助
```

> 注意：升级Phabricator或更改配置时，应通过运行`phd restart重新启动`守护程序。

### 设置Base URI

```bash
phabricator/ $ ./bin/config set phabricator.base-uri 'http://phabricator.thoughts.vip/'
```

### 支持大文件传输

:link: [php.ini 配置选项列表](http://php.net/manual/zh/ini.list.php)

:link: [配置文件存储](https://secure.phabricator.com/book/phabricator/article/configuring_file_storage/)

```bash
# 定位配置文件开始编辑
sudo vim /etc/php/7.2/fpm/php.ini
# 定位配置项
/post_max_size # set to 32M
/upload_max_filesize # set to 32M

sudo service nginx restart
sudo service php7.2-fpm reload
```

```bash
# 定位nginx配置文件开始编辑
sudo vim /etc/nginx/nginx.conf
# 在custom下面加入一行
client_max_body_size 32M;
```



:link: [B.5.2.10 Packet Too Large](https://dev.mysql.com/doc/refman/5.5/en/packet-too-large.html)

:link: [5.1.7服务器系统变量](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html)

```bash
# 暂时修改
# 进入Mysql server
set global max_allowed_packet = 33554432； # 32M
# 关闭此Mysql server链接，重新进入
show VARIABLES like '%max_allowed_packet%'; # 查看是否编辑成功

# 永久修改
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
# 在末尾添加
max_allowed_packet = 33554432
# 重启mysql
sudo service mysql restart
```

### MySQL ONLY_FULL_GROUP_BY Mode Set

On database host "localhost", the global `sql_mode` is set to `ONLY_FULL_GROUP_BY`. It is strongly encouraged that you disable this mode when running Phabricator.

With `ONLY_FULL_GROUP_BY` enabled, MySQL rejects queries for which the select list or (as of MySQL 5.0.23) `HAVING` list refer to nonaggregated columns that are not named in the `GROUP BY` clause. More importantly, Phabricator does not work properly with this mode enabled.

You can find more information about this mode (and how to configure it) in the MySQL manual. Usually, it is sufficient to change the `sql_mode` in your `my.cnf` file (in the `[mysqld]` section) and then restart `mysqld`:  

```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
# 在末尾添加
sql_mode=STRICT_ALL_TABLES
# 重启mysql
sudo service mysql restart
```

(Note that if you run other applications against the same database, they may not work with `ONLY_FULL_GROUP_BY`. Be careful about enabling it in these cases and consider migrating Phabricator to a different database.)

### MySQL May Run Slowly

Database host "localhost" is configured with a very small `innodb_buffer_pool_size` (`128 MB`). This may cause poor database performance and lock exhaustion.

There are no hard-and-fast rules to setting an appropriate value, but a reasonable starting point for a standard install is something like 40% of the total memory on the machine. For example, if you have 4GB of RAM on the machine you have installed Phabricator on, you might set this value to `1600M`.

You can read more about this option in the MySQL documentation to help you make a decision about how to configure it for your use case. There are no concerns specific to Phabricator which make it different from normal workloads with respect to this setting.

To adjust the setting, add something like this to your `my.cnf` file (in the `[mysqld]` section), replacing `1600M` with an appropriate value for your host and use case. Then restart `mysqld`:

  ```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
# 在末尾添加
innodb_buffer_pool_size = 256M
# 重启mysql
sudo service mysql restart
  ```

### 修改时区

:link: [PHP>日期/时间](http://php.net/manual/zh/timezones.php)

在WebUI中设置为`Asia/Shanghai`。

### Create Local Repository

```bash
sudo mkdir -p '/var/repo/'
```

:link: [配置 repository.default-local-path](http://193.112.69.100:8080/config/edit/repository.default-local-path/?issue=repository.default-local-path.empty)

### 配置Pyments

```bash
sudo pip install Pygments
```

:link: http://193.112.69.100:8080/config/edit/pygments.enabled/

### OPcache Config

:link: [官方说明](http://php.net/manual/zh/opcache.configuration.php)

```bash
sudo vim /etc/php/7.2/fpm/php.ini
# 开启如下行，并将值设置为0
;opcache.validate_timestamps=1
sudo service nginx restart
sudo service php7.2-fpm restart
```

### 安装APCu

```bash
# 下载
sudo apt install php-apcu
# 配置
sudo vim /etc/php/7.2/fpm/php.ini
# 在开头([PHP]之前)添加如下内容
[apc]
extension = apcu.so
apc.enabled= on
apc.shm_size= 64M
apc.enable_cli = on
```

### 配置邮件服务（出站）

:link: [官方说明](https://secure.phabricator.com/book/phabricator/article/configuring_outbound_email/)

:link: [腾讯云25端口解封](https://console.cloud.tencent.com/secctrl/smtp)

:link: [参考：安装和配置Postfix](https://chloerei.com/2015/04/22/install-and-configure-postfix/)

:link: [参考：在Ubuntu 16.04 LTS上安装Postfix邮件系统，配置发送邮件服务器](https://www.jianshu.com/p/78e2e1914ec1)

```bash
# 下载Postfix
sudo apt install mailutils
sudo apt install --reinstall --purge postfix
# 全部选Y
Do you want to continue?[Y/n]

# Postfix需要在配置中监听loopbackinterface，打开主Postfix配置文件：
sudo vim /etc/postfix/main.cf
inet_interfaces = all -> inet_interfaces = loopback-only
sudo service postfix restart

# 指定主机名
sudo hostnamectl set-hostname vice.thoughts.vip

# 测试发送邮件
echo "测试邮件正文" | mail -s "邮件标题" your_email_address

# 改变发件地址，打开Postfix配置文件：
sudo vim /etc/postfix/main.cf

# 修改
myhostname = vice.thoughts.vip

# 在末尾添加
# smtp_generic_maps = hash:/etc/postfix/generic

sudo vim /etc/postfix/generic
# 添加以下内容 你的用户名@你的服务器名字   你希望对方看到的电子邮件地址
# 这里是
# ubuntu@VM-0-11-ubuntu Thoughts-Robot@phabricator.thoughts.vip

sudo service postfix restart

# 配置Phabricator
cd ~/software/phacility/phabricator/conf/local
vim local.json
# 添加如下内容
"cluster.mailers": [
    {
      "key": "Thoughts-Robot",
      "type": "sendmail"
    }
  ]

```

## 重启Phabricator

:link: [官方操作手册](https://secure.phabricator.com/book/phabricator/article/restarting/)

Phabricator的设置和配置说明有时需要重新启动服务器进程，尤其是在进行配置更改之后。

通常，需要重新启动提供HTTP请求的任何内容以及提供PHP请求的任何内容。在某些情况下，这些将是相同的过程并使用一个重启命令处理。在其他情况下，它们将是两个不同的进程，并使用两个不同的重启命令进行处理。

:exclamation: 如果有两个不同的进程（例如，nginx和PHP-FPM），则需要发出两个不同的重新启动命令。

重启HTTP服务器和PHP服务器非常重要，因为每个服务器都会缓存不同的配置和设置。进行更改后重新启动两台服务器可确保系统运行最新配置。

```bash
$ sudo service nginx restart
$ sudo service php7.2-fpm restart
```

## 清除缓存

如果需要清除Phabricator的缓存，可以使用CLI工具。使用`--help`标志运行它以查看选项：

```
phabricator / $ ./bin/cache purge --help
```

此工具可以以细粒度方式清除缓存，但通常最简单的方法是清除所有缓存：

```
phabricator / $ ./bin/cache purge --purge-all
```

可以安全地清除缓存。如果Phabricator需要，它们包含的数据总是可以从其他数据重建。

## 其它操作



[删除Phabricator项目或任务](https://stackoverrun.com/cn/q/4550345)

