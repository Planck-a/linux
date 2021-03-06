```
几个重要目录：
~       用户的家目录，日常工作都是在自己的家目录中进行
/       根目录，所有用户共享，/home/[username]  就是 ~
~/.bash   存放所有别名
/bin/bash    解释器，可以运行.sh文件
/ec/passwd   存放所有用户的信息
/etc/group    用户组信息
/etc/vimrc     vim相关的配置，高亮行号

linux下的文件

Dev：该目录中主要存放的是外接设备，例如盘、其他的光盘等。在其中的外接设备是不能直接被使用的，需要挂载。
Etc：该目录主要存储一些配置文件。
Home：表示“家”，表示除了root用户以外其他用户的家目录，类似于windows下的User/用户目录。
```
Linux下进入root权限： su root；然后会提示输入密码

如果忘记密码，可以 sudo password root 进行密码重置。

一 文件操作
---
1、cat

2、vi

3、wc

-l统计行数(line)，-w统计单词数(word)，-c统计字符数(character)

4、cp

```cpp
cp 源文件 目标文件
cp  -r   递归地复制
cp ../文件1 .   复制上级目录的文件1到本目录下
cp -p   连同档案的属性一起复制过去
```
5、mv

6、rm

7、less

查看文件，辅助键帮助查看  输入20页表示从20页开始打开，空格下页，上下键表示一行

8、tail -n

```c
作用1：tail -n 20 文件名，显示后20行     不加-n默认后10行
tail -n -r 20 filename，逆序显示后20行
作用2：tail-f 文件路径       查看一个文件的动态变化
手动打开编辑没用，只能用系统操作，输出重定向可以看到
ls-lf > ./ 1.txt      tail-f ./1.txt
一般用于查看日志
```
9、head -n

只看前n行,方便只看脚本前几行的注释；  不加-n默认前10行

head -n 7 /ect/serves

9、touch

10、chmod

ls-l可以查看文件属性，windows下根据文件后缀判断文件类型，可执行文件为.exe，但是Linux下不看后缀，只要文件属性中rwx中x指为1就是可执行

11、chown

12、find

find path -option [-print] [-exec  或者  -ok  command] {}\;  #[]是可选项的意思，实际使用并不加[]

find /home -type f -size +10k -exec rm-f {};

```c
（1）选项：
-name:按文档名
-type: d表示文件夹，f表示文件  find /etc -type f|wc-l 统计文件数目
（2）根据文件名查找：find / -name *wuxun*      *匹配任意多个字符
find /etc  -name  wuxun?    ?匹配单个字符
严格区分大小写=======》find /etc -iname wuxun?，不区分大小写
作用相当于==》ls /etc |grep wuxun
（3）根据大小查找：find /etc -size +204800  204800字节块=102400k=100M
（4）根据属性查找：find /etc -cmin 30  30分钟内修改过的文件
（5）与或非 ： -a 与   find /etc -size +204800 -a -size -409600
-0 或
（6）查询结果再处理：find /etc -name init* -a -type f -exec ls -l {}\;               查找目录下名称是以init开头的，只要文件不要目录或者软链接，查找出的内容显示其详细属性信息
 find /home -user wuxun -ok rm{} \;  	查找目录下由wuxun用户创建的文件，删除，在删除的时候会提示是否删除  
 ```
13、locate 

===》虽然仅适用于系统文件的查找，但是快速

14、grep
```c
文本中的关键字匹配：grep 关键字 路径  =====》可以得出关键字所在的行及信息
不区分大小写：grep -i 关键字 路径
多个匹配：grep -e “关键字1” -e “关键字2”
打印行号：grep -n
递归查找包含指定字符的文件：grep -r guanjianzi /etc/acpi
排除指定字符串所在的行：grep -v ^# /etc/initab   排除以#开头的所有行
```

15、sed

```
流编辑器
在输出窗口替换文本（不改变原文）：
sed -e 4a\newline pets.txt;   //在第四行后添加新行， 内容为newline
sed '2d' pets.txt;           //删除第二行
sed '2,$d' pets.txt;         //删除2到最后一行
sed '/root/p' pets.txt;     //查找root并打印
sed '/root/d' pets.txt;     //查找root并删除所在行
sed 's/bash/BASH/g'pets.txt;   //查找bash并替换为BASH
sed 's/^/#/g' pets.txt ;      //文档每一行前加#：
sed 's/$/ --- /g' pets.txt;   //文档每一行最后加东西：

sed "s/my/Hao Chen's/g" pets.txt
解释：s表示替换命令，/my/表示匹配my，/Hao Chen’s/表示把匹配替换成Hao Chen’s，/g 表示一行上的替换所有的匹配。

直接修改并在文件中保存（改变原文）：
sed -i "s/my/Hao Chen's/g" pets.txt

```
16、管道

 ls |wc -w  =====>统计某个目录下的文件数目
 
17、正则表达式
```
* 匹配任意多个
.  匹配一个  
[]   匹配一个范围内的    [ 1-9]   [ a-z ]   
$  只匹配行首
^  只匹配行尾    举例：^...4  匹配第四个数是4的
pattern\{n,m\}     举例：A\{3,5\}B   匹配AAAB AAAAB AAAAAB ,B前面有3-5个A
```

18、xargs

xargs可以将管道或者标准输入的数据转换成命令行参数，而日常工作中很多命令是不支持管道来传参的

```cpp
cat test.txt|xargs       //把文本变成单行输出
cat test.txt|xargs -n3   //把文本连起来后每3个单词为一行
cat test.txt|xargs -dx -n3   //把文本连起来后把x替换成空格，并以空格为分界线每三个单词为一行

find /etc/test -type f | xargs ls-l //查找出符合条件的文件后显示行的详细情况

find /etc/test -type f | grep -E "*.oss$"|xargs -i{} python3 1.py fetch {} //查找符合模糊查询条件的文件xxx，依次执行python3 1.py fetch xxx
```
18、sed命令

sed命令包括对文本的 增（行首+行尾） 删 改 查 替换

```
1、新增 a(行首)

(1)sed '2a testContent' test.txt   在第 2 行后面新增一行内容

(2)sed '1,3a testContent' test.txt   在原文的第 1~3 行后面各新增一行内容

2、插入 i （行尾）

(1)sed '2i testContent' test.txt   在第 2 行前面插入一行内容

(2)sed '1,3i testContent' test.txt   在原文的第 1~3 行前面各插入一行内容

3、替换 c

（1）sed '2c testContent' test.txt  将第 2 行内容整行替换

（2）sed '1,3c testContent' test.txt  将第 1~3 行内容替换成一行指定内容


4、删除 d

sed '2d' test.txt    删除第 2 行

sed '1,3d' test.txt  删除第1~3行

5、替换 s

sed 's/old/new/' test.txt  匹配每一行的第一个old替换为new

sed 's/old/new/gi' test.txt  匹配所有old替换为new，g 代表一行多个，i 代表匹配忽略大小写

sed '3,9s/old/new/gi' test.txt  匹配第 3~9 行所有old替换为new


6、多命令参数 -e

sed -e 's/系统/00/g' -e '2d' test.txt     执行多个指令

7、打印 p
sed -n '/user/p' test.txt  打印匹配到user的行，类似grep

sed -n 's/old/new/gp' test   只打印匹配替换的行

```

19、grep sed awk  

```
文本查找------------->小范围查找
grep ------>按行
grep -n  显示行号
grep-v   显示除了这一行的其他行

awk-------->按列
awk -F “:” ‘{print $1}’  ./passwd     作用：打印passwd中，以：为分隔符，打印出每一行：前的数据

sed ------->全文
替换  sed 's/day01/DAY01/g' 1.txt    把文本中所有的day01替换为DAY01
删除  sed '1d' 1.txt    删除第一行        sed ' $d' 1.txt     删除最后一行
```

awk使用方法：awk就是把文件逐行的读入，在每一行以分隔符将这行文本切片（以空格为默认分隔符），切开的部分再进行各种分析处理。
```
awk 'BEGIN{ commands } pattern{ commands } END{ commands }'
```
```
举例：
ls -lh | awk '{print $1}'
ls -lh | awk '{print NR " " $1}'
awk  -F ':'  '{printf("filename:%10s,linenumber:%s,columns:%s,linecontent:%s\n",FILENAME,NR,NF,$0)}' /etc/passwd

pattern是每一行文本都要进行的操作，一般就是执行打印操作，关于打印操作的几点说明：

$0 当前这一行的所有内容（这个变量中存放着整个行的内容）
$1~$n 切片后，当前记录的第n个字段； -F ':'表示以：分割，默认是以空格或者tab键分开
NR 就是行号，从1开始
NF 当前记录中的字段个数，就是有多少列
FILENAME 当前输入文件的名字
```


例题1：awk，统计url中出现频率次数前10的url，并按出现次数的降序排序。
```
数据 t1.txt：
http://192.168.00.1
http://192.168.00.2
http://192.168.00.3
...

cat  t1.log | sort | uniq -c | sort -k 1 -n -r | awk -F '//'  '{print  $2}'| head -10

1、原始访问日志:cat t1.txt

2、排序:cat t1.txt|sort
表示对data文件中的内容进行排序。sort命令是对于每一行的内容根据字典序（ASCII码）进行排序，这样可以保证重复的记录时相邻的。

3、合并相邻重复记录，统计重复数:cat  t1.log | sort | uniq -c
niq -c表示合并相邻的重复记录，并统计重复数。因为uniq -c 只会合并相邻的记录，所以在使用该命令之前需要先排序。
wc -l #统计行数    uniq -c #在输出行前面加上每行在文本里出现的次数

4、重新排序:cat  t1.log | sort | uniq -c | sort -k 1 -n -r
sort -k 1表示对于每行的第一个字段进行排序，这里即指代表重复记录数的那个字段。因为sort命令的默认排序是按照ASCII，
这就会导致按从大到小进行排序时，数值2会排在数值11的前面，所以需要使用-n 参数指定sort命令按照数值大小进行排序。
-r 表示逆序，即按照从大到小的顺序进行排序。

5、wk -F命令分隔IP地址,取第二部分:cat  t1.log | sort  | uniq -c | sort -k 1 -n -r | awk -F '//'  '{print  $2}'

6、使用head -n命令选取前n行 ：cat  t1.log | sort | uniq -c | sort -k 1 -n -r | awk -F '//'  '{print  $2}'| head -10

```
例题2：訪问最多的IP（前10名） 对ip反复的数据进行去重的时候还要汇总，取前10名

awk '{print $1}' clean2_2013_05_30.log | sort -n | uniq -c |sort -nr -k 1|head -10 > top10_2013_05_30

二 帮助
---
1、man

man1 命令信息         man5 配置文件信息

man查完之后   /选项，可以快速查看选型的信息

2、whatis 
```
Whatis	  ls   得到命令的简短信息/
--help
可以得到命令的一些常用选项
```
3、help

查shell内置命令的信息，这些命令用man查不出结果

umask  pwd  if     help if

4、alias:

取别名操作：存放在 ~/.bash中

vim  ~/.bash=======》alias wuxun=’ls-lf’;

## 终端、进程组、SIGHUP以及守护进程的关系

终端就是也是一个进程组，进程组长为bash(pid和ppid都为1)，之后的进程都是由组长fork+exec创建并运行的。bash进程和终端相连，捕捉终端的硬件行为。

终端断开时，会发送SIGHUP给组长bash，组长会把这个SIGHUP转发给所有的组员，使得组员退出。所有当终端关闭时其上的进程都会退出。

如果进程不想退出，有两个办法：

(1)屏蔽SIGHUP：signal(SIGHUP,SIGING);  或者 nohup COMMAD

(2)把进程设置为守护进程，脱离终端控制。



## 如何开启守护进程

（1）调用umask将文件模式创建屏蔽字设置为0

例如，有个文件权限掩码是050，它就屏蔽了文件组拥有者的可读与可执行权限（对应二进制为，rwx, 101）。把文件权限掩码设置为0（即，不屏蔽任何权限），可以增强该守护进程的灵活性。

（2）调用fork创建子进程，然后使父进程退出（exit）。保证守护进程不是会话首进程，这样open的时候就不会被分配终端 。虽然第1个子进程成为了新的进程和会话组长,没有终端关联.但它却可以重新开打控制终端.只有使进程不再成为会话组长才能禁止进程重新打开控制终端。

（3）调用setsid以创建一个新会话，脱离控制终端和进程组。

（4）将当前工作目录更改为根目录。改变工作目录到/tmp防止无法卸载文件系统

（5）关闭不再需要的文件描述符。将标准输入、标准输出、标准错误重定向至/dev/null

（6） 处理SIGCHLD信号.忽略信号    signal(SIGCHLD, SIG_IGN);

三 磁盘操作
---
1、df-h  查看特定目录的内存情况，站在系统角度，一些rm删除的文件，如果还在被某个进程占用，那么df查出的内存还没有发生变化，直到进程结束这个文件才被真正的删除。
```
===>df-h  对应 我的电脑 以kb显示中各个盘占的大小
显示结果：第一列表示是硬盘、光碟还是其他存储设备，最后一列是这些设备的挂载路径；
===>df-hs   只显示文件夹大小，不显示文件夹内具体文件大小
```
2、du-h  查看特定目录的内存情况，不同于df，du统计文件夹大小是通过遍历所有文件求和，所以rm删除一个正在被调用的文件，du是有变化的，df是没变化的。

通过 Isof | grep -i deleted ，可以查看rm删掉但仍占用内存的文件列表

du -ah --max-depth=1

du-sh 只显示汇总的大小

3、mount   =====》 查看当前系统所挂载的硬件设备

4、free-m   查看 系统的内存情况，包括真实可用内存和虚拟内存、buffer 、 cache

![示意图]()

 对应任务管理器 ，-m表示以MB为单位查看内存占用
 
真实可用的内存：1665mb,包含了操作系统分配给进程但是进程还没使用的，也就是第一行的buffers和cached

buffer是输入缓存

cache是输出缓存

计算方法：真实可用的内存free=Mem.free+buffers+cached+1

总的内存=真实可用的内存+真实使用的used

Swap：虚拟内存，用于临时内存，当内存不够时，能够使用硬盘空间，充当临时内存

那buffers和cached都是缓存，两者有什么区别呢？

答：为了提高磁盘存取效率, Linux 采取了两种主要Cache方式：Buffer Cache和Page Cache。Buffer 主要针对I/O操作的，比如网络通信中的read和write，为了防止过于频繁地访问磁盘；而cache是为了权衡cpu和硬盘速度的差距，比如你一个很复杂的计算做完了，下次还要用结果，就把结果放手边一个好拿的地方存着，下次不用再算了。加快了数据取用的速度。


5、ps-aux   ps-ef   ----》进程的pid  

ps-ef|grep chrome

6、top ----->每个进程的资源占用情况
![示意图]()

top -H 可以显示出所有线程情况，不加-h只显示进程

top -H -p <pid> 可以查看特定线程的

多少个正在运行进程、僵尸进程、停止进程、cpu总占比、内存总占比等

实时更新显示的，q退出更新

top显示后按快捷键：

M：将结果按内存占用从大到小排列

P：将结果按照cpu使用率从高到低排列



共享内存：有的进程要依赖其他进程才能运行，这部分共享的内存不属于二者，进程运行的实际内存=常驻内存-共享内存


 top中虚拟内存VIRT和RES常驻内存的关系？
 
VIRT：进程“需要的”虚拟内存大小，包括进程使用的库、代码、数据，以及malloc、new分配的堆空间和分配的栈空间等；如果new了100M，但是我现在只往里面写1M，这时VIRT增加100M，RES增加1M
```
#include <iostream>
 
int main()
{
    char * p = new char [1024*1024*512];
    getchar();
    return 0;
 }
 
top结果如下：
PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
401 huyiyang  17   0  523m  916  792 S  0.0  0.0   0:00.00 ./main
```

8、uptime

查看计算机开机到现在运行了多长时间；

平均负载：0.54, 0.40, 0.20，最近1分钟、5分钟、15分钟系统的负载

9、kill和kill-9

执行kill命令，相当于执行kill-15，系统会发送一个SIGTERM信号给对应的程序。SIGTERM多半是会被阻塞的。kill -9命令，系统给对应程序发送的信号是SIGKILL，即exit。exit信号不会被系统阻塞，所以kill -9能顺利杀掉进程。kill-9能杀掉守护进程和后台进程，kill不可以。

通常有两种情况是不能kill掉的：

一是进程已经成为僵死进程，当它的父进程将它回收或将它的父进程kill掉即可在ps输出看不到了；

二是进程正处在内核状态中，Linux进程运行时分内核和用户两种状态，当进程进入内核状态后，会屏蔽所有信号，包括SIGKIL，所以这个时候kill -9也变得无效了。

10、软连接ln-s  硬连接ln
---
区别：
1、软链接文件的大小和创建时间和源文件不同。硬链接文件和源文件的大小和创建时间一样。

2、硬连接相当于copy-p同步时间 + 实时的内容同步

3、软连接删除源文件，则软连接失效。而硬链接不会失效，因为硬链接和源文件拥有相同的innode号

四、系统相关
---
1、date
```
作用：读取时间，shell脚本的操作都会用到date
用法1：date +%F    输出2018-03-24
等价于 date “%Y+%m+%d”
用法2：date “+%F %T ”    输出2018-03-24 15:34:56
等价于 date “%Y+%m+%d %H:%M:%s”
用法3：获取之前或者之后的某个时间 （用于数据库自动备份）
date”-1 day” “%Y+%m+%d %H:%M:%s”   获取一天前的时间

例如：检查一个命令花费的时间

#!/bin/bash

start=$(date +%s)
nmap man.linuxde.net &> /dev/null   //nmap命令是一款开放源代码的网络探测和安全审核									工具，它的设计目标是快速地扫描大型网络。
end=$(date +%s)
difference=$(( end - start ))
echo $difference seconds.
```
2、crond    

 * * * * *代表 分 时  日 月 周
*：表示取值范围中的每一个数字
,：表示多个取值，比如想在1点，2点6点执行，则可以在时的位置写：1,2,6

问题1：`制定一个计划任务，每间隔1分钟执行一次`

* * * * * /root/test19.sh             表示每天的每分都备份

问题2：`每月1、10、22日的4:45重启network服务`

45  4  1,10,22  *  *  service network restart

3、service

`作用：用于控制一些软件的重启、停止、启动`
sercive 服务名 restart/stop/start

例：启动本机的Apache（网站服务器软件）    sercive httpd start

4、reboot  

重启    shutdown要慎用，关机后就连不上服务器了

5、运行模式（运行级别）

在linux中存在一个进程：init （initialize，初始化），进程id是1。
查看进程：#ps -ef|grep init


该进程存在一个对应的配置文件：inittab（系统运行级别配置文件，位置/etc/inittab）
vim /etc/inittab=====》initdefault值设置成3=====》纯命令行模式
文件的主要内容：

根据上述的描述，可以得知，Centos6.5中存在7种运行级别/模式。

与该级别相关的几个命令：

#init 0		表示关机

#init 3		表示切换到纯命令行模式（命令行输入只是临时切换，重启之后又恢复）

#init 5		切换到图形界面

#init 6 		重启电脑

注意：init指令需要root

这些命令其实都是调用的init进程，将数字（运行级别）传递给进程，进程去读配置文件执行对应的操作。

！！设置模式永久为命令行模式：将/etc/inittab文件中的initdefault值设置成3，然后重启操作系统。

6、chkconfig

开机启动项管理

查看：chkconfig --list

删除：chkconfig --del 服务名

添加：chkconfig --add 服务名

8、rpm管理

查询、卸载、安装。

五、用户
---
1、hostname

#hostname     ======》查看主机名
#hostname -f	  ======》查询FQDN（全限定域名）
hostname 设置的主机名======》临时设置主机名


②永久设置主机名（需要重启）

先找到一个文件

/etc/sysconfig/network		【主机名的配置文件】

修改其中的HOSTNAME为自己需要设置的永久主机名


③修改主机的hosts文件，将yunwei指向本地（设置FQDN），就是在本机的DNS备案

Hosts文件的位置：/etc/hosts    把127.0.0.1绑定给FQDN



问题：不设置FQDN会怎么样？

	①很多开源服务器软件（例如Apache）则无法启动，或出现报错；
	
	②方便记忆，看到主机名对其作用有一个初步判断；
	
	③如果不设置则会影响本地的域名的解析（本地访问）；

2、id指令

===》查看用户和用户组信息

id输出的结果对不对呢？怎么验证？  

验证用户信息：通过文件cat /etc/passwd          <====== ~/.bash存放别名

验证用户组信息：通过文件cat /etc/group

3、Whoimi

====》用于shell脚本写日志

4、用户与用户组管理（重点）

用户账号的添加、删除、修改以及用户密码的管理。

用户组的管理。
```cpp
注意三个文件：
/etc/passwd				存储用户的关键信息
/etc/group				存储用户组的关键信息
/etc/shadow				存储用户的密码信息
```
（1）、用户管理

①添加用户  useradd

常用语法：#useradd 选项 用户名

常用选项：

	-g：表示指定用户的用户主组，选项的值可以是用户组的id，也可以是组名
	
	-G：表示指定用户的用户附加组，选项的值可以是用户组的id，也可以是组名
	
	-u：uid，用户的id（用户的标识符），系统默认会从500之后按顺序分配uid，如果不想使用系统分配的，可以通过该选项自定义【类似于腾讯QQ的自选靓号情况】
	-c comment：添加注释
	
案例：创建用户zhangsan，不带任何选项

验证是否成功：

	a. 验证/etc/passwd的最后一行，查看是否有zhangsan的信息；
	
	b. 验证是否存在家目录（ls  /home/）；

扩展：认识passwd文件

用户名:密码:用户ID:用户组ID:注释:家目录:解释器shell

用户名：创建新用户名称，后期登录的时候需要输入；

密码：此密码位置一般情况都是“x”，表示密码的占位；

用户ID（UID）：用户的识别符；

用户组ID（GID）：该用户所属的主组ID；

注释：解释该用户是做什么用的；

家目录：用户登录进入系统之后默认的位置；

解释器shell：等待用户进入系统之后，用户输入指令之后，该解释器会收集用户输入的指令，传递给内核处理；

注意：在不添加选项的时候，执行useradd之后会执行一系列的操作

	a. 创建同名的家目录；
	
	b. 创建同名的用户组；

案例：添加选项，创建用户lisi，让lisi属于501主组，附加组500，自选靓号666。

注意：查看用户的主组可以查看passwd文件，查看附加组可以查看group文件。

②修改用户 usermod

常用语法：#usermod 选项 用户名   （user modify）

常用选项：
	-g：表示指定用户的用户主组，选项的值可以是用户组的id，也可以是组名
	
	-G：表示指定用户的用户附加组，选项的值可以是用户组的id，也可以是组名
	
	-u：uid，用户的id（用户的标识符），系统默认会从500之后按顺序分配uid，如果不想使用系统分配的，可以通过该选项自定义【类似于腾讯QQ的自选靓号情况】
	-l：修改用户名

案例：修改zhangsan用户主组为500，附加组改为501

#usermod -g 500 -G 501 zhangsan

案例：修改zhangsan用户用户名，改为wangerma

#usermod -l 新的用户名 旧的用户名

#usermod -l wangerma zhangsan


③设置密码 passwd

Linux不允许没有密码的用户登录到系统，因此前面创建的用户目前都处于锁定状态，需要设置密码之后才能登录计算机。

常用语法：#passwd 用户名

案例：设置wangerma用户的密码

在设置密码的时候也是没有任何输入提示的，放心输入，确保两次输入的密码一致，按下回车即可。

也可以使用弱密码，但是不建议，否则会看到以下的提示：


设置密码之后shadow文件中的体现：能够看出lisi用户没有密码的。


在设置用户密码之后可以登录帐号，例如此处需要登录wangerma

切换用户命令：#su [用户名]	（switch user）

如果用户名不指定则表示切换到root用户。

切换用户需要注意的事项：

	a. 从root往普通用户切换不需要密码，但是反之则需要root密码；
	
	b. 切换用户之后前后的工作路径是不变的；
	
	c. 普通用户没有办法访问root用户家目录，但是反之则可以；

④删除用户 userdel 

常用语法：#userdel 选项 用户名   （user delete）

常用选项：

	-r：表示删除用户的同时，删除其家目录；
	
案例：删除wangerma用户

注意：已经登录的wangerma用户删除的时候提示删除失败，但是没有登录的lisi用户可以正常删除。

解决办法：简单粗暴，kill对应用户的全部进程    3151是3152的父进程，要删3151


提示：所有跟用户操作的命令（除passwd外）只有root超级管理员有权限执行。

（2）、用户组管理

每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。不同Linux 系统对用户组的规定有所不同，如Linux下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。

用户组的管理涉及用户组的添加、删除和修改。组的增加、删除和修改实际上就是对/etc/group文件的更新。

文件结构：

用户组名:密码:用户组ID:组内用户名

密码：X表示占位符，虽然用户组可以设置密码，但是绝大部分的情况下不设置密码；

组内用户名：表示附加组是该组的用户名称；

①用户组添加  gropuadd

常用语法：#groupadd 选项 用户组名

常用选项：
	-g：类似用户添加里的“-u”，-g表示选择自己设置一个自定义的用户组ID数字，如果自己不指定，则默认从500之后递增；

案例：使用groupadd指令创建一个新的用户组，命名为Administrators


②用户组改名改id  groupmod

常用语法：#groupmod 选项 用户组名

常用选项：

	-g：类似用户修改里的“-u”，-g表示选择自己设置一个自定义的用户组ID数字
	
	-n：类似于用户修改“-l”，表示设置新的用户组的名称
	
案例：修改Administrators用户组，将组ID从502改成520，将名称改为admins


③用户组删除  groupdel

常用语法：#groupdel 用户组名


注意：当如果需要删除一个组，但是这个组是某个用户的主组时，则不允许删除；如果确实需要删除，则先从组内移出所有用户(usermod -g )，0表示root组



六、练习

1、删除/tmp下所有以A开头的文件？  rm-f /tmp/A*

2、如何查看系统最后创建的3个用户？ tail -3 /etc/passwd

3、查看系统有多少用户？  cat ect/passwd|wc-l   或者 wc-l ect/passwd

4、快速返回家目录？  cd ~

5、查看/etc 的大小？  du-sh /etc

6、删除/tmp下所有的文件？ rm-rf /tmp/*

7、删除/tmp下所有的文件以及自己？rm-rf /tmp/

8、杀死Apach进程？   killall httpd   或者 ps-ef  然后再kill pid号

七、编辑器vim

sublime或者vim

1、vim的三种模式：

命令模式、编辑模式、末行模式

命令模式：通过命令进行操作，包括删除行、复制行、移动、粘贴（打开后的默认模式）

编辑模式：文本修改

末行模式：搜索、替换、保持、退出、高亮

vim的打开：

vim +数字 文件的路径 	   作用：打开文件，并且将光标移动到指定行

vim +/关键词 文件的路径  作用：打开文件，并且将关键词高亮显示

cp /ec/passwd ~/  在拷贝出来的文件中进行编辑

2、命令模式（vim打开就是命令模式）

光标的移动
（1）移动到行首：shift+6（不是小键盘的6）   ^

（2）移动到行尾：shift+4                     $

（3）移动到首行：gg  最后一行：G    指定行：n或者 数字+G

（4）pgup和pgdn  

复制、剪切和粘贴

（1）dd → 剪切行；n+dd，向下剪切n行；D，删除后保留空行；

（2）x → 删当前光标所在的一个字符。

（3）yy，复制；n+yy，复制向下n行；

（4）可视化选择，ctrl+v，上下左右选择后按yy复制，p粘贴

3、末行模式

(1)保存

:wq → 存盘 + 退出 (:w 存盘, :q 退出) （注：:w 后可以跟文件名）；:q!，不保存； 

即使文件没有修改，:wq也会更新修改时间（会混淆对文件的修改时间，不推荐）

:x，（小写）是二者的结合，没有修改时表示直接退出（不更新修改时间），修改时就保存退出

：X（大写） 不要和小写x混淆，X表示对文件加密

(2)调用外部命令
:! ls，在vim钟调用外部命令；可以用来检查是不是成功保存了
(3)搜索查找
：/关键词，查找结果中N向上下一个，n向下下一个
(4)替换
:s/关键词/新的内容 ----->只替换光标所在行的第一个
:s/关键词/新的内容/g------->只替换光标所在行的全部
:%s/关键词/新的内容-------->替换整个文档，每行只替换一处
:%s/关键词/新的内容/g------->替换整个文档
(5)显示行号
:set nu=========>:set nonu，关闭显示行号

4、编辑模式

进入：命令模式下，i/a即可进入，i是光标前，a是光标后

退出：esc

5、常用功能

（1）关闭语法高亮：:syntax off ====>syntax on

（2）配置文件 /etc/vimrc ：

第一种：只在本文件起作用    :set nu

第二种：只在本个人的所有文件起作用 cd ~     touch .vimrc    vim .vimrc进行修改并保存

第三种：全局，所有用户都配置   vim  /etc/vimrc 

（3）异常退出 

解决办法：异常退出会产生交换文件，命名方式为：.文件名.swp，删除掉交换文件即可

（4）取别名 

alias    在 ~/.bashrc 中进行修改 alias xiewuxun=’命令’，启动仍有效

alias gj=’shutdown -h now’ 重制关机命令

八、编译g++

1.make   --------》makefile

针对的是一个文件夹中有很多个要编译的文件，先编写makefile文档，这个文档标明需要编译的文件、编译的规则等，然后在命令行中运行make；

make 会在当前的目录下搜寻 Makefile ，并执行相应的内容；

makefile例子：

test : wuxun.cpp  xiaohuang.cpp

gcc-o test wuxun.o xiaohuang.o


2、gcc

gcc-o       生成.exe并指定的文件名   gcc -o hello.exe   wuxun.c

gcc -g      -g 选项告诉 GCC 产生能被 GNU 调试器使用的调试信息以便调试你的程序

gcc -E ======>gcc -S========>gcc -c
预处理       汇编          .o二进制文件

3、gdb调试

g++ -g hello.cpp -o  hello
gdb hello


直接回车的作用是重复上一指令，对于单步调试非常方便；

九、shell

1、shell的种类？

答：sh，csh，bash

2、hello word

流程：touch test.sh======>vim test.sh=====>chmod u+x test.sh=======>./test.sh

!在运行时一定要写成 ./test.sh，而不是 test.sh;

#!/bin/bash
echo “hello word”       打印字符串单双引号都可以

3、打印一个变量

#!/bin/bash

testname=’hello word’      注意等号前后不要加空格

echo $testname

4、定义一个变量，输出当前时间，要求格式为“年-月-日 时:分:秒”。

#!/bin/bash

dt=`date +’%F %T’`     反引号

echo $dt

关于单双引号的问题：

1、双引号能够识别变量 ; 双引号能够实现转义（类似于“\*”）

2、单引号是不能识别变量，

3、table上边的反引号：当在脚本中需要执行一些指令，并且将执行的结果赋给变量的时候，需要使用“反引号”。

5、接收用户输入

read  -p  提示信息  变量名

read -p ‘输入要创建的文件路径 ：’ filepath     <========= /root/wuxun.txt

touch $filepath

ls -l $filepath

6、条件判断
if condition

then

    command1 
    
else

command2

fi

7、算数运算

下表列出了常用的算术运算符，假定变量 a 为 10，变量 b 为 20：
```cpp
运算符	说明	举例
+	加法	`expr $a + $b` 结果为 30。
-	减法	`expr $a - $b` 结果为 -10。
*	乘法，需要转义	`expr $a \* $b` 结果为  200。
/	除法	`expr $b / $a` 结果为 2。
%	取余	`expr $b % $a` 结果为 0。
=	赋值	a=$b 将把变量 b 的值赋给 a。
==	相等。用于比较两个数字，相同则返回 true（真）。	[ $a == $b ] 返回 false（假）。
!=	不相等。用于比较两个数字，不相同则返回 true。	[ $a != $b ] 返回 true。
条件表达式要放在方括号之间，并且要有空格，例如: [$a==$b] 是错误的，必须写成 [ $a == $b ]。

原生bash不支持运算，调用命令expr
#!/bin/bash
val=`expr 2 + 2`          反引号，2 + 2必须有空格
echo "两数之和为 : $val"

#!/bin/bash
a=10
b=20
echo’$a + $b =’`expr $a + $b`
echo’$a * $b =’`expr $a \* $b`     通配符*在这里需要转义，\*
```
8、判断数字    
```cpp
equal  great    less     great orequal    
运算符	说明	举例
-eq	检测两个数是否相等，相等返回 true。	[ $a -eq $b ] 返回 false。
-ne	检测两个数是否相等，不相等返回 true。	[ $a -ne $b ] 返回 true。
-gt	检测左边的数是否大于右边的，如果是，则返回 true。	[ $a -gt $b ] 返回 false。
-lt	检测左边的数是否小于右边的，如果是，则返回 true。	[ $a -lt $b ] 返回 true。
-ge	检测左边的数是否大于等于右边的，如果是，则返回 true。	[ $a -ge $b ] 返回 false。
-le	检测左边的数是否小于等于右边的，如果是，则返回 true。	[ $a -le $b ] 返回 true。
```

9、逻辑运算与或非
```cpp
运算符	说明	举例
!	非运算，表达式为 true 则返回 false，否则返回 true。	[ ! false ] 返回 true。
-o	或（或者）运算，有一个表达式为 true 则返回 true。	[ $a -lt 20 -o $b -gt 100 ] 返回 true。
-a	与（并且）运算，两个表达式都为 true 才返回 true。	[ $a -lt 20 -a $b -gt 100 ] 返回 false。
```

10、字符串运算符
```cpp
下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：
运算符	说明	举例
=	检测两个字符串是否相等，判断数字是==	[ $a = $b ] 返回 false。
!=	检测两个字符串是否相等，不相等返回 true。	[ $a != $b ] 返回 true。
-z	检测字符串长度是否为0，为0返回 true。	[ -z $a ] 返回 false。
-n	检测字符串长度是否为0，不为0返回 true。	[ -n $a ] 返回 true。
str	检测字符串是否为空，不为空返回 true。	[ $a ] 返回 true。
```

```
12、脚本附加选项（重点）

传递：./test.sh a b c      自己手动打造一个 ls-lh  难点在怎么把参数传给脚本

脚本中接收：$0表示文件名，$1表示第一个，$2表示第二个

===》./test.sh -add user_xie

vim ~/.bash ======>取别名 alias user= ‘./test.sh ’======>user -add user_xie

十、mysql

masql，mariadb（完全兼容mysql）

数据库：可以看作是整个excel文件,内包含多个文件表。

数据表：可以看作是一个excel文件中的工作表。

行（记录）：可以看作是一个工作表中的一行

列（字段）：可以看作是一个工作表总的一列

1、库操作
```cpp
命令在MySQL终端命令行中执行（大小写均可）：
show datebases；       显示所有的数据库
CREATE DATABASE 库名;			创建数据库
DROP DATABASE 库名;			删除数据库
USE 库名;					切换数据库
```
2、表操作
```
（1）SHOW TABLES;				显示当前数据库中所有的表名（必须先use数据库）	
（2）CREATE TABLE 表名称				在当前数据库下创建数据表
(
列名称1 数据类型 [NOT NULL AUTO_INCREMENT], ====》主键  自动递增
列名称2 数据类型,           ======》常见的数据类型：int，char，varchar(不定长字符)
列名称3 数据类型,										
PRIMARY KEY(主键字段名)   ========》主键一般就是序号所在的那一列（主键不能重复）。
);
```
（3）DESC 表名;	描述一个数据表（查看表结构）

（4）DROP TABLE [IF EXISTS] 表名;			删除一个数据表

例子：在当前库创建表，名为wuxun，要求字段为：

ID段，11位整形，不为空，递增==》主键

Username， varchar类型，20长度

Password，char类型，32长度

creat table wuxun(
id int(11) nou null auto_increase,
Username varchar(20),
Password char(32)
primary key(id)
);

3、记录操作
（1）增加记录
语法1：INSERT INTO 表名称 VALUES (值1, 值2,....);
语法2：INSERT INTO 表名称 (列1, 列2,...) VALUES (值1, 值2,....);
例子：insert into wuxun (username,password) valus(‘zhangsan’,’OWEJ88288\8DSADSA88’)；
字符串必须要要’  ’
（2）更新记录
语法：UPDATE 表名称 SET 列名称1 = 新值1,列名称2 = 新值2… WHERE 列名称 = 某值;
更新where一定要写，否则就是改变整个表!!!
例子：UPDATE wuxun set password=’fjdsflclkdsnl’ where id>=2;
（3）查询记录
SELECT 列名称1,列名称2… FROM 表名称 WHERE 条件;
SELECT 列名称1 别名1,列名称2 别名2 FROM 表名称 WHERE 条件;
SELECT * FROM 表名称 WHERE 条件;                     =======》列出全部列
例子：select username,password from wuxun where id=1;
SELECT id, score points, name FROM students WHERE gender = 'M';
SELECT * FROM students WHERE (score < 80 OR score > 90) AND gender = 'M';

按照自定义的排序条件（正序）：
SELECT id, name, gender, score FROM students ORDER BY score;
按照自定义的排序条件（倒序）：
SELECT id, name, gender, score FROM students WHERE class_id = 1 ORDER BY score DESC;
4、删除
DELETE FROM 表名称 WHERE 列名称 = 值;
5、备份
 全量备份（数据+结构）：#mysqldump -uroot -p123456 -A > 备份文件路径       （A指的是all）
！！指定库备份（数据+结构）：# mysqldump -uroot -p123456 库名 > 备份文件路径
多个库备份（数据+结构）：# mysqldump -uroot -p123456 --databases db1 db2 >  备份文件路径
备份文件一般是：xxx.sql文件
例子：mysqldump -uroot -p123456 -A > /root/sql_20190609.sql
vim /root/sql_20190609.sql
例子：每1分钟备份一次.sql
mkfile 19.sh      //相当于touch 然后 chmod +x 
vim 19.sh  

#!/bin/bash
filename=”test_”`date+’%T%m%d%m%s’`”.sql”
mysqldump -uroot -p123456 -A > /root/$filename

crond    制定一个计划任务，每间隔1分钟执行一次
* * * * * /root/test19.sh              * * * * *代表分 时  日 月 周，表示每天的每分都备份

6、还原
在mysql中： source 备份文件路径
例子：source  sql_20190609.sql

例子2：还原table.sql文件到test库
首先要设置mysql连接字符集： （mysql中执行） set names utf8;


ls -r
ssh 用户名@ip地址
gdb
find和grep：find根据文件名查找，grep根据文件内容查找
makefile:gcc -L 库路径 -lcast   查找库路径中结尾为cast的库
共享库、静态库：

文件流：stdin stdout stdeer------》FILE *stdout

刷新终端缓冲区：
1、fflush
2、\n
3、文件正常关闭
4、缓存区满了



```
int main()
{
printf(“gcc”);//为什么在终端不显示呢？因为数据存到了行缓存中了，需要刷新才能显示
while(1)
{}
printf(“2”);
}

int main()
{
FILE *p=fopen(“tmp.txt”,”w+”);
fprintf(p,”wdjwd”);
//fprintf(p,”wdjwd\n”);行缓存，对全缓存无效
//fflush(p);//行缓存和全缓存都奏效
while(1){}
return 0;
}
```
