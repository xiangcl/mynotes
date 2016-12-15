---
title: 搭建Hexo博客
tags:
  - Hexo
  - 博客搭建
categories: 工具配置
permalink: tool-config/hexo-blog-build
---
之前用wordpress做了一段时间的Bolg，后来才了解到Hexo。Hexo的好处就是配置灵活，简约而且轻巧，不会像wordpress那么一个庞然大物。所以干脆就将博客搬到了Hexo，而且还有免费的 github page 空间可以用。

搭建调试过程历经三天多。中间踩了很多坑，现一一记录下来，供诸君参考。

写到一半，中间有很多忘了，后面有空再补上吧。

# 准备工作

## 服务器：
使用的是腾讯云主机，一块钱一个月。大家如果有还在上大学的亲戚或朋友也可以借他们的学生身份。[腾讯云学生优惠链接](https://www.qcloud.com/act/campus)，腾讯云是可以创建子帐号的，所以大家不比担心帐号的问题。

## 软件：

    Centos 7.0      //云主机的系统
    node  v6.2.0    //运行环境
    git 1.8.3.1     //必备软件
    MarkdownPad 2   //md写作工具
    Xhell 5         //linux的远程终端
    FileZilla v3.21.0   //FTP软件

# 安装

## 云主机

很简单，自行解决。

## Node环境

Nodejs中文官网：[http://nodejs.cn/](http://nodejs.cn/)

    # sudo yum install gcc gcc-c++  //安装编译软件
    # wget https://nodejs.org/dist/v6.2.0/node-v6.2.0.tar.gz    //下载源码
    # tar xzvf node-v6.2.0.tar.gz   //解压源码
    # cd node-v6.2.0/   //进入解压目录
    # ./configure   //编译&&安装
    # make
    # make install
    # node -v   //查看node版本
    # npm -v    //查看npm版本

## 安装Git

    $ sudo yum install git

## Hexo安装

Hexo中文官网：[https://hexo.io/zh-cn/](https://hexo.io/zh-cn/)（上面有详细教程）


    $ npm install -g hexo-cli

上面是官方的安装办法，我登了许久都没有反应，后来才知道可能是墙的原因。后来更换了[淘宝npm镜像](http://npm.taobao.org/)，安装方法：

    $ npm install -g cnpm --registry=https://registry.npm.taobao.org

因为更换了npm源所以在`npm`命令的前面都要加`c`,即为`cnpm`

再次安装Hexo：

    $ sudo cnpm install -g hexo-cli
    $ mkdir /home/blog
    $ cd /home/
    $ hexo init blog
    $ cd blog
    $ cnpm install
    $ hexo server

经过上面的操作后，会在本地`/home`下新建一个`/blog`的目录，并在该文件夹下生成所需的文件。

默认产生的目录结构如下：

    .
    |-- _config.yml
    |-- db.json
    |-- debug.log
    |-- node_modules
    |-- package.json
    |-- public
    |-- scaffolds
    |-- source
    `-- themes



| 目录          | 描述           |
| ------------- |:-------------:|
| _config.yol   | 网站的配置信息  |
| package.json  | 应用程序的信息  |
| publice       | 执行hexo generate命令，输出的静态网页内容目录 |
| scaffolds     | 模版文件夹      |
| source        | 资源文件夹      |
| themes        | 主题文件夹      |


## 基本命令


    hexo new "postName"     #新建名为postName的文章
    hexo new page "pageName"    #新建名为pageName页面
    hexo generate   #生成静态页面至public目录
    hexo server     #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
    hexo deploy     #将.deploy目录部署到GitHub

    hexo clean  #清楚缓存文件(`db.json`)和已生产的静态文件(public)
    hexo list <type>    #列出网站资料
    hexo version    #显示hexo版本



## 配置

基本的配置可以参考官网


## 先写这么多吧，后面的有点忘了，等有空补上




