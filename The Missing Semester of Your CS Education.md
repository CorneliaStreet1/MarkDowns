# Lecture 1 Shell

shell 基于空格分割命令并进行解析，然后执行第一个单词代表的程序，并将后续的单词作为程序可以访问的参数。如果您希望传递的参数中包含空格（例如一个名为 My Photos 的文件夹），您要么用使用单引号，双引号将其包裹起来，要么使用转义符号 `\` 进行处理

- 方法1：`mkdir "My Photos"`
- 2：`mkdir My\Photos`
- `mkdir My Photos`将会建立两个目录，一个是My，另一个是Photo

shell 是一个编程环境，所以它具备变量、条件、循环和函数（下一课进行讲解）。当你在 shell 中执行命令时，您实际上是在执行一段 shell 可以解释执行的简短代码。如果你要求 shell 执行某个指令，但是该指令并不是 shell 所了解的编程关键字，那么它会去咨询 *环境变量* `$PATH`，它会列出当 shell 接到某条指令时，进行程序搜索的路径：

```
missing:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
missing:~$ which echo
/bin/echo
missing:~$ /bin/echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

当我们执行 `echo` 命令时，shell 了解到需要执行 `echo` 这个程序，随后它便会在 `$PATH` 中搜索由 `:` 所分割的一系列目录，基于名字搜索该程序。当找到该程序时便执行（假定该文件是 *可执行程序*，后续课程将详细讲解）。确定某个程序名代表的是哪个具体的程序，可以使用 `which` 程序。我们也可以绕过 `$PATH`，通过直接指定需要执行的程序的路径来执行该程序

## 路径

shell 中的路径是一组被分割的目录，在 Linux 和 macOS 上使用 `/` 分割，而在Windows上是 `\`。路径 `/` 代表的是系统的根目录，所有的文件夹都包括在这个路径之下，在Windows上每个盘都有一个根目录（例如： `C:\`）。 我们假设您在学习本课程时使用的是 Linux 文件系统。如果某个路径以 `/` 开头，那么它是一个 *绝对路径*，其他的都是 *相对路径* 。相对路径是指相对于当前工作目录的路径，当前工作目录可以使用 `pwd` 命令来获取。此外，切换目录需要使用 `cd` 命令。在路径中，`.` 表示的是当前目录，而 `..` 表示上级目录：

- `pwd`：print working directory

一般来说，当我们运行一个程序时，如果我们没有指定路径，则该程序会在当前目录下执行。

- `ls`默认在当前目录下执行，但是可以指定`ls`的路径
- `ls ..`，列出上一级目录
- 特别的，`~`字符，为`/home/jyq`目录
- 另外一个，`-`，如果`cd -`，会回到刚才你所在的目录
- 大多数的命令接受标记和选项（带有值的标记），它们以 `-` 开头，并可以改变程序的行为。通常，在执行程序时使用 `-h` 或 `--help` 标记可以打印帮助信息，以便了解有哪些可用的标记或选项。
- 一般来说接受一个值的称为选项option，而不需要接受值的称为标记flag
- 命令`ls`中的标志`-l`，`ls -l`，打印出更加详细地列出目录下文件或文件夹的信息。

```c
jyq@jyq-PC:~$ ls -l
总用量 3464
drwxr-xr-x 2 jyq jyq    4096 6月  27 19:17 迅雷下载//左侧最开头的d表示这是一个directory，而没有d则不是
//然后接下来的九个字符，每三个字符构成一组。它们分别代表了文件所有者（missing），用户组（users） 以及其他所有人具有的权限。
//- 表示该用户不具备相应的权限。r:read w:write x:execute
//对于文件夹来说，read表明可以读取文件夹内容（看作是ls），write表明可以编辑文件夹内容，比如编辑文件名，增删文件
    //如果有文件的write权限而没有所在目录的write权限，无法删除这个文件,因为删除需要对文件夹进行write
//对于文件夹来说，x意味着“search”，你是否允许进入这个目录，如果你想cd某个目录，必须拥有该目录所有上级目录和该目录本身的x权限
-rw-r--r-- 1 jyq jyq 3504535 9月  15 20:30 clash-freebsd-amd64-v1.7.1.gz
-rw-r--r-- 1 jyq jyq     355 9月  18 19:59 clash.gz
```

- `mv`：用于移动文件或重命名文件（`mv  name1 name2`）
- `cd path1 path2` :拷贝文件
- `rm path`:删除文件，`rm`不是递归删除的
- `rmdir`：删除一个空目录
- `man `：接受一个程序名作为参数，然后将它的文档（用户手册）展现给您。使用 `q` 可以退出该程序。
- `Ctrl L`：清理干净shell 

## 在程序间创建连接

最简单的重定向是 `< file` 和 `> file`。这两个命令可以将程序的输入输出流分别重定向到文件

- 还可以使用 `>>` 来向一个文件追加内容。
- `>`会覆盖文件原有的内容
-  `|` 操作符允许我们将一个程序的输出和另外一个程序的输入连接起来，左边的输出作为右边的输入

## ROOT

根用户（root user）。 您应该已经注意到了，在上面的输出结果中，根用户几乎不受任何限制，他可以创建、读取、更新和删除系统中的任何文件。 通常在我们并不会以根用户的身份直接登录系统，因为这样可能会因为某些错误的操作而破坏系统。 取而代之的是我们会在需要的时候使用 `sudo` 命令。顾名思义，它的作用是让您可以以 su（super user 或 root 的简写）的身份执行一些操作。 当您遇到拒绝访问（permission denied）的错误时，通常是因为此时您必须是根用户才能操作。

- `sudo su`：得到一个作为superuser的shell

```c
jyq@jyq-PC:/sys/class/backlight/acpi_video0$ echo 50 > brightness 
bash: brightness: 权限不够
jyq@jyq-PC:/sys/class/backlight/acpi_video0$ sudo echo 50 > brightness 
bash: brightness: 权限不够
jyq@jyq-PC:/sys/class/backlight/acpi_video0$ sudo su
请输入密码:
验证成功
root@jyq-PC:/sys/devices/pci0000:00/0000:00:01.1/0000:01:00.0/backlight/acpi_video0# echo 100 > brightness 
root@jyq-PC:/sys/devices/pci0000:00/0000:00:01.1/0000:01:00.0/backlight/acpi_video0# echo 10 > brightness 
root@jyq-PC:/sys/devices/pci0000:00/0000:00:01.1/0000:01:00.0/backlight/acpi_video0# cat brightness 
10
root@jyq-PC:/sys/devices/pci0000:00/0000:00:01.1/0000:01:00.0/backlight/acpi_video0# echo 42 > brightness 
//退出root模式
root@jyq-PC:/sys/devices/pci0000:00/0000:00:01.1/0000:01:00.0/backlight/acpi_video0# exit
exit
jyq@jyq-PC:/sys/class/backlight/acpi_video0$ 
```

- `xdg-open`：接受文件名作为参数，用合适的程序打开文件

# Lecture 3 Vim

在默认设置下，Vim会在左下角显示当前的模式。 Vim启动时的默认模式是正常模式。通常你会把大部分 时间花在正常模式和插入模式。

- *正常模式*：在文件中四处移动光标进行修改

- *插入模式*：插入文本，键入 `i` 进入插入 模式

- *替换模式*：替换文本，`R` 进入替换模式，在deepin下貌似是SHIFT + R

- *可视化（一般，行，块）模式*：选中文本块， `v` 进入可视（一般）模式， `V` 进入可视（行）模式， `<C-v>` （Ctrl-V, 有时也写作 `^V`）进入可视（块）模式

- *命令模式*：用于执行命令，`:` 进入命令模式。在deepin下貌似`SHIFT +:`

  - `:q` 退出（关闭窗口）

  - `:w` 保存（写）

  - `:wq` 保存然后退出

  - `:e {文件名}` 打开要编辑的文件

  - `:ls` 显示打开的缓存

  - ```plaintext
    :help {标题}打开帮助文档
    ```

    - `:help :w` 打开 `:w` 命令的帮助文档
    - `:help w` 打开 `w` 移动的帮助文档

- 你可以按下 `<ESC>` （退出键） 从任何其他模式返回正常模式。