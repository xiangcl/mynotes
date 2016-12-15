---
  title: 了解Git
  tags:
    - Git
    - 工具使用
  categories: 工具配置
  permalink: tool-config/know-git
---
## git历史

git属于分散型版本管理系统，是为版本管理而设计的软件。

众所周知，Linux内核开源项目有着为数众广的参与者。绝大多数的Linux内核维护工作都花在了提交补丁和保存归档的繁琐事务上(1991-2002)。

很多人都知道，Linus在1991年创建了开源的Linux，从此，Linux系统不断的发展，已经成为了最大的服务器系统软件。

Linus虽然创建了Linux，但Linux的壮大是靠全世界热心的志愿者参与的，这么多人在世界各地为Linux编写代码，那Linux的代码是如何管理的？

实事是，在2002年以前，世界各地的志愿者把源代码文件通过diff方式发给Linus，然后由Linus本人通过手工方式合并代码！

你也许会想，为什么Linus不把Linux代码放到版本控制系统里呢？不是有CVS、SVN这些免费的版本控制系统吗？因为Linus坚定地反对CVS和SVN，这些集中式的版本控制系统不但速度慢，而且必须联网才能使用。有一些商用的版本控制系统，虽然比CVS、SVN好用，但那是付费的，和Linux的开源精神不符。

不过，到了2002年，Linux系统已经发展了十年了，代码库之大让Linus很难继续通过手工方式管理了，社区的弟兄们也对这种方式表达了强烈不满，于是Linus选择了一个商业的版本控制系统BitKeeper，BitKeeper的东家BitMover公司出于人道主义精神，授权Linux社区免费使用这个版本控制系统。

安定团结的大好局面在2005年就被打破了，原因是Linux社区牛人聚集，不免沾染了一些梁山好汉的江湖习气。开发Samba的Andrew试图破解BitKeeper的协议（这么干的其实也不只他一个），被BitMover公司发现了（监控工作做得不错！），于是BitMover公司怒了，要收回Linux社区的免费使用权。

Linus可以向BitMover公司道个歉，保证以后严格管教弟兄们，嗯，这是不可能的。

开发 BitKeeper 的商业公司同 Linux 内核开源社区的合作关系结束，他们收回了免费使用 BitKeeper 的权力。这就迫使 Linux 开源社区（特别是 Linux 的缔造者 Linus Torvalds ）不得不吸取教训，只有开发一套属于自己的版本控制系统才不至于重蹈覆辙。他们对新的系统制订了若干目标：

- 速度
- 简单的设计
- 对非线性开发模式的强力支持（允许上千个并行开发的分支）
- 完全分布式
- 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

最后实际情况是这样的：Linus花了两周时间自己用C写了一个分布式版本控制系统，这就是Git！一个月之内，Linux系统的源码已经由Git管理了！牛是怎么定义的呢？大家可以感受一下。

> 以上故事来自Git诞生背后的一些故事

## 什么是版本控制？

版本管理就是管理更新的历史记录。它为我们提供了一些在软件开发过程中必不可少的功能，例如记录一款软件添加或更改源代码的过程，回滚到特定阶段，恢复误删除的文件等。

### 集中型与分散型

#### 集中型

代表：Subversion

图示：
![](http://i.imgur.com/JFQIGUF.png)

将仓库集中存放在服务器之中，所以只存在一个仓库。

##### 优点：

- 便于管理

##### 缺点：

- 环境限制
- 服务器宕机的危险
- 服务器故障可能导致数据丢失

#### 分散型

图示：
![](http://i.imgur.com/vqDb0zh.png)

GitHub 将仓库 Fork 给了每一个用户。 Fork 就是将 GitHub 的某个特定仓库复制到自己的账户下。 Fork 出的仓库与原仓库是两个不同的仓库，开发者可以随意编辑。

##### 优点：

- 不比需要远程链接仓库
- 便于开发者沟通

##### 缺点：

- 流程复杂
- 学习成本相对较高

### 安装

    $ sudo yum isntall git


### 初始设置

#### 设置姓名和邮箱地址
    
    $ git config --global user.name "Firstname Lastname"
    $ git config --global user.email "your_email@example.com"

这个命令，会在“ ~/.gitconfig”中以如下形式输出设置文件

    [user]
        name = Firstname Lastname
        email = your_email@example.com

这里设置的姓名和邮箱地址会用在 Git 的提交日志中。由于在 GitHub 上公开仓库时，这里的姓名和邮箱地址也会随着提交日志一同被公开。

#### 提高命令输出的可读性

顺便一提，将 color.ui 设置为 auto 可以让命令的输出拥有更高的可读性。

    $ git config --global color.ui auto

“ ~/.gitconfig”中会增加下面一行。
    
    [color]
        ui = auto

这样一来，各种命令的输出就会变得更容易分辨。


