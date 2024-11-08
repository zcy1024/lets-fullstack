# Git

## Git概述

Git是一个免费的、开源的**分布式版本控制系统**，可以快速高效地处理从小型到大型的各种项目。

Git易于学习，占地面积小，性能极快。它具有廉价的本地库（在本地磁盘上），方便的暂存区域和多个工作流分支等特性。其性能优于Subversion、CVS、Perforce和ClearCase等版本控制工具。

### 版本控制介绍

版本控制是一种记录文件内容变化，以便将来查阅特定版本修订情况的系统。

版本控制其实最重要的是可以记录文件修改历史记录，从而让用户能够查看历史版本，方便版本切换。

Q：为什么需要版本控制？

A：个人开发过渡到团队协作。

### 版本控制工具

#### 集中式版本控制工具

CVS、SVN(Subversion)、VSS......

集中化的版本控制系统，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。多年以来，这已成为版本控制系统的标准做法。

这种做法带来了许多好处，每个人都可以在一定程度上看到项目中的其他人正在做些什么。而管理员也可以轻松掌控每个开发者的权限，并且管理一个集中化的版本控制系统，要远比在各个客户端上维护本地数据库来得轻松容易。

事分两面，有好有坏。这么做显而易见的缺点是中央服务器的单点故障。如果服务器宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。

#### 分布式版本控制工具

Git、Mercurial、Bazaar、Darcs......

分布式的版本控制系统，客户端提取的不是最新版本的文件快照，而是把代码仓库完整地镜像下来（本地库）。这样任何一处协同工作用的文件发生故障，事后都可以用其它客户端的本地仓库恢复。因为每个客户端的每一次文件提取操作，实际上都是一次对整个文件仓库的完整备份。

分布式的版本控制系统出现之后，解决了集中式版本控制系统的缺陷：

1. 服务器断网的情况下也可以进行开发（因为版本控制是在本地进行的）
2. 每个客户端保存的也都是整个完整的项目（包含历史记录，更加安全）

### Git工作机制

工作区（写代码） --`git add`--> 暂存区（临时存储） --`git commit`--> 本地库（历史版本） --`git push`--> 远程库

工作区和暂存区的东西是可以删除的，但是一旦提交本地库生成历史版本后，该版本将无法被单独删除（除非删除整个库重新开始）。

### Git和代码托管中心

代码托管中心是基于网络服务器的远程代码仓库，一般简称为**远程库**。

局域网：

- GitLab

互联网：

- GitHub（外网）
- Gitee码云（国内网站）

## Git常用命令

### 设置用户签名

- `git config --global user.name <用户名>` => 设置用户名
- `git config --global user.email <邮箱>` => 设置邮箱

说明：签名的作用是用来区分不同操作者身份。用户的签名信息在每一个版本的提交信息中能够看到，以此确认本次提交是谁做的。**Git首次安装必须设置一下用户签名，否则无法提交代码**。

**注意：**这里设置的用户签名和将来登录GitHub（或其它代码托管中心）的账号没有任何关系。

### 初始化本地库

`git init` => 让Git获取目录的管理权<br>它会在当前目录下初始化一个隐藏文件`.git`

### 查看本地库状态

`git status` => 查看状态

键入命令回车后将会显示三块内容：

1. 当前所处的分支
2. 目前是否有提交的内容（本地库没有版本信息）
3. 目前是否有需要提交的内容（工作区、暂存区相关）

此时，新建文件后再次执行命令，第三块内容将提示该文件未被追踪（仅仅存在于工作区），并且提醒你让你使用`git add`命令让该文件被追踪（添加暂存区）。

### 添加暂存区

`git add <file_name>` => 将当前目录下的文件添加到暂存区

`git add .` => 将当前目录下的所有（未被追踪的）文件都添加到暂存区

此时，再次执行查看本地库状态的命令，第三块内容将提示Git已经追踪到了该文件（暂存区），并且提醒你如果你需要移出暂存区，可以使用`git rm --cached <file_name>`<br>**注意：**这条删除命令只是移出暂存区，该文件依旧保存在工作区<br>这个时候，继续查看本地库状态，相关提示将会与上一小节一致。

为了与下一小节内容接壤，请重新将移出暂存区的文件添加进去。

### 提交本地库

`git commit -m "日志信息" <file_name>` => 将暂存区的文件提交到本地库，并且附上对应的日志信息<br>如果没有用`-m`参数注明日志信息，后续依旧会提示你本次提交需要日志信息；如果不指定文件名，将提交暂存区的所有文件。

成功后可以从输出信息中得知一个7位的字符码，也就是该历史版本的版本号。

此时，执行查看本地库状态的命令会得到两块内容：

1. 当前所处的分支
2. 当前工作树是干净的（没有需要再次提交的东西）

`git reflog` => 查看简略版本信息

`git log` => 查看详细版本信息（不止7位的完整版本号）

### 修改文件

修改文件后查看状态，会得到两块内容：

1. 当前所处的分支
2. 当前有需要提交的内容（工作区、暂存区相关）

不难发现，相较于最初的三块内容，这里少了一块，因为本地库已经有了版本信息。

此时，可以通过`git add, git commit`来重复前几节内容，并且可以在途中查看本地库状态来加深理解。

最后，查看版本信息，会发现有两条历史版本的记录，指针`HEAD`指向最新提交的记录。

### 穿梭历史版本

`git reflog` => 找到想要穿梭的版本的版本号复制

`git reset --hard <版本号>` => 穿梭历史版本

此时`git reflog`会自动添加一条穿梭版本的记录，同时指针指向了指定的版本，这一点可以通过查看当前目录下的文件的内容进行核对确认。

## Git分支操作

### 什么是分支

在版本控制过程中，同时推进多个任务，可以为每个任务创建每个任务独立的分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开，开发自己分支的时候，不会影响主线分支的运行。

对于初学者而言，分支可以简单理解为副本，一个分支就是一个单独的副本。（分支底层其实也是指针的引用）

### 分支的好处

并行推进多个功能开发，提高开发效率。

各个分支在开发过程中，如果某一个分支开发失败，不会对其它分支有任何影响。失败的分支删除重新开始即可。

### 分支的操作

#### 查看分支

`git branch -v` => 查看分支<br>如果有多个分支，分支名前打星号的为当前所在分支

#### 创建分支

`git branch <branch_name>` => 创建分支

#### 切换分支

`git checkout <branch_name>` => 切换分支

#### 修改分支

在新分支下修改文件，添加暂存区并且提交本地库。

#### 正常合并分支

切换回原分支，查看文件，发现内容复原了。（因为原分支并没有被修改）

`git merge <branch_name>` => 将指定分支合并到当前分支

再次查看文件内容，发现被修改成了新分支下变化后的内容。

#### 冲突合并分支

在两个分支分别修改文件内容，刻意制造冲突（比如同一个文件同一个位置都有变化），记得都得在各自分支上添加暂存区并提交本地库。

尝试用上一小节的命令合并，报错有冲突，并且会注明是哪个文件有冲突。此时打开文件，会发现冲突位置有类似如下符号：

```
<<<<<<< HEAD
line1 head
line2
=======
line1
line2 test
>>>>>>> hot-fix
```

其中，`HEAD`与`=`之间为当前分支内容，`=`与`hot-fix`之间为将要合并过来的分支的内容。

修改冲突也很简单，只需要删除不想要的，保留想要的，当然，特殊符号也需要删除，例如：

```
line1 head
line2 test
```

保存文件后，将该文件添加暂存区，然后提交本地库。<br>**注意：**这里提交本地库的时候不能写文件名，因为当前处于`MERGING`状态，需要对合并过来的内容一并操作而不只是解决冲突的文件。同时，合并成功后只针对当前分支，另一个分支的内容不会被修改。其本质是指针指向切换。

#### 团队内协作

员工1和员工2都拥有代码托管中心中某一个远程库的权限（视为在同一个团队），他们可以分别将这一个远程库`git clone`到自己的本地库。

在各自的本地库，员工1和员工2分别开发新的功能，任何一方将自己的功能完善之后就可以`git push`到远程库。

此时，另一方就可以通过`git pull`将代码拉取到本地，使自己的本地库与远程库的版本同步。反之同理。

通过这一系列循环往复地操作，员工1和员工2开发的功能都将存储在同一个远程库中。

#### 跨团队协作

书接上回，一个开源项目爱好者发现了这个远程库中存在一个bug，但是，他没有这个库的权限（与员工1员工2不在一个团队），即使修改了bug也没办法直接操作这个远程库存储的内容。

于是，他先将这个远程库`fork`出一份新的远程库，而他拥有这个新远程库的权限。

所以，这位开源项目爱好者通过`git clone`将新的远程库下载到本地，修改完bug之后`git push`回去。

这个时候，在这个新的远程库向员工1员工2所在的团队的远程库创建一个`Pull request`，简称为`pr`。之后，那个团队的负责人就可以看到这个申请，如果他审核完觉得这个开源项目爱好者改得好，就可以将他提的`pr`合并`(merge)`进团队的远程库中。