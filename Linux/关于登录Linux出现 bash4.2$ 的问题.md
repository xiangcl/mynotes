---
title: 关于登录Linux出现 bash4.2$ 的问题
tags:
  - Linux
  - Linux问题集
categories: Linux问题宝典
permalink: linux-collection/linux-answer-two
---

## 问题

创建用户时出现`bash-4.2$`

### 如图所示

![](http://i.imgur.com/jW7CWxs.png)

## 环境

![](http://i.imgur.com/CotossG.png)


## 问题分析

通过`useradd`方式创建新用户时，都会将所有的配置文件从`/etc/skel`到`/home`目录的新用户录下。但现在没有创建默认的文件。

## 解决方案

- root远程登录
- 手动创建xiangcl的家目录
        `# mkdir /home/xiangcl
        # chown xiangcl:xiangcl /home/xiangcl
        # chmod 700 /home/xiangcl`
- 将`/etc/skel`这个目录的文件复制到`/home/xiangcl`中
       ` # cd /etc/skel/
        # ls -a
        . ..  .bash_logout  .bash_profile .bashrc  .mozilla
        # cp .bash_logout  /home/xiangcl/
        # cp .bash_profile  /home/xiangcl/
        # cp .bashrc  /home/xiangcl`
