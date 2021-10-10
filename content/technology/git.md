+++
title = "学习Git"
date = 2021-10-09T16:48:00+08:00
draft = false
+++

- **Git配置文件目录**

```bash
# --system 写入系统文件目录 /etc/gitconfig 
# --global 写入本地文件目录 ~/.gitconfig 或 ~/.config/git/config 
# --local  写入本地仓库目录 
git config --list --show-origin 
```

- **设置用户信息**

```bash
git config --global user.name "UserName"
git config --global user.email "UserEmail@gmail.com"
```
	
- **设置默认打开文本编辑器**

```bash
git config --global core.editor nvim
```

- **检查配置**

```bash
git config --list
```

- **精简检查Git状态**

```bash
git status -s # git status --short
```

- **忽略文件**
	- 仓库目录中创建  `.gitignore` 文件，语法如下：
	-  [所有语言的.gitignore文件](https://github.com/github/gitignore) 

```bash
# 注释内容与空行被自动忽略
*.a # 忽略所有以 .a 为后缀的文件
!lib.a # 跟踪所有 lib.a 文件
/LIB # 只忽略当前目录的 LIB 文件
LIB/ # 忽略所有的 LIB 目录
# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt
# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
```

- **查看将要提交的修改**
	- `git diff` 本身只显示尚未暂存的改动

```bash
# git diff --cached
git diff --staged
```

- **删除文件**

```bash
# 强制删除Git跟踪的文件
git rm -f FILE
# 删除Git跟踪的文件
git rm FILE # rm FILE & git add FILE
# 删除Git跟踪的文件但是保留本地文件
git rm --cached FILE
```

- **重命名文件**

```bash
git mv FILE FILENEW
# 也可以直接使用如下方式
mv FILE FILENEW & git add .
mv FILE FILENEW & git add FILE FILENEW
```

- **查看提交记录**
	- [常用格式](https://git-scm.com/book/zh/v2/ch00/pretty_format) 

```bash
# 普通查看
git log
# 显示修改
git log -p/--patch
# 显示最近两条
git log -2
# 简略信息
git log --stat
# 添加输出格式
git log --pretty=PAEAMETERS
# 自定义输出格式
git log --pretty=format:""
```

- **撤销提交信息**

```bash
git commit --amend
```

- **多个远程仓库**

```bash
# 查看远程仓库
git remote
# 查看远程仓库详情
git remote -v
# 添加远程仓库
git remote add REPONAME REPOURL
# 重命名
git remote rename REPONAME NEWREPONAME
# 删除
git remote remove REPONAME
# 拉取另一个仓库中的修改
git fetch REPONAME
# 推送到远程仓库
git push REPONAME main
# 查看远程仓库详细信息
git remote show REPONAME
```
- **标签Tag**

```bash
# 创建附注标签
git tag -a TAGNAME
# 创建轻量标签
git tag TAGNAME
# 后期打标签
git tag -a TAGNAME HASHVALUE
# 共享标签到仓库
git push REPONAME TAGNAME
# 删除本地标签
git tag -d TAGNAME
# 删除仓库标签
git push REPONAME --delete TAGNAME
```

- **分支**

```bash
# 创建分支
git branch BRANCHNAME
# 切换分支
git checkout BRANCHNAME
# 创建并切换
git checkout -b BRANCHNAME
# 删除本地分支
git branch -d BRANCHNAME
# 删除远程分支
git push REPONAME --dalete BRANCHNAME
# 提交分支
git push REPONAME BRANCHNAME
# 查看分支信息
git log --oneline --decorate
# 图形化查看分支信息
git log --oneline --decorate --graph --all
# 合并分支
git merge BRANCHNAME
# 查看完成合并分支
git branch --merged
# 查看未完成合并分支
git branch --no-merged
```

- **远程分支**

```bash
# 添加远程分支
git remote add REMOTENAME REMOTEURL
# 更新远程分支
git fetch REMOTENAME
# 创建远程分支的本地分支
git checkout -b BRANCHNAME REMOTENAME/BRANCHNAME
# 合并远程分支到本地分支
git merge BRANCHNAME
# 拉取远程分支
git pull = git fetch REMOTENAME/BRANCHNAME & git merge BRANCHNAME
```











