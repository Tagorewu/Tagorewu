---
title: 'git 中心服务搭建'
date: Wed, 22 Aug 2018 12:47:34 +0000
draft: false
categories:
  - "Software"
tags: ['git', 'Software Design']
---

#### 中心服务方式选择

git 本身是一个分布式的版本管理系统，但如果要设置一个中心库方便很多开发者同步，或者像SVN 一样使用它，就需要搭建一个中心库。有几种方式可以选择：

*   gitosis ：  
    这个是比较老的方式。不推荐  
    详情参考： [https://git-scm.com/book/en/v1/Git-on-the-Server-Gitosis](https://git-scm.com/book/en/v1/Git-on-the-Server-Gitosis)

*   GitLab:   
    git 结合web 服务来管理，方便issue 和权限管理。比较推荐。收费版还可提供更多功能。参考：[https://about.gitlab.com/install/](https://about.gitlab.com/install/)
*   只用ssh git 用户管理  
    开一个git 用户，设定好权限，也比较方便。但是缺少管理issue 功能。  
    参考：[https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server](https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server)

#### ssh git用户设置

*    ssh key证书生成

```
$ ssh-keygen –t rsa –C “user@host”
```

 将id\_rsa.pub 发给git服务器管理员添加进/home/git/.ssh/authorized\_keys 即可，或者直接用 ssh-copy-id 到服务器即可。

```
$ git clone git@IP:/srv/git/test.git
```

有几个注意点：

*   使用 ssh key 登陆 git 用户时，home目录只能是git 可写的，否则当git组包含多个用户时会出现不用能ssh key 登录的问题。具体debug ssh key 登录问题，可以查看 /var/log/auth 下的日志来解决。.ssh/ 的权限是700，.ssh/authorized\_keys 权限是600。
*   最后用chsh 修改 git用户shell 为 git-shell，不让git 用户有其它多余的权限。
*   创建仓库可以用git 组的其它用户来创建。注意使用newgrp 将创建文件夹时用户的默认组改成git, 这样整个git 组的用户都有读写权限。

#### 一些实用的git 命令记录

```
git checkout --patch BRANCH FILE 
git checkout --theirs PATH/FILE
git clean -n //演习
git ls-files
git ls-files | xargs -n 1 dirname | uniq
git diff master sync --name-only
git config --global core.autocrlf true
git branch -vv
git push origin  local\_branch:remote\_branch
git init --bare

# 添加组
usermod -a -G git tagore
# 更改home 目录
usermod -d /home/git/ -m git

# 更改git 用户 shell
sudo chsh git -s $(which git-shell)
cp /usr/share/doc/git/contrib/git-shell-commands /home/git -R
# 提交分支
git push origin local\_branch:remote\_branch
# 同步远程已删分支 prune
git fetch -p

# git 重命名本地分支，并提交到远程
1.重命名 git branch -m oldBranchName newBranchName
2.删除远程分支：git push origin :oldBranchName 
3.将重命名过的分支提交：git push origin newBranchName

# 中文文件名乱码
git config –global core.quotepath false

# 打tag
git tag -a v1.4 -m 'my version 1.4'
git push --tags

# 修订历史实用命令
git commit --amend
git rebase -i
git commit --author "Tagore Wu <tagorewu@gmail.com>"
```

git 相关使用教程可以看[这里](https://git-scm.com/book/en/v2)  

#### git 代理设置

有时候从github 上面 clone 东西很慢，这就要用上你的翻墙工具，然后设置git 的代理即可解决。  
  
注：git 不会使用全局的代理设置： eg: http\_proxy=ip:port  

```
git config --global https.proxy https://yourproxyserverip:port
git config --global http.proxy http://yourproxyserverip:port 
git config --global core.gitproxy "git-proxy“
git config --global socks.proxy "localhost:1080“
git config --global url.https://github.com/.insteadOf git://github.com/
```