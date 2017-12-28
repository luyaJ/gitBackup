---
title: 学习linux之第二篇
toc: true
date: 2017-11-02 18:35:28
tags: linux
categories: 工具
---

这是linux后半部分的学习，记录RPM包以及shell，正则表达式等内容。

<!--more-->

# 安装rpm包及源码包

## RPM(RedHat Package Manager)工具

RPM是以一种数据库记录的方式来将我们需要的套件安装到linux主机的一套管理程序。

```bash
[root@localhost jly]# mount /dev/cdrom /mnt/
mount: /dev/sr0 写保护，将以只读方式挂载
[root@localhost jly]# ls /mnt/
CentOS_BuildTag  GPL       LiveOS    RPM-GPG-KEY-CentOS-7
EFI              images    Packages  RPM-GPG-KEY-CentOS-Testing-7
EULA             isolinux  repodata  TRANS.TBL
[root@localhost jly]# ls /mnt/Packages/|head
389-ds-base-1.3.5.10-11.el7.x86_64.rpm
389-ds-base-libs-1.3.5.10-11.el7.x86_64.rpm
abattis-cantarell-fonts-0.0.16-3.el7.noarch.rpm
abrt-2.1.11-45.el7.centos.x86_64.rpm
abrt-addon-ccpp-2.1.11-45.el7.centos.x86_64.rpm
abrt-addon-kerneloops-2.1.11-45.el7.centos.x86_64.rpm
abrt-addon-pstoreoops-2.1.11-45.el7.centos.x86_64.rpm
abrt-addon-python-2.1.11-45.el7.centos.x86_64.rpm
abrt-addon-vmcore-2.1.11-45.el7.centos.x86_64.rpm
abrt-addon-xorg-2.1.11-45.el7.centos.x86_64.rpm
```

### 安装和升级一个rpm包

```bash
[root@localhost jly]# rpm -ivh /mnt/Packages/abattis-cantarell-fonts-0.0.16-3.el7.noarch.rpm 
准备中...                          ################################# [100%]
    软件包 abattis-cantarell-fonts-0.0.16-3.el7.noarch 已经安装

# 升级包(-U表示升级)
[root@localhost jly]# rpm -Uvh /mnt/Packages/abattis-cantarell-fonts-0.0.16-3.el7.noarch.rpm 

# 卸载包
[root@localhost jly]# rpm -e /mnt/Packages/

# 查询一个包是否已安装
[root@localhost jly]# rpm -q abattis-cantarell-fonts
abattis-cantarell-fonts-0.0.16-3.el7.noarch

# 查询当前系统已安装的rpm包
root@localhost jly]# rpm -qa

# 得到一个已安装的rpm包的相关信息
[root@localhost jly]# rpm -qi abattis-cantarell-fonts
```

* `-i`:表示安装
* `-v`:表示可视化
* `-h`:表示显示安装进度

另外在安装rpm包时，常用的附带参数还包括：
`--force`:表示强制安装，即使覆盖属于其他包的文件也要安装。
`--nodeps`:表示当要安装的rpm包依赖于其他包时，即使其他包没有安装，也要安装这个包。

## yum工具

```bash
# 列出所有可用rpm包
[root@localhost jly]# yum list

# 搜索一个包
[root@localhost jly]# yum search vim
已加载插件：fastestmirror, langpacks
Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
Loading mirror speeds from cached hostfile
 * base: centos.ustc.edu.cn
 * extras: centos.ustc.edu.cn
 * updates: centos.ustc.edu.cn
================================================================================================ N/S matched: vim =================================================================================================
protobuf-vim.x86_64 : Vim syntax highlighting for Google Protocol Buffers descriptions
vim-X11.x86_64 : The VIM version of the vi editor for the X Window System
vim-common.x86_64 : The common files needed by any version of the VIM editor
vim-enhanced.x86_64 : A version of the VIM editor which includes recent enhancements
vim-filesystem.x86_64 : VIM filesystem layout
vim-minimal.x86_64 : A minimal version of the VIM editor

  名称和简介匹配 only，使用“search all”试试。

# 安装一个rpm包（不加-y选项会以与用户交互的方式安装）
[root@localhost jly]# yum install [-y] [rpm包名]

# 卸载一个rpm包
[root@localhost jly]# yum remove [-y] [rpm包名]

# 升级一个rpm包
[root@localhost jly]# yum update [-y] [rpm包名]
```

# Shell基础知识

shell是系统跟计算机硬件交互时使用的中间介质，它只是系统的一个工具。如果把计算机硬件比作一个人的躯体，那系统内核就是人的大脑，shell称为人的五官更为贴切。

## 一些命令

### 记录历史命令

* `!!`:表示执行上一条指令。
* `!n`:表示执行命令历史中的第n条指令。

```bash
[root@localhost jly]# pwd
/home/jly
[root@localhost jly]# !!
pwd
/home/jly

[root@localhost jly]# history |grep 25
   25  exit
   34  history |grep 25
[root@localhost jly]# !25
exit
exit
```

### 别名

使用alias命令把一个常用的且很长的指令别名为一个简单易记得指令；unalias解除别名功能。
```bash
[root@localhost jly]# alias
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
[root@localhost jly]# alias aming='pwd'
[root@localhost jly]# aming
/home/jly
[root@localhost jly]# unalias aming   
[root@localhost jly]# aming
bash: aming: 未找到命令...
```

### 通配符

使用“*”来匹配0个或多个字符，用“？”匹配一个字符。
```bash
# ls -d test*
# test1.txt test2 test3 test.pl
# ls -d test?
# test2 test3 
```

### 作业控制

使用vi命令编辑test.txt:
```bash
[jly@localhost ~]$ vi test.txt
testtesttest
```

按ESC键后使用Ctrl+z暂停任务，如下：
```bash
[jly@localhost ~]$ vi test.txt

[1]+  已停止               vim test.txt
```

上面提示“vi test.txt” 已经停止了，使用fg命令可以恢复它，然后又会进入刚才的vi窗口。再次使其暂停，然后输入jobs，可以看到被暂停或者在后台运行的任务：
```bash
[jly@localhost ~]$ jobs
[1]+  已停止               vim test.txt
```

如果想把暂停的任务放到后台重新运行，就是用bg命令(但是vi似乎不支持在后台运行)：
```bash
[jly@localhost ~]$ bg
[1]+ vim test.txt &

[1]+  已停止               vim test.txt
```

## linux shell中的特殊符号

* 注释符号`#`：在`#`后面的内容都会被忽略。
* 脱义字符`\`：会将后面的特殊符号（如"*"）还原为普通字符。
```bash
[jly@localhost ~]$ ls -d test\*
ls: 无法访问test*: 没有那个文件或目录
```
* 管道符`|`：将前面命令的输出作为后面命令的输入。
* `wc`命令用于统计文档的行数、字符数或词数。
* 符号"$"用于作变量前面的标志符。
```bash
[jly@localhost ~]$ ls test.txt
test.txt
[jly@localhost ~]$ ls !$
ls test.txt
test.txt
```
* 符号"~"表示用户的家目录，root用户的家目录是`/root`,普通用户则是`/home/username`

### 重定向符号

* '>' 和 '>>':分别表示取代和追加。
* 当我们运行一个命令报错时，报错信息会输出到当前屏幕，如果想重定向到一个文本里，则要用重定向符号“2>”或者“2>>”,分别表示错误重定向和错误追加重定向。

# 正则表达式

## grep工具的使用

命令格式：`grep [-cinvABC] 'word' filename`。
* -c: 表示打印符合要求的行数
* -i: 表示忽略大小写
* -n: 表示输出符合要求的行及行号
* -v: 表示打印不符合要求的行号grep
* -A: 后面跟一个数字（有无空格都可以）；-A2：表示打印符合要求的行以及下面两行
* -B: 后面跟一个数字；-B2：表示打印符合要求的行以及上面两行
* -C: 后面跟一个数字；-C2：表示打印符合要求的行以及上下各两行

### 过滤(不)带有某个关键词的行并输出行号
```bash
[jly@localhost ~]$ grep -n 'root' /etc/passwd
1:root:x:0:0:root:/root:/bin/bash
10:operator:x:11:0:operator:/root:/sbin/nologin
43:dockerroot:x:989:984:Docker User:/var/lib/docker:/sbin/nologin
```

```bash
[jly@localhost ~]$ grep -nv 'nologin' /etc/passwd
1:root:x:0:0:root:/root:/bin/bash
6:sync:x:5:0:sync:/sbin:/bin/sync
7:shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
8:halt:x:7:0:halt:/sbin:/sbin/halt
42:jly:x:1000:1000:jly:/home/jly:/bin/bash
```

### 过滤所有包含数字的行
```bash
[jly@localhost ~]$ grep '[0-9]' /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
```
在正则表达式中，`^`表示行的开始，`$`表示行的结尾，空行则用`^$`表示。

`[^字符]`,表示除[]内字符之外的字符。
```bash
[jly@localhost ~]$ cat test.txt
123
abc
456
abc2323
#lsadfg
alllll
[jly@localhost ~]$ grep '^[^a-zA-Z]' test.txt
123
456
#lsadfg
[jly@localhost ~]$ grep '[^a-zA-Z]' test.txt
123
456
abc2323
#lsadfg
```

### 指定要过滤的字符的出现次数

```bash
[jly@localhost ~]$ grep 'o\{2\}' /etc/passwd
root:x:0:0:root:/root:/bin/bash
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
setroubleshoot:x:991:988::/var/lib/setroubleshoot:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
dockerroot:x:989:984:Docker User:/var/lib/docker:/sbin/nologin
```

## egrep工具的使用

### 筛选一个或多个前面的字符
```bash
[jly@localhost ~]$ egrep 'o+' test.txt
rot:x:0:0:/rot:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
[jly@localhost ~]$ egrep 'oo+' test.txt
operator:x:11:0:operator:/root:/sbin/nologin
```

### 筛选零个或一个前面的字符
```bash
[jly@localhost ~]$ egrep 'o?' test.txt
rot:x:0:0:/rot:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
```

### 筛选字符串1或字符串2
```bash
[jly@localhost ~]$ egrep 'aaa|111|oo' test.txt
operator:x:11:0:operator:/root:/sbin/nologin
1111111111111111111111111111111111
aaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

### ()的应用
```bash
[jly@localhost ~]$ egrep 'r(oo)|at(o)' test.txt
operator:x:11:0:operator:/root:/sbin/nologin
[jly@localhost ~]$ egrep '(oo)+' test.txt
operator:x:11:0:operator:/root:/sbin/nologin
```

## sed工具的使用

grep工具的功能还不够强大，它实现的只是查找功能，而不能把查找的内容替换。

### 打印某行

命令格式：`sed -n 'n'p filename`。
```bash
[jly@localhost ~]$ sed -n '2'p test.txt
operator:x:11:0:operator:/root:/sbin/nologin
[jly@localhost ~]$ sed -n '1,$'p test.txt
rot:x:0:0:/rot:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
1111111111111111111111111111111111
aaaaaaaaaaaaaaaaaaaaaaaaaaaa
[jly@localhost ~]$ sed -n '1,3'p test.txt
rot:x:0:0:/rot:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
1111111111111111111111111111111111
```
第一个是打印指定的第二行，第二个是打印所有行，第三个是打印1到3行。

### 打印包含某个字符串的行

```bash
[jly@localhost ~]$ sed -n '/root/'p test.txt
operator:x:11:0:operator:/root:/sbin/nologin
[jly@localhost ~]$ sed -n '/^1/'p test.txt
1111111111111111111111111111111111
[jly@localhost ~]$ sed -n '/in$/'p test.txt
operator:x:11:0:operator:/root:/sbin/nologin
```

sed命令加上-e选项可以实现多个行为：
```bash
[jly@localhost ~]$ sed -e '1'p -e '/111/'p -n test.txt
rot:x:0:0:/rot:/bin/bash
1111111111111111111111111111111111
```

### 删除某行或者多行

参数“d”表示删除的动作，可以删除指定的单行以及多行，可以删除匹配某个字符的行，可以删除从某一行开始到文档最后一行的所有行。
```bash
[jly@localhost ~]$ sed '1'd test.txt
operator:x:11:0:operator:/root:/sbin/nologin
1111111111111111111111111111111111
aaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

### 替换字符或者字符串

“s”就是替换的动作，“g”表示本行全局替换，不加g则只替换本行出现的第一个。
```bash
[jly@localhost ~]$ sed  '1,2s/ot/to/g' test.txt
rto:x:0:0:/rto:/bin/bash
operator:x:11:0:operator:/roto:/sbin/nologin
1111111111111111111111111111111111
aaaaaaaaaaaaaaaaaaaaaaaaaaaa
[jly@localhost ~]$ sed  's#ot#to#g' test.txt
```
上面两种格式都可以。

### 直接修改文件的内容

```bash
[jly@localhost ~]$ sed -i 's/ot/to/g' test.txt
[jly@localhost ~]$ cat test.txt
rto:x:0:0:/rto:/bin/bash
operator:x:11:0:operator:/roto:/sbin/nologin
1111111111111111111111111111111111
aaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

# shell脚本

## 什么是shell？

### shell脚本的创建和执行

编写我的第一个shell脚本（vim进入时是一般模式，按i键，进入编辑模式，按esc键退出编辑模式，然后“:wq”保存并退出vim）：
```bash
[root@localhost jly]# cd /usr/local/sbin/
[root@localhost sbin]# vim first.sh
#! /bin/bash
## this is my first shell script
## writen by luyaj 2017-11-18

date
echo "hello world!"
```

shell脚本通常是以.sh为后缀名的。本例中，脚本文件first.sh的第一行是以“#! /bin/bash”开头，表示该文件使用的是bash语法。下面执行脚本：
```bash
[root@localhost sbin]# sh first.sh
2017年 11月 18日 星期六 20:31:16 CST
hello world!
```

使用-x选项来查看脚本的执行过程：
```bash
[root@localhost sbin]# sh -x first.sh
+ date
2017年 11月 18日 星期六 20:41:38 CST
+ echo 'hello world!'
hello world!
```

### 命令date

* date +%Y : 以四位数字格式打印年份
* date +%y : 以两位数字格式打印年份
* date +%m : 月份
* date +%d : 日期
* date +%H : 小时
* date +%M : 分钟
* date +%S : 秒
* date +%w : 日期（0表示周日）

写此命令时是：2017年11月18日 20:43
```bash
[root@localhost sbin]# date +"%Y-%m-%d %H:%M:%S"
2017-11-18 20:42:27
[root@localhost sbin]# date -d "-1 day" +%d
17
[root@localhost sbin]# date -d "-1 hour" +%H
19
[root@localhost sbin]# date -d "-1 min" +%M
42
```

## shell脚本中的变量

### 数学运算
```bash
a=1
b=2
sum=$[$a+$b]
echo "$a+$b=$sum"

[root@localhost sbin]# sh first.sh
1+2=3
```

### 和用户交互(read)
```bash
read -p "please input a number:" x
read -p "please input another number:" y
sum=$[$x+$y]
echo "the sum of the two num is: $sum"

[root@localhost sbin]# sh first.sh
please input a number:2
please input another number:4
the sum of the two num is: 6
```

### shell脚本预设变量
```bash
[root@localhost sbin]# vim first.sh
sum=$[$1+$2]
echo "$sum"
[root@localhost sbin]# sh -x first.sh 1 2
+ sum=3
+ echo 3
3
```

脚本中的$1和$2就是shell脚本的预设变量。另外还有$0,表示脚本本身的名字：
```bash
echo "$1 $2 $0"
[root@localhost sbin]# sh first.sh 1 2
1 2 first.sh
```

## shell脚本中的逻辑判断

### 不带else

```bash
[root@localhost sbin]# cat first.sh
#! /bin/bash
read
read -p "please input you score:" a
if((a<60)); then
    echo "you fail"
fi
[root@localhost sbin]# sh first.sh
please input you score:34
you fail
```
"((a<60))"是shell脚本中特有的格式，只用一个小括号或者不用都会报错。

### 带有else

```bash
[root@localhost sbin]# cat first.sh
#! /bin/bash

read -p "please input you score:" a
if((a<60)); then
 echo "you fail."
else
 echo "good!pass it."
fi
[root@localhost sbin]# sh first.sh
please input you score:45
you fail.
[root@localhost sbin]# sh first.sh
please input you score:90
good!pass it.
```

### 带有elif

```bash
[root@localhost sbin]# cat first.sh
#! /bin/bash

read -p "please input you score:" a
if((a<60)); then
 echo "you fail."
elif ((a>=60)) && ((a<85)); then
 echo "good!pass it."
else
 echo "very good!is so high."
fi
```

判断数值大小除了可以用(())的形式外，还可以使用[]，但是不能使用>、<、=这样的符号了，要使用-lt(小于)、-gt(大于)、-le(小于或等于)、-ge(大于或等于)，-eq(等于)，-ne(不等于)。

### case逻辑判断

```bash
read -p "input a number:" n
a=$[$n%2]
case $a in
1)
 echo "the num is odd"
 ;;
0)
 echo "the num is even"
 ;;
*)
 echo "it's not a num"
 ;;
esac
[root@localhost sbin]# sh first.sh
input a number:20
the num is even
```

## shell脚本的循环

### for循环

"seq 1 5"表示从1到5的一个序列：
```bash
for i in `seq 1 5`; do
 echo $i
done
[root@localhost sbin]# sh first.sh
1
2
3
4
5
```

循环条件也可以是一组字符串或者数字：
```bash
[root@localhost sbin]# for i in 1 a b;do echo $i;done
1
a
b
```

## shell脚本中的函数

```bash
function sum(){
 sum=$[$1+$2]
 echo $sum
}
sum $1 $2
[root@localhost sbin]# sh first.sh 1 2
3
```
注意，在shell脚本中，函数一定要写在最前面，因为函数是要被调用的，如果还没有出现就被调用，肯定会出错的。

# linux系统日常管理

## 监控系统的状态

### w查看当前系统的负载

主要关注load average后的三个值，表示单位时间段内CPU的活动进程数，这个值越大就说明服务器压力越大，一般情况下，这个值只要不超过服务器的CPU数量就没有关系。
```bash
[jly@localhost ~]$ w
 21:40:29 up 2 min,  2 users,  load average: 1.68, 0.99, 0.39
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
jly      :0       :0               21:39   ?xdm?  24.29s  0.42s gdm-session-worker [pa
jly      pts/0    :0               21:39    5.00s  0.11s  0.04s w
```

查看服务器有几个CPU的方法：
```bash
[jly@localhost ~]$ cat /proc/cpuinfo
processor   : 0
vendor_id   : GenuineIntel
cpu family  : 6
model       : 61
model name  : Intel(R) Core(TM) i5-5200U CPU @ 2.20GHz
stepping    : 4
microcode   : 0x1f
cpu MHz     : 2200.448
cache size  : 3072 KB
physical id : 0
siblings    : 1
core id     : 0
cpu cores   : 1
apicid      : 0
initial apicid  : 0
fpu     : yes
fpu_exception   : yes
cpuid level : 20
wp      : yes
flags       : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts nopl xtopology tsc_reliable nonstop_tsc aperfmperf eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch ida arat epb pln pts dtherm fsgsbase tsc_adjust bmi1 avx2 smep bmi2 invpcid rdseed adx smap xsaveopt
bogomips    : 4402.00
clflush size    : 64
cache_alignment : 64
address sizes   : 42 bits physical, 48 bits virtual
power management:
```

### vmstat监控系统的状态

命令w查看系统整体上的负载，可以查看系统有没有压力，但无法判断具体是哪里（CPU、内存、磁盘等）有压力，所以就用vmstat。vmstat命令打印有以下6部分(列举常用的)：

* procs显示进程的相关信息
  * r：表示运行和等待CPU时间片的进程数，该数值如果长期大于服务器CPU的个数，则说明CPU不够用了。
  * b：表示等待资源的进程数，比如等待I/O、内存等。
* memory显示内存的相关信息
* swap显示内存的交换情况
  * si：表示由交换区写入到内存的数据量
  * so：表示由内存写入到交换区的数据量
* io显示磁盘的使用情况
  * bi：表示从块设备读取数据的量（读磁盘）
  * bo：表示从块设备写入数据的量（写磁盘）
* system显示采集间隔内发生的中段次数
* cpu显示CPU的使用状态
  * wa：表示I/O等待所占用CPU的时间百分比 

```bash
[jly@localhost ~]$ vmstat
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 3  0   3156  66000     36 436188    0    2   894    78  125  213  3  2 92  3  0
```

也可以使用命令`vmstat 1 5`或者`vmstat 1`，前一条命令表示每隔1秒输出一次状态，共输出5次；后一条命令表示每隔1秒输出一次状态且一直输出。

### top显示进程占有的系统资源

### free查看内存使用的状态

```bash
[jly@localhost ~]$ free
              total        used        free      shared  buff/cache   available
Mem:         999936      574212       90404        4844      335320      224880
Swap:       2097148        7244     2089904
```

### ps查看系统进程

## 网络相关

* ifconfig：查看网卡IP
* service network restart：重启网卡
* hostname：查看（修改）主机名
```bash
[jly@localhost ~]$ hostname
localhost.localdomain
[root@localhost jly]# hostname jiangluya   //修改主机名
[root@localhost jly]# hostname
jiangluya
```

下次登录时，命令提示符[root@localhost ~]中的localhost就会改为jiangluya。但是这样的修改只是保存在内存在，如果重启，主机名还会变成未改之前的名称。所以更改主机名的同时还需要更改相关配置文件/etc/sysconfig/network。
```bash
vim /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=jiangluya.localdomain
```

# 

