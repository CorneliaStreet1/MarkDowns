# gcc

[Gcc](https://www.cprogramming.com/gcc.html) is the de facto compiler in Linux or any other *nix system. It also has Windows ports but on Windows, you'll probably find the [debugger in Visual Studio](https://www.cprogramming.com/tutorial/debugging_concepts.html) 'easier'.

Suppose you have a file called main.cpp containing your c++ code. You should compile it with the following command:

```
g++ main.cpp -o main
```

While this will work fine and produce an executable file called main, you also need to put a -g flag to tell the compiler that you may want to debug your program later.

So the final command turns into:

```
g++ main.cpp -g -Wall -Werror -o main
```

# gdb

Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

To start GDB, in the terminal,

```
gdb <executable name>
```

For the above example with a program named main, the command becomes

```
gdb main
```

### Setting Breakpoints

You'll probably want you program to stop at some point so that you can review the condition of your program. The line at which you want the program to temporarily stop is called the breakpoint.

```
break <source code line number>
```

### Running your program

To run your program, the command is, as you guessed,

```
run
```

### Looking at the code

When the program is stopped, you can do a number of important things, but most importantly you need to see which part of the code you've stopped. The command for this purpose is "list". It shows you the neighbouring 10 lines of code.

```
list
```

**NOT** `ls`

### Next and Step

Just starting and stopping isn't much of a control. GDB also lets you to run the program line-by-line by the commands 'next' and 'step'. There is a little difference between the two, though. Next keeps the control strictly in the current scope whereas step follows the execution through function calls.

Look at this example carefully;

Suppose you have a line in the code like

```
value=display();``readinput();
```

If you use the next command, the line (and the function, provided there aren't breakpoints in it) gets executed and the control advances to the next line, readinput(), where you can perhaps examine 'value' to get an idea of how display() worked.

But if you use the step command, you get to follow what display() does directly, and the control advances to the first line of display(), wherever it is.

### Examining your Variables

When you want to find the misbehaving portion of your program, it often helps to examine local variables to see if anything unexpected has occurred. To examine a variable, just use

```
print <var name to print>
```

Note: You can also modify variables' values by

```
set <var> = <value> //wrong
Ambiguous set command "i = 0": inferior-tty, input-radix, interactive-mode.
set var <var> = <value>
```

You can modify variables to see if an issue is resolved if the variable has another value or to force the program to follow a particular path to see if the reason for a bug was due to a variable having the wrong value.

# Makefile

```makefile
hello:hello.c				
#试验了一下，貌似规则名和目标文件名不需要一致
    gcc hello.c -o hello    # 注意开头的tab, 而不是空格

.PHONY: clean

clean:			#clean规则没有依赖文件
    rm hello    # 注意开头的tab, 而不是空格
    
```

返回命令行, 键入`make`, 你会发现`make`程序调用了`gcc`进行编译. `Makefile`文件由若干规则组成, 规则的格式一般如下:

```
目标文件名:依赖文件列表
    用于生成目标文件的命令序列   # 注意开头的tab, 而不是空格
```

我们来解释一下上文中的`hello`规则. 这条规则告诉`make`程序, 需要生成的目标文件是`hello`, 它依赖于文件`hello.c`, 通过执行命令`gcc hello.c -o hello`来生成`hello`文件.

如果你连续多次执行`make`, 你会得到"文件已经是最新版本"的提示信息, 这是`make`程序智能管理的功能. 如果目标文件已经存在, 并且它比所有依赖文件都要"新", 用于生成目标的命令就不会被执行. 你能想到`make`程序是如何进行"新"和"旧"的判断的吗?

上面例子中的`clean`规则比较特殊, 它并不是用来生成一个名为`clean`的文件, 而是用于清除编译结果, 并且它不依赖于其它任何文件. `make`程序总是希望通过执行命令来生成目标, 但我们给出的命令`rm hello`并不是用来生成`clean`文件, 因此这样的命令总是会被执行. 你需要键入`make clean`命令来告诉`make`程序执行`clean`规则, 这是因为`make`默认执行在`Makefile`中文本序排在最前面的规则. 但如果很不幸地, 目录下已经存在了一个名为`clean`的文件, 执行`make clean`会得到"文件已经是最新版本"的提示. 解决这个问题的方法是在`Makefile`中加入一行`PHONY: clean`, 用于指示"`clean`是一个伪目标". 这样以后, `make`程序就不会判断目标文件的新旧, 伪目标相应的命令序列总是会被执行.