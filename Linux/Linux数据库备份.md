---
title: Linux数据库备份
tags:
  - Linux
  - 数据库备份
categories: Linux
permalink: linux/linux-database-backup
---

## 目标: 每隔1分钟,导出.sql,压缩,并按日期存储在/data 下,每分钟后删除.sql文件,每隔2分钟删除.tar.gz文件

## 知识: 定时任务 crontab , mysqldump 导出 , tar 打包压缩, 按日期创建文件 date

## 准备部分

1.建立mysqldump软链接(<font color=red>必须在~目录下建立软链接</font>)

    ln -s /usr/local/mysql/bin/mysqldump /usr/bin/mysqldump

2.将mysql.bak.sql导出备份到~目录下

    mysqldump -uroot -p123 -B mysql > ./mysql.bak.sql

3.将导出的mysql.bak.sql打包成.tar.gz

    tar zcvf mysql.bak.sql.tar.gz mysql.bak.sql

## 正式写shell脚本

1.新建bak.sh脚本文件

    vim bak.sh

往脚本里写的内容:

    #!/bin/bash
    cd /data
    rm -f *.sql
    
    old=`date -d '-2 minute' +%Y%m%d%H%M`
    tad=`date +%Y%m%d%H%M`
    
    /usr/local/mysql/bin/mysqldump -uroot -p123 -B mysql > ./$tad.sql
    
    tar zcf $tad.sql.tar.gz $tad.sql
    
    # -f是判断文件是否存在

    if [ -f /data/$old.sql.tar.gz ]
    then
    rm -rf /data/$old.sql.tar.gz
    fi



2.在/目录下创建一个data目录

    mkdir -p /data/

3.创建定时任务:

    crontab -e

4:编辑定时任务:

    */1  *  *  *  *   /data/bak.sh




