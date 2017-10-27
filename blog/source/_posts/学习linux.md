---
title: 学习linux
toc: true
date: 2017-10-15 20:02:49
tags: linux
categories: 工具
---

简介：使用虚拟机安装linux以及linux学习。

<!--more-->

* VMware Workstation Pro：[下载地址](https://www.vmware.com/cn/products/workstation-pro.html)
* CentOS：[下载地址](http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1708.iso)
* 书籍推荐：《跟阿铭学Linux》2014年10月第1版
* 实验楼：[学习地址](https://www.shiyanlou.com/courses/1)

# 虚拟机安装Linux

《跟阿铭学Linux》这里面也有详细的安装过程，这里我就记录两点：

1. ![图形界面](http://oxv4lmd01.bkt.clouddn.com/%E9%80%89%E6%8B%A9gui.png)

习惯于用图形界面的同学一定要记得选择这项，不然之后安装有点麻烦。

2. 刚点击`install CentOS 7` ，就发现界面黑屏，怎么弄都没有反应，这个时候就要注意，是不是电脑禁止了虚拟机功能。解决办法：重启电脑进入BIOS，找到`virtualization`的一项,我的BIOS中在`Advanced-cpu setup-virtualization`，现为`Disabled`状态，改为`Enable`，再开机安装这个问题应该解决了！

其他的可以参考这篇文章，讲的很详细。（[CentOs安装教程等](https://zm10.sm-tc.cn/?src=l4uLj8XQ0IiIiNGcl5qRmIeKhoqekYzRnJCS0KqRlofQx8zLysvRl4uSkw%3D%3D&uid=ce6674418dc941a250cb1bbb92abaff3&hid=2e837fc7c03ffc154a4bd83c8d2925af&pos=1&cid=9&time=1507991385827&from=click&restype=1&pagetype=0000004000000402&bu=structure_web_info&query=centos7%E5%88%86%E5%8C%BA%E6%96%B9%E6%A1%88&mode=&v=1&uc_param_str=dnntnwvepffrgibijbprsvdsdichei)）

# 基础

## 小笔记

* 切换用户：`su 用户名` (`su root`)
* 新建目录：`mkdir 目录名`
* 新建文件：`touch 文件名`
* 切换路径：`cd 路径地址`
* 关机命令：`init 0`

## 重要快捷键

* tab键:写命令时，命令不记得，那就用Tab补全
* Ctrl+c:强行终止当前程序 

| 按键 | 作用 |
| --- | ---- |
| Ctrl+d | 键盘输入结束或退出终端 |
| Ctrl+s | 暂停当前程序，暂停后按下任意键恢复运行 |
| Ctrl+z | 将当前程序放到后台运行，恢复到前台为命令fg |
| Ctrl+a | 将光标移至输入行头，相当于Home键 |
| Ctrl+e | 将光标移至输入行末，相当于End键 |
| Ctrl+k | 删除从光标所在位置到行末 |
| Alt+Backspace | 向前删除一个单词 |
| Shift+PgUp | 将终端显示向上滚动 |
| Shift+PgDn | 将终端显示向下滚动 |

* 方向键↑:恢复之前输入过的命令

## Shell常用通配符

| 字符 | 含义 |
| ---- | --- |
| * | 匹配0或多个字符 |
| ? | 匹配任意一个字符 |
| [list] | 匹配list中的任意单一字符 |
| [!list] | 匹配除list中的任意单一字符以外的字符 |
| [c1-c2] | 匹配c1-c2中的任意单一字符,如：[0-9] [a-z] |
| {string1,string2,...} | 匹配string1或string2(或更多)其一字符串 |
| {c1...c2} | 匹配c1-c2中全部字符，如{1..10} |

一次性创建多个文件：
```bash
[luyaj@localhost test]$ touch hello{1..3}.html
[luyaj@localhost test]$ ls *.html
hello1.html  hello2.html  hello3.html
```

## 在命令行中获取帮助

你可以使用如下方式来获得某个命令的说明和使用方式的详细介绍:
```bash
$ man <command_name>
```

由于`man`的命令很多，所以手册进行了分区段处理，通常被分为以下8个区段：

| 区段 | 说明 |
| --- | ---- |
| 1 | 一般命令 |
| 2 | 系统调用 |
| 3 | 库函数，涵盖了C标准函数库 |
| 4 | 特殊文件（通常是/dev中的设备）和驱动程序 |
| 5 | 文件格式和约定 |
| 6 | 游戏和屏保 |
| 7 | 杂项 |
| 8 | 系统管理命令和守护进程 |

例如：
```bash
$ man 1 ls
```

如果你知道某个命令的作用，只是想快速查看一些它的某个具体参数的作用，那么你可以使用--help参数，大部分命令都会带有这个参数，如：
```bash
$ ls --help
```

# Linux文件和目录管理

## 跟文件和目录有关的命令

### 命令cd

命令cd是用来变更用户所在目录的，后面只能跟目录名，跟了文件名会报错；命令pwd是用来打印当前所在目录：
```bash
[luyaj@localhost ~]$ cd home
bash: cd: home: 没有那个文件或目录
[luyaj@localhost ~]$ cd /home
[luyaj@localhost home]$ pwd
/home
```

在linux文件系统中，有两个特殊的符号也可以表示目录。"."表示当前目录，".."表示当前目录的上一级目录：
```bash
[luyaj@localhost test]$ cd .
[luyaj@localhost test]$ pwd
/home/luyaj/test
[luyaj@localhost test]$ cd ..
[luyaj@localhost ~]$ pwd
/home/luyaj
```

### 命令mkdir

`mkdir`是*make directory*的缩写。命令格式是：`mkdir [-mp][目录名称]`。-m选项用于指定要创建目录的权限（不常用）；-p选项很有用处，我们先来看看：

```bash
[luyaj@localhost ~]$ mkdir luya/test/123
mkdir: 无法创建目录"luya/test/123": 没有那个文件或目录
[luyaj@localhost ~]$ mkdir -p luya/test/123
[luyaj@localhost ~]$ ls luya/test
123
```

我们发现当我们想创建目录`luya/test/123`时，提示无法创建、`/luya/test`文件不存在。这是因为在linux中，如果发现要创建的目录的上一级目录不存在就会报错。所以-p这个选项就是帮我们创建一大串级联目录，并且当创建一个已经存在的目录时不会报错。

相应的，`rmdir`用于删除空目录。

### 命令rm

`rm`命令既可以删除目录，也可以删除文件。一般使用命令`rm -rf 目录名`来删除。

### 命令cp

`cp`是copy的简写，命令格式：`cp [选项][来源文件][目的文件]`。

* -r :如果要复制一个目录，必须加这个选项，否则不能复制目录，类似于rm命令。 

```bash
[luyaj@localhost ~]$ mkdir 123
[luyaj@localhost ~]$ cp 123 456
cp: 略过目录"123"
[luyaj@localhost ~]$ cp -r 123 456
[luyaj@localhost ~]$ ls -l
总用量 0
drwxrwxr-x. 2 luyaj luyaj  6 10月 16 19:42 123
drwxrwxr-x. 2 luyaj luyaj  6 10月 16 19:42 456
```

* -i :如果遇到一个已经存在的文件，会询问是否覆盖。

```bash
[luyaj@localhost ~]$ cd 123
[luyaj@localhost 123]$ ls
[luyaj@localhost 123]$ touch 111
[luyaj@localhost 123]$ touch 222
[luyaj@localhost 123]$ cp -i 111 222
cp：是否覆盖"222"？ n
[luyaj@localhost 123]$ echo 'acb' > 111
[luyaj@localhost 123]$ echo 'def' > 222
[luyaj@localhost 123]$ cat 111 222
acb
def
[luyaj@localhost 123]$ /bin/cp 111 222
[luyaj@localhost 123]$ cat 111
acb
```

上例中，`echo`用于打印，将'abc'和'def'分别写入文件"111"和"222"中，起写入符号的就是符号">"(在linux中叫做重定向)。`cat`命令用于读一个文件，并把读出的文件打印在当前屏幕上。

### 命令mv

`mv`是move的简写，命令格式为：`mv [选项][源文件或目录][目标文件或目录]`。

有几种情况，先看示例：

```bash
[luyaj@localhost ~]$ mkdir dira  dirb
[luyaj@localhost ~]$ ls
dira  公共  视频  文档  音乐
dirb  模板  图片  下载  桌面
[luyaj@localhost ~]$ mv dira dirc
[luyaj@localhost ~]$ ls
dirb  公共  视频  文档  音乐
dirc  模板  图片  下载  桌面
```

这里，目标文件是目录dirc，并且dirc不存在，所以相当于把目录dira重命名为dirc。

```bash
[luyaj@localhost ~]$ mv dirc dirb
[luyaj@localhost ~]$ ls
dirb  公共  模板  视频  图片  文档  下载  音乐  桌面
[luyaj@localhost ~]$ ls dirb
dirc
```

这里，目标文件是目录dirb，并且dirb存在，所以会把目录dirc移动到目录dirb里。

同理，文件的移动也是和上面目录的移动是一样的。

## 几个与文档相关的命令

### 命令cat/tac

`cat`命令用来查看一个文件的内容并显示在屏幕上；而`tac`命令是反序输出。

* -n ：查看文件时，把行号也显示在屏幕上
* -A ：显示所有内容，包括特殊字符

```bash
[luyaj@localhost ~]$ mkdir dirb
[luyaj@localhost ~]$ cd dirb
[luyaj@localhost dirb]$ touch filee
[luyaj@localhost dirb]$ echo '111111' > filee
[luyaj@localhost dirb]$ echo '222222' >>filee
[luyaj@localhost dirb]$ cd
[luyaj@localhost ~]$ cat dirb/filee
111111
222222
[luyaj@localhost ~]$ cat -n dirb/filee
     1  111111
     2  222222
[luyaj@localhost ~]$ cat -A dirb/filee
111111$
222222$
```

这里'>>'也是重定向，是**追加**的意思。只是使用'>'符号时，如果文件中有内容会删除文件中原来的内容，而使用'>>'符号不会删除原有内容。

### 命令more/less/head/tail

* 当文件内容太多时，一屏不能全部显示时，用`cat`肯定是看不了前面的内容，而使用`more`可以解决这个问题。当看完一屏后，按空格键可以继续看下一屏。如果想提前退出，按"q"键即可。
* `less`和more命令一样，但是命令less可以实现上翻和下翻。
* 命令`head`用于显示文件的前10行，如果加-n选项则显示文件的前几行。
* 命令`tail`用于显示文件的最后10行，如果加-n选项则显示文件的后几行。

## linux文件属性

```bash
[luyaj@localhost ~]$ ls -l
总用量 0
drwxrwxr-x. 2 luyaj luyaj 19 10月 16 22:16 dirb
drwxr-xr-x. 2 luyaj luyaj  6 10月 16 01:43 公共
```

使用命令`ls -l`共显示了9列内容，那么他们都代表什么含义呢？
* 第1列：包含该文件的类型、所属主、所属组以及其他用户对该文件的权限。第1位用来描述该文件的类型。
 * d表示该文件为目录
 * -表示该文件为普通文件
 * l表示该文件为链接文件
 * b表示该文件为块设备，比如/dev/sda就是这样的文件
 * c表示该文件为串行端口设备文件（又称字符设备文件），比如键盘、鼠标、打印机、tty终端等
 * s表示该文件为套接字文件，用于进程之间的通信
 * 文件类型后面9位，每3个一组，前3位为所属主（user）的权限，中间3位为所属组（group）的权限，最后3位为其他非本群组用户（others）的权限。"r"表示可读，"w"表示可写，"x"表示可执行。
* 第2列：表示链接占用的节点（inode），如果是目录，那么与该目录下是子目录数量有关
* 第3列：表示文件的所属主
* 第4列：表示文件的所属组
* 第5列：表示该文件的大小
* 第6列、第7列和第8列：表示文件最后一次被修改的时间，依次为月份、日期、时间
* 第9列：表示文件名

## 更改文件的权限

### 命令chgrp

更改文件和目录的所属组，命令格式：`chgrp [组名] [文件名]`

```bash
[root@localhost ~]# groupadd testgroup
[root@localhost ~]# touch test1
[root@localhost ~]# ls -l test1
-rw-r--r--. 1 root root 0 10月 16 23:17 test1
[root@localhost ~]# chgrp testgroup test1
[root@localhost ~]# ls -l test1
-rw-r--r--. 1 root testgroup 0 10月 16 23:17 test1
```

`groupadd`命令的含义为增加一个用户组。

### 命令chown

更改文件和目录的所属主，命令格式：`chown [-R] 账户名 文件名` 或者 `chown [-R] 账户名：组名 文件名`。这里的-R选项只适用于目录，作用是级联更改，即不仅更改当前目录，连目录里的目录或者文件也全部更改。

### 命令chmod

`chmod`命令用于改变用户对文件的读写执行权限，其格式为：“chmod [-R] xyz 文件名”。其中-R表示级联更改；xyz表示数字，即在linux中为了方便更改文件的权限，使用数字代替“rwx”，规则为：r等于4，w等于2，x等于1，-等于0。例如，“-rwxrwx---”用数字表示就是770。

```bash
[luyaj@localhost ~]$ ls -ld test
drwxrwxr-x. 2 luyaj luyaj 19 10月 17 03:25 test
[luyaj@localhost ~]$ chmod 750 test
[luyaj@localhost ~]$ ls -ld test
drwxr-x---. 2 luyaj luyaj 19 10月 17 03:25 test
```

在linux系统中，一个目录的默认权限为755，而一个文件的默认权限为644。

## 在linux下搜索文件

### 命令which

用which命令查找PATH环境变量中的可执行文件的绝对路径。

```bash
[luyaj@localhost ~]$ which vi
alias vi='vim'
    /usr/bin/vim
```

### 命令whereis

which命令是通过预先生成的一个文件列表库去查找与给出的文件名相关的文件，格式：`whereis [-bmsu] [文件名称]`。

```bash
[luyaj@localhost ~]$ whereis ls
ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz /usr/share/man/man1p/ls.1p.gz
```

这里，我们有3个文件被找到了，类似于模糊查询。

### 命令find

使用find命令搜索文件，命令格式：`find [路径] [参数]`。
* -atime +n/-n:表示访问或执行时间大于或小于n天的文件。
* ctime +n/-n:表示写入、更改inode属性（如更改所有者、权限或者链接）时间大于或小于n天的文件。
* -mtime +n/-n:表示写入时间大于或小于n天的文件。
* -name filename:表示直接查找该文件名的文件。
```bash
[root@localhost luyaj]# mkdir test
[root@localhost luyaj]# mkdir test/test2
[root@localhost luyaj]# find . -name test2
./test/test2
[root@localhost luyaj]# find -name test2
./test/test2
```

# linux系统用户与用户组管理

## 用户和用户组管理

### 新增组的命令groupadd

命令格式：`groupadd [-g GID] groupname`。

```bash
[root@localhost luyaj]# groupadd grptest1
[root@localhost luyaj]# tail -n1 /etc/group
grptest1:x:1003:
```

如果不加“-g”选项则按照系统默认的gid创建组。跟uid一样，gid也是从500开始，我们也可以自定义gid：

```bash
[root@localhost luyaj]# groupadd -g 511 grptest2
[root@localhost luyaj]# tail -n2 /etc/group
grptest1:x:1003:
grptest2:x:511:
```

### 删除组的命令groupdel

```bash
[root@localhost luyaj]# groupdel grptest2
[root@localhost luyaj]# tail -n3 /etc/group
luyaj:x:1000:
user1:x:1002:
grptest1:x:1003:
```

groupdel命令无特殊选项，但有一种情况不能删除组：

```bash
[root@localhost luyaj]# groupdel user1
groupdel：不能移除用户“user1”的主组
```
这个例子中，user1组中包含user1账户，只有先删除账户才可以删除该组。

### 增加用户的命令useradd

命令格式：`useradd [-u UID][-g GID][-d HOME][-M][-s]`，各个选项的含义如下：
* -u：表示自定义UID。
* -g：表示使新增用户属于已经存在的某个组，后面可以跟组id，也可以跟组名。
* -d：表示自定义用户的家目录。
* -M：表示不建立家目录。
* -s：表示自定义shell。

下面我们新建一个用户test10，示例如下：
```bash
[root@localhost luyaj]# useradd test10
[root@localhost luyaj]# tail -n1 /etc/passwd
test10:x:1002:1004::/home/test10:/bin/bash
[root@localhost luyaj]# tail -n1 /etc/group
test10:x:1004:
```
如果useradd不加任何选项，直接跟用户名，那么会创建一个跟用户名同名的组。有的时候需要我们自己去定义：

```bash
[root@localhost luyaj]# useradd -u510 -g 513 -M -s /sbin/nologin user11
useradd：“513”组不存在
```

### 删除账户的命令userdel

命令格式：`userdel [-r] username`，其中-r选项的作用是，当删除用户时一并删除该用户的家目录。

```bash
[root@localhost luyaj]# userdel user1
[root@localhost luyaj]# tail -n3 /etc/passwd
tcpdump:x:72:72::/:/sbin/nologin
luyaj:x:1000:1000:luyaJ:/home/luyaj:/bin/bash
test10:x:1002:1004::/home/test10:/bin/bash
[root@localhost luyaj]# ls -ld /home/user1
drwx------. 3 1001 1002 78 10月 16 23:35 /home/user1
```
这里可以看出，未加选项时，只删除了用户，没有家目录还在。

## 用户密码管理

### 命令passwd

为用户设置密码可以使用命令passwd，命令格式：`passwd [username]`。

```bash
[root@localhost luyaj]# passwd
更改用户 root 的密码 。
新的 密码：
重新输入新的 密码：
passwd：所有的身份验证令牌已经成功更新。
```

如果你登陆的是root账号，后面可以跟普通用户的名字，意思是修改指定账户的密码：
```bash
[root@localhost luyaj]# passwd test10
更改用户 test10 的密码 。
新的 密码：
无效的密码： 密码未通过字典检查 - 它基于字典单词
重新输入新的 密码：
passwd：所有的身份验证令牌已经成功更新。
```

需要注意的是，只有root才能修改其他账户的密码，普通账户只能修改自己的密码。

### 命令mkpasswd

用于生成密码。

# linux磁盘管理

## 查看磁盘或者目录的容量

### 命令df

命令df用来查看已挂载磁盘的总容量、使用容量、剩余容量，可以不加任何参数，默认以KB为单位显示：

```bash
[luyaj@localhost ~]$ df
文件系统               1K-块    已用     可用 已用% 挂载点
/dev/mapper/cl-root 17811456 3454476 14356980   20% /
devtmpfs              484108       0   484108    0% /dev
tmpfs                 499968     156   499812    1% /dev/shm
tmpfs                 499968    7188   492780    2% /run
tmpfs                 499968       0   499968    0% /sys/fs/cgroup
/dev/sda1            1038336  176564   861772   18% /boot
tmpfs                  99996      24    99972    1% /run/user/1000
/dev/sr0             4276440 4276440        0  100% /run/media/luyaj/CentOS 7 x86_64
```

df命令的常用选项有-i、-h、-k和-m：
* -i：表示查看inodes的使用状况：
```bash
[luyaj@localhost ~]$ df -i
文件系统              Inode 已用(I) 可用(I) 已用(I)% 挂载点
/dev/mapper/cl-root 8910848  119944 8790904       2% /
devtmpfs             121027     391  120636       1% /dev
tmpfs                124992      10  124982       1% /dev/shm
tmpfs                124992     546  124446       1% /run
tmpfs                124992      16  124976       1% /sys/fs/cgroup
/dev/sda1            524288     330  523958       1% /boot
tmpfs                124992      30  124962       1% /run/user/1000
/dev/sr0                  0       0       0        - /run/media/luyaj/CentOS 7 x86_64
```

* -h：表示使用合适的单位显示，例如G：
```bash
[luyaj@localhost ~]$ df -h
文件系统             容量  已用  可用 已用% 挂载点
/dev/mapper/cl-root   17G  3.3G   14G   20% /
devtmpfs             473M     0  473M    0% /dev
tmpfs                489M  156K  489M    1% /dev/shm
tmpfs                489M  7.1M  482M    2% /run
tmpfs                489M     0  489M    0% /sys/fs/cgroup
/dev/sda1           1014M  173M  842M   18% /boot
tmpfs                 98M   24K   98M    1% /run/user/1000
/dev/sr0             4.1G  4.1G     0  100% /run/media/luyaj/CentOS 7 x86_64
```

* -k、-m：分别以"K"和"M"为单位显示：
```bash
[luyaj@localhost ~]$ df -k
文件系统               1K-块    已用     可用 已用% 挂载点
/dev/mapper/cl-root 17811456 3454460 14356996   20% /
devtmpfs              484108       0   484108    0% /dev
tmpfs                 499968     156   499812    1% /dev/shm
tmpfs                 499968    7188   492780    2% /run
tmpfs                 499968       0   499968    0% /sys/fs/cgroup
/dev/sda1            1038336  176564   861772   18% /boot
tmpfs                  99996      24    99972    1% /run/user/1000
/dev/sr0             4276440 4276440        0  100% /run/media/luyaj/CentOS 7 x86_64
[luyaj@localhost ~]$ df -m
文件系统            1M-块  已用  可用 已用% 挂载点
/dev/mapper/cl-root 17394  3374 14021   20% /
devtmpfs              473     0   473    0% /dev
tmpfs                 489     1   489    1% /dev/shm
tmpfs                 489     8   482    2% /run
tmpfs                 489     0   489    0% /sys/fs/cgroup
/dev/sda1            1014   173   842   18% /boot
tmpfs                  98     1    98    1% /run/user/1000
/dev/sr0             4177  4177     0  100% /run/media/luyaj/CentOS 7 x86_64
```

/dev/shm为内存挂载点，如果你想把文件放到内存里，就可以放到这个目录下。

### 命令du

用来查看某个目录或文件所占空间的大小，命令格式：`du [-abckmsh][文件或者目录名]`。
* -a：表示全部文件和目录的大小都列出来。如果不指定单位，默认显示单位为"KB"。
* -b：表示列出的值以"bytes"为单位输出。
* -k：表示以"KB"为单位输出，这和不加选项是一样的。
* -m：表示以"MB"为单位输出。
* -h：表示系统自动调节单位。
* -c：表示最后加总。
* -s：表示只列出总和。
```bash
[luyaj@localhost ~]$ du test
0   test/test2
0   test/test3
0   test
[luyaj@localhost ~]$ du -a test
0   test/test2
0   test/test3/1.txt
0   test/test3
0   test
[luyaj@localhost ~]$ du -b /etc/passwd
2233    /etc/passwd
[luyaj@localhost ~]$ du -k /etc/passwd
4   /etc/passwd
[luyaj@localhost ~]$ du -m /etc/passwd
1   /etc/passwd
[luyaj@localhost ~]$ du -h /etc/passwd
4.0K    /etc/passwd
[luyaj@localhost ~]$ du -c test
0   test/test2
0   test/test3
0   test
0   总用量
[luyaj@localhost ~]$ du -s test
0   test
```

## 磁盘的分区和格式化

用文件来表示硬件分区——`/dev/sda5`解释一下：
* dev是存放所有硬件设备文件的目录
* sd是硬件设备的代号，hd代表IDE设备，sd代表SCSI和sata设备
* a是同类型设备的编号，a代表第一个硬盘，b代表第二个硬盘，以此类推
* 5是分区号：1~4表示主分区或拓展分区，5表示逻辑分区
举几个例子：
* /dev/sdb3：计算机中第二块SATA硬盘的第三个主分区。
* /dev/sda8：计算机中第一块SATA硬盘的第四个逻辑分区。
* /dev/hda1：计算机中第四块IDE硬盘的第一个主分区

### 增加虚拟磁盘

在系统关机的状态下，选择你的虚拟机，右键**设置**，然后**添加**硬盘，创建**新虚拟磁盘**，将虚拟磁盘拆分成多个文件，完成。

### 命令fdisk

这里我已经创建好了新的磁盘，所以执行命令`fdisk -l`会发现新增了一个磁盘"/dev/sdb"。

如果只执行命令`fdisk /dev/sda`就会进入另一种模式，在这个模式下，可以对磁盘进行分区操作。

```bash
[root@localhost luyaj]# fdisk /dev/sdb
欢迎使用 fdisk (util-linux 2.23.2)。

更改将停留在内存中，直到您决定将更改写入磁盘。
使用写入命令前请三思。

Device does not contain a recognized partition table
使用磁盘标识符 0xad054854 创建新的 DOS 磁盘标签。

命令(输入 m 获取帮助)：m
命令操作
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   g   create a new empty GPT partition table
   G   create an IRIX (SGI) partition table
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition’s system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)
```

输入p命令，我们可以看到当前无分区：
```bash
命令(输入 m 获取帮助)：p

磁盘 /dev/sdb：8589 MB, 8589934592 字节，16777216 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0xad054854

   设备 Boot      Start         End      Blocks   Id  System
```

那么我们开始创建分区：
```bash
命令(输入 m 获取帮助)：n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
分区号 (1-4，默认 1)：1
起始 扇区 (2048-16777215，默认为 2048)：2048
Last 扇区, +扇区 or +size{K,M,G} (2048-16777215，默认为 16777215)：+1000M
分区 1 已设置为 Linux 类型，大小设为 1000 MiB
```
使用n命令新建分区，选择p（主分区），分区号，起始扇区都可以默认不填（或者你也可以填），
Last扇区填“+1000M”，这样方便又不容易出错。一直创建主分区到4，我们可以看到：

```bash
命令(输入 m 获取帮助)：n
Partition type:
   p   primary (3 primary, 0 extended, 1 free)
   e   extended
Select (default e): 
Using default response e
已选择分区 4
起始 扇区 (6146048-16777215，默认为 6146048)：
将使用默认值 6146048
Last 扇区, +扇区 or +size{K,M,G} (6146048-16777215，默认为 16777215)：+1000M
分区 4 已设置为 Extended 类型，大小设为 1000 MiB
```

上面就是，创建完第3个分区之后，创建第4个分区时选择拓展分区，输入命令p查看：
```bash
命令(输入 m 获取帮助)：p

磁盘 /dev/sdb：8589 MB, 8589934592 字节，16777216 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0xbfa73b9f

   设备 Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048     2050047     1024000   83  Linux
/dev/sdb2         2050048     4098047     1024000   83  Linux
/dev/sdb3         4098048     6146047     1024000   83  Linux
/dev/sdb4         6146048     8194047     1024000    5  Extended
```

接下来我们继续创建分区：
```bash
命令(输入 m 获取帮助)：n
All primary partitions are in use
添加逻辑分区 5
起始 扇区 (6148096-8194047，默认为 6148096)：
将使用默认值 6148096
Last 扇区, +扇区 or +size{K,M,G} (6148096-8194047，默认为 8194047)：
将使用默认值 8194047
分区 5 已设置为 Linux 类型，大小设为 999 MiB

命令(输入 m 获取帮助)：p

磁盘 /dev/sdb：8589 MB, 8589934592 字节，16777216 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0xbfa73b9f

   设备 Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048     2050047     1024000   83  Linux
/dev/sdb2         2050048     4098047     1024000   83  Linux
/dev/sdb3         4098048     6146047     1024000   83  Linux
/dev/sdb4         6146048     8194047     1024000    5  Extended
/dev/sdb5         6148096     8194047     1022976   83  Linux
```
此时再分区就跟之前的不一样了，只需要直接定义分区大小就可以了。需要注意的是，当创建完前3个主分区后，要把剩余的磁盘空间全部划分给第4个拓展分区，不然剩下的空间就浪费了。因为创建完拓展分区后，再划分新的分区时是在已经划分的拓展分区里来分的。

**注意**：“/dev/sdb4”为拓展分区，这个分区是不可以格式化的。“/dev/sdb5”是“/dev/sdb4”的子分区，称为逻辑分区。删除分区使用命令d，在弄好的分区界面直接按“Ctrl+c”键可以直接退出，这样刚做的分区便全部取消。


