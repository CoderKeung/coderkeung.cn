+++
title = "Virtualbox 中安装 Arch Linux 后的增强功能"
date = 2021-12-05T10:21:48+08:00
draft = true
indent = false
initial = false
toc = true
+++

## 安装 `virtualbox-guest-utils`

```bash
sudo pacman -S virtualbox-guest-utils
```

## 启动 `vboxservice.service`

```bash
sudo systemctl start vboxservice.service
```

这样当你 启动 xorg 窗口的时候,你的显示分辨率就会随心意变化了.
