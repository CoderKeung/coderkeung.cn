+++
title = "MacOS上使用npm的正确姿势"
date = 2020-11-28T20:04:36+08:00
draft = false
indent = false
initial = false
toc = false
+++

使用 `brew`  安装 `node` :

```bash
brew update
brew install node
```

如果已经安装了 `node`，选择卸载删除它：

```bash
rm -rf /usr/local/lib/node_modules
brew uninstall node
brew uninstall yarn # 如果安装了
```

使用命令查看软件是否安装成功：

```bash
node -v
npm -v
```

为我们的 npm 新建一个你需要它安装软件的目录（我新建在家目录之下）：

```bash
mkdir ~/.npmGlobal
```

在家目录之下新建 `npm` 的配置文件 `.npmrc`：

```bash
touch ~/.npmrc
```

编辑配置文件：

```bash
vim ~/.npmrc
# 添加如下一行配置
prefix=~/.npmGlobal # 你需要 npm 安装软件的文件夹，最好是家目录之下，防止权限报错
```

将 `npm` 的安装软件目录 （`~/.npmGlobal`）添加环境变量：

```bash
# 我使用的是zsh，故编辑 ~/.zshrc
vim ~/.zshrc

export PATH=$HOME/.npmGlobal/bin:$PATH
```

使用如下命令安装软件：

```bash
npm install -g npm
```

一切归于平静...
