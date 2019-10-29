* [常用的gdb命令](#常用的gdb命令)
* [Linux下网络编程调试工具以及多线程调试](#Linux下网络编程调试工具以及多线程调试)
* [coredump](#coredump)
* [如何进行coredump调试](#如何进行coredump调试)


## 常用的gdb命令

l(list) ，显示源代码，并且可以看到对应的行号；

b(break)x, x是行号，表示在对应的行号位置设置断点；

break foo if x>0 条件断点

```
break foo if x>0
 commads 
 printf "x is %d" ,x
 continue
 end
```

p(print)x, x是变量名，表示打印变量x的值

r(run), 表示继续执行到断点的位置

n(next),单步调试，遇到函数体不会进入

s(step)，单步调试，遇到函数体会进入

c(continue),表示继续执行，到下一断点处结束。类似于r

q(quit)，表示退出gdb

finish，执行完func函数返回上一个函数调用中

u，从循环体中退出

bt 函数调用栈，第一行也就是栈顶，是程序最后执行的语句。

## Linux下网络编程调试工具以及多线程调试
```
(gbd) b thread1
(gbd) b thread2    //先给两个线程都打上断点
(gdb) r          //运行到断点处
(gdb) info threads
(gdb) threadd 2
(gdb) bt   查看函数栈，查看线程阻塞的位置
在使用gdb调试的时候，只有一个线程为活动线程
```

## coredump

通常情况下，core文件会包含了程序运行时的内存，寄存器状态，堆栈指针，内存管理信息还有各种函数调用堆栈信息等，我们可以理解为是程序工作当前状态存储生成第一个文件，许多的程序出错的时候都会产生一个core文件，通过工具分析这个文件，我们可以定位到程序异常退出的时候对应的堆栈调用等信息，找出问题所在并进行及时解决。在Linux下，一般读取Core的命令为：gdb exec_file core_file。

比如程序崩溃后产生了“core.15667”这样的文件，用GDB调试它，假设你的可执行程序名为test，则在命令行执行： gdb test core.15667。输入并执行命令 

(gdb) bt，就能查看调用函数调用栈。第一行就是出错的地方，也就是程序最后终止的地方。

如何保证能产生core文件？
---
先需要确认当前会话的ulimit –c，若为0，则不会产生对应的coredump，需要进行修改和设置。

ulimit  -c unlimited  (可以产生coredump且不受大小限制) 

或者可以指定固定大小： ulimit –c [size] （小于4页不行，至少需要4才生成core文件)

coredump产生的几种可能情况
---
造成程序coredump的原因有很多，这里总结一些比较常用的经验吧：

 1，内存访问越界

  a) 由于使用错误的下标，导致数组访问越界。

  b) 搜索字符串时，依靠字符串结束符来判断字符串是否结束，但是字符串没有正常的使用结束符。

  c) 使用strcpy, strcat, sprintf, strcmp,strcasecmp等字符串操作函数，将目标字符串读/写爆。应该使用strncpy, strlcpy, strncat, strlcat, snprintf, strncmp, strncasecmp等函数防止读写越界。

 2，多线程程序使用了线程不安全的函数。


 3，多线程读写的数据未加锁保护。

对于会被多个线程同时访问的全局数据，应该注意加锁保护，否则很容易造成coredump

 4，非法指针

  a) 使用空指针

  b) 随意使用指针转换。一个指向一段内存的指针，除非确定这段内存原先就分配为某种结构或类型，或者这种结构或类型的数组，否则不要将它转换为这种结构或类型的指针，而应该将这段内存拷贝到一个这种结构或类型中，再访问这个结构或类型。这是因为如果这段内存的开始地址不是按照这种结构或类型对齐的，那么访问它时就很容易因为bus error而core dump。

 5，堆栈溢出

不要使用大的局部变量（因为局部变量都分配在栈上），这样容易造成堆栈溢出，破坏系统的栈和堆结构，导致出现莫名其妙的错误。  

## 如何进行coredump调试

```
gcc -g a.c a //出错后会在文件目录下自动生成my_core_file文件
gdb -c my_core_file
(gcc) bt   //查看调用栈
```
