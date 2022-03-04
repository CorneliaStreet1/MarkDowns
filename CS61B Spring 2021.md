BSTCS61B Fall 2020

算是二刷吧。神课总是值得二刷的。

# Lecture 1 Intro, Hello World Java

关于课程安排

![image-20211113110720868](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111131107020.png)

![image-20211113110344006](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111131103227.png)

![image-20211113110513998](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111131105163.png)

![image-20211113111554851](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111131115018.png)

![image-20211113113127127](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111131131283.png)

# Project 0

The skeleton exhibits two *design patterns* in common use: the Model-View-Controller Pattern (MVC), and the Observer Pattern.

The MVC pattern divides our problem into three parts:

- The **model** represents the subject matter being represented and acted upon – in this case incorporating the state of a board game and the rules by which it may be modified. Our model resides in the `Model`, `Side`, `Board`, and `Tile` classes. The instance variables of `Model` fully determine what the state of the game is. Note: You’ll only be modifying the `Model` class.
- A **view** of the model, which displays the game state to the user. Our view resides in the `GUI` and `BoardWidget` classes.
- A **controller** for the game, which translates user actions into operations on the model. Our controller resides mainly in the `Game` class, although it also uses the GUI class to read keystrokes.

The MVC pattern is not a topic of 61B, nor will you be expected to know or understand this design pattern on exams or future projects.

The second pattern utilized is the “Observer pattern”. Basically this means that the **model** doesn’t actually report changes to the **view**. Instead, the **view** *registers* itself as an *observer* of the `Model` object. This is a somewhat advanced topic so we will provide no additional information here.

# Lab2

- 关于`Junit`的测试

```java
import static org.junit.Assert.*;
import org.junit.Test;
@Test
public void testMethod() {
assertEquals(<expected>, <actual>);
}
```

```java
public class SquarePrimesTest {
import static org.junit.Assert.*;
import org.junit.Test;

/**
* Here is a test for isPrime method. Try running it.
* It passes, but the starter code implementation of isPrime
* is broken. Write your own JUnit Test to try to uncover the bug!
*/
@Test
public void testSquarePrimesSimple() {
IntList lst = IntList.of(14, 15, 16, 17, 18);
boolean changed = IntListExercises.squarePrimes(lst);
assertEquals("14 -> 15 -> 16 -> 289 -> 18", lst.toString());
assertTrue(changed);
}
}
```

# Lecture 5 SLLists, Nested Classes, Sentinel Nodes

- 2D数组采用的也是行优先，每一个数组单元存储的是某一行数组的指针。

# Lecture 7  ALists, Resizing, vs. SLists

- ArrayList，删除的时候不要`lazy deletion`，因为使用这种策略会导致要删除的内容仍然被保留
- 如下图，要删除item[2]，如果只是改变size，那么那副红色图片仍然存在。

![屏幕截图 2021-11-14 215524](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111142156560.png)

# Lecture 9  Extends, Casting, Higher Order Functions

- HOFDemo:

![image-20211120222142113](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111202221356.png)

# Lab 4：Debugging and Using Git

- Unstage a file that you haven’t yet committed:

```
$ git reset HEAD [file]
```

This will take the file’s status back to modified, leaving changes intact. Don’t worry about this command undoing any work. This command is the equivalent of deleting one of the temporary images that you’re going to combine into a panorama.

Why might we need to use this command? Let’s say you accidentally started tracking a file that you didn’t want to track. (an embarrassing video of yourself, for instance.) Or you were made some changes to a file that you thought you would commit but no longer want to commit quite yet.

Please note that this new amended commit will replace the previous commit.

- Revert a file to its state at the time of the most recent commit:

```
$ git checkout -- [file]
```

This next command is useful if you would like to actually undo your work. Let’s say that you have modified a certain `file` since committing previously, but you would like your file back to how it was before your modifications.

**Note**: This command is potentially quite dangerous because any changes you made to the file since your last commit will be removed. Use this with caution!

- Amend latest commit (changing commit message or add forgotten files):

```
$ git add [forgotten-file]
$ git commit --amend
```

Please note that this new amended commit will replace the previous commit.



- `git clone [remote-repo-URL]`: Makes a copy of the specified repository, but on your local computer. Also creates a working directory that has files arranged exactly like the most recent snapshot in the download repository. Also records the URL of the remote repository for subsequent network data transfers, and gives it the special remote-repo-name “origin”.
- `git remote add [remote-repo-name] [remote-repo-URL]`: Records a new location for network data transfers.
- `git remote -v`: Lists all locations for network data transfers.
- `git pull [remote-repo-name] master`: Get the most recent copy of the files as seen in remote-repo-name
- `git push [remote-repo-name] master`: Pushes the most recent copy of your files to the remote-repo-name.

## Git Branching

![graph1](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111241550013.svg)

Every command that we’ve covered so far was working with the default branch. This branch is conventionally called the `master` branch. However, there are cases when you may want to create branches in your code.

Branches allow you to keep track of multiple different versions of your work simultaneously. One way to think of branches are as alternate dimensions. Perhaps one branch is the result of choosing to use a linked list while another branch is the result of choosing to use an array.

![graph2](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111241552237.svg)

### Creating, Deleting, & Switching Branches

A special branch pointer called the `HEAD` references the branch you currently have as your working directory. Branching instructions modify branches and change what your `HEAD` points to so that you see a different version of your files.

- The following command will create a branch off of your current branch.

```
$ git branch [new-branch-name]
```

- This command lets you switch from one branch to another by changing which branch your `HEAD` pointer references.

```
$ git checkout [destination-branch]
```

By default, your initial branch is called `master`. It is advised that you stick with this convention. Every other branch, however, can be named whatever you’d like. It’s generally a good idea to call your branch something descriptive like `fixing-ai-heuristics` so that you can remember what commits it contains.

- You can combine the previous two commands on creating a new branch and then checking it out with this single command:

```
$ git checkout -b [new-branch-name]
```

- You can delete branches with the following command:

```
$ git branch -d [branch-to-delete]
```

- You can easily figure out which branch you are on with this command:

```
$ git branch -v
```

More particular, the `-v` flag will list the last commit on each branch as well.

### Merging

There are often times when you’d like to merge one branch into another. For example, let’s say that you like the work you’ve done on `fixing-ai-heuristics`. Your AI is now super-boss, and you’d like your `master` branch to see the commits you’ve made on `fixing-ai-heuristics` and delete the `fixing-ai-heuristics` branch.

![graph3](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111241614066.svg)

In this case, you should checkout the `master` branch and merge `fixing-ai-heuristics` into `master`.

```
$ git checkout master
$ git merge fixing-ai-heuristics
```

This `merge` command will create a new commit that joins the two branches together and change each branch’s pointer to reference this new commit. While most commits have only one parent commit, this new merge commit has two parent commits. The commit on the `master` branch is called its *first parent* and the commit on the `fixing-ai-heuristics` branch is called its *second parent*.

### Merge Conflicts

It may happen that two branches you are trying to merge have conflicting information. This can occur if commits on the two branches changed the same files. Git is sophisticated enough to resolve many changes, even when they occur in the same file (though distinct places).

However, there are times that conflicts cannot be resolved by Git because changes impact the same methods/lines of code. In these cases, it will present both changes from the different branches to you as a *merge conflict*.

### Resolving Merge Conflicts

Git will tell you which files have conflicts. You need to open the files that have conflicts and resolve them manually. After doing this, you must commit to complete the merge of the two branches.

The files with conflicts will contain segments that look something like this:

```java
<<<<<<< HEAD
for (int i = 0; i < results.length; i++) {
println(results[i]);
println("FIX ME!");
}
=======
int[] final = int[results.length];
for (int i = 0; i < results.length - 1; i++) {
final[i] = results[i] + 1;
println(final[i]);
}
>>>>>>> fixing-ai-heuristics
```

Basically, you’ll see two segments with similar pieces of code:

1. The top code snippet is from the branch you originally had checked out when you ran the `merge` command. It’s called `HEAD` because the `HEAD` pointer was referencing this branch at the time of the `merge`. Continuing our example above, this code would be from the `master` branch.
2. The bottom code snippet is from the branch you were merging into your checked out branch. This is why it shows that the code is from `fixing-ai-heuristics`.

Basically, you’ll need to go through all marked sections and pick which snippet of code you’d like to keep.

In the previous example, I like the bottom piece of code better because I just fixed the AI while the top piece still prints “FIX ME!” Thus, I will delete the top segment as well as the extraneous lines to get this:

```java
int[] final = int[results.length];
for (int i = 0; i < results.length - 1; i++) {
final[i] = results[i] + 1;
println(final[i]);
}
```

Doing this for all segments demarcated by conflict-resolution markers resolves your conflict. After doing this for all conflicting files, you can commit. This will complete your merge.

## Remote Repositories (Advanced)

#### Adding Remotes

Adding a remote repository means that you are telling git where the repo is located. You do not necessarily have read/write access to every repo you can add. Actually accessing and modifying files in a remote is discussed later and relies on having added the remote.

```
$ git remote add [short-name] [remote-url]
```

The remote URL will look something like `https://github.com/berkeley-cs61b/skeleton.git` if you are using HTTP or `git@github.com:berkeley-cs61b/skeleton.git` if you are using SSH.

By convention, the name of the primary remote is called `origin` (for original remote repository). So either of the following two commands would allow me to add the `berkeley-cs61b/skeleton` repository as a remote.

```
$ git remote add origin https://github.com/berkeley-cs61b/skeleton.git
$ git remote add origin git@github.com:berkeley-cs61b/skeleton.git
```

After adding a remote, all other commands use its associated short name.

#### Renaming, Deleting, & Listing Remotes

- You can rename your remote by using this command:

```
$ git remote rename [old-name] [new-name]
```

- You can also remove a remote if you are no longer using it:

```
$ git remote rm [remote-name]
```

- To see what remotes you have, you can list them. The `-v` flag tells you the URL of each remote (not just its name).

```
$ git remote -v
```

You can read more about [working with remotes](http://git-scm.com/book/en/Git-Basics-Working-with-Remotes) in the Pro Git book.

#### Cloning a Remote

There are often remote repos with code that you would like to copy to your own computer. In these cases, you can easily download the entire repo with its commit history by cloning the remote:

```
$ git clone [remote-url]
$ git clone [remote-url] [directory-name]
```

The top command will create a directory of the same name as the remote repo. The second command allows you to specify a different name for the copied repository.

When you clone a remote, the cloned remote become associated with your local repo by the name `origin`. This is by default because the cloned remote was the `origin` for your local repo.

#### Pushing Commits

You may wish to update the contents of a remote repo by adding some commits that you made locally. You can do this by `pushing` your commits:

```
$ git push [remote-name] [remote-branch]
```

Note that you will be pushing to the remote branch from the branch your `HEAD` pointer is currently referencing. For example, let’s say that I cloned a repo then made some changes on the `master` branch. I can give the remote my local changes with this command:

```
$ git push origin master
```

- `pull`: This is equivalent to a `fetch + merge`. Not only will `pull` fetch the most recent changes, it will also merge the changes into your `HEAD` branch.

```
$ git pull [remote-name] [remote-branch-name]
```

Let’s say that my boss partner has pushed some commits to the `master` branch of our shared remote that fix our AI heuristics. I happen to know that it won’t break my code, so I can just pull it and merge it into my own code right away.

```
$ git pull origin master
```

# Project 1





**这些都是没有更新的，有一些思考可能有点问题，于2022.2.1重新写了一遍Project1但是并没有更新笔记**

**关于Resize的部分的思考还是看源代码的注释比较好。**

## ArrayDeque

### Resize的问题

#### ResizeBigger()

- 从旧数组到新数组的数据迁移过程：
- ArrayCopy直接从旧数组的头开始，到尾结束，全部放入新数组即可

#### ResizeSmaller()

- 从旧数组到新数组的数据迁移过程：
- 分情况讨论：

### NextFirst和NextLast问题

- 在resize()之后，原来的Deque都是按逻辑上从头到尾的顺序放到了新的数组之中
- 也就是说，Deque的最前面一个元素在新数组的第0位
- Deque的最后一个元素在新数组的第size - 1位
- 所以resize之后NextFirst为`newArray.length - 1`
- NextLast为`size`

# Lecture 12 Command Line Programming, Git, Project 2 Preview

![image-20220207214522734](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202072145953.png)



- **第0个参数并不是程序本身的名字，就是第0个命令行参数。**
- **每个命令行参数都是一个字符串，即便是阿拉伯数字字符(1,2,3..)，也当做字符串处理。**

![image-20220207214653555](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202072146814.png)



![image-20220207215732040](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202072157251.png)

![image-20220207221949507](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202072219185.png)

![image-20220207222226481](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202072222745.png)

![image-20220207222532275](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202072225521.png)

![image-20220207222943567](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202072229836.png)

![image-20220207231440898](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202072314115.png)

![image-20220207230827693](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202072308959.png)

![image-20220207231055512](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202072310764.png)

![image-20220207231538016](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202072315213.png)

# Lecture 16 ADTs, Sets, Maps, BSTs

![image-20211129152330533](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111291523746.png)

- 关于删除节点：
  - 要删除的节点只有一个child或没有child都好处理。
    - 一个child的情况下，让待删除节点的parent指向其child。
    - **不要采用将待删除节点的child的<Key,Value>覆盖到待删除节点的方法来删除节点，可能会出错。就老实一点。**
      - 非要采用的话记得将child的left和right指针也覆盖到待删除节点的对应项去。

![image-20220211220025327](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202112200615.png)

- 关于图里的why：如果不是只有一个或零个child，那么我们选中的这个节点(elf或cat)，就不是左子树最大或右子树最小的，cat的左child会比它更小，而elf的右子树会比它更大。

![image-20211129163032866](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111291630070.png)

- BST as Map

![image-20211129164224093](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111291642277.png)

- Summary

![image-20211129164907210](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111291649416.png)



# Lecture 17 B-Trees (2-3, 2-3-4 Trees)

![image-20211129190158416](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111291901605.png)

![image-20211129195845383](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111291958573.png)

![image-20211129200834736](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111292008914.png)



![image-20220209210924717](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202092109993.png)

![image-20220209211029119](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202092110348.png)



![](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202092208090.png)

![image-20220209221622581](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202092216800.png)

# Lecture 18 Red Black Trees

- Bad news About B-Tree：
  - 需要维护不同的节点类型（一个节点里可能有1、2、3）个值。
  - 不同节点类型直接的转换，如L等于2时，临时3值节点到2值节点的转换。

![image-20220213201847414](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202132018686.png)

- 因此引出红黑树，更加简单且更加快的数据结构。
- 通过树的旋转，可以在具有相同节点个数的树的不同状态间转换。如下图可以通过旋转在这5中状态间转换。
  - 还有一个描述具有N个节点的二叉树具有的可能的形态的个数的`Catalan(N)`
  - 可以在`2n - 6`次旋转之内从一个状态转移到任何其他可能的状态。
    - 因此如果给定一棵极度不平衡的树，存在一个旋转的序列将它变成一棵平衡的树。

![image-20220213202542968](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202132025209.png)

- 树的旋转：
  - 以左旋为例：记X为G的右子节点，旋转使得G称为X的新左子节点。
  - 下图示例，可以看作G和P暂时合并，然后再分裂。
  - 仍然会保持BST树的属性，在这个示例中树的高度增加了。

![image-20220213203229765](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202132032006.png)

- 右旋： 记X为P的左子节点，旋转使得P成为X的新右子节点。
- 示例降低了树的高度。

![image-20220213203618068](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202132036291.png)

- 一个示例，说明如何通过一系列旋转使得树变成平衡状态。

![image-20220213203909365](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202132039570.png)

- 红黑树的定义：
  - BST树：实现起来容易，会变得不平衡，但是可以使用旋转来平衡，但是目前我们还没有算法。
  - 2-3树：从结构上来说就是平衡的，也就是不需要旋转。
  - 试图结合起来二者的优点：建立一棵BST树，使得它在结构上与一棵2-3树完全对应。由于2-3树是平衡的，我们这棵奇怪的BST树也会是。
    - 因此引出，如何用一棵BST树来表现一棵2-3树。

![image-20220214202037658](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142020885.png)

- Representing a 2-3 tree as a BST：
  - 只有两个子节点的2-3树和BST是一模一样的。关键是具有3个子节点的2-3树怎么办。

![image-20220214202538205](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142025432.png)

- 使用**glue link**来呈现`3-node`，将这个`glue-link`标记为红色。正常的`link`标记为黑色。

![image-20220214203012019](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142030262.png)



- 关于红黑树的一些说明：
  - 在课程中我们永远选择把假象出来的`glue-link`放在靠左边，称作左倾红黑树。
  - LLRB就是正常的BST树。
  - 在一棵2-3树和一棵LLRB之间存在1-1对应。
  - 红色的`link`只是一个方便的假想，红色链接并不会起任何特殊的作用。

![image-20220214204602504](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142046764.png)

- 红黑树的一些属性：
  - 查找key值，把红黑树当成一棵BST来对待就好。
  - 左倾红黑树的属性看图。
  - **红黑树的高度的最大值大概就是与之相对应的2-3树的高度的2倍，因此红黑树的高度也是对数的。**

![image-20220214214057737](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142140986.png)

- 关于红黑树的合法性：
  - 第三个不是合法的是因为它本身对应的2-3树不是平衡的（一棵B树的叶子层永远是等深度的，所以第二个也是不平衡的）。

![image-20220214205941044](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142059290.png)

- **红黑树的建造：**
  - 并不是先建立一棵2-3树然后在转换成一棵红黑树。
  - 而是插入一棵BST树然后通过旋转保持**左倾**红黑树属性。

![image-20220214214503767](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142145009.png)

- 建造红黑树的例子：插入的时候假装自己是那棵与自己对应的2-3树。
  - 例子1：**无论何时，对一棵红黑树插入一个节点时，都是使用红色的链接（也许后面可能会进行翻转，但是插入的那一瞬间就是用红色的）。**

![image-20220214215315775](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142153009.png)

- 例子2：
  - 插入S的时候用`红色link`（假装部分，如果你是2-3树你会怎么做），**由于我们要得到左倾红黑树，所以对第二种形态进行旋转**（旋转部分）。

![image-20220214215032069](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142150307.png)

- **例子3**：由于B树在插入时会出现那种暂时的4-node（也就是2-3树的某个节点会暂时性的出现有3个值在里面，然后随后进行节点的分裂）
  - 这个例子就是在相应的红黑树中也允许临时性的出现这种不合法的情况。 
  - 使用具有两个`暂时性的红色link`的节点来表现这种情况。
  - **并且两个红色的link一个左倾一个右倾，也就是说允许暂时性的破坏左倾要求。**
  - **并且只允许同一个节点的两个红色link一个左倾一个右倾，不能出现连续两个左倾redlink。**

![image-20220214215658911](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142156167.png)

- **例子4**：连续的两个左倾红色`link`怎么处理。
  - 这个例子第一个关键点：按照BST的规则插入E之后是第二个状态，属于是暂时性的4-node，但是这个状态和上个例子所提到的长得不一样。
  - 所以进行旋转。但是还是没有到B树分裂的那一步。
  - **关于为什么要进行这一步的旋转：因为我们的最终目的是得到稳定形态的2-3树，而不是现在这个具有临时4-node的B树。旋转之后得到的第三个状态是可以通过`flip()`得到最终目的的，这一步的旋转就是为了将形态二变成可以翻转的形态3**

![image-20220214220057683](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142200933.png)

- 例子5：由于暂时性的`4-node`在随后是需要进行`split()`的，也就是节点分裂，所以红黑树也需要去表现相应的过程。
  - 第二个关键点：如何去处理临时性非法的红黑树。也就是如何在得到第三个状态之后，去模仿2-3树的分裂过程。
  - 进行颜色的翻转。将所有与节点B接触的`link`的颜色翻转。
  - 如何想到的？
  - 很简单，画出分裂后的2-3树的形态，然后画出它对应的左倾红黑树的形态，观察一下就会发现节点B的各个`link`的颜色刚好翻转了。

![image-20220214220734495](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142207754.png)

- 至此，我们就发明了红黑树。一些关于上述例子的总结。
  - 红色链接右倾：左旋。
  - 连续两个左倾红色链接：右旋。
  - 红色链接一左一右：颜色翻转。
  - 此外还有最后一个细节，有可能一次旋转或者翻转操作会导致一个额外的需要修复的错误。
    - 比如如果翻转后得到的`redlink`恰好是右倾而不是左倾的。(例子5翻转后的`redlink`恰好是左倾的。)
    - 例子6。

![image-20220214222052882](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142220136.png)



- 例子6。
  - 翻转模仿B树分裂过程，修复临时性4-node问题。但是出现了新的问题，新得到的`redlink`是右倾而不是左倾的。
  - 所以左旋B。

![image-20220214222426009](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142224274.png)

![image-20220214222557901](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142225156.png)

- [LLRB/2-3TreeInsertionDemo](https://docs.google.com/presentation/d/1jgOgvx8tyu_LQ5Y21k4wYLffwp84putW8iD7_EerQmI/edit#slide=id.g463de7561_042)

- 总结：
  - 运行时间基本上都是`logN`。
  - 删除不讲。较难
  - Java的内置TreeMap是红黑树，只不过是对2-3-4树的映射。也不是一一对应的，更复杂的工作，更快的速度。
  - 还有其他很多种类型的树，实现Map也还有其他很多种方法，比如Hash。

![image-20220214232437080](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142324312.png)

![image-20220214232817837](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142328091.png)

![image-20220214233239561](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202142332800.png)

# Lecture 19 Hashing

- BST、BT、LLRBT用作Set的缺点：为了解决这些缺点我们引出hashtable

![image-20211129211446353](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111292114572.png)



- 关于由于整数溢出导致的理论上的哈希值达不到且与其他对象的哈希值恰好冲突的可能
- 下面的例子里的俩字符串的哈希值由于整数溢出，变成一样的。

![image-20211129214319152](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111292143340.png)

- hash要解决的两个问题

![image-20211129214821320](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111292148464.png)

- final idea,about the bucket,could be anything we can iterate.ANY collenction.

![image-20211201151754231](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011518477.png)

- work flow:Use LinkedList as bucket：
  - how to search for existed items
  - how to add items.

![image-20211201152259183](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011522375.png)

- add（insert）的最坏复杂度也是O(Q)，因为我们必须在插入前遍历表来确定要插入的元素是否事先存在

![image-20211201152646504](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011526681.png)

- 使用取余的方法的downside:
  - 表变长，当然，可以通过resize改进

![image-20211201152900208](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011529395.png)

- 哈希表的工作流程：
  - 稍后说明如何处理负的hash code

![image-20211201153116249](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011531428.png)

- Performing Analysis：

![image-20211201153255434](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011532598.png)

- 一个小小的问题 反映出哈希表的一个弊端
  - bucket could get very long
  - 一千万个item，最好的情况下也是每个链表两百万个item，插入与查询变得很慢

![image-20211201154301213](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011543396.png)

- 提高：由于复杂度与Q有关，而Q与item的个数N成线性关系，假设我们有M个bucket，最好的情况下Q = N / M
  - 所以可不可以设法保证Q是O(1)的？如果可以，那么线性优化成了常数

![image-20211201154610625](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011546836.png)

- 解决上述问题的方法就是，我们不采用固定个数的bucket，而是让bucket数也跟着涨
  - 当N / M大于某一个装载因子的时候，我们增加更多的bucket数(也就是让`ArrayList<LinkdedList>`的size变大)

![image-20211201154834277](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011548455.png)

- 一个`resize()`的例子，值得注意的是增加bucket数目后各个item的位置变化
  - 使得items move around ，lists get shorter
  - resize前后各个item的位置取决于新的M和各自的hashcode，原来不在一个bucket的可能变得在一个bucket，而原来处于同一个bucket的item可能会分开。

![image-20211201155228222](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011552397.png)

- 运行时间分析
  - 总体来说变成了O(1)，对于触发resize的情况我们另做分析，但是均摊下来就是常数时间

![image-20211201155741948](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011557124.png)

![image-20211201155940650](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011559833.png)

- Summary

![image-20211201160220900](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011602067.png)

- item的平均分布对哈希表的运行时间保持常数时间是至关重要的。下图右侧比左侧好，但二者的装载因子是一样的
- 接下来将会讨论如何确保items平均分布，但首先讨论一下java的哈希表实践 

![image-20211201160345217](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011603386.png)

- hashtable in java
  - hashCode()的默认方法返回对象的内存地址

![image-20211201161106661](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011611840.png)

- 实现java中的对hashcode取模的运算，由于`%`的语义问题，处理负数的时候会出错，使用另外一个函数：
  - `Math.floorMod()`

![image-20211201161641506](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011616702.png)

- java  Hashtable workflow：

![image-20211201161848268](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011618445.png)

- 使用hashmap和hashset的一些警告

![image-20211201162348018](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011623210.png)

- 关于如何写出一个好的hashCode函数：使得items分布均匀的关键

![image-20211201163125281](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011631475.png)

- String hashCode()

![image-20211201163151266](C:/Users/jiangyiqing/AppData/Roaming/Typora/typora-user-images/image-20211201163151266.png)

![image-20211201163550837](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011635006.png)

- 关于选择126的弊端

![image-20211201163619002](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011636169.png)

![image-20211201163731006](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011637170.png)

- hashCode() in LinkedList：
  - 一个链表的哈希值等于表中存储的对象的哈希值之和
  - 省事的情况下只看前几个对象

![image-20211201163915436](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011639618.png)

- BST hashcode：

![image-20211201164117964](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112011641153.png)



# Lecture 20 Heaps and PQs

- 优先队列接口。
  - 并不一定是要是最小，可以是各种的最，最好，最大。

![image-20220215145049657](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202151450958.png)

- 一些可能的实现：但都不太理想，各有缺点。

![image-20220215162604179](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202151626439.png)

- **引入堆：堆是一颗二叉树，但不是BST。**遵循堆的性质，并且是完备的。
  - 最小堆性：每个节点都小于或等于它的两个子节点。
    - 图四节点5不满足。
  - 完备性：缺失的节点只会出现在最底层（如果有的话），并且所有的节点都尽可能的靠左放置（也就是缺口会尽量靠右）
    - 也就是说，每个节点都应该有两个子节点，除非它的子节点刚好是处于最底层的。
    - 图三缺口就应该是靠右的。

![image-20220215163409388](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202151634670.png)



- 堆非常适合实现优先队列。
- 各个堆运算的实现：
  - `getSmalless()`：返回根节点的值。
  - `add(x)`：先把临时性的放在最底层(**最靠左的空位，也就是遵循堆的完备性**)，然后把它一层层往上移，直到找到合适的位置。
  - `removeSmallest()`：先临时把最底层最右侧的节点替代掉根节点，然后把根节点向下移动，直到找到合适的位置。
  - [Demo:插入和删除](https://goo.gl/wBKdFQ)

![image-20220216143737111](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202161437427.png)

- Java中的堆的实现
- 首先是树的各种实现，并不只有BST的那种实现方法。
- 第一大类：直接保存从节点到子节点的引用。
  - 第一种：一个节点包含四个域：Key ，左、中、右子节点的引用。
  - ![image-20220216145356362](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202161453500.png)
  - 第二种：两个域：Key和一个子节点的数组，这个实现方法可以用来实现多叉树，使得一个节点可以有任意多个子节点。
  - ![image-20220216145531478](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202161455620.png)
  - 第三种：每个节点有三个域，一个Key，一个指向它的最左侧子节点，一个域指向它的亲兄弟节点。
  - ![image-20220216145640249](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202161456380.png)
  - 对同一棵树有不同的实现方法。
  - ![image-20220216145920120](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202161459260.png)
- 第二大类：不直接保存节点到子节点的引用，而是对节点进行层序编号。
  - 第一种：类似对并查集的实现。parents[i] 存储的就是keys[i]的父节点。
  - ![image-20220216150214145](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202161502281.png)
  - 第二种：只适用于**完全树**，因为直接省去了parents数组。由于parents数组永远是00011223344这样下去，所以对完全树可以直接省去。
  - ![image-20220216151147345](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202161511481.png)

- 这最后一种就是Java中堆的实现方式了。因为堆是完备的（所有的节点都是尽可能靠左的，所以parents数组不会在中间出现缺口）
  - swim的实现方法。
  - parent的实现方法。
  - 这种实现方法代码简单，使用的内存大概是指针映射（第一类）的三分之一。

![image-20220216152934437](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202161529573.png)

- 总结：
  - 堆很适用于实现优先队列。而BST则不太适合，因为BST要求所有的插入的值必须是能够分个大小的，处理不了相等值的情况。
  - **堆的增加和删除是对数级别复杂度的，但是是均摊的，因为涉及数组的`resize()`**。

![image-20220216153503258](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202161535391.png)

- 关于优先队列还有一些小问题
  - 一个优先队列怎么知道如何去确定在队列中的哪个项更大？这个问题有两个解。
    - 一是限制我们的类型参数是`Comparable`的。`<Item extends Comparable<Item>>`这个是默认的行为。
    - 二是实现一个构造器，这个构造器有一个参数是一个`Comparator`。

![image-20220216154215882](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202161542021.png)



- 最后就是关于数据结构的一些总结了

![image-20220216165003471](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202161650615.png)



# Lecture 21 Tree and Graph Traversals

- 三种深度优先遍历、层序遍历

![image-20220216221515301](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202162215454.png)

- 三种深度优先遍历的快速方法。
  - 前序：经过左边的时候。
  - 中序：经过底下的时候。
  - 后序：经过右边的时候。

![image-20220216221854841](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202162218976.png)

- 三种深度优先遍历的使用例子
  - 先序遍历：打印目录
  - ![image-20220216224551915](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202162245058.png)
  - 后序遍历：计算目录总大小
  - ![image-20220216224621531](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202162246667.png)



- 图的深度优先遍历
  - 首先是一个s-t连通性问题。也就是给定起点和终点，求一条连通路径。[demo](https://docs.google.com/presentation/d/1OHRI7Q_f8hlwjRJc8NPBUc1cMu5KhINH1xGXWDfs_dA/edit?usp=sharing)

![image-20220217112609193](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202171126345.png)

- 给定起点s，求从s到任意其可到达的定点的路径。[demo](![](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202171229374.png))
  - 深度优先搜索。


![image-20220217122847777](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202171228915.png)

![image-20220217122933238](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202171229374.png)

- 关于树和图的遍历的对比。
  - **图的遍历是不具有唯一性的，而树的遍历具有唯一性**。
  - 图的深度优先遍历同样有前序和后序。此外还有广度优先遍历。
  - 前序：操作先于调用。
  - ![image-20220217125248352](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202171252505.png)
  - 后序：操作后于调用
  - ![image-20220217125325360](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202171253696.png)
  - 广度优先：按从起点到节点的距离的顺序来遍历，先遍历距离为1的所有节点，然后距离为2的..
  - ![image-20220217125420558](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202171254713.png)







# Lecture 22 Graph Traversals and Implementations

- 广度优先算法
  - 只适用于无权图，在这里的图都是没有带权的。
  - 所以不能拿来给地图应用做导航算法捏，因为每条路的长度不一定一样。
  - **广度优先算法求某点到任意点的最短路径。**这里的最短指的是经过的边的数目最小。
  - 使用一个队列。队列里的元素要么是距原点等距K的，要么是K + 1 的
  - 即使没有`edgeTo[]`和`distTo[]`也是广度优先。
  - [demo](https://docs.google.com/presentation/d/1JoYCelH4YE6IkSMq_LfTJMzJ00WxDj7rEa49gYmAtc4/edit?usp=sharing)


![image-20220217182916301](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202171829456.png)

## Graph API

- 我们需要：
  - 图的底层表示
  - 图的合适的API

- ![image-20220226160940546](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261609812.png)


- 第一个简化：
  - 对图的节点编号，不考虑图的标签。实现图的时候也是使用相应的编号。
  - 将节点的标签和编号用Map<Label,Integer>映射。
- ![image-20220226161201804](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261612049.png)
- Graph API：
  - 构造器必须提前给定节点的个数，并且节点的个数是固定不变的。
  - 可以在v号节点和w号节点之间添加边，但是不允许向图中添加节点。
  - `adj(int v)`返回的是`v`号节点的所有相邻节点，这些邻居保存在一个可迭代的容器中，如`ArrayList<Integer>`。
    - Iterable的类型参数之所以是整数是因为所有节点都是用编号呈现的。
  - 不支持有权的节点或边。
  - 没有求度的方法，但是可以借助API实现。
    - ![image-20220226162450304](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261624531.png)
  - 可以得到图的节点数和边数。
  - 可以借助API实现打印图的所有边的方法。
    - ![image-20220226162504871](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261625079.png)

![image-20220226161649746](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261616008.png)

## Graph Representations

- 邻接矩阵
  - 使用一个二维`boolean`矩阵
  - 有一个复杂度问题没看。
  - ![image-20220226163036748](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261630195.png)
- HashSet< Edge >：边的HashSet，每条边是一对int

![image-20211227164044065](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/image-20211227164044065.png)



- Adjacency List：邻接表
  - Vertex Indexed Array


![image-20220226163619371](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261636771.png) 

- 一个思考题：打印一幅图的时间复杂度。以及一些特殊情况下复杂度的简化。

![image-20220226164248812](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261642243.png)

![image-20220226164318149](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261643749.png)

## Runtime Analysis

![image-20220226164427183](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261644659.png)



## Graph Traversal Implementation And Runtime Analysis

- 图的算法的通常的设计模式不是在`Graph`类里面实现各种方法，而是`Graph`类里只有存粹的呈现`Graph`有关的东西。
  - 将类型与算法分离。另起一个类，让这个类来负责图的有关算法的进行（这个类就是伟大的图中全能神）。
  - 创建一个图对象，并传给全能神，向全能神索引图的信息。
  - ![image-20220228152548630](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281525829.png)

- 深度优先的实现：
  - ![image-20220228153740160](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281537316.png)
  - ![image-20220228153754963](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281537105.png)

- Runtime Analysis
  - ![image-20220228153845881](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281538023.png)
  - ![image-20220228153904061](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281539199.png)



- 广度优先的实现：
  - ![image-20220228154005843](C:/Users/jiangyiqing/AppData/Roaming/Typora/typora-user-images/image-20220228154005843.png)

- 复杂度分析：
  - ![image-20220228154028498](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281540645.png)

## Summary

![image-20220228154144560](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281541727.png)

![image-20220228154215738](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281542875.png)

# Lecture 23 Shortest Paths

- [DFSDemo](https://docs.google.com/presentation/d/1lTo8LZUGi3XQ1VlOmBUF9KkJTW_JWsw_DOPq8VBiI3A/edit?usp=sharing)
- [BFSDemo](https://docs.google.com/presentation/d/1JoYCelH4YE6IkSMq_LfTJMzJ00WxDj7rEa49gYmAtc4/edit?usp=sharing)

![image-20220228155517766](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281555912.png)

- 深度优先和广度优先，哪一个更好？
  - 两种都适用于所有的无权图。
  - 使用广度优先搜索的情况下，不仅得到了路径，并且得到的边的数目是最短的。
  - 二者的时间效率相差不大。
  - ![](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281601658.png)
  - 空间效率上：
    - DFS针对细长的图时差一点。因为递归的原因，调用栈会非常的深。
    - DFS针对一个节点有巨大数量的邻接点时不太行。
    - ![image-20220228160216597](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281602740.png)

- **还是想特别提醒一下，BFS得到的最短路径的最短指的是路径的数目最短。**
  - ![image-20220228163631294](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281636460.png)



- ![image-20220228195643309](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281956448.png)
  - 适用于有向和无向图。用于有权图的寻找起点到所有可达点的最短路径。
  - **不考虑有环图，也不考虑某一条边的权值为负。**
  - **从源点到其他任意可到达点的路径的集合在图上画出来总会是一棵树(Shortest path tree,SPT)。**
    - ![image-20220228195914575](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202281959736.png)
  - **最短路径所构成的树永远有 `V - 1` 条边。这是因为除了源点，其他的点都有且仅有一条从其他地方指向该点的入路径。**
    - ![image-20220228200139548](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202282001696.png)

- 算法主体：
  - [demo](https://docs.google.com/presentation/d/1_bw2z1ggUkquPdhl7gwdVBoTaoJmaZdpkV6MoAgxlJc/pub?start=false&loop=false&delayms=3000)
  - 访问节点的时候进行的是距离优先访问，最先访问距离最小的节点。使用优先队列。
  - 对于当前访问的节点V，调整所有从V指出的路径。
  - ![image-20220228213339591](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202282133760.png)
  - 伪代码：在不存在负数权值的路径的情况下是正确的。
  - ![image-20220228230022369](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202282300560.png)
  - 复杂度：
  - ![image-20220228230152252](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202282301413.png)

- <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202282302576.png" alt="image-20220228230225437"  />





- 对于一个终点明确的最短路径问题来说，Dijkstra能给出正确的回答，但是它的效率不够高。

  - 可以这么想：Dijkstra是以起点为中心的圆周扩散寻找最近点的，而对于终点明确的最短路径来说，只需要往靠终点近的那边就可(A*）。

  - 这是Dijkstra：

    - ###### <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202282313667.png" alt="image-20220228231312512" style="zoom: 50%;" />

- 引出A*：

  - 在访问节点时仍然是最近优先访问，只不过这个最近的定义是：**从起点到点V的距离加上点V到终点的距离最近。**(Dijkstra是起点到点V的距离)
  - 这个从点V到终点的距离由估算函数h给出：根据已有经验来估算。
    - <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202282309164.png" alt="image-20220228230950023" style="zoom:67%;" />
  - ![image-20220228230627398](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202282306582.png)
  - 这是A*：http://qiao.github.io/PathFinding.js/visual/
    - <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202282315566.png" alt="image-20220228231506427" style="zoom: 67%;" />



[不同路径算法的在线可视化网站](http://qiao.github.io/PathFinding.js/visual/)

# Lecture 24 Minimum Spanning Trees

- **最开头有一个warmup。关于如何确定图中是否有环。对第二个算法来说很重要。**

- 最小生成树：将无向有权图的所有节点都连接在一起，并且使得总权值最小的树。
  - 应用：将各个家庭使用电线连接在一起使得每一家都能通电，这时就要选择总电线最短的连接方式，这种方式就是最小生成树。
  - 无环。
  - 连通。
  - <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203012115594.png" alt="image-20220301211504422" style="zoom:67%;" />

- 最小生成树和最短路径树的对比：
  - 最小生成树没有起点一说，是一个全局的属性，而最短路径树则是与起点相关。
  - <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203012332886.png" alt="image-20220301233247743" style="zoom:67%;" />
  - 某个起点的最短路径树也可能恰恰是整个图的最小生成树。
    - <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203012333411.png" alt="image-20220301233353288" style="zoom:67%;" />
    - 所以这提示我们一种求最小生成树的方法：我们找到这个特殊的节点，然后以这个节点作为起点求它的最短路径树，得到的就是最小生成树。**但是这种方法实际上是不可行的，反例：**
    - ![image-20220301234022037](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203012356445.png)

- Cut Property：
  - Cut：将图的节点分成两个非空集合。
  - Crossing Edge：两个断点分别在两个集合的边。
  - Cut Property：给定任意一个分割，Crossing Edge中权值最小的那一条一定是属于最小生成树。
    - 证明：![image-20220302152816988](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203021528135.png)
  - <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203021526312.png" alt="image-20220302152616097" style="zoom:80%;" />

- 根据Cut Property引出最基本的最小生成树算法：
  - 最开始最小生成树中没有边。
  - 找到一个分割，这个分割的crossing edge都不在最小生成树中。
  - 将这个分割里的权值最小的边加入最小生成树中。
  - 重复直到最小生成树中有V - 1条边。
  - <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203021529889.png" alt="image-20220302152944755" style="zoom:80%;" />
- <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203022108088.png" alt="image-20220302210828947" style="zoom:80%;" />
  - 其实是Cut Property的特殊情况的应用。
    - 任意取一个点作为起点（将它单独成集合，其他点成另外一个集合），选取与之相连的（也就是crossing edge）权值最小的边，这就是MST的第一条边。
    - MST的第一条边连接起来的两个节点构成一个集合，其他点构成另外一个集合，选取第二个集合的crossing edge中权值最小的，得到第二条边。
    - 本质上就是，在构造的过程中，不完整的MST的边将所有的点分成了两个集合，一个集合是已经在MST中的点，另外一个就是其他点。
    - 重复这个过程直到得到V - 1条MST边。
  - [demo](https://docs.google.com/presentation/d/1NFLbVeCuhhaZAM1z3s9zIYGGnhT4M4PWwAc-TLmCJjc/edit#slide=id.g9a60b2f52_0_0)<img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203022116661.png" alt="image-20220302211648499" style="zoom:80%;" />
  - 实际的实现：[demo](https://docs.google.com/presentation/d/1GPizbySYMsUhnXSXKvbqV4UhPCvrt750MiqPPgU-eCY/edit#slide=id.g9a60b2f52_0_0)
  - <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203022137899.png" alt="image-20220302213719741" style="zoom:80%;" />
  - 与最短路径算法对比：<img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203022143631.png" alt="image-20220302214301474" style="zoom:80%;" />
  - 伪代码：
    - <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203022143048.png" alt="image-20220302214339903" style="zoom:67%;" />
    - <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203022143034.png" alt="image-20220302214352876" style="zoom:67%;" />
  - Runtime：<img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203022144305.png" alt="image-20220302214453148" style="zoom: 67%;" />



- <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203022145499.png" alt="image-20220302214541372" style="zoom:67%;" />
  - 其实是Cut Property的伪装。
    - [kruskals conceptual demo](https://docs.google.com/presentation/d/1RhRSYs9Jbc335P24p7vR-6PLXZUl-1EmeDtqieL9ad8/edit?usp=sharing)
    - [kruskals realistic implementation demo](https://docs.google.com/presentation/d/1KpNiR7aLIEG9sm7HgX29nvf3yLD8_vdQEPa0ktQfuYc/edit?usp=sharing)
  - 将所有的边按权重排好序，从小到大考虑每条边。
  - 对于正在考虑的边E，如果加入E到最小生成树中不会导致树中成环，就加入E，否则抛弃E。
  - <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203022155467.png" alt="image-20220302215525308" style="zoom:80%;" />
  - Runtime![image-20220302224109048](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202203022241213.png)









# Lecture 25 Range Searching and Multi-Dimensional Data



# Lecture 26 Prefix Operations and Tries

- Tries的基本思想：
  - **Tries可以是一个<String(key),Integer>的Map，Value部分帮助我们确定字符串出现了几次。也可以是一棵Tree**
  - 将字符串中的每一个字符都保存为树中的一个节点。
  - 每个节点只保存一个字符。
  - 同一个节点的字符可以被多个不同的字符串共享。

![image-20220225165052362](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202251650557.png)

- 一个Tries的树的示例：
  - **将每个字符串的结尾的字符节点标位了蓝色来表示是字符的结尾。解决了下图底部提出的问题**

![image-20220225170038011](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202251700154.png)

![image-20220225170156889](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202251701033.png)

- Trie Map :[demo](http://www.cs.princeton.edu/courses/archive/spring15/cos226/demo/52DemoTrie.mov)

![image-20220225175347676](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202251753826.png)

- Trie Implemention

  - DataIndexedCharMap< V >作为记录节点的子节点的一个Map。

  - ![image-20220225175600046](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202251756182.png)
  - `isKey`，就是节点是蓝色还是白色。
  - `next`是节点的子节点的合集，包含128个单元，大部分会是`null`，这也是我们后续要解决的问题。
  - ![image-20220225180309674](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202251803815.png)
  - 下图是一个关于`a、aw`两个字符串的节点的图。`next.items[w]` 保存着保存`w`的那个节点。
  - ![image-20220225180448143](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202251804279.png)
  - 对上图的实现做一些改进，由于`items[]`的下标本身就是那个字符，所以可以把`ch`域去掉。
  - ![image-20220225180910076](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202251809218.png)
- RunTime Analysis：
  - 都是常数时间，复杂度与关键词的个数无关。因为你永远都只需要去走那包含特定几个字符的一条路径。
    - 例如contains("Potato"):P --> o-->t--->a-->t-->o。
  - ![image-20220225181539123](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202251815270.png)
- 接下来是对`next`的一些改进：
  - 使用BSTMap：内存上更有效率，时间上可能会差一点，毕竟BST是log的
  - ![image-20220225184107384](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202251841529.png)
  - 使用HashMap：时间上可能稍微差一点点，毕竟是均摊的O(1)
  - ![image-20220225184120667](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202251841806.png)
- Tries的真正的特殊用处在于一些字符串的特殊运算：
  - 前缀匹配


- 一个练习：如何将tries里的所有关键字都取出：

![image-20220226150918503](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261509853.png)

- 搜索引擎中的自动补全是如何完成的：

  - 建立一个`TrieMap< Integer >`：权值决定了字符串的重要程度。
  - 调用前缀匹配函数，并返回权值最高的几个字符串。

  - ![image-20220226151117037](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261511287.png)
  - 但是这样的方法有一个致命缺陷：当输入的字符很短时与之前缀匹配的字符串很多，我们可能需要从一个很大的数据集中寻找到最优的十条。
  - ![image-20220226152656535](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261526768.png)
  - 解决方法：每个节点保存它自身的值，以及它的最佳子串的值。
  - ![image-20220226152808738](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261528982.png)
  - 甚至跟高效的tries：对于那种无分支的节点，合并起来。
  - ![image-20220226152916601](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261529847.png)

- Summary：

![image-20220226152942961](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202261529218.png)

# Lecture 27 Software Engineering I

- 什么是复杂度
  - 复杂度的定义是以人为中心的。

![image-20220213150056728](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202131501033.png)

- 复杂度的表现：
  - 代码修改膨胀：为了实现软件的某个改变，你可能需要去修改许多处代码。
  - 认知负担：为了实现改变你需要知道的东西有多少。
  - 未知的未知：你甚至不知道为了实现改变你需要知道什么。

![image-20220213154426885](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202131544147.png)

- 复杂度是慢慢积累起来的。
  - 每次引入的复杂度都不算大问题，可能只是一个特殊情况的`if`，可能只是一个`workaround`。但是会积累。
  - 在屎山将现的时候重构代码。

![image-20220213155016928](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202131550183.png)

- 短平快的编程：

![image-20220213160216911](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202131602157.png)

- 从战略全局考虑的编程：

![](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202131605834.png)

![image-20220213160623566](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202131606811.png)

![image-20220213160835138](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202131608376.png)

# Lecture 28 Reductions and Decomposition





# Lecture 29 basic sort

# Lecture 30 QuickSort

![image-20211214194533990](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112141946846.png)

![image-20211214200731637](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112142007858.png)

- 一些快排不适用的情况：
  - 数组已经有序，或者已经几乎有序
  - 数组中有重复元素，全部是重复元。

![image-20211214200940939](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112142009287.png)



# Lecture 31 Software Engineering II

- 一个短视性编程的例子，就是最后一个project。
  - 很多代码是相似的，完全可以抽象成一个`helper method`
    - 比如if括号内的判断是否达到边界的后一个条件，完全可以抽象成`private boolean hasReachedTheEdge(.....)`
    - 以及if正文内的相似代码，也可以抽象成一个函数。
  - 所有的行为都发生在一个抽象程度比较低的层级，不好理解。
  - 如果找到问题想要修改就很麻烦，因为要修改多处。
  - 条件很长并且很难读懂。完全可以抽象成`helper method`
  - 很多数值是硬编码的。
  - 没有注释

![image-20220217184618540](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202171846676.png)

- 修改的一些例子

![image-20220217185004249](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202171850383.png)

- 复杂度的两个来源
  - 相互依赖性（耦合度）：代码不能被独立的读、理解和修改。
  - 晦涩程度：重要信息不明显。

![image-20220217200227343](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202172002465.png)

- 第二个例子：处理不同的输入方式，字符串输入还是键盘输入。

![image-20220217201001276](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202172010418.png)

- 接下来是关于模块化设计

![image-20220217201337052](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202172013180.png)

- 在建造程序时一个非常重要的事情是隐藏复杂度。因为人没办法一次性理解所有的复杂度。所以需要封装复杂度来保证程序员一次只考虑一部分复杂度。
- 理想的情况：系统被分解成许多模块，模块与模块直接完全独立。
  - 模块可以是一个类，一个接口，一个包，一个代码单元。
  - 但是模块不可能完全独立，因为模块与模块之前是需要通信的，至少需要方法调用。
  - 所以我们的目标是最小化模块之间的依赖关系。
  - ![image-20220217201943605](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202172019730.png)

![image-20220217202358671](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202172023793.png)



![image-20220217202409071](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202172024200.png)

![image-20220217202420994](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202172024117.png)

- 尽可能的隐藏信息，避免信息泄漏。
  - 所谓信息泄漏就是类似的代码出现在许多模块中，于是修改时就要修改许多处。

![image-20220217202845788](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202172028908.png)

![image-20220217202944602](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202172029730.png)

- 避免**TemporalDecomposition**:
  - In temporal decomposition, the structure of your system reflects the order in which events occur.

![image-20220217203547154](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202172035294.png)

![image-20220217203537154](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202172035285.png)

# Lecture 33 Software Engineering III

- 很强的人文关怀，我永远喜欢UCBerkeley

# Lecture 35 Radix Sort

- 一种非常离谱的排序方法
  - SleepSort()

![image-20211228134628928](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/image-20211228134628928.png)

- Runtime Analysis for CountingSort

![image-20211228140455908](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/image-20211228140455908.png)

# Lecture 37 Software Engineering IV

- 飘着听完的，讲了一些CS 61 B的进化史，一些课程评价。没什么知识性的东西

# Lecture 38 Compression

- 这堂课讲的是关于压缩的，比如文件压缩。
- 先建立了一个压缩模型：
  - 给定一串比特串，经过一个压缩算法得到压缩后的比特串。
  - 压缩后的比特串经过解压缩算法得到原始的比特串。
  - 我们假定这个过程信息是无损的。
    - 即便这种情况下文本文件也可以压缩百分之七十。

![image-20220218212212647](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202182122808.png)

- 优化编码的方式：
  - 使用更短的编码。
  - 当然，要编码和符号对应。

![image-20220218220236627](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202182202773.png)

- 摩斯码是一种编码的方式，但是它的问题是，某一个符号的编码可能是另外一个符号的编码的前缀。
  - 这样会导致我们收到的压缩后的信息解压后存在歧义。
  - 实践中不存在，因为可以进行人为停顿。
  - 但是电脑中没办法这样作弊，所以我们需要无前缀编码。
    - 也就是任意一个字符的编码都不可能是另外一个字符的前缀。
  - 摩斯码的编码树体现了这一点：

![image-20220218220627906](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202182206057.png)

- 如果把编码写成树的形式，所有的字符都是处于叶子节点上，那么这个编码应该就是一个无前缀编码。
- ![image-20220218220911475](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202182209619.png)
- ![image-20220218220849813](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202182208954.png)

- 对于无前缀编码又引入了一个设计问题:
  - 某些编码设计针对某些文本，可能比其他的编码设计要好。
    - 比如EEEAT，左边的编码方式比右边好。
    - 而对于JOSH，右边的编码方式比左边好。
  - 所以我们可能需要找到一个流程，这个流程能够找到给定字符串的最佳编码方式。

![image-20220218223943776](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202182239918.png)

- `shannon fano code`：
  - 三个节点分开时，权重最大的那一个节点单独作为一半。
  - **但是香农凡诺编码并不是最优的，它能用，但是可以找到更好的。**
  - 更好的是`huffman coding`。

![image-20220219121422416](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202191214566.png)

![image-20220219121348483](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202191213669.png)

![image-20220219121402912](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202191214061.png)

- 霍夫曼编码

![image-20220219153911510](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202191539673.png)

![image-20220219153929962](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202191539101.png)

- 关于霍夫曼编码的效率与Unicode的对比。

  - ###### 使用霍夫曼编码大概是14倍的效率

![image-20220219154026399](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202191540539.png)

- 关于霍夫曼的编码与解码：
  - 编码：使用从符号到编码的Map来映射，或者使用符号(的ASCII，etc)作为索引的编码的数组。
  - ![image-20220219154840490](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202191548638.png)
  - 解码：使用前缀树，try。解码时寻找最长的匹配前缀。
  - ![image-20220219154937591](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202191549727.png)

- 有关于霍夫曼编码的实践问题：
  - 对于霍夫曼编码，有两种使用霍夫曼压缩的方法。
    - 第一种，对不管什么类型的输入，不管什么文件，英文也好中文也好，每一类建立一个标准的编码，英语一类，中文一类。
    - 第二种，仅针对输入的文件，对于每一种可能的输入文件，创建一个只适用于它的独一无二的编码。将编码和已压缩的文件
  - 两种编码方法的优缺点：
    - 第一种：我们无法得到最佳编码，因为不同的文件可能相同的字符所占的权重并不是一样的，而这种情况可能才是最常见的。所以强行忽略不同输入文件中相同字符的权重不同来建立一个统一的编码库，无法得到最佳编码，也即无法极限压缩。
      - 举个极端一点的例子，可能某一个文件全是字符A，另外一个文件全是字符B，我们给这俩字符都恰好使用4位的编码。但是实际上只针对这一个文件，一位的压缩编码就是足够的。
    - 第二种：我们会需要额外的空间，因为要把编码表也发送过去。
    - **实践采用的往往是第二种，因为很多时候，包含编码表的花费对于整个文件来说是微不足道的。**
      - 举个例子，有一个很大的英文文本，我们针对它使用了第二种方法，压缩之后减少了10兆，但是这个编码表本身可能也就KB大小。

- 霍夫曼编码和解码实例：
  - 编码：[demo](https://docs.google.com/presentation/d/1DWuSkE9MxQPUTjbSJCMe54rCim4eAwM4aFRvhqq5_Hs/edit#slide=id.g2159afc5e6_0_1068)
    - 1.计算各个符号的权重。
    - 2.建立编码数组和阶码前缀树
    - 3.将阶码前缀树写入输出文件
    - 4.将编码也写入输出文件。
  - ![image-20220219170512286](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202191705456.png)
  - 解码：[demo](https://docs.google.com/presentation/d/1DWuSkE9MxQPUTjbSJCMe54rCim4eAwM4aFRvhqq5_Hs/edit#slide=id.g2159afc5e6_0_1068)
    - 1.读入解码前缀树。
    - 2.根据前缀树使用最大匹配前缀得到相应符号。
  - ![image-20220219170731362](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202191707526.png)

- Summary：[Huffman.java](http://algs4.cs.princeton.edu/55compression/Huffman.java)
- ![image-20220219171012200](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202191710340.png)

- Compression Theory:

  - 世界上除了霍夫曼还有许多种其他的压缩方式。
  - Run Length Encoding：行程长度压缩法，即根据字符串的连续重复字符进行编码的一种方法。
    - 处理处理连续重复字符串的效果较佳，最差的情况就是没有连续的字符，这样的话除了没有压缩不算，而且还增加了字符串的长度

  ![image-20220219183017649](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202191830817.png)

- 关于压缩模型的改进：将解压的代码也加入到最终的输出文件中去。

![image-20220219183901706](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202202191839859.png)

# Lecture 39 Compression, Complexity, and P=NP?

- 太难啦！！！
