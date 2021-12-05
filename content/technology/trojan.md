+++
title = "终端使用 Trojan 代理"
date = 2021-12-05T09:39:25+08:00
draft = false
indent = false
initial = false
toc = false
+++

## 安装 Trojan

这里使用 ArchLinux 作为演示系统.

```bash
sudo pacman -S trojan
```

```bash
sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/trojan-gfw/trojan-quickstart/master/trojan-quickstart.sh)"
```

```bash
sudo bash -c "$(wget -O- https://raw.githubusercontent.com/trojan-gfw/trojan-quickstart/master/trojan-quickstart.sh)"
```

以上三种任选一种, 其中 第一种 为 ArchLinux 所特有.

## 修改 Trojan 配置文件

```toml
{
    "run_type": "client",
    "local_addr": "127.0.0.1",
    "local_port": 1080,
    "remote_addr": "example.com",
    "remote_port": 443,
    "password": [
        "password1"
    ],
    "log_level": 1,
    "ssl": {
        "verify": true,
        "verify_hostname": true,
        "cert": "",
        "cipher": "ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES128-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:AES128-SHA:AES256-SHA:DES-CBC3-SHA",
        "cipher_tls13": "TLS_AES_128_GCM_SHA256:TLS_CHACHA20_POLY1305_SHA256:TLS_AES_256_GCM_SHA384",
        "sni": "",
        "alpn": [
            "h2",
            "http/1.1"
        ],
        "reuse_session": true,
        "session_ticket": false,
        "curves": ""
    },
    "tcp": {
        "no_delay": true,
        "keep_alive": true,
        "reuse_port": false,
        "fast_open": false,
        "fast_open_qlen": 20
    }
}
```
- `remote_addr` 你的服务器域名
- `remote_port` 你的服务器监听端口 一般为 443
- `password1` 你的服务器上的 trojan 设置的密码
- `local_addr` 默认 127.0.0.1
- `local_port` 你本地系统上开放的端口 我设置为 1080 (这个比较重要!)
- `run_type` 你的 trojan 服务类型 此处为客户端

## 安装 proxychains

```bash
sudo pacman -S proxychains-ng
```

## 配置 proxychains

```bash
sudo vim /etc/proxychains.conf
```

修改文件末尾 `ProxyList` 中的 `socks4 127.0.0.1 9050` 为 `socks5 127.0.0.1 1080`

这里的 `1080` 就是你 trojan 配置中开放的本地端口

## 启动 trojan

```bash
sudo systemctl start trojan
```

查看 trojan 是否启动成功, 有 active (running) 说明启动成功了

```bash
sudo systemctl status trojan
```

## 验证终端是否可以使用代理

```bash
proxychains curl ip.gs
```

如果启动成功,那么他会返回你的服务器的 ip 地址.

## 如何使用?

在每一条命令行之前都加上 `proxychinas` 就是都走代理,不加上就是走本地直连. 当让你可以设置一个 `alisa` 方便自己:

```bash
vim ~/.bashrc
# 添加
alisa p="proxychains"
```
