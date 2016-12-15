---
title: 解决：sudo useradd xxx`时，出现`xxx is not in the sudoers file. This incident will be reported.
tags:
  - Linux
  - Linux问题集
categories: Linux问题宝典
permalink: linux-collection/linux-answer-one
---

## 问题描述

使用`sudo useradd xxx`时，出现`xxx is not in the sudoers file. This incident will be reported.`

### 如图所示

![](http://i.imgur.com/95zsyZz.png)

## 当前环境

![](http://i.imgur.com/BYICogz.png)

## 问题分析

用户没有加入到sudo的配置文件里，可以通过编辑sudoers文件，来解决这个问题。

## 解决方案

1. 切换到root用户
2. 添加sudo文件的写权限
    `chmod u+w /etc/sudoers`
1. 编辑sudoers文件
    `vim /etc/sudoers`
    `找到 root ALL=(ALL) ALL,在他下面添加xxx ALL=(ALL) ALL (这里的xxx是你的用户名)`
            youuser            ALL=(ALL)                ALL
            youuser           ALL=(ALL)                ALL
            youuser            ALL=(ALL)                NOPASSWD: ALL
            youuser           ALL=(ALL)                NOPASSWD: ALL
    - 第一行:允许用户youuser执行sudo命令(需要输入密码).
    - 第二行:允许用户组youuser里面的用户执行sudo命令(需要输入密码).
    - 第三行:允许用户youuser执行sudo命令,并且在执行的时候不输入密码.
    - 第四行:允许用户组youuser里面的用户执行sudo命令,并且在执行的时候不输入密码.
1. 撤销sudoers的文件的写权限
`chmod u-w /etc/sudoers`


