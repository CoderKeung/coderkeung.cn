+++
title = "优化服务器登录安全"
date = 2021-11-28T02:27:02+08:00
draft = false
toc = true
+++
## 更改 SSH 登录端口
```bash
vim /etc/ssh/sshd_config
```
添加 `Port` 选项为 自定义端口。

```bash
systemctl restart sshd # CentOS 8
```
重启 ssh 服务，在服务器供应商处的后台防火墙将相应端口打开。
```bash
ssh root@hostname -p Port
```
之后使用新的端口进行登录。
## 添加非 ROOT 用户
```bash
useradd userName #创建用户
passwd userName #设置密码
```
随后对用户赋予临时 root 权限—— `sudo`
```bash
visudo
```
添加 `userName ALL=(ALL) NOPASSWD: ALL` 到 `root ALL=(ALL) ALL` 下。
## 禁用 ROOT 用户通过 SSH 登录
``` bash
vim /etc/ssh/sshd_config
```
找到 `PermitRootLogin yes` 并更改为 `PermitRootLogin no`，随后重启服务。
```bash
systemctl restart sshd # CentOS 8
```
