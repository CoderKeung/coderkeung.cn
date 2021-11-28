+++
title = "安装 Chevereto 图床"
date = 2020-12-02T20:02:09+08:00
draft = false
indent = false
initial = false
toc = true
+++

## 安装系统必要软件

更新系统软件（Ubuntu20.4）

```bash
sudo apt update && sudo apt upgrade
```

安装 `MySQL` 、 `Nginx`

```bash
sudo apt install nginx mysql-server
```

创建一个数据库、数据库用户、访问所需的密码并赋予权限

```bash
# 进入Mysql命令行控制台
sudo mysql -u root
# 创建一个新的数据库，Name is： chevereto
CREATE DATABASE chevereto;
# 创建用户并设置密码
CREATE USER 'chevereto' IDENTIFIED BY 'enter_a_password_here';
# 赋予权限
GRANT ALL PRIVILEGES ON * . * TO 'chevereto'@'%';
```

MySQL安全安装

```bash
sudo mysql_secure_installation
# 针对问题使用如下方式
Set root password? [Y/n] n
Remove anonymous users? [Y/n] y
Disallow root login remotely? [Y/n] y
Remove test database and access to it? [Y/n] y
Reload privilege tables now? [Y/n] y
```

安装 `PHP`

```bash
sudo apt install php-fpm php-zip php-curl php-mbstring php-gd php-mysql
```

创建并编辑 `chevereto.ini` 文件

```bash
# 使用 neovim 编辑文件, 注意下php的版本号（7.4），可能不同
sudo nano /etc/php/7.4/fpm/conf.d/chevereto.ini
# 将一下内容粘贴进入文件
upload_max_filesize = 20M;
post_max_size = 20M;
max_execution_time = 30;
memory_limit = 512M;
```

## 安装图床

创建网站文件夹路径，并未 `www-data` 设置好用户权限



```bash
sudo mkdir -p /var/www/html/base.tsiang.cc
sudo chown www-data:www-data /var/www/html/base.tsiang.cc
```

使用 `nvim` 编辑器创建网站配置文件

```bash
sudo nvim /etc/nginx/conf.d/base.tsiang.cc.conf
```

以下为网站配置文件内容：

```bash
server {
   listen 443 ssl;
   listen [::]:443 ssl;
   root /var/www/html/base.tsiang.cc;
   ssl_certificate       /root/base.tsiang.cc.crt;
   ssl_certificate_key   /root/base.tsiang.cc.key;
   ssl_session_timeout 1d;
   ssl_session_cache shared:MozSSL:10m;
   ssl_session_tickets off;

   ssl_protocols         TLSv1.2 TLSv1.3;
  # ssl_ciphers           ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY13      05:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
   ssl_prefer_server_ciphers off;

   server_name           base.tsiang.cc;
   location / {
     root /var/www/html/base.tsiang.cc;
     index index.php index.html index.htm;
     try_files $uri $uri/ /index.php$is_args$query_string;
     }
   location ~.*\.php(\/.*)*$ {
     include /etc/nginx/fastcgi.conf;
     fastcgi_pass unix:/run/php/php7.4-fpm.sock;
     fastcgi_param   SCRIPT_FILENAME    $document_root$fastcgi_script_name;
     fastcgi_param   SCRIPT_NAME        $fastcgi_script_name;
   }
   location /work { # 与 V2Ray 配置中的 path 保持一致
     if ($http_upgrade != "websocket") { # WebSocket协商失败时返回404
         return 404;
     }
     proxy_redirect off;
     proxy_pass http://127.0.0.1:8889; # 假设WebSocket监听在环回地址的10000端口上
     proxy_http_version 1.1;
     proxy_set_header Upgrade $http_upgrade;
     proxy_set_header Connection "upgrade";
     proxy_set_header Host $host;
     # Show real IP in v2ray access.log
     proxy_set_header X-Real-IP $remote_addr;
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
   }
    # Context limits
    client_max_body_size 20M;

    # Disable access to sensitive files
    location ~* (app|content|lib)/.*\.(po|php|lock|sql)$ {
        deny all;
    }

    # Image not found replacement
    location ~ \.(jpe?g|png|gif|webp)$ {
        log_not_found off;
        error_page 404 /content/images/system/default/404.gif;
    }

    # CORS header (avoids font rendering issues)
    location ~ \.(ttf|ttc|otf|eot|woff|woff2|font.css|css|js)$ {
        add_header Access-Control-Allow-Origin "*";
    }
}
server {
   listen 80;
   server_name base.tsiang.cc;
   return 301 https://$host$request_uri;
   location / {
       root   /var/www/html/base.tsiang.cc;
       index  index.php index.html index.htm;
   }
   location ~.*\.php(\/.*)*$ {
       include /etc/nginx/fastcgi.conf;
       fastcgi_pass 127.0.0.1:9000;
   }

}
```

重启 `PHP` 和 `Nginx`

```bash
sudo systemctl restart php7.4-fpm
sudo systemctl restart nginx
```

将`chevereto` 的安装文件下载下来，在浏览器中访问这个文件并按照流程操作

```bash
sudo -u www-data wget -O /var/www/html/base.tsiang.cc/installer.php https://chevereto.com/download/file/installer
```

这个图床的作用其实不言而喻，通过我的 `Nginx` 配置就可以发现一些端倪，我就不细说，懂得都懂。
