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

可以多次合并到统一分支， 也可以选择在合并之后直接删除被并入的分支。 

## git log  【查看提交历史】
在使用 Git 提交了若干更新之后，又或者克隆了某个项目，想回顾下提交历史，我们可以使用 git log 命令查看。 
> $ git log
> $ git log --oneline   //--oneline 选项来查看历史记录的简洁的版本。
> $ git log --oneline --graph   //--graph 选项，查看历史中什么时候出现了分支、合并

## git tag 【打标签】
为我们的项目发布一个"1.0"版本。 我们可以用 git tag -a v1.0 命令给最新一次提交打上（HEAD）"v1.0"的标签。-a 选项意为"创建一个带注解的标签"。 不用 -a 选项也可以执行的，但它不会记录这标签是啥时候打的，谁打的，也不会让你添加个标签的注解。 推荐一直创建带注解的标签。
> $ git tag -a v1.0
> $ git tag -a v0.9 85fc7e7   //通过提交标识打历史标签

## git remote  【远程仓库操作】
> $ git remote -v  //要查看当前配置有哪些远程仓库
> $ git remote rm [别名]  //删除远程仓库

## git fetch   【从远程仓库下载新分支与数据】
> $ git fetch  //从远程仓库下载新分支与数据

## git pull   【从远端仓库提取数据并尝试合并到当前分支】
> $ git pull  //从远端仓库提取数据并尝试合并到当前分支,在执行 git fetch 之后紧接着执行 git merge 远程分支到你所在的任意分支

## git push   【推送你的新分支与数据到某个远端仓库】
> $ git push [alias] [branch]  //推送你的新分支与数据到某个远端仓库,将你的 [branch] 分支推送成为 [alias] 远程仓库上的 [branch] 分支


# 重要场景
## 解决合并分支冲突
首先，我们创建一个叫做"change_site"的分支，切换过去，我们将内容改为 www.w3cschool.cn 。

> $ git checkout -b change_site
> Switched to a new branch 'change_site'
> $ vim test.txt 
> $ head -1 test.txt 
> www.w3cschool.cn
> $ git commit -am 'changed the site'
> [change_site d7e7346] changed the site
>  1 file changed, 1 insertion(+), 1 deletion(-)
 

将修改的内容提交到 "change_site" 分支中。 现在，假如切换回 "master" 分支我们可以看内容恢复到我们修改前的，我们再次修改test.txt文件。

> $ git checkout master
> Switched to branch 'master'
> $ head -1 test.txt 
> w3cschool.cn
> $ vim test.txt 
> $ cat test.txt
> w3cschool.cn
> 新增加一行
> $ git diff
> diff --git a/test.txt b/test.txt
> index 704cce7..f84c2a4 100644
> --- a/test.txt
> +++ b/test.txt
> @@ -1 +1,2 @@
>  w3cschool.cn
> +新增加一行
> $ git commit -am '新增加一行'
> [master 14b4dca] 新增加一行
>  1 file changed, 1 insertion(+)
 

现在这些改变已经记录到我的 "master" 分支了。接下来我们将 "change_site" 分支合并过来。

>  $ git merge change_site
> Auto-merging test.txt
> CONFLICT (content): Merge conflict in test.txt
> Automatic merge failed; fix conflicts and then commit the result.
> $ cat test.txt 
> <<<<<<< HEAD w3cschool.cn 新增加一行 ======= www.w3cschool.cn >>>>>>> change_site

我们将前一个分支合并到 "master" 分支，一个合并冲突就出现了，接下来我们需要手动去修改它。

> $ vim test.txt 
> $ cat test.txt 
> www.w3cschool.cn
> 新增加一行
> $ git diff
> diff --cc test.txt
> index f84c2a4,bccb7c2..0000000
> --- a/test.txt
> +++ b/test.txt
> @@@ -1,2 -1,1 +1,2 @@@
> - w3cschool.cn
> + www.w3cschool.cn
>  +新增加一行

在 Git 中，我们可以用 git add 要告诉 Git 文件冲突已经解决

> $ git status -s
> UU test.txt
> $ git add test.txt 
> $ git status -s
> M  test.txt
> $ git commit
> [master 88afe0e] Merge branch 'change_site'

现在我们成功解决了合并中的冲突，并提交了结果。