---
  title: Linux文件与目录管理2
  tags:
    - Linux
    - Linux基础
  categories: Linux
  permalink: Linux/linux-file-dir-man-two
---

## 文件和目录的默认权限与隐藏权限

### 文件默认权限： umask

umask：指定目前使用者在创建文件或目录时的权限默认值。

```shell
[xiangcl@centos-rpi3 ~]$ umask
0002                                # 与一般权限有关是后三位数字
[xiangcl@centos-rpi3 ~]$ umask -S
u=rwx,g=rwx,o=rx
[xiangcl@centos-rpi3 ~]$ su -
密码：
上一次登录：一 11月 14 05:53:08 UTC 2016pts/0 上
[root@centos-rpi3 ~]# umask
0022                                # 基于安全的考量， root默认的是022
[root@centos-rpi3 ~]# umask -S
u=rwx,g=rx,o=rx
```
*Note：umask的第一组数字是特殊权限使用。*

在默认权限的属性上，目录与文件是不一样的。x 权限对于目录是非常重要的！ 但是一般文件的创建则不应该有执行的权限，因为一般文件通常是用在于数据的记录嘛！当然不需要执行的权限了。 因此，默认的情况如下：

- 若使用者创建为“文件”则默认“没有可执行（x ） 权限”，亦即只有 rw 这两个项目，也就是最大为 666 分，默认权限如下： -rw-rw-rw-

- 若使用者创建为“目录”，则由于 x 与是否可以进入此目录有关，因此默认为所有权限均开放，亦即为 777 分，默认权限如下： drwxrwxrwx

因为 umask 为 022 ，所以 user 并没有被拿掉任何权限，不过 group 与 others 的权限被拿掉了 2 （ 也就是 w 这个权限） ，那么当使用者：
- 创建文件时：（ -rw-rw-rw-） - （ -----w--w-） ==> -rw-r--r--
- 创建目录时：（ drwxrwxrwx） - （ d----w--w-） ==> drwxr-xr-x

```shell
[root@centos-rpi3 ~]# umask
0022
[root@centos-rpi3 ~]# touch test1
[root@centos-rpi3 ~]# mkdir test2
[root@centos-rpi3 ~]# ll -d test*
-rw-r--r--. 1 root root    0 11月 14 06:04 test1
drwxr-xr-x. 2 root root 4096 11月 14 06:04 test2
```

```shell
umask #   :     设定

范例一：修改默认的 umask 的值
[root@centos-rpi3 ~]# umask 002
[root@centos-rpi3 ~]# touch test3
[root@centos-rpi3 ~]# mkdir test4
[root@centos-rpi3 ~]# ll -d test[34]
-rw-rw-r--. 1 root root    0 11月 14 06:10 test3
drwxrwxr-x. 2 root root 4096 11月 14 06:10 test4
```

*Note： umask对于新建文件与目录的默认权限是很重要的*

```shell
例题：假设你的 umask 为003，请问该 umask 情况下，创建的文件和目录的权限为？

umask 为 003
所以拿掉的权限为：
--------wx

文件：
-rw-rw-rw-
-
--------wx
=
-rw-rw-r--

目录：
drwxrwxrwx
-
d-------wx
=
drwxrwxr--
```

### 文件隐藏属性

*下面的 chattr 指令只能在Ext2/Ext3/Ext4 的Linux传统文件系统上面完整的生效，其它文件系统不能完整支持。*

#### chattr （设置文件隐藏属性）

```shell
 chattr [ -RV ] [ -v version ] [ mode ] files...
OPTION：
选项与参数：
+ ：增加某一个特殊参数，其他原本存在参数则不动。
- ：移除某一个特殊参数，其他原本存在参数则不动。
= ：设置一定，且仅有后面接的参数

A ：当设置了 A 这个属性时，若你有存取此文件（ 或目录） 时，他的存取时间 atime 将不会被修改，
可避免 I/O 较慢的机器过度的存取磁盘。（ 目前建议使用文件系统挂载参数处理这个项目）
S ：一般文件是非同步写入磁盘的（ 原理请参考[前一章sync](../Text/index.html#sync)的说明） ，如果加上 S 这个属性时，
当你进行任何文件的修改，该更动会“同步”写入磁盘中。
a ：当设置 a 之后，这个文件将只能增加数据，而不能删除也不能修改数据，只有root 才能设置这属性
c ：这个属性设置之后，将会自动的将此文件“压缩”，在读取的时候将会自动解压缩，
但是在储存的时候，将会先进行压缩后再储存（ 看来对于大文件似乎蛮有用的！）
d ：当 dump 程序被执行的时候，设置 d 属性将可使该文件（ 或目录） 不会被 dump 备份
i ：这个 i 可就很厉害了！他可以让一个文件“不能被删除、改名、设置链接也无法写入或新增数据！”
对于系统安全性有相当大的助益！只有 root 能设置此属性
s ：当文件设置了 s 属性时，如果这个文件被删除，他将会被完全的移除出这个硬盘空间，
所以如果误删了，完全无法救回来了喔！
u ：与 s 相反的，当使用 u 来设置文件时，如果该文件被删除了，则数据内容其实还存在磁盘中，
可以使用来救援该文件喔！
注意1：属性设置常见的是 a 与 i 的设置值，而且很多设置值必须要身为 root 才能设置
注意2：xfs 文件系统仅支持 AadiS 而已
```

```shell
范例一：尝试在 /tmp 下创建文件，并加入 i 的参数，然后删除看看
[root@centos-rpi3 ~]# cd /tmp/
[root@centos-rpi3 tmp]# ls
[root@centos-rpi3 tmp]# touch attrtest
[root@centos-rpi3 tmp]# chattr +i attrtest
[root@centos-rpi3 tmp]# rm attrtest
rm：是否删除普通空文件 "attrtest"？y
rm: 无法删除"attrtest": 不允许的操作
# 是不是无法删除？

# 取消 -i 属性又能删除了
[root@centos-rpi3 tmp]# chattr -i attrtest
[root@centos-rpi3 tmp]# rm attrtest
rm：是否删除普通空文件 "attrtest"？y
```

#### lsattr     （显示文件隐藏属性）

```shell
lsattr [ -RVadv ] [ files...  ]
OPTION:
-a ：将隐藏文件的属性也秀出来；
-d ：如果接的是目录，仅列出目录本身的属性而非目录内的文件名；
-R ：连同子目录的数据也一并列出来！

[root@centos-rpi3 tmp]# chattr +aiS attrtest
[root@centos-rpi3 tmp]# lsattr attrtest
--S-ia-------e-- attrtest
```

### 文件特殊权限：SUID，SGIG, SBIT

```shell
[root@centos-rpi3 tmp]# ls -ld /tmp; ls -l /usr/bin/passwd
drwxrwxrwt. 7 root root 4096 11月 14 06:38 /tmp
-rwsr-xr-x. 1 root root 27236 5月  23 2015 /usr/bin/passwd
```
权限里竟然冒出了 t 和 s。

#### Set UID

当 s 这个标志出现在文件拥有者的 x 权限上时，例如刚刚提到的 /usr/bin/passwd 这个文件的权限状态：“-rwsr-xr-x”，此时就被称为 Set UID，简称为 SUID 的特殊权限。 那么SUID的权限对于一个文件的特殊功能是什么呢？基本上SUID有这样的限制与功能：

- SUID 权限仅对二进制程序（ binary program） 有效；
- 执行者对于该程序需要具有 x 的可执行权限；
- 本权限仅在执行该程序的过程中有效 （ run-time） ；
- 执行者将具有该程序拥有者 （ owner） 的权限。

例：我们的 Linux 系统中，所有帐号的密码都记录在 /etc/shadow 这个文件里面，这个文件的权限为：“---------- 1 root root”，意思是这个文件仅有root可读且仅有root可以强制写入而已。既然这个文件仅有 root 可以修改，那么自己的帐号这种一般帐号使用者能否自行修改自己的密码呢？ 你可以使用你自己的帐号输入“passwd”这个指令来看看。一般使用者当然可以修改自己的密码了！

*SUID对目录无效*


#### Set GID

默认情况下，用户创建文件时，其属组为此用户所属的基本组；

一旦某目录被设定了SGID，则对此目录有写权限的用户在此目录中创建的文件所属的组为此目录的属组；

权限设定：
```shell
	chmod g+s DIR...
	chmod g-s DIR...
```

对文件来说，SGID有如下功能：

- SGID 对二进制程序有用；
- 程序执行者对于该程序来说，需具备 x 的权限；
- 执行者在执行的过程中将会获得该程序群组的支持！

对目录来说，SGID有如下功能：

- 使用者若对于此目录具有 r 与 x 的权限时，该使用者能够进入此目录；
- 使用者在此目录下的有效群组（ effective group） 将会变成该目录的群组；
- 用途：若使用者在此目录下具有 w 的权限（ 可以新建文件） ，则使用者所创建的新文件，该新文件的群组与此目录的群组相同。

#### Sticky

SBIT 目前只针对目录有效，对于文件已经没有效果了。SBIT 对于目录的作用是：

- 当使用者对于此目录具有 w, x 权限，亦即具有写入的权限时；
- 当使用者在该目录下创建文件或目录时，仅有自己与 root 才有权力删除该文件

对于一个多人可写的目录，如果设置了sticky，则每个用户仅能删除自己的文件；

```shell
权限设定：
	chmod o+t DIR...
	chmod o-t DIR...

SUID SGID STICKY
	000 0
	001 1
	010 2
	011 3
	100 4
	101 5
	110 6
	111 7

	chmod 4777 /tmp/a.txt

几个权限位映射：
	SUID: user, 占据属主的执行权限位；
		s: 属主拥有x权限
		S：属主没有x权限
	SGID: group,  占据group的执行权限位；
		s: group拥有x权限
		S：group没有x权限
	Sticky: other, 占据ohter的执行权限位；
		t: other拥有x权限
		T：other没有x权限
```

## 指令与文件的搜索

### 指令文件名的搜索

在终端机模式当中，连续输入两次[tab]按键就能够知道使用者有多少指令可以下达。那你知不知道这些指令的完整文件名放在哪里？

#### which      （搜索“可执行文件”）

```shell
which [options] [--] programname [...]
OPTIONS:
-a, --all
    将所有由 PATH 目录中可以找到的指令均列出，而不止第一个被找到的指令名称
--skip-alias:
    禁止显示别名


范例一：搜索 ifconfig 这个指令的完整文件名
[root@centos-rpi3 tmp]# which ifconfig
/sbin/ifconfig

范例二：用 which 去找出 which 的文件名
[root@centos-rpi3 tmp]# which which
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
	/bin/alias
	/usr/bin/which
# alias 是命令别名

范例三：请找出 history 这个指令的完整文件名
[root@centos-rpi3 tmp]# which history
/usr/bin/which: no history in (/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin)
# history是“bash内置的指令”，但是 which 默认是找 PATH 内所规范的目录。所以当然搜寻不到
```

这个命令是根据 “PATH” 这个环境变量所规范的路径，去搜索“可执行文件”的文件名。


### 文件、文件名的搜索

搜索文件时 find 不很常用，因为速度慢之外，也很损耗硬盘。一般我们都是先使用whereis 或者是 locate 来检查，如果真的找不到了，才以 find 来搜寻。whereis 只找系统中某些特定目录下面的文件而已，locate 则是利用数据库来搜寻文件名，当然两者就相当的快速， 并且没有实际的搜寻硬盘内的文件系统状态，比较省时间。

#### whereis        （由一些特定的目录中寻找文件、文件名）

```shell
whereis [options] [-BMS directory... -f] name...
OPTION:
-l :可以列出 whereis 会去查询的几个主要目录而已
-b :只找 binary 格式的文件
-m :只找在说明文档 manual 路径下的文件
-s :只找 source 来源文件
-u :搜寻不在上述三个项目当中的其他特殊文件

范例一：请找出 ifconfig 这个文件名
[root@centos-rpi3 tmp]# whereis ifconfig
ifconfig: /usr/sbin/ifconfig /usr/share/man/man8/ifconfig.8.gz

范例二：只找出跟 passwd 有关的“说明文档”文件名 （man page）
[root@centos-rpi3 tmp]# whereis passwd      # 列出全部的文件名
passwd: /usr/bin/passwd /etc/passwd /usr/share/man/man1/passwd.1.gz
[root@centos-rpi3 tmp]# whereis -m passwd   # 只有在 man 里面的文件名才列出来
passwd: /usr/share/man/man1/passwd.1.gz
```

*whereis 主要是针对 /bin /sbin 下面的可执行文件， 以及 /usr/share/man 下面的 man page 文件，跟几个比较特定的目录来处理*

*要知道 whereis 到底查了多少目录？可以使用 whereis -l 来查看。*

#### locate / updatedb

```shell
locate  -   按名称查找文件

 locate [OPTION]... PATTERN...

OPTION：
-i，--ignore-case
    忽略大小写的差异；
-c，--count
    不输出文件名，仅计算找到的文件数量
-l， --limit, -n LIMIT
    仅输出几行的意思，例如输出五行则是 -l 5
-S，, --statistics
    输出 locate 所使用的数据库文件的相关信息，包括该数据库纪录的文件/目录数量等
-r，--regexp REGEXP
    后面可接正则表达式的显示方式
```

```shell
范例一：找出系统中所有与 passwd 相关的文件名，且只列出5个
[xiangcl@VM_0_26_centos ~]$ locate -l 5 passwd
/etc/passwd
/etc/passwd-
/etc/pam.d/passwd
/etc/security/opasswd
/root/node-v6.2.0/deps/openssl/openssl/apps/passwd.c

范例二：列出 locate 查询所使用的数据库文件之文件名与各数据数量
[xiangcl@VM_0_26_centos ~]$ locate -S
数据库 /var/lib/mlocate/mlocate.db:
	18,507 文件夹                     # 总记录目录数
	120,727 文件                      # 总记录文件数
	7,442,050 文件名中的字节数
	2,959,140 字节用于存储数据库

```

*忘记某个文件的完整文件名时，可以使用locate*

*locate 寻找的数据是由“已创建的数据库 /var/lib/mlocate/” 里面的数据所搜寻到的，所以不用直接在去硬盘当中存取数据*

*使用locate会有限制，因为他是经由数据库来搜寻的，而数据库的创建默认是在每天执行一次 （ 每个 distribution 都不同，CentOS 7.x 是每天更新数据库一次！） ，所以当你新创建起来的文件， 却还在数据库更新之前搜寻该文件，那么 locate 会告诉你“找不到！”。因为必须更新数据库*

*更新 locate 数据库的方法非常简单，直接输入“updatedb ”就可以了！ updatedb 指令会去读取 /etc/updatedb.conf 这个配置文件的设置，然后再去硬盘里面进行搜寻文件名的动作， 最后就更新整个数据库文件啰！因为 updatedb 会去搜寻硬盘，所以当你执行 updatedb 时，可能会等待数分钟的时间喔！*

##### 执行步骤
- updatedb：根据 /etc/updatedb.conf 的设置去搜寻系统硬盘内的文件名，并更新
/var/lib/mlocate 内的数据库文件；
- locate：依据 /var/lib/mlocate 内的数据库记载，找出使用者输入的关键字文件名。


#### find   (递归地在层次目录中处理文件)

概述：实事查找工具，通过便利指定路径下的文件系统完成文件查找

特点：
- 查找速度略慢
- 精确查找
- 实事查找

```shell
find [OPTION]... [查找路径] [查找条件] [处理动作]
    查找路径： 指定具体目标路径；默认为当前目录；
    查找条件： 指定的查找标准，可以文件名、大小、类型、权限等标准进行；默认为找出指定路径下的所有文件；
    处理动作： 对符合条件的文件做什么操作；默认输出至屏幕；

查找条件：
    根据文件名查找：
        -name “文件名”
        -iname “文件名”：不区分字母大小写
        -regex "PATTERN"：以PATTERN（文件名与正则表达式）匹配整个文件路径字符串，而不仅仅是文件名称；

    根据属主、属组查找：
    	-user USERNAME：查找属主为指定用户的文件；
    	-group GRPNAME: 查找属组为指定组的文件；

    	-uid UserID：查找属主为指定的UID号的文件；
    	-gid GroupID：查找属组为指定的GID号的文件；

    	-nouser：查找没有属主的文件；
    	-nogroup：查找没有属组的文件；

    根据文件类型查找：
		-type TYPE:
			f: 普通文件
			d: 目录文件
			l: 符号链接文件
			s：套接字文件
			b: 块设备文件
			c: 字符设备文件
			p: 管道文件

	组合条件：
		与：-a
		或：-o
		非：-not, !

		!A -a !B = !(A -o B)
		!A -o !B = !(A -a B)

		范例：找出/tmp目录下，属主不是root，且文件名不是fstab的文件；
			find /tmp \( -not -user root -a -not -name 'fstab' \) -ls
			find /tmp -not \( -user root -o -name 'fstab' \) -ls
            # 加上 -ls ，是查找并显示结果

	根据文件大小来查找：(示意图如下：find指令文件大小范围示意图)
		-size [+|-]#UNIT
			常用单位：k, M, G

			#UNIT: (#-1, #]
			-#UNIT：[0,#-1]
			+#UNIT：(#,oo)

	根据时间戳：(示意图如下：find指令时间戳范围示意图)
		以“天”为单位；
			-atime [+|-]#,
				#: [#,#+1)
				+#: [#+1,oo]
				-#: [0,#)
			-mtime
			-ctime

		以“分钟”为单位：
			-amin
			-mmin
			-cmin

	根据权限查找：
		-perm [/|-]MODE
			MODE: 精确权限匹配
			/MODE：任何一类(u,g,o)对象的权限中只要能一位匹配即可；
			-MODE：每一类对象都必须同时拥有为其指定的权限标准；

处理动作：
	-print：默认的处理动作，显示至屏幕；
	-ls：类似于对查找到的文件执行“ls -l”命令；
	-delete：删除查找到的文件；
	-fls /path/to/somefile：查找到的所有文件的长格式信息保存至指定文件中；
	-ok COMMAND {} \; 对查找到的每个文件执行由COMMAND指定的命令；
		对于每个文件执行命令之前，都会交互式要求用户确认；
	-exec COMMAND {} \; 对查找到的每个文件执行由COMMAND指定的命令;
		{}: 用于引用查找到的文件名称自身；

	注意：find传递查找到的文件至后面指定的命令时，查找到所有符合条件的文件一次性传递给后面的命令；
	有些命令不能接受过多参数，此时命令执行可能会失败；另一种方式可规避此问题：
		find | xargs COMMAND

范例一：将过去系统上24小时内有更动过内容（mtime）的文件列出
[root@centos-rpi3 ~]# find / -mtime 0
# 那个 0 是重点！0 代表目前的时间，所以，从现在开始到 24 小时前，
# 有变动过内容的文件都会被列出来！那如果是三天前呢？
# find / -mtime 3 有变动过的文件都被列出的意思！

范例二：寻找 /etc 下面的文件，如果文件日期比 /etc/passwd 新就列出
[root@centos-rpi3 ~]# find /etc -newer /etc/passwd
# -newer 用在分辨两个文件之间的新旧关系是很有用的。
```

##### find指令文件大小范围示意图

![](http://xiangclimg-10066880.cos.myqcloud.com/find1.png)


##### find指令时间戳范围示意图

![](http://xiangclimg-10066880.cos.myqcloud.com/find2.png)




