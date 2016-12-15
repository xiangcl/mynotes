---
  title: 详解Linux用户和组的权限管理及相关命令
  tags:
    - Linux
    - Linux基础
  categories: Linux
  permalink: linux/linux-user-groups
---



> 在Linux中，用户和组的权限管理是很基础，也是很至关重要的。本文，对这个知识点做了一次详细的梳理；


## 一、文件的权限主要针对3类用户定义

>**owner**：属主，简写为：**u**；
>**group**：属组，简写为：**g**；
>**other**：其它，简写为：**o**；

- 每个文件针对每类访问者都定义了三种权限：
>    - **r**：Readable：**可读**
>    - **w**：Writable：**可写**
>    - **x**：eXcutable：**可执行**

- 对**文件**来说以上三种权限的意义：
>    - **r**：可使用文件查看类工具获取其内容；
>    - **w**：可修改其内容；
>    - **x**：可以把此文件提请内核启动为一个进程；


- 对**目录**来说，以上三种权限的意义：

    >- **r**：可以使用ls查看此目录文件中的列表；
    >- **w**：可在此目录中创建文件，也可删除此目录中的文件；
    >- **x**：可以使用ls -l查看此目录中文件列表，可以cd进入此目录；

|         |            |   |
| ------------- |:-------------:| -----:|
| - - -   | 000 | 0  |
| - - x   | 001 | 1  |
| - w -  | 010 | 2  |
| - w x  | 011 | 3  |
| r - -   | 100 | 4  |
| r - x   | 101 | 5  |
| r w -   | 110 | 6  |
| r w x   | 111 | 7  |

- 从上图可以看出：

    - **r** : 4
    - **w** : 2
    - **x** : 1

- 例如：
    - 640：rw-r-----
    - 755：rwxr-xr-x


## 二、修改用户权限

> **chmod [OPTION]... MODE[,MODE]... FILE...**
> **chmod [OPTION]... OCTAL-MODE FILE...**
> **chmod [OPTION]... --reference=RFILE FILE...**

### 1.chmod [OPTION]... OCTAL-MODE FILE...
- -R:递归修改权限


- **实际示例：**

![](http://i.imgur.com/N7xHbey.png)


### 2.chmod [OPTION]... MODE[,MODE]... FILE...

- **MODE：修改一类用户的所有权限：**
    - u=#
    - g=#
    - o=#
    - ug=#
    - a=#
    - u=#,g=#

>- Note：#代表你所需要改的权限

>**实际示例：**
![](http://i.imgur.com/29duzVL.png)


- **MODE:修改一类用户的某位或某些权限：**

    - u+#
    - u-#
    - g+#
    - g-#
    - o+#
    - o-#
    - gu+#

>- **Note**：#表示某位或某些权限
**实际示例：**
![](http://i.imgur.com/qlF8s5P.png)


### 3.chmod [OPTION]... --reference=RFILE FILE...

- 参考**RFILE**文件的权限，将**FILE**的修改为同**RFILE**一样的权限；
- **图片示例：**
    - ![](http://i.imgur.com/ZZU7Rqg.png)


### 4.修改文件属主和属组：
>- **仅root可用**


- **修改文件的属主：chown**
    - chown [OPTION]... [OWNER][:[GROUP]] FILE...
    - 用法：
        - OWNER：仅修改属主
        - OWNER:GROUP：修改属主和属组
        - :GROUP：仅修改属组
    - **Note**：命令中的`：`可用 `.` 号替换；
    - **-R**：递归
    - **实际示例：**
        - ![](http://i.imgur.com/v37SIae.png)

    - chown [OPTION]... --reference=RFILE FILE...
        - 同上面chmod的用发一样

### 5.修改文件属组：chgrp

- chgrp [OPTION]... GROUP FILE...
- chgrp [OPTION]... --reference=RFILE FILE...
- -R：递归
- **实际示例：**
    - ![](http://i.imgur.com/OS11hF3.png)

### 6.文件或目录创建时的遮罩码：umask

- **FILE**：666-umask
    - **Note**：如果某类的用户的权限减得的结果中存在x权限，则将其权限+1
- **DIR**：777-umask
- **umask**：查看
- **umask+#**：设定
- **Note**：仅对当前用户的当前shell进程有效；
- **实际示例：**
    - ![](http://i.imgur.com/RUICi7n.png)



