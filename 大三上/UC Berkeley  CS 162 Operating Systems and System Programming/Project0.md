# 2.1 Find the Faulting Instruction

```c
FAIL
Test output failed to match any acceptable form.

Acceptable output:
  do-nothing: exit(162)
Differences in `diff -u' format:
- do-nothing: exit(162)
+ Page fault at 0xc0000008: rights violation error reading page in user context.
+ do-nothing: dying due to interrupt 0x0e (#PF Page-Fault Exception).
+ Interrupt 0x0e (#PF Page-Fault Exception) at eip=0x80488ee
+  cr2=c0000008 error=00000005
+  eax=00000000 ebx=00000000 ecx=00000000 edx=00000000
+  esi=00000000 edi=00000000 esp=bfffffe4 ebp=bffffffc
+  cs=001b ds=0023 es=0023 ss=0023
```



1. What virtual address did the program try to access from userspace that caused it to crash?

   - Page fault at 0xc0000008: rights violation error reading page in user context.
2. What is the virtual address of the instruction that resulted in the crash?

   - EIP寄存器，用来存储CPU要读取指令的地址，CPU通过EIP寄存器读取即将要执行的指令
   - Interrupt 0x0e (#PF Page-Fault Exception) at eip=0x80488ee
3. To investigate, disassemble the do-nothing binary using objdump (you used this tool in Homework 0).
  What is the name of the function the program was in when it crashed? Copy the disassembled code
  for that function onto Gradescope, and identify the instruction at which the program crashed.

  - ```c
    do-nothing.o:     file format elf32-i386
    
    
    Disassembly of section .text:
    
    00000000 <main>:
       0:   55                      push   %ebp//将%ebp的值(bffffffc)入栈
       1:   89 e5                   mov    %esp,%ebp//将%esp的值传递给%ebp
       3:   b8 a2 00 00 00          mov    $0xa2,%eax//将返回值0xa2放入%eax
       8:   5d                      pop    %ebp//将栈顶的值弹出%ebp
       9:   c3                      ret    //返回
    ```
4. Find the C code for the function you identified above (hint: it was executed in userspace, so it’s either
  in do-nothing.c or one of the files in proj-pregame/src/lib or proj-pregame/src/lib/user), and
  copy it onto Gradescope. For each instruction in the disassembled function in #3, explain in a few
  words why it’s necessary and/or what it’s trying to do. Hint: see 80x86 Calling Convention.
5. Why did the instruction you identified in #3 try to access memory at the virtual address you identified
  in #1? Don’t explain this in terms of the values of registers; we’re looking for a higher-level explanation.

# 