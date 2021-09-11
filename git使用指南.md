# GIt简介

### git的诞生

由Linux之父用C写成，一个分布式版本控制系统

### 集中式VS分布式

#### 集中式版本控制系统：

CVS及SVN都是集中式的版本控制系统

版本库是集中存放在中央服务器的，必须联网才能工作，干活的时候先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。

#### 分布式版本控制系统

分布式版本控制系统没有“中央服务器”，每个人的电脑上都是一个完整的版本库，工作的时候不需要联网

分布式不会因为某地分支出了问题就全盘崩溃，集中式的容易出现单点出了问题，全盘崩溃；

Git有完整的代码版本库，可以离线编辑修改，每个人都有所有的代码，除非每个人都出了问题，就一定会有一个人有完整的代码库，重新创建一个新的库就行，重新上传，但是集中式的，如果服务器崩了，所有人的都会出问题，不完整，删库跑路一个性质。

```java
集中式和分布式的区别是：
你的本地是否有完整的版本库历史！
假设SVN服务器没了，那你丢掉了所有历史信息，因为你的本地只有当前版本以及部分历史信息。
假设GitHub服务器没了，你不会丢掉任何git历史信息，因为你的本地有完整的版本库信息。你可以把本地的git库重新上传到另外的git服务商。
```

**分布式的版本控系统如果要在多个人之间协作不也是需要一个像github一样的的远程版本库吗，这与集中式的有什么区别呢？**

```java
Git 其实就是每个人电脑上都装一个svn服务器，你写了代码提交到自己电脑服务器上就是Commit；但是如果你想多人协作，就要把你的改动发送到你**每一个同事 **的svn服务器上就是push；
    接着按照上面的理解，假如你还有10个同事，你每一次更改都要提交10次，其他同事有更改也要分别向我们提交，是不是觉得好烦，所以我们说干脆找一台固定电脑（服务器）用来统一规定把修改推给这台电脑，这样只需要提交1次就行了，其他人去这台机器上同步就好了。

发现没有，Git的中央服务器可以没有，我们只是为了方便才这么做的。

此时，如果这个中央服务器坏了，你只需要重新弄个电脑，把自己电脑上的同步一份过去，大家约定好都提交到这个新电脑上就行了。【所有的版本和历史都在，因为大家电脑上都是一样的】

而集中式的SVN就不同了，你从中央服务器上下载好完整的代码，正常工作（写代码）是可以，但是如果断网了，你就无法回滚版本；这时你可能谁说，先不提交，等联网了我再提交。

不好意思，这次断网是因为服务器报废了，是蒸发的那种，硬盘灰飞烟灭了。等你再次连上网的时候，就是你永远丢失了历史版本的时候，想回滚就只能靠做梦了。
 **********************************************************************************************************
    这里有几个概念：本地、服务器、中央服务器（远程服务器）。每一次commit是提交到本本机的服务器，这个不需要联网，正所谓的版本管理，就是要方便我们知道每一个版本，比如回到之前的某个版本（这是其一），而且回退到某个之前的版本，也是从本机的服务器拿的数据，这些都不需要联网。而 SVN 的每一次 commit 都需要联网，这就需要网络的等待。 Git只有在Push、pull 的时候需要联网，而我们平时更
多的操作应是commit。再有就是，断网的情况下，SVN也能工作，但是由于没有版本控制的记录，当多人修改
后就比较难以快速的合并，但是Git都在本地保存了版本记录，所以大家合并起来就方便得多了。
    ************************************************************************************************
SVN和Git的最大区别在于版本库或者版本控制的位置在中央服务器还是本地电脑（泛指，可以是本地的任意电脑）。
   
1. 都可以在断网的情况下工作？

如果是一家小公司，SVN服务器就在局域网内，SVN的确可以在断网情况下工作。但是对于中大型公司，SVN服务器一般是独立出来的，不在局域网内的，你完成计划的工作内容，需要提交做成一个版本，但是由于版本库是在中央服务器，于是你要等网络联通之后，才可以提交。因此对于SVN服务器不在局域网内的情况下，断网是不可以使用的。

Git版本库在本地，在完成计划的工作内容后，不管是否联网，都不会影响在本地做成一个版本。

2. 分布式还是集中式都是存在中央服务器概念？

不是的，Git没有中央服务器。中央服务器SVN是用来做版本控制的。Git的版本控制在本地，所以没有中央服务器一说。而Git要多人合作时，可以选择把自己修改的内容即某一版，推给其他合作者，然后其他合作者将该版本合并到自己的修改里，然后在把自己合并后的新版推给其他合作者，这样10个人的团队，假设，每个人都把自己的修改推给其他人，共推了90次，耗时，耗带宽，不如统一规定把修改推给某台设备，这样只修要推10次。

3.Git每次的commit是提交到本地，然后本地记录了自己每次的提交历史记录；而svn的每次commit都是要提交到中央服务器的。那么如果git和svn的许多用户都修改了文件，需要向中央服务器提交新的记录（svn是commit，git是push），此时不是都需要合并所有用户的代码吗，到这的结果都是一样的。 只是说分布式在本地保存了版本记录，用户可以不联网就回溯到历史版本，这个是集中式所没有的。那这样的区别其实在某些场景下是不重要的。这样理解对吗？

关于代码合并，Git和SVN的合并好像没什么区别。 关于区别，回复的内容首部就说了SVN和Git的最大区别在于版本库或者版本控制的位置在中央服务器还是本地电脑。
```

### 安装Git

已经装好，装好后配置姓名和邮箱：

```java
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
//git config 是一个命令
//--global  是一个参数，表示全局，也即这台机器的所有repo都使用同样的名字和邮箱
```

检查是否配置成功：

```java
$ git config user.name//查看姓名
$ git config user.email //查看邮箱
```

### 创建版本库(repo)

版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录，这个空目录即将作为我们的版本库

也不一定必须在空目录下创建Git仓库，选择一个已经有东西的目录也是可以的。

```java
$ mkdir learngit //创建新目录
$ cd learngit	//切换目录
$ pwd	//查看当前所在绝对目录
/Users/michael/learngit
```

第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：

```java
git init //下面一行为gitbash给的反馈，如下图
Initialized empty Git repository in /Users/michael/learngit/.git/
    
//这样就建立了一个空的repo，并且gittest目录下会多一个名为".git ""的隐藏文件夹，这个目录是Git来跟踪管理版本库的，别乱改
```

![屏幕截图 2021-09-10 203718](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-10 203718.png)

#### 把文件添加到版本库

**所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。**

**如果要真正使用版本控制系统，就要以纯文本方式编写文件。**

现在我们编写一个`readme.txt`文件，内容如下：

**一定要放把readme.txt（换句话说，要添加的文件）到刚才创建的repo目录下（子目录也行），因为这是一个Git仓库，放到其他地方Git再厉害也找不到这个文件。**

```
版本0测试
```

然后，第一步：用命令`git add`告诉Git，把文件添加到仓库：

```java
$ git add readme.txt//执行完以后没有gitbash任何反馈，会另起一行供你输下一个命令
    Unix的哲学是“没有消息就是好消息”，说明添加成功。
```

![屏幕截图 2021-09-10 204720](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-10 204720.png)

第二步，用命令`git commit`告诉Git，把文件提交到仓库：

图例乱码应该是编码问题

```java
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
 /*解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，这样你就能从历史记录里方便地找到改动记录。*/
 /*1 file changed：1个文件被改动（我们新添加的readme.txt文件）；2 insertions：插入了两行内容（readme.txt有两行内容）*/
```

![image-20210910205558725](C:\Users\jiangyiqing\AppData\Roaming\Typora\typora-user-images\image-20210910205558725.png)

为什么Git添加文件需要`add`，`commit`一共两步？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：

```java
$ git add file1.txt
$ git add file2.txt file3.txt //一行写多个文件名，中间空格隔开
$ git commit -m "add 3 files."
```

#### 常见问题

```java
输入git add readme.txt，得到错误：fatal: not a git repository (or any of the parent directories)
    Git命令必须在Git仓库目录内执行（git init除外），在仓库目录外执行是没有意义的。
输入git add readme.txt，得到错误fatal: pathspec 'readme.txt' did not match any files。
    添加某个文件时，该文件必须在当前目录下存在，用ls或者dir命令查看当前目录的文件，看看文件是否存在，或者是否写错了文件名。
```

# 版本控制

### 时光穿梭机

我们已经成功地添加并提交了一个readme.txt文件，然后我们继续修改readme.txt文件:

```
版本1修改测试
```

修改完毕，运行`git status`命令看看结果：

```java
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   test.txt

no changes added to commit (use "git add" and/or "git commit -a")
    //git status命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，readme.txt被修改过了，但还没有准备提交的修改。
```

![屏幕截图 2021-09-10 211428](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-10 211428.png)

如果想看看具体修改了什么内容，用`git diff`这个命令看看：

```java
$ git diff readme.txt 
    //文件名与上下文叙述有区别，不影响，红绿两行为修改前后对比，由于使用中文字符只显示了编码
    //git diff顾名思义就是查看difference，显示的格式正是Unix通用的diff格式
```

![屏幕截图 2021-09-10 211807](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-10 211807.png)

![屏幕截图 2021-09-10 212244](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-10 212244.png)

知道了对`readme.txt`作了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步是`git add`：

```java
$ git add readme.txt//“没有消息就是最好的消息”
```

在执行第二步`git commit`之前，我们再运行`git status`看看当前仓库的状态：

```java
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   readme.txt
```

`git status`告诉我们，将要被提交的修改包括`readme.txt`，下一步，就可以放心地提交了：

```java
$ git commit -m "add distributed"
[master e475afc] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)
```

提交后，我们再用`git status`命令看看仓库的当前状态：

```java
$ git status
On branch master
nothing to commit, working tree clean
    //Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working tree clean）的。
```

![屏幕截图 2021-09-10 212630](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-10 212630.png)

### 版本回退

现在修改得到第三个版本，然后add再commit

你不断对文件进行修改，然后不断提交修改到版本库里，就好比玩RPG游戏时，每通过一关就会自动把游戏状态存盘，如果某一关没过去，你还可以选择读取前一关的状态。有些时候，在打Boss之前，你会手动存盘，以便万一打Boss失败了，可以从最近的地方重新开始。Git也是一样，每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为`commit`。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

现在我们已经修改了三个版本的文件

我们用`git log`命令查看文件每次都改了什么内容：

```java
$ git log
//git log命令显示从最近到最远的提交日志
    //如果无法操作，按q退出
```

![屏幕截图 2021-09-10 220258](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-10 220258.png)

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

```java
$ git log --pretty=oneline
86299115f61bc2b1bcda15f3b27feede4b792e11 (HEAD -> master) version2
65f1809347aff2bd88cfb7cbfcca5013b145f99b changed 2 files
81353a020ee297ca482aa161d9e31d66a86aa742 version0 //前面一大串是commit id
7d0f6ee09e7a91f8267c3f0043e8122c2851784d <E6><B7><BB><E5><8A><A0><E4><BA><86><E7><89><88><E6><9C><AC>0
```

现在我们准备把readme`.txt`回退到上一个版本，该怎么办？

在Git中，用`HEAD`表示当前版本，也就是最新的提交，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

我们要把当前版本回退到上一个版本就可以使用`git reset`命令：

```java
$ git reset --hard HEAD^
HEAD is now at 65f1809 changed 2 files
```

![屏幕截图 2021-09-11 100250](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-11 100250.png)

还可以继续回退到上一个版本，不过如果想回去，怎么办？

找到那个想回退的版本的`commit id`于是就可以指定回到未来的某个版本：

Git提供了一个命令`git reflog`用来记录你的每一次命令：用这个命令来找commit id

```
$ git reflog
7d0f6ee (HEAD -> master) HEAD@{0}: reset: moving to HEAD^^
65f1809 HEAD@{1}: reset: moving to HEAD^
8629911 HEAD@{2}: commit: version2
65f1809 HEAD@{3}: commit: changed 2 files
81353a0 HEAD@{4}: commit: version0
7d0f6ee (HEAD -> master) HEAD@{5}: commit (initial): 添加了版本0


$ git reset --hard 65f1809
HEAD is now at 65f1809 changed 2 files
```

![屏幕截图 2021-09-11 100812](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-11 100812.png)

### 工作区与暂存区

### 工作区（Working Directory）

就是你在电脑里能看到的目录，比如我的gittest文件夹就是一个工作区

### 版本库（Repository）

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![0](D:\Pictures Of Markdown\GIt使用手册\0.jpg)

分支和`HEAD`的概念我们以后再讲。

把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

举个栗子：

先对`readme.txt`做个修改，然后，在工作区新增一个`LICENSE`文本文件（内容随便写）。

先用`git status`查看一下状态：

![屏幕截图 2021-09-11 105257](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-11 105257.png)

Git非常清楚地告诉我们，`english.txt`被修改了，而`LICENSE`还从来没有被添加过，所以它的状态是`Untracked`。

现在，使用两次命令`git add`，把`readme.txt`和`LICENSE`都添加后，用`git status`再查看一下：

license 因为先commit后再修改了再add了，所以显示为modified，而不是new file

![屏幕截图 2021-09-11 105809](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-11 105809.png)

`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

![屏幕截图 2021-09-11 105505](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-11 105505.png)

再使用git status查看状态

```
git status
```

![屏幕截图 2021-09-11 110114](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-11 110114.png)

现在暂存区没有任何内容，master分支新增了提交

### 管理修改

Git跟踪并管理的是修改，而非文件。创建一个新文件，也算一个修改。

实验：

第一步，先对English.txt做个修改，添加一行内容

然后add：

```java
$ git add english.txt //add后查看状态

$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   english.txt
```

然后，**再修改**english.txt：注意此时进行第二次修改并且还没有commit

修改完毕后：commit 并查看状态

```
$ git commit -m"add first change"
[master c2c3437] add first change
 1 file changed, 2 insertions(+), 1 deletion(-)

$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   english.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

git只提交了第一次修改，因为只有第一次修改被add了，在commit之前第二次修改没有被add，要把第二次修改也提交给git管理必须先把它add

### 撤销修改

先往English.txt里添加一行，然后我们撤销这一行

先修改：

```
this is a new line to be deleted
```

撤销修改：

如果用`git status`查看一下：

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   english.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Git会告诉你，`git restore <file>  `可以丢弃工作区的修改：

在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

```
从暂存区恢复工作区，

git resotre --worktree FileName.txt

从master恢复暂存区 

git restore --staged fileName.txt

从master同时恢复工作区和暂存区

git restore --source=HEAD --staged --worktree fileName.txt
```

![](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-11 220735.png)

上图是add后如何撤销掉被add进暂存区的内容

### 删除文件

在Git中，删除也是一个修改操作，先添加一个新文件`test.txt`到Git并且提交：

```java
$ git add test.txt

$ git commit -m "add test.txt"
[master b84166e] add test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
```

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：

```
$ rm test.txt
```

这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了：

![屏幕截图 2021-09-11 221515](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-11 221515.png)

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：



**先演示第二种：**

```java
$ git restore test.txt
//文件被恢复，其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
    // 注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！
```

第一种：`git rm`删掉，并且`git commit`：

![屏幕截图 2021-09-11 221913](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-11 221913.png)

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。

# 远程仓库

Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。怎么分布呢？最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分。

你肯定会想，至少需要两台机器才能玩远程库不是？但是我只有一台电脑，怎么玩？

其实一台电脑上也是可以克隆多个版本库的，只要不在同一个目录下。不过，现实生活中是不会有人这么傻的在一台电脑上搞几个远程库玩，因为一台电脑上搞几个远程库完全没有意义，而且硬盘挂了会导致所有库都挂掉，所以我也不告诉你在一台电脑上怎么克隆多个仓库。

实际情况往往是这样，找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交。

完全可以自己搭建一台运行Git的服务器，不过现阶段，为了学Git先搭个服务器绝对是小题大作。好在这个世界上有个叫[GitHub](https://github.com/)的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。

在继续阅读后续内容前，请自行注册GitHub账号。**由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的**，所以，需要一点设置：

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

![屏幕截图 2021-09-11 222704](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-11 222704.png)

```java
cd ~//进入用户主目录
$ ssh-keygen -t rsa -C "youremail@example.com"//创建ssh key
```

把邮件地址换成你自己的邮件地址，一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

![屏幕截图 2021-09-11 223230](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-11 223230.png)

第2步：登陆GitHub，找到account ===》SSH and GHG Keys，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

### 添加远程库

现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作

先在GitHub创建了一个名为test的repo，在GitHub上的这个仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

现在，我们根据GitHub的提示，在本地的`gittest`仓库下运行命令：

```java
git remote add origin https://github.com/CorneliaStreet1/test.git
//添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。
```

下一步，就可以把本地库的所有内容推送到远程库上：

```java
$ git push -u origin main
Enumerating objects: 27, done.
Counting objects: 100% (27/27), done.
Delta compression using up to 12 threads
Compressing objects: 100% (17/17), done.
Writing objects: 100% (27/27), 2.25 KiB | 577.00 KiB/s, done.
Total 27 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/CorneliaStreet1/test.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

![屏幕截图 2021-09-11 225800](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-11 225800.png)

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

**成功了，好耶**

从现在起，只要本地作了提交，就可以通过命令：

```java
$ git push origin master//git上的建议好像不一样
```

把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！![屏幕截图 2021-09-11 231618](D:\Pictures Of Markdown\GIt使用手册\屏幕截图 2021-09-11 231618.png)

关联一个远程库时必须给远程库指定一个名字，`origin`是默认习惯命名；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

### 删除远程库

此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。使用前，建议先用`git remote -v`查看远程库信息：

```
$ git remote -v
origin  https://github.com/CorneliaStreet1/test.git (fetch)
origin  https://github.com/CorneliaStreet1/test.git (push)
```

然后，根据名字删除，比如删除`origin`：

```
$ git remote rm origin
```

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步