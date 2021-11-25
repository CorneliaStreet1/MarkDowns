# CS61B Fall 2020

算是二刷吧。神课总是值得二刷的。

# Lecture 1

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

# Lecture 5

- 2D数组采用的也是行优先，每一个数组单元存储的是某一行数组的指针。

# Lecture 7

- ArrayList，删除的时候不要`lazy deletion`，因为使用这种策略会导致要删除的内容仍然被保留
- 如下图，要删除item[2]，如果只是改变size，那么那副红色图片仍然存在。

![屏幕截图 2021-11-14 215524](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111142156560.png)

# Lecture 9

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