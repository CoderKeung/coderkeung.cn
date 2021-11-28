+++
title = "博客自动部署"
date = 2021-08-31T09:18:56+08:00
draft = false
toc = true
+++

## 前言

<!--more-->
在此之前，我习惯用 Notion 记录博客，Notion是一个十分便捷且高效的笔记工具，依托块布局，Notion 比其他笔记有太多的优点，但当我希望用它记录博客文章的时候，这类的排版又让人很不适应，故，我转投了 Hugo 的怀抱。为了让博客更加简单，我选择将博客源文件用 Github 保存，同时部署到自己服务器。而这就面临着「自动部署」的问题，对于这个问题，在昨天半天的学习下，初有成效，记录之。

这段文字的前提是，我已经会使用 Hugo 进行博客的部署，并且了解 Hugo 生成的静态网页位于博客文件夹中的 Public 文件夹。且已经将博客文件使用 Github 存储。

## 服务器端的操作

在服务器端安装好相应版本的 [Hugo](https://gohugo.io/getting-started/installing)。Hugo 有普通版，以及添加一些拓展支持的拓展版（Extended），如果你需要使用 Sass/SCSS 的话，就下载 “Extended”版本的 Hugo 。

拉取博客仓库到服务器，你需要确定好自己需要使用哪个路径作为你的网站根目录。（例如我的就是 “/root/coderkeung.cn/public” ），确定好之后便可以使用 Hugo 生成静态网页。这里需要说明一下，因为我直接拉取我的仓库文件到 /root 目录之下，所以我的博客文件夹是 /root/coderkeung.cn ，而 Hugo 生成的网页文件是在博客文件夹下的子目录 public 中，所以才会有博客网站根目录为 /root/coderkeung.cn/public。

```bash
# 所处的位置 /root 我的博客仓库为：https://github.com/CoderKeung/coderkeung.cn
> git clone https://github.com/CoderKeung/coderkeung.cn
> cd coderkeung.cn
> pwd
> /root/coderkeung.cn
```

之后用 Hugo 在我们的博客文件夹下生成静态网页文件：

```bash
> hugo
# 之后 Hugo 会在目录下生成一个 public 子目录，里面就是博客网站所需要的网页文件，同时 /root/coderkeung.cn/public 就是我的博客网站根目录（上文提过，再提一遍）
```

在我们生成博客网页文件之后，剩下的就是让我们的服务器能够接收到 Github 仓库的更新，然后自动拉取仓库更新并对服务器内的博客文件夹进行更新部署

针对这个需求，Github 为我们提供了一个名为 Webhook 的好东西：

> Webhooks allow you to build or set up integrations, such as [GitHub Apps](https://docs.github.com/en/apps/building-github-apps) or [OAuth Apps](https://docs.github.com/en/apps/building-oauth-apps), which subscribe to certain events on GitHub.com. When one of those events is triggered, we'll send a HTTP POST payload to the webhook's configured URL. Webhooks can be used to update an external issue tracker, trigger CI builds, update a backup mirror, or even deploy to your production server. You're only limited by your imagination.

这里非常明确的告诉我们，Webhoook 的作用是在我们的 Github 仓库发生某些变化时（更新事件）后，向我们自己配置的 URL 发送一个 HTTP POST 请求，而我们要做的就是让我们的服务器接收这个请求并做出反应。

首先创建一个 shell 脚本用于执行我们服务器在接收到 POST 之后需要干的事情，文件如下：

```bash
# 文件名： gitPull.sh 存放目录：/root/webhook/gitPull.sh
cd /root/coderkeung.cn/ # 打开我们的博客目录
git pull origin main # 更新目录
hugo # 生成网页文件
exit 0 # 退出
```

之后我们需要创建一个接受 POST 的小脚本，这边使用 Nodejs 编写：

```js
// 文件名：githubWebhook.js 存放目录： /root/webhook/githubWebhook.js
var http = require('http')
var exec = require('child_process').exec
var createHandler = require('github-webhook-handler')
// 在这里面 path 和 secret 都是 Github Webhook 所需要的，在下文中的 Github 设置中需要
var handler = createHandler({ path: '/webhook', secret: 'coderkeungcn' })
http.createServer(function (req, res) {
handler(req, res, function (err) {
    res.statusCode = 404
    res.end('no such location')
  })
}).listen(8888) // 这边使用监听的事 8888 端口，可以根据自己需要更改

handler.on('push', function (event) {
    let currentTime = new Date();
    console.log('\n--> ' + currentTime.toLocaleString());
    console.log('Received a push event for %s to %s', event.payload.repository.name, event.payload.ref);
    exec('sh ./gitPull.sh', function (error, stdout, stderr) {
        if(error) {
            console.error('error:\n' + error);
            return;
        }
        console.log('stdout:\n' + stdout);
        console.log('stderr:\n' + stderr);
    });
})
```

当做完这些，再来查看一下自己的目录结构：

```bash
> cd /root
> tree
/root
├── coderkeung.cn
│   ├── archetypes
│   ├── assets
│   ├── config.toml
│   ├── content
│   ├── public
│   ├── resources
│   ├── static
│   └── themes
└── webhook
    ├── githubWebhook.js
    └── gitPull.sh
```

在这时候，需要确保服务器安装 Nodejs 和 npm 包管理器两个工具：

```bash
# Ubuntu Install npm
> apt install npm
# 随后只用 npm 在 /root/webhook/ 目录下安装 pm2 进程管理器
> cd /root/webhook
> npm i pm2
# 通过 pm2 启动我们的 Nodejs 应用 即是 /root/webhook/githubWebhook.js
> ./node_modules/pm2/bin/pm2 startup ./githubWebhook.js
> ./node_modules/pm2/bin/pm2 status
```

在做完这些之后，就是配置 Nginx 配置文件：

```yaml
# 我的配置文件，仅供参考
server {
  listen 443 ssl;
  listen [::]:443 ssl;

# 配置 Https
  ssl_certificate       /root/vpn/coderkeung.cn.crt;
  ssl_certificate_key   /root/vpn/coderkeung.cn.key;
  ssl_session_timeout 1d;
  ssl_session_cache shared:MozSSL:10m;
  ssl_session_tickets off;

  ssl_protocols         TLSv1.2 TLSv1.3;
  ssl_prefer_server_ciphers off;

  server_name           coderkeung.cn;

  location / {
      root   /root/coderkeung.cn/public;
      index  index.php index.html index.htm;
        }
   # NodeJS 将 Web 服务跑在了 8888 端口，我们可以用 Nginx 反向代理到 80 端口
   location /webhook {
     alias /root/webhook;
     proxy_pass http://127.0.0.1:8888;
   }
   error_page 404 /404.html;
       location = /40x.html {
   }

   error_page 500 502 503 504 /50x.html;
       location = /50x.html {
   }


}

server {
#    listen       80 default_server;
#   listen       [::]:80 default_server;
    listen     80;
    server_name  coderkeung.cn;
    return 301 https://$host$request_uri;

    # Load configuration files for the default server block.
    # include /etc/nginx/default.d/*.conf;

    location / {
       root   /root/coderkeung.cn/public;
       index index.html index.htm index.php;
       autoindex on;
    }
}
```

如此下来，服务器端配置完成！

## Github 设置

在存储的 Github 仓库的设置选项中找到 Webhooks 并添加新的 webhook：

![webHook-1](/img/webHook-1.jpg)

对添加的 webhook 进行相应的设置：

![webhook-2](/img/webhook-2.jpg)

![webhook-3](/img/webhook-3.jpg)

如此下来，在本地更改博客文件和更新文件，只要推送到 Github 仓库，便会同步部署到服务器啦！
