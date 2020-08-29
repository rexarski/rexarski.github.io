---
title: "如何部署一台数据科学专用的简易服务器"
date: 2019-01-12T16:52:40+10:00
slug: "deploy-a-server-for-data-science"
description: ""
keywords: ["server"]
draft: false
tags: ["conda", "mysql", "linux"]
math: false
toc: true
---

主要是两个部分，一个是 conda，一个是 MySQL.

## 准备

1. 在[Vultr](https://www.vultr.com/?ref=7316917)上购置一台服务器，选择\$5/月的配置就已经够了，系统选择 CentOS。当然了，Ubuntu 也是极好的。
2. 使用`ssh`连接服务器，ip 地址以及临时密码都在 Vultr 的 dashboard 里可以查阅。

## conda

1. 安装[Anaconda distribution](https://www.anaconda.com/download/#linux)。因为服务器端没有图形界面，一切都在我们的 terminal 里完成。在服务器中运行`wget https://repo.continuum.io/archive/Anaconda3-2018.12-Linux-x86_64.sh` 之前服务器选择的是 64 位系统，这里就从 conda 的页面找到对应的 64 位系统下载地址。
2. 由于 CentOS 没有`bzip2`，此时直接安装则会报错，应先使用`yum install bzip2`安装。
3. 接着使用`bash Anaconda3-*.*.*-Linux-x86_64.sh`安装 anaconda. 依照提示，都很直白。
4. 默认安装路径会在`~`之下，安装完成后把安装路径添加到环境变量里。
5. 如果此时没有文本编辑器的话，建议`yum install vim`，临时抱佛脚学习使用一下。
6. 使用`vim ~/.bashrc`打开环境变量储存文件`.bashrc`. Vim 主要有两种模式，一种是插入模式 (insert mode)，一种是指令模式 (command mode)。当前我们处于指令模式下，按`i`键切入到 insert mode，然后在文件末端录入代码`export PATH=“$HOME/anaconda3/bin:$PATH”`. 按`Esc`键退出到 command mode，录入`:wq`保存并退出，此时光标会出现在 vim 的最后一行。推出之后，记得`source ~/.bashrc`一下。然后我们就可以用 conda 的指令来检查 conda 是否成功安装，或者安装其他的 package。
7. 此时 conda 的安装告一段落，接下来是 MySQL 数据库的部署。

## MySQL

1. 在[MySQL 官网](https://dev.mysql.com/downloads/repo/yum/)寻找 yum 源，类似的方法，找到链接就行。这里因为我们用的是 CentOS，使用 Red Hat Enterprise Linux 7 版本。`yum localinstall https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm`
2. `yum install mysql-community-server -y`, 并用`mysqld --version`查看是否安装成功。
3. 配置 MySQL 使其自动启动`systemctl enable mysqld.service`.
4. 启动`systemctl start mysqld.service`.
5. 对于 5.7 以上版本的 MySQL，还需要以下的方法修改默认 root 用户的登录密码。第一步是先找到临时密码：`sudo grep 'temporary password' /var/log/mysqld.log`
6. `mysql -u root -p`后接 temporary password 进入 MySQL.
7. MySQL 现在的密码要求规则比较复杂，如果自己想设置简单密码，则需要`SET GLOBAL validate_password.policy=LOW;`.
8. 最后修改密码`ALTER USER 'root'@'localhost' IDENTIFIED BY 'mynewpassword';`
9. `\q`退出 MySQL, `exit`退出 ssh 连接。

到此为止就基本能用了，然后就是 develop, play, and enjoy.
