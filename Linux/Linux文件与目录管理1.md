---
  title: Linux文件与目录管理1
  tags:
    - Linux
    - Linux基础
  categories: Linux
  permalink: Linux/linux-file-dir-man-one
---

## 一、目录与路径

### 相对路径与绝对路径

#### 1.绝对路径：路径的写法“一定由跟目录 `/` 写起”
- 例如：`/usr/share/doc`

#### 2.相对路径：路径的写法“不是由`/`写起”
- 例如：由 `/usr/share/doc` 要到 `/usr/share/man` 下面
时，可以写成： “`cd ../man`”这就是相对路径的写法啦！相对路径意指“相对于目前工作目录的路径！”


### 目录的相关操作

#### 特殊目录

```shell
dr-xr-xr-x. 19 root root  4096 11月  5 10:25 .       #代表此层目录
dr-xr-xr-x. 19 root root  4096 11月  5 10:25 ..      #代表上一层目录

~       #代表“目前使用者身份”所在的主文件夹
~xiangcl        #代表 xiangcl 这个使用者的主文件夹（ xiangcl是个帐号名称）
```

#### 常见的目录处理指令

##### cd (change directory, 变换目录)

```shell
$ cd ~      #回到当前用户的主目录
$ cd ~USERNAME       #切换到指定用户目录的主目录
$ cd        #回到当前用户的主目录
$ cd ..     #切换到当前位置的上一层目录
$ cd -      #在上一个目录和当前目录之间来回切换
$ cd /usr/share     #进入/usr/share这个目录，绝对路径的写法
$ cd ../src     #进入/usr/src这个目录，相对路径的写法
```

##### pwd [OPTION]  （显示目前所在的目录）

```shell
$ pwd       #显示当前所在目录的绝对路径
$ pwd -P        #显示出真实路径，而非链接(link)路径
例如：
[xiangcl@VM_0_26_centos ~]$ cd /var/mail/       #切换到/var/mail这个目录下
[xiangcl@VM_0_26_centos mail]$ pwd
/var/mail       #列出当前目录的绝对路径
[xiangcl@VM_0_26_centos mail]$ pwd -P
/var/spool/mail     #感觉和没加-P的差别有点大
[xiangcl@VM_0_26_centos mail]$ cd ..
[xiangcl@VM_0_26_centos var]$ ls -l mail
lrwxrwxrwx 1 root root 10 11月  1 09:52 mail -> spool/mail       #原来/var/mail 是链接文件，链接到 /var/spool/mail
```

*pwd是Print Working Directory的缩写，是显示目前所在目录的指令。*



##### mkdir [options] file...   (创建新目录)

```shell
-m mode, --mode=mode
    设置文件的权限喔！直接设置，不需要看默认权限 （ umask）
-p, --parents
    帮助你直接将所需要的目录（包含上层目录） 递回创建起来！
-v
    显示详细信息

[xiangcl@VM_0_26_centos ~]$ cd /tmp/    #切换到/tpm下去做实验
[xiangcl@VM_0_26_centos tmp]$ mkdir test    #创建test这个目录，没有返回消息代表创建成功
[xiangcl@VM_0_26_centos tmp]$ mkdir test1/test2/test3      #创建失败
mkdir: 无法创建目录"test1/test2/test3": 没有那个文件或目录
[xiangcl@VM_0_26_centos tmp]$ mkdir -p test1/test2/test3    #需要加上-p去递归创建
[xiangcl@VM_0_26_centos tmp]$ tree test1    #显示test1这个目录下的层级结构
test1
`-- test2
    `-- test3

2 directories, 0 files

[xiangcl@VM_0_26_centos tmp]$ mkdir -m 711 test2
[xiangcl@VM_0_26_centos tmp]$ ls -ld test*
drwxrwxr-x 2 xiangcl xiangcl 4096 11月  5 11:32 test
drwxrwxr-x 3 xiangcl xiangcl 4096 11月  5 11:33 test1
drwx--x--x 2 xiangcl xiangcl 4096 11月  5 11:37 test2
#没有加-m选项的目录会使用系统默认的属性
#[umask]设置默认属性
```

##### rmdir  （删除“空”目录）

```shell
rmdir[options]directory...
OPTION:
    -v：显示过程

[xiangcl@VM_0_26_centos tmp]$ ls -ld test*      #查看/tmp这个目录下的文件
drwxrwxr-x 2 xiangcl xiangcl 4096 11月  5 11:32 test
drwxrwxr-x 3 xiangcl xiangcl 4096 11月  5 11:33 test1
drwx--x--x 2 xiangcl xiangcl 4096 11月  5 11:37 test2
drwxr--r-- 2 xiangcl root    4096 11月  1 13:44 testing

[xiangcl@VM_0_26_centos tmp]$ rmdir test        #删除test这个目录，Note：test这个目录为空
[xiangcl@VM_0_26_centos tmp]$ rmdir test1       #删除test1这个目录失败，因为这个目录不为空
rmdir: 删除 "test1" 失败: 目录非空

[xiangcl@VM_0_26_centos tmp]$ rmdir -p test1/test2/test3/       #加上那个-p选项
[xiangcl@VM_0_26_centos tmp]$ ls -ld test*
drwx--x--x 2 xiangcl xiangcl 4096 11月  5 11:37 test2
drwxr--r-- 2 xiangcl root    4096 11月  1 13:44 testing
```

### 文件内容类型查看命令：file

```shell
file /PAHT/TO/SOMEWHERE

范例：查看 /etc/passwd 的文件类型
[xiangcl@VM_0_26_centos tmp]$ cd /etc/
[xiangcl@VM_0_26_centos etc]$ file ./passwd
./passwd: ASCII text
```

### 回显命令：echo

```shell
-n: 禁止自动添加换行符号；
-e: 允许使用转义符；
	\n: 换行
	\t: 制表符

echo "$VAR_NAME": 变量会替换，双引号表弱引用
echo '$VAR_NAME': 变量不会替换，强引用
```

### which：显示命令对应的程序文件路径

```shell
which [OPTION] COMMAND
	--skip-alias：禁止显示别名

范例一：显示 ls 对应的程序文件路径
[xiangcl@VM_0_26_centos tmp]$ which ls
alias ls='ls --color=auto'
	/usr/bin/ls

范例二：在范例一的基础上不显示别名
[xiangcl@VM_0_26_centos tmp]$ which --skip-alias ls
/usr/bin/ls
```

### whatis :用于查询一个命令执行什么功能，并将查询结果打印到终端上

```shell
使用mkwhatis命令可将当前系统上所有的帮助手册及与之对应的关键字创建为一个数据库；

whatis命令在用catman命令创建的数据库中查找command参数指定的命令、系统调用、库函数或特殊文件名。whatis命令显示手册部分的页眉行。然后可以发出man命令以获取附加的信息。whatis命令等同于使用man -f命令。（需要root权限）

范例：显示 ls 手册部分的页眉行
[root@VM_0_26_centos ~]# whatis ls
ls (1)               - 列目录内容
```

## 二、文件与目录管理


### ls [options] [files...]       （列出目录的内容）

```shell
-a，--all
    全部的文件，连同隐藏文件（ 开头为 . 的文件） 一起列出来（ 常用）
-A，--almost-all
    全部的文件，连同隐藏文件，但不包括 . 与 .. 这两个目录
-d，--directory
    仅列出目录本身，而不是列出目录内的文件数据
-f
    直接列出结果，而不进行排序 （ ls 默认会以文件名排序！）
-F
    根据文件、目录等信息，给予附加数据结构
    例如：*:代表可可执行文件； /:代表目录； =:代表 socket 文件； &#124;:代表 FIFO 文件；
-h，--human-readable
    将文件大小以人类较易读的方式（ 例如 GB, KB 等等） 列出来；
-i
    列出 inode 号码，inode 的意义下一章将会介绍；
-l，, --format=long, --format=verbose
    长数据串行出，除每个文件名外，增加显示文件类型、权限、硬链接数、所有者名、组名、大小（byte）、及时间信息（如未指明是其它时间即指修改时间）。对于6个月以上的文件或超出未来1小时的文件，时间信息中的时分将被年代取代。；（ 常用）
-n，--numeric-uid-gid
    列出 UID 与 GID 而非使用者与群组的名称 （ UID与GID会在帐号管理提到！）
-r，--reverse
    将排序结果反向输出，例如：原本文件名由小到大，反向则为由大到小；
-R，--recursive
    连同子目录内容一起列出来，等于该目录下的所有文件都会显示出来；
-S，--sort=size
    以文件大小大小排序，而不是用文件名排序；
-t，--sort=time
    依时间排序，而不是用文件名。
--color=never
    不要依据文件特性给予颜色显示；
--color=always
    显示颜色
--color=auto
    让系统自行依据设置来判断是否给予颜色
--full-time
    以完整时间模式（包含年、月、日、时、分）输出
--time={atime,ctime}
    输出 access 时间或改变权限属性时间（ctime），而非内容变更时间（modification time）
```

### 复制(cp)、删除(rm)与移动(mv)


#### cp    (复制文件或目录)

```shell
cp [OPTION]... [-T] SOURCE DEST
cp [OPTION]... SOURCE... DIRECTORY
cp [OPTION]... -t DIRECTORY SOURCE...

cp SRC DEST
    SRC是文件：
    	如果目标不存在：新建DEST，并将SRC中内容填充至DEST中；
    	如果目录存在：
    		如果DEST是文件：将SRC中的内容覆盖至DEST中；
    			此时建议为cp命令使用-i选项；
    		如果DEST是目录：在DEST下新建与原文件同名的文件，并将SRC中内容填充至新文件中；

cp SRC DEST
	SRC是目录：
		此时使用选项：-r

		如果DEST不存在：则创建指定目录，复制SRC目录中所有文件至DEST中；
		如果DEST存在：
			如果DEST是文件：报错
			如果DEST是目录：在DEST下新建与源目录同名的目录及文件，将SRC中的文件内容填充至新目录的文件中。

cp SRC... DEST
	SRC...：多个文件
		DEST必须存在，且为目录，其它情形均会出错；

Common options:
-a,--archive
    归档，相当于 -dr --preserve=all 的意思，至于 dr 请参考下列说明；（ 常用）
-d, --no-dereference --preserv=links
    --preserv[=ATTR_LIST]
    	mode: 权限
    	ownership: 属主属组
    	timestamp:
    	links
    	xattr
    	context
    	all
    复制符号链接作为符号链接而不是复制它指向的文件,并且保护在副本中原文件之间的硬链接.
-f,--force
    为强制（ force） 的意思，若目标文件已经存在且无法打开，则移除后再尝试一次；
-i,--interactive
    交互式
    若目标文件（ destination） 已经存在时，在覆盖时会先询问动作的进行（ 常用）
-l,--link
    进行硬式链接（ hard link） 的链接文件创建，而非复制文件本身；
-p,--preserve
    连同文件的属性（权限、用户、时间、许可）一起复制过去，而非使用默认属性（ 备份常用） ；
-r
    递回持续复制，用于目录的复制行为；（ 常用）
-R, --recursive
    递归地复制目录,保留非目录
-s，--symbolic-link
    复制成为符号链接文件 （ symbolic link） ，亦即“捷径”文件；
-u，--update
    destination 比 source 旧才更新 destination，或 destination 不存在的情况下才复制。
--preserve=all
    除了 -p 的权限相关参数外，还加入 SELinux 的属性, links, xattr 等也复制了。
    最后需要注意的，如果来源文件有两个以上，则最后一个目的文件一定要是“目录”才行！
```


#### rm  (移除文件或目录)

```shell
rm [optiongs] files...
options:
    -f, --force
        强制删除，就是 force 的意识，忽略不存在的文件，不会出现警告讯息；
    -i, --interactive
        交互式
        互动模式，在删除前会询问使用者是否删除这一动作；
    -r, -R，--recursive
        递归删除，最常用在目录的删除。
    -v，--verbose
        在移除每个文件之前打印其名称。

范例一：将创建的文件删除
[xiangcl@VM_0_26_centos tmp]$ touch test
[xiangcl@VM_0_26_centos tmp]$ rm -i test
rm：是否删除普通空文件 "test"？y
# 加上 -i 选项就会主动询问是否删除，避免文件的误删。

范例二：通过*，删除所有以test开头的文件
[xiangcl@VM_0_26_centos tmp]$ rm -i test*
# * 代表删除所有以test开头的文件

范例三：将不为空的目录删除
[root@VM_0_26_centos tmp]# rmdir ./testing
rmdir: 删除 "./testing" 失败: 目录非空
[root@VM_0_26_centos tmp]# rm -r ./testing
rm：是否进入目录"./testing"? y
rm：是否删除普通空文件 "./testing/1.sh"？y
rm：是否删除普通空文件 "./testing/2.sh"？y
rm：是否删除普通空文件 "./testing/3.sh"？y
rm：是否删除目录 "./testing"？y
# 因为是root身份，默认已经加入了 -i 的选项，这是一种保护机制

范例四：删除一个带有 - 开头的文件
[root@VM_0_26_centos tmp]# touch ./-aaa-
[root@VM_0_26_centos tmp]# ls -l
 -rw-r--r-- 1 root root    0 11月  9 14:41 -aaa-
[root@VM_0_26_centos tmp]# rm -aaa-
rm：无效选项 -- a
Try 'rm ./-aaa-' to remove the file "-aaa-".
Try 'rm --help' for more information.
[root@VM_0_26_centos tmp]# rm ./-aaa-
rm：是否删除普通空文件 "./-aaa-"？y
```

##### 小结

- 为了防止文件被root误杀，很多distributions都默认在 `rm` 指令中加了 `-i` 选项；
- 递归删除使用 `-r` 选项，但是`-r`选项威力太过强大，一般只有确定文件要删除才会使用；
- 文件名最好不要使用 `-` 开头；


#### mv     (移动文件与目录、或用于文件更名)

```shell
mv [option]... source destination
mv [option]... source... directory
mv [option]... --target-directory=DIRECTORY SOURCE...

option:
    -f, --force
        强制的意思，如果咪表文件已经存在，不会询问而直接覆盖；
    -i, --interactive
        覆盖提示
    -u, --update
        若目标文件已经存在，且source比目标文件新，才是更新覆盖；

范例一：创建一目录，将文件移动到目录中
[root@VM_0_26_centos tmp]# cp ~/.bashrc bashrc
[root@VM_0_26_centos tmp]# mkdir test
[root@VM_0_26_centos tmp]# mv bashrc test/
# 将某个文件移动到目录中

范例二：文件更名
[root@VM_0_26_centos tmp]# mv test test1
# 将 test 更名为 test1；

范例三：将多个文件移动到目录中
[root@VM_0_26_centos tmp]# cp ~/.bashrc bashrc1
[root@VM_0_26_centos tmp]# cp ~/.bashrc bashrc2
[root@VM_0_26_centos tmp]# mv bashrc1 bashrc2 test1
# 如果有多个源文件或目录，则最后一个目标文件一定是目录；
```


### 取得文件名与完整路径的目录

```shell
[root@VM_0_26_centos tmp]# basename /etc/sysconfig/network
network
[root@VM_0_26_centos tmp]# dirname /etc/sysconfig/network
/etc/sysconfig
# 取得文件路径对于在shell script中是很重要的；
```

## 三、文件内容查看

- cat 由第一行开始显示文件内容
- tac 从最后一行开始显示，可以看出 tac 是 cat 的倒着写！
- nl 显示的时候，顺道输出行号！
- more 一页一页的显示文件内容
- less 与 more 类似，但是比 more 更好的是，他可以往前翻页！
- head 只看头几行
- tail 只看尾巴几行
- od 以二进制的方式读取文件内容！

### 直接检视文件内容

#### cat    (concatenate)

```shell
cat [选项列表] [文件列表]...
选项：
 -A, --show-all
    相当于 -vET 的整合选项，可列出一些特殊字符而不是空白而已；
 -b, --number-nonblank
    列出行号，仅针对非空白行做行号显示，空白行不标行号！
 -E,  --show-ends
    将结尾的断行字符 $ 显示出来；
 -n, --number
    打印出行号，连同空白行也会有行号，与 -b 的选项不同；
 -T, --show-tabs
    将 [tab] 按键以 ^I 显示出来；
 -v, --show-nonprinting
    列出一些看不出来的特殊字符

范例一：查看 /etc/issue 这个文件的内容
[root@VM_0_26_centos ~]# cat /etc/issue
\S
Kernel \r on an \m

范例二：在上一题的基础上加上行号
[root@VM_0_26_centos ~]# cat -n /etc/issue  #第一种方法
 1	\S
 2	Kernel \r on an \m
 3
[root@VM_0_26_centos ~]# cat -b /etc/issue  #第二种方法
 1	\S
 2	Kernel \r on an \m
```

#### tac    (反向列出文本)

```shell
[xiangcl@VM_0_26_centos ~]$ tac /etc/issue

Kernel \r on an \m
\S
# 与上面 cat的显示效果相反，是由最后一行开始显示的。
```

#### nl - number lines of files     (添加行号打印)

```shell
nl [OPTION]... [FILE]...
OPTION:
-b, --body-numbering=STYLE ：指定行号指定的方式，主要有两种：
    -b a ：表示不论是否为空行，也同样列出行号（类似 cat -n） ；
    -b t ：如果有空行，空的那一行不要列出行号（默认值） ；
-n, --number-format=FORMAT ：列出行号表示的方法，主要有三种：
    -n ln ：行号在屏幕的最左方显示；
    -n rn ：行号在自己字段的最右方显示，且不加 0 ；
    -n rz ：行号在自己字段的最右方显示，且加 0 ；
-w, --number-width=NUMBER ：行号字段的占用的字符数。

范例一：用 nl 列出 /etc/issue 的内容
[xiangcl@VM_0_26_centos ~]$ nl /etc/issue
 1	\S
 2	Kernel \r on an \m

# 这个文件有三行，第三行为空白(没有任何字符)
# 因为他是空白行，所以 nl 不会加上行号，所以如果确定要加上行号，可以这样做：

[xiangcl@VM_0_26_centos ~]$ nl -b a /etc/issue
 1	\S
 2	Kernel \r on an \m
 3
# 行号加上了，如果要让行号前面自动补上0，可以这样做：

[xiangcl@VM_0_26_centos ~]$ nl -b a -n rz /etc/issue
000001	\S
000002	Kernel \r on an \m
000003
# 自动在字段的地方补上 0 了，默认字段是六位数，如何改为三位数呢？

[xiangcl@VM_0_26_centos ~]$ nl -b a -n rz -w 3 /etc/issue
001	\S
002	Kernel \r on an \m
003
```


### 可翻页查看文本

前面提到的nl、cat与tac命令，都是一次性将数据一口气显示到屏幕上，接下来介绍一下可翻页显示的指令。

#### more   (在显示器上阅读文件的过滤器)

```shell
more [-dlfpcsu] [-num] [+/ pattern] [+ linenum] [file ...]
OPTION：
    -num:  这个选项指定屏幕的行数 (以整数表示).     # num不是输入num而是输入数字；
    -f: 使 more 计数 逻辑行, 而不是 屏幕行 (就是说, 长行 不会 断到 下一行).   -d：显示翻页及退出提示

[xiangcl@VM_0_26_centos ~]$ more  /etc/man_db.conf
# 使用这个指令后就会进入阅读模式，那么，怎么操作呢？


- 空白键 （space）：代表向下翻一页；
- Enter： 代表向下翻“一行”；
- / 字串 ：代表在这个显示的内容当中，向下搜寻“字串”这个关键字；
- :f： 立刻显示出文件名以及目前显示的行数；
- q： 代表立刻离开 more ，不再显示该文件内容。
- b 或 [ctrl]-b ：代表往回翻页，不过这动作只对文件有用，对管道无用。
```

![](http://xiangclimg-10066880.cos.myqcloud.com/more_user.gif)


这里特别强调的是搜索这个指令，在 more 的阅读模式下，直接输入 / 后，光标就会跑到最下面一行，并且等待你的输入，你输入需要搜索的字串后并按下[enter]，more就会向下搜索该字符串，向下使用 n 即可。最后按 q 键退出。


#### less   (一页一页翻动)

```shell
[xiangcl@VM_0_26_centos ~]$ less /etc/man_db.conf
```

在 less 查看文件时常用的指令：

- 空白键： 向下翻动一页；
- [pagedown]： 向下翻动一页；
- [pageup]： 向上翻动一页；
- / 字串： 向下搜寻“字串”的功能；
- ? 字串： 向上搜寻“字串”的功能；
- n： 重复前一个搜寻 （与 / 或 ? 有关！）
- N： 反向的重复前一个搜寻 （与 / 或 ? 有关！）
- g： 前进到这个数据的第一行去；
- G： 前进到这个数据的最后一行去（注意大小写） ；
- q： 离开 less 这个程序；

个人觉得相对于`more`指令来说`less`要好用，不过存在即有存在的道理，这其中好处还需诸君自行体会。


### 数据撷取

有的时候，我们仅仅只需要对文件有个了解。而前面所了解到的指令都是将全文输出，下面我们来了解一下怎样对数据进行简单的撷取。取前面几行文本(head)或取后面(tail)几行文文本。

注意：head与tail都是以 行 为当位进行数据的撷取。

#### head   (输出文件开始的部分)

```shell
-c #
    指定获取前#字节
-n #, --lines=NUMBER
      显示起始的NUMBER行,而非默认的起始10行

-q, --quiet, --silent
      不显示包含给定文件名的文件头

-v, --verbose
      总是显示包含给定文件名的文件头
```

```shell
    [xiangcl@VM_0_26_centos ~]$ head /etc/man_db.conf   #默认显示前10行
    #
    #
    # This file is used by the man-db package to configure the man and cat paths.
    # It is also used to provide a manpath for those without one by examining
    # their PATH environment variable. For details see the manpath(5) man page.
    #
    # Lines beginning with `#' are comments and are ignored. Any combination of
    # tabs or spaces may be used as `whitespace' separators.
    #
    # There are three mappings allowed in this file:

    [xiangcl@VM_0_26_centos ~]$ head -n 3 /etc/man_db.conf    #可以直接指定要显示的行数
    #
    #
    # This file is used by the man-db package to configure the man and cat paths.

    [xiangcl@VM_0_26_centos ~]$ head -n 3 -v  /etc/man_db.conf    #显示前三行且在开始处显示文件路径
    ==> /etc/man_db.conf <==
    #
    #
    # This file is used by the man-db package to configure the man and cat paths.
```

*如果 `-n` 后面接的是负数的话，则仅仅显示前面的行数。例如： CentOS 7.1 的 `/etc/mandb.conf` 共有131行，则上述的指令`head -n -100 /etc/man_db.conf` 就会列出前面31行，后面100行不会被打印出来。*


#### tail   (输出文件末尾部分)

```shell
-c, --bytes=N
      输出最后N个字节

-f, --follow[={name|descriptor}]
      当文件增长时,输出后续添加的数据;  -f, --follow以及 --follow=descriptor
      都是相同的意思

-n, --lines=N
      输出最后N行,而非默认的最后10行

--pid=PID
      与-f合用,表示在进程ID,PID死掉之后结束.

-q, --quiet, --silent
      从不输出给出文件名的首部

-s, --sleep-interval=S
      与-f合用,表示在每次反复的间隔休眠S秒

-v, --verbose
      总是输出给出文件名的首部
```

```shell
[xiangcl@VM_0_26_centos ~]$ tail /etc/man_db.conf
# 默认情况下显示的是最后10行
# 如果要显示最后20行，则可以写成：
[xiangcl@VM_0_26_centos ~]$ tail -n 20 /etc/man_db.conf

范例一 ：如果不知道 /etc/man_db.conf 有多少行，却只想列出100行以后的数据：
[xiangcl@VM_0_26_centos ~]$ tail -n +100 /etc/man_db.conf

范例二 ：持续侦测 /var/log/messages 的内容
[root@VM_0_26_centos ~]# tail -f /var/log/messages
# 如果文件内容有变化的话会持续更新，退出请按[Crtl]+c;
```

*Note：*范例二必须使用root身份才行。*


```shell
例题一： 显示 /etc/man_db.conf 的第 11 到第 20 行
# 思路：先取前20行，再取后10行；
$ head -n 20 /etc/man_db.conf | tail -n 10

例题二： 显示 /etc/man_db.conf 的第 11 到第 20 行，且带行号
$ cat -n /etc/man_db.conf | head -n 20 | tail -n 10
```


### 非纯文本文件：od

```shell
od [OPTION]... [FILE]...
od [-abcdfilosx]... [FILE] [[+]OFFSET[.][b]]
od --traditional [OPTION]... [FILE] [[+]OFFSET[.][b] [+][LABEL][.][b]]

OPTION:
    -t, --format=TYPE ：后面可以接各种“类型 （ TYPE） ”的输出，例如：
        a ：利用默认的字符来输出；
        c ：使用 ASCII 字符来输出
        d[size] ：利用十进制（ decimal） 来输出数据，每个整数占用 size Bytes ；
        f[size] ：利用浮点数值（ floating） 来输出数据，每个数占用 size Bytes ；
        o[size] ：利用八进位（ octal） 来输出数据，每个整数占用 size Bytes ；
        x[size] ：利用十六进制（ hexadecimal） 来输出数据，每个整数占用 size Bytes ；

范例一： 将/usr/bin/passwd的内容使用ASCII方式来展现
[root@VM_0_26_centos ~]# od -t c /usr/bin/passwd

范例二：将/etc/issue这个文件的内容以8进位列出储存值与ASCII的对照表
[root@VM_0_26_centos ~]# od -t oCc /etc/issue
0000000 134 123 012 113 145 162 156 145 154 040 134 162 040 157 156 040
          \   S  \n   K   e   r   n   e   l       \   r       o   n
0000020 141 156 040 134 155 012 012
          a   n       \   m  \n  \n
0000027
# 如上所示，可以发现每个字符可以对应到的数值为何！要注意的是，该数值是 8 进位
# 例如 S 对应的记录数值为 123 ，转成十进制：1x8^2+2x8+3=83。
```


### 修改文件时间或创建新文件：touch

我们在 `ls` 这个指令的介绍时，有稍微提到每个文件在linux下面都会记录许多的时间参数， 其实有三个主要的变动时间，那么三个时间的意义是什么呢？

- modification time （mtime） ： 当该文件的“内容数据”变更时，就会更新这个时间！内容数据指的是文件的内容，而不是文件的属性或权限喔！
- status time （ctime） ： 当该文件的“状态 （status）”改变时，就会更新这个时间，举例来说，像是权限与属性被更改了，都会更新这个时间啊。
- access time （atime） ： 当“该文件的内容被取用”时，就会更新这个读取时间（access） 。举例来说，我们使用 cat 去读取 /etc/man_db.conf ， 就会更新该文件的atime 了。


举例：查看 `/etc/man_db.conf` 这个文件的时间

```shell
[root@VM_0_26_centos ~]# date; ls -l /etc/man_db.conf ;ls -l --time=atime /etc/man_db.conf ; ls -l --time=ctime /etc/man_db.conf
2016年 11月 12日 星期六 14:57:46 CST  # 显示目前的时间
-rw-r--r--. 1 root root 5171 6月  10 2014 /etc/man_db.conf   # 创建内容的时间（mtime）
-rw-r--r--. 1 root root 5171 6月  10 2014 /etc/man_db.conf   # 读取内容的时间（atime）
-rw-r--r--. 1 root root 5171 1月  20 2015 /etc/man_db.conf   # 更新状态的时间（ctime）
```

*默认情况下 `ls` 显示的是该文件的mtime，也就是这个文件内容上次被更改的时间。*

文件的时间是很重要的，因为，如果文件的时间误判的话，可能会造成某些程序无法顺利的运行。

如果我们要修改文件时间则使用 `touch` 命令修改。

```shell
touch - 修改文件的时间戳记.
 touch [-acm][-r ref_file(参照文件)|-t time(时间值)] file(文件名)...
OPTION:
    -a
        修改文件 file 的存取时间，也就是仅修订 access time；
    -c
        仅修改文件的时间，若该文件不存在则不创建新文件；
    -d, --date=time
        后面可以接欲修订的日期而不用目前的日期，也可以使用 --date="日期或时间"
    -m, --time=mtime, --time=modify
        仅修改 mtime ；
    -t STAMP，decimtime
        后面可以接欲修订的时间而不用目前的时间，格式为[YYYYMMDDhhmm]
```

```shell
范例一：新建一个空的文件并观察时间
[xiangcl@VM_0_26_centos ~]$ cd /tmp/
[xiangcl@VM_0_26_centos tmp]$ touch testtouch
[xiangcl@VM_0_26_centos tmp]$ ls -l testtouch
-rw-rw-r-- 1 xiangcl xiangcl 0 11月 14 09:13 testtouch

# 新文件的大小为0。在默认状态下，如果 touch 后接有文件，则该文件的三个时间（atime/ctime/mtime）都会更新为目前的时间，若该文件不存在，则会主动的创建一个新的空文件。

范例二：将 ~/.bashrc 复制为 bashrc ，复制完整的属性，并检查其日期。
[xiangcl@VM_0_26_centos tmp]$ cp -a ~/.bashrc bashrc
[xiangcl@VM_0_26_centos tmp]$ date; ll bashrc; ll --time=atime bashrc; ll --time=ctime bashrc
2016年 11月 14日 星期一 09:21:02 CST                      # 这是当前的时间
-rw-r--r-- 1 xiangcl xiangcl 231 8月   3 00:00 bashrc    # 这是mtime的时间
-rw-r--r-- 1 xiangcl xiangcl 231 8月   3 00:00 bashrc    # 这是atime的时间
-rw-r--r-- 1 xiangcl xiangcl 231 11月 14 09:20 bashrc    # 这是ctime的时间

# 在执行结果中，可以看到数据的内容与属性完全被复制过来，因此文件内容时间（mtime）与原本文件相同，但是由于这个文件是刚刚被创建的，因此状态（ ctime） 就变成现在的时间。

范例三：修改范例二的 bashrc 文件，将日期（ctime）调整为两天前
[xiangcl@VM_0_26_centos tmp]$ date; ll bashrc; ll --time=atime bashrc; ll --time=ctime bashrc;
2016年 11月 14日 星期一 09:46:44 CST
-rw-r--r-- 1 xiangcl xiangcl 231 11月 12 09:46 bashrc
-rw-r--r-- 1 xiangcl xiangcl 231 11月 12 09:46 bashrc
-rw-r--r-- 1 xiangcl xiangcl 231 11月 14 09:46 bashrc
# （atime/ctime）变成了当前时间的 2 天以前，不过 ctime 并没有跟着改变。

范例四：将上个范例的bashrc日期改为 2015/11/11 11:11
[xiangcl@VM_0_26_centos tmp]$ touch -t 201511111111 bashrc
[xiangcl@VM_0_26_centos tmp]$ date; ll bashrc; ll --time=atime bashrc; ll --time=ctime bashrc;
2016年 11月 14日 星期一 09:50:25 CST
-rw-r--r-- 1 xiangcl xiangcl 231 11月 11 2015 bashrc
-rw-r--r-- 1 xiangcl xiangcl 231 11月 11 2015 bashrc
-rw-r--r-- 1 xiangcl xiangcl 231 11月 14 09:50 bashrc
# 时间atime和mtime都改变了，唯独ctime记录了当前的时间。
```

*Note：1、即使我们复制一个文件时，复制所有的属性，但也没有办法复制ctime 这个属性的。 ctime 可以记录这个文件最近的状态 （ status） 被改变的时间。2、平时看的文件属性中，比较重要的还是属于那个 mtime*


#### touch 指令常用情景

- 创建一个空文件
- 修改文件时间（mtime和atime）

















