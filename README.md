# 0 Git的有趣诞生
很多人都知道，Linus在1991年创建了开源的Linux，从此，Linux系统不断发展，已经成为最大的服务器系统软件了。

Linus虽然创建了Linux，但Linux的壮大是靠全世界热心的志愿者参与的，这么多人在世界各地为Linux编写代码，那Linux的代码是如何管理的呢？

事实是，在2002年以前，世界各地的志愿者把源代码文件通过diff的方式发给Linus，然后由Linus本人通过手工方式合并代码！

你也许会想，为什么Linus不把Linux代码放到版本控制系统里呢？不是有CVS、SVN这些免费的版本控制系统吗？因为Linus坚定地反对CVS和SVN，这些集中式的版本控制系统不但速度慢，而且必须联网才能使用。有一些商用的版本控制系统，虽然比CVS、SVN好用，但那是付费的，和Linux的开源精神不符。

不过，到了2002年，Linux系统已经发展了十年了，代码库之大让Linus很难继续通过手工方式管理了，社区的弟兄们也对这种方式表达了强烈不满，于是Linus选择了一个商业的版本控制系统BitKeeper，BitKeeper的东家BitMover公司出于人道主义精神，授权Linux社区免费使用这个版本控制系统。

安定团结的大好局面在2005年就被打破了，原因是Linux社区牛人聚集，不免沾染了一些梁山好汉的江湖习气。开发Samba的Andrew试图破解BitKeeper的协议（这么干的其实也不只他一个），被BitMover公司发现了（监控工作做得不错！），于是BitMover公司怒了，要收回Linux社区的免费使用权。

Linus可以向BitMover公司道个歉，保证以后严格管教弟兄们，嗯，这是不可能的。实际情况是这样的：

Linus花了两周时间自己用C写了一个分布式版本控制系统，这就是Git！一个月之内，Linux系统的源码已经由Git管理了！牛是怎么定义的呢？大家可以体会一下。

Git迅速成为最流行的分布式版本控制系统，尤其是2008年，GitHub网站上线了，它为开源项目免费提供Git存储，无数开源项目开始迁移至GitHub，包括jQuery，PHP，Ruby等等。

历史就是这么偶然，如果不是当年BitMover公司威胁Linux社区，可能现在我们就没有免费而超级好用的Git了。

# 1 Git简介
Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。

**Git与SVN区别**
+ Git 是分布式的，SVN 不是：这是 Git 和其它非分布式的版本控制系统，例如 SVN，CVS 等，最核心的区别。
+ Git 把内容按元数据方式存储，而 SVN 是按文件：所有的资源控制系统都是把文件的元信息隐藏在一个类似 .svn、.cvs 等的文件夹里。
+ Git 分支和 SVN 的分支不同：分支在 SVN 中一点都不特别，其实它就是版本库中的另外一个目录。
+ Git 没有一个全局的版本号，而 SVN 有：目前为止这是跟 SVN 相比 Git 缺少的最大的一个特征。
+ Git 的内容完整性要优于 SVN：Git 的内容存储使用的是 SHA-1 哈希算法。这能确保代码内容的完整性，确保在遇到磁盘故障和网络问题时降低对版本库的破坏。

**基本概念**
+ 工作区：就是你在电脑里能看到的目录。
+ 暂存区：英文叫stage, 或index。一般存放在 ".git目录下" 下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
+ 版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

![Git业务流程](https://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg "Git业务流程" )
 图中左侧为工作区，右侧为版本库。在版本库中标记为 "index" 的区域是暂存区（stage, index），标记为 "master" 的是 master 分支所代表的目录树。

图中我们可以看出此时 "HEAD" 实际是指向 master 分支的一个"游标"。所以图示的命令中出现 HEAD 的地方可以用 master 来替换。

图中的 objects 标识的区域为 Git 的对象库，实际位于 ".git/objects" 目录下，里面包含了创建的各种对象及内容。

+ git add：暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
+ git commit：暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。
+ git reset HEAD：暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
+ git rm --cached <file>：直接从暂存区删除文件，工作区则不做出改变。
+ git checkout或git checkout -- <file>：用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。
+ git checkout HEAD ." 或 "git checkout HEAD <file>" 命令时，用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。 


# 命令概览
![Git常用命令速查表](https://7n.w3cschool.cn/attachments/image/20170206/1486348362884912.jpg)

# Git基本操作
## git init  【获取与创建项目命令】
用 git init 在目录中创建新的 Git 仓库。 你可以在任何时候、任何目录中这么做，完全是本地化的。在目录中执行 git init，就可以创建一个 Git 仓库了。

## git clone   【拷贝一个 Git 仓库到本地】
使用 git clone 拷贝一个 Git 仓库到本地，让自己能够查看该项目，或者进行修改。如果你需要与他人合作一个项目，或者想要复制一个项目，看看代码，你就可以克隆那个项目。 
>  git clone [url]
>  $ git clone git://github.com/schacon/simplegit.git

## git add 【将该文件添加到缓存】
新项目中，添加所有文件很普遍，可以在当前工作目录执行命令：git add .。
> $ git add .
> $ git status -s
> A  README
> A  hello.php

## git status  【查看项目的当前状态】
通过该命令可以查看当前项目文件的状态信息,加了 -s 参数，以获得简短的结果输出
>$ git status -s
>   M README.md

## git diff  【显示已写入缓存与已修改但尚未写入缓存的改动的区别】
git diff 命令显示已写入缓存与已修改但尚未写入缓存的改动的区别。git diff 有两个主要的应用场景。

+ 尚未缓存的改动：git diff
+ 查看已缓存的改动： git diff --cached
+ 查看已缓存的与未缓存的所有改动：git diff HEAD
+ 显示摘要而非整个 diff：git diff --stat

## git commit  【提交本地仓库】
使用 git add 命令将想要快照的内容写入了缓存， 而执行 git commit 记录缓存区的快照。Git 为你的每一个提交都记录你的名字与电子邮箱地址，所以第一步需要配置用户名和邮箱地址。
> $ git config --global user.name 'w3cschool'
> $ git config --global user.email w3c@w3cschool.cn
> $ git add hello.php
> $ git status -s
> A  README
> A  hello.php
> $ git commit -m 'test comment from w3cschool.cn'

## git reset   【用于取消缓存已缓存的内容】
 这里我们有两个最近提交之后又有所改动的文件。我们将两个都缓存，并取消缓存其中一个。

> $ git status -s
>  M README
>  M hello.php
> $ git add .
> $ git status -s
> M  README
> M  hello.pp
> $ git reset HEAD -- hello.php 
> Unstaged changes after reset:
> M hello.php
> $ git status -s
> M  README
>  M hello.php

现在你执行 git commit 将只记录 README 文件的改动，并不含现在并不在缓存中的 hello.rb。

## git rm  【将文件从缓存区中移除】
如我们删除 hello.php文件：

>  $ git rm hello.php 

默认情况下，git rm file 会将文件从缓存区和你的硬盘中（工作目录）删除。 如果要在工作目录中留着该文件，可以使用命令：
> git rm --cached。

## git branch 【分支操作】
> git branch   //列出分支
> git branch (branchname)   //创建名为branchname的分支
> git branch -d (branchname)   //删除名称为branchname的分支

## git checkout  【切换分支】
> git checkout (branchname)

切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。

## git merge  【合并分支】
> git merge 

## 

可以多次合并到统一分支， 也可以选择在合并之后直接删除被并入的分支。 

# 2 功能场景  
## Git创建仓库
### 通过git init
Git 使用 git init 命令来初始化一个 Git 仓库，在执行完成 git init 命令后，Git 仓库会生成一个 .git 目录，该目录包含了资源的所有元数据，其他的项目目录保持不变（不像 SVN 会在每个子目录生成 .svn 目录，Git 只在仓库的根目录生成 .git 目录）。

> $ git init 或者 git init newrepo
> $ git add *.c
> $ git add README
> $ git commit -m '初始化项目版本'

### 通过git clone
使用 git clone 从现有 Git 仓库中拷贝项目（类似 svn checkout）。 
> $ git clone repo 

## 修改文件
修改文件后可以通过下面的命令查看仓库当前的状态和比对修改点
> git status //查看仓库当前的状态
> git diff filename

在通过增加和提交命令提交版本
> $ git add 
> $ git commit

## 版本回退
通过下面的命令查看历史记录
> $ git log

如果我们想把 readme.txt 文件退回到上一个版本，就可以使用下面的命令
> $ git reset --hard HEAD^ //HEAD表示当前版本，则HEAD^表示上一个版本，那么上上版本就是HEAD^^

我们现在想要回到最新的版本，还是使用reset命令，如下
> $ git reset --hard 1094a //这里不能用HEAD而必须使用 commit id ，因为最新版本在之前返回时已经被删除了，1094a就是最新版本的 commit id，可以在之前的代码中查到

## 撤销修改
### 没有提交到缓存区前
可以手动删除最后一行，手动把文件恢复到上一个版本的状态。然后使用checkout命令丢弃缓存区的修改
> git checkout -- readme.txt //把readme.txt文件在工作区的修改全部撤销。

### 已经提交到了缓存区，但是没有提交到本地仓库中
我们可以使用 git reset HEAD file 命令把把暂存区的修改撤销掉，重新放回工作区,再丢弃工作区的修改就OK了
> $ git reset HEAD readme.txt //git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区，HEAD表示最新版本。
> $ git checkout -- readme.txt //丢弃工作区的修改。

## 删除文件
### 从本地仓库中删除文件
> $ git rm test.txt
> $ git commit -m "remove test.txt"

### 本地文件删除错了
文件被删错了。因为版本库里有，所以很好恢复
> $ git checkout -- test.txt //用版本库里的版本替换工作区的版本。

## 

+ 添加文件
+ 提交文件

