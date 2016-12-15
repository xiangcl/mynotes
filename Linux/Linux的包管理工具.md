---
  title: yum:CentOS包管理工具
  tags:
    - Linux
    - yum
    - CentOS
  categories: 工具配置
  permalink: tool-config/yum-centos-package-tool
---
##Linux系统的包管理工具


## 优点

1.自动处理依赖性关系

2.简化安装过程

## 包管理器的分类

- CentOS
    - yum
- Ubuntu
    - apt


## yum
    
### 语法
    
    yum [options] [command] [package ...]

### options

    -h：显示帮助信息；
    -e 静默执行 
    -t 忽略错误
    -R[分钟] 设置等待时间
    -y 自动应答yes


### command


#### 清除yum缓存

    $ yum check               #检查 RPM 数据库问题
    $ yum check-update        #检查是否有可用的软件包更新
    
    $ yum clean packages      #清除临时包文件（/var/cache/yum 下文件）
    $ yum clean headers       #清除rpm头文件
    $ yum clean all           #清除全部



#### 搜索与检查要安装的包

    $ yum search php    #搜索和php相关的包
    $ yum info php-common   #查看`php-common`这个包的具体的信息
    $ yum deplist php-common    #查看`php-common`的依赖
    $ yum search all php    #加上了包的详细描述


##### `yum info php-common`

![](http://i.imgur.com/NjKIE7r.png)

##### `yum deplist php-common`

![](http://i.imgur.com/XHxAOkT.png)



#### 安装包

    $ sudo yum install php-cli  #安装`php-cli`
    Is this ok [y/d/N]: y
    或者:
    如果确定要安装这个包后，直接使用:
    $ sudo yum install php-cli -y   #不用确认直接安装

    $ php --help    #若出现php的相关帮助信息就证明安装成功


#### 升级包

    $ yum check-update  #检测可升级的包
    $ sudo yum install  #更新所有的包
    $ sudo yum upgrade python   #升级python这个包

#### 删除包

    $ yum list installed    #查看系统当前都安装了哪些包
    $ yum list installed | grep php     #仅仅显示和php相关的包
    $ sudo yum remove php-common    #删除php-common这个包
    $ yum list installed | grep php     #再次搜索与php相关的包


##### `sudo yum remove php-common`

![](http://i.imgur.com/Hm9FJlf.png)

![](http://i.imgur.com/fybaBjt.png)

#### 仓库

    $ yum repolist      #列出系统启用的仓库列表
    $ yum search epel       #搜索和`epel`相关的包
    $ sudo yum epel-release -y      #安装`epel-release`
    $ yum repolist      #列出仓库列表
    $ yum list available        #查看所有可用的包
    $ sudo yum install https://centos7.iuscommunity.org/ius-release.rpm     #直接指定地址安装`ius`仓库的包
    $ yum list available | grep php     #列出所有和php相关的包
    $ yum info php70u-common        #查看`php70u-common`包的详细信息


##### `yum repolist`

![](http://i.imgur.com/6ZexIqk.png)


##### `yum search epel`

![](http://i.imgur.com/xaXQEUa.png)


### 主要介绍了CentOS下`yum`的包管理方式，以及一些yum常用的命令。因为主要使用的CentOS系统。


