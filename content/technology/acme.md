+++
title = "使用 acme.sh 申请 TLS 证书"
date = 2021-11-28T02:19:23+08:00
draft = false
+++

## acme.sh 简单介绍
**acme.sh** 实现了 `acme` 协议, 可以从 `letsencrypt` 生成免费的证书。[^acme]
而 `letsencrypt` 就是一个数字证书认证：
> Let's Encrypt是一个于2015年三季度推出的数字证书认证机构，旨在以自动化流程消除手动创建和安装证书的复杂流程，并推广使万维网服务器的加密连接无所不在，为安全网站提供免费的传输层安全性协议（TLS）证书。[^letsencrypt]
## 安装 acme.sh
使用官方脚本进行安装：
```bash
curl  https://get.acme.sh | sh -s email=my@example.com
```
让我们 `acme.sh` 命令生效
```bash
. .bashrc
```
> 安装过程不会污染已有的系统任何功能和文件, 所有的修改都限制在安装目录中: ~/.acme.sh/。[^acme]

## 确保你的网站成功配置 Nginx
在我们使用 `acme.sh` 申请一个证书的时候，我们必须确保我们自己的 `Nginx` 配置正确，并且域名成功解析到我们的服务器。也就是通过域名在浏览器能够访问你的服务器。
使用 `acme.sh` 验证你的服务器是否正确配置：
- 下面的 `www.mudomain.com` 修改为你的二级域名，`/home/wwwroot/mydomain.com/` 修改为你的网站根目录。
```bash
acme.sh  --issue --test  -d www.mydomain.com  --webroot  /home/wwwroot/mydomain.com/
```
如果提示 `Please update your account with an email address first.` 那么我们就要更新一下我们的邮箱：
```bash
acme.sh --register-account -m my@example.com
```
最后出现 `Cert success.` 说明配置成功了。
> 只需要指定域名, 并指定域名所在的网站根目录. acme.sh 会全自动的生成验证文件, 并放到网站的根目录, 然后自动完成验证. 最后会聪明的删除验证文件. 整个过程没有任何副作用。[^acme]
## 开始申请证书
设置默认服务商并开始申请「**删除 --test 选项**」：
```bash
acme.sh --set-default-ca --server letsencrypt
acme.sh  --issue --test  -d www.mydomain.com  --webroot  /home/wwwroot/mydomain.com/ --keylength ec-256 --force
```
在上面涉及两个选项，`--keylength` 表示使用的 密钥交换算法，俺也不知道，就是大佬觉得厉害，我就用了。[^xray]
> `--force` 参数的意思就是，在现有证书到期前，手动（强行）更新证书。上一步我们从“测试服”申请的证书虽然不能直接用，但是它本身是尚未过期的，所以需要用到这个参数。[^xray]
## 安装证书
创建一个存放证书的文件夹（确保 nginx 有访问权限，至少为 `x` 权限）：
```bash
mkdir ~/Cert
```
使用 `acme.sh` 拷贝证书到文件夹：
```bash
acme.sh --install-cert -d  www.mydomain.com --ecc \
            --fullchain-file ~/Cert/your.crt \
            --key-file ~/Cert/your.key
```
## 在 Nginx 配置中合理配置证书
一般情况下，nginx 的配置如下：
```bash
ssl_certificate       /home/userName/Cert/your.crt;
ssl_certificate_key   /home/userName/Cert/your.key;
```
然后就是开启 HTTP 自动跳转 HTTPS，在 80 端口的服务下删除 `root` 和 `index`，并添加如下一条配置：
```bash
return 301 https://$http_host$request_uri;
```

[^acme]: [acme.sh github 中文说明](https://github.com/acmesh-official/acme.sh/wiki/%E8%AF%B4%E6%98%8E)
[^letsencrypt]: [Let's Encrypt Wikipedia](https://zh.wikipedia.org/wiki/Let's_Encrypt)
[^xray]: [小小白白话文【第 6 章】证书管理篇 ](https://xtls.github.io/document/level-0/ch06-certificates.html#_6-2-%E5%AE%89%E8%A3%85-acme-sh)
