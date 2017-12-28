---
title: 学习linux之Docker
toc: true
date: 2017-12-07 14:43:19
tags: linux
categories: 工具
---

Docker是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的Linux机器上，也可以实现虚拟化，容器是完全使用沙箱机制，相互之间不会有任何接口。
<!--more-->

# ubuntu

## 安装

我这里学习的docker，是在ubuntu下使用的，对于ubuntu安装中的一些问题记录在这里。

[ubuntu镜像文件下载地址](https://www.ubuntu.com/download/desktop)

安装时，选择其他选项（即可以自己创建、调整分区）；继而出现安装类型，自己进行分区：

![分区1](http://oxv4lmd01.bkt.clouddn.com/ubuntu-install.png)

![分区2](http://oxv4lmd01.bkt.clouddn.com/ubuntu-install2.png)

![安装类型](http://oxv4lmd01.bkt.clouddn.com/ubuntu-install3.png)

在这个过程中，我发现自己的界面不能完全显示（有一部分挡在下面，怎么拉都看不到），解决办法很简单：按键`Alt+F7`就可以拖动看到下面的内容。

## 自适应客户机

操作方法见网址：[解决屏幕太小问题](https://jingyan.baidu.com/article/fc07f98977b60f12ffe5199b.html)

安装好后，点击最上方-- 查看 --> 立即适应客户机

# 安装和使用docker

安装之前，我们首先确保自己的linux系统内核版本高于3.10，并且系统是64位:
```bash
[root@izuf6alst6rugagpxl8wizz ~]# uname -ir
3.10.0-514.26.2.el7.x86_64 x86_64
```

在root下安装docker，使用命令(下面的运行结果是我第二次运行的结果，docker安装完成):
```bash
[root@izuf6alst6rugagpxl8wizz ~]# sudo yum -y install docker
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
Package 2:docker-1.12.6-68.gitec8512b.el7.centos.x86_64 already installed and latest version
Nothing to do
```

接下来，启动Docker后台服务：
```bash
[root@izuf6alst6rugagpxl8wizz ~]# sudo service docker start
Redirecting to /bin/systemctl start  docker.service
```

下一步，测试运行hello-world,如果本地没有hello-world这个镜像，就会下载一个hello-world的镜像，并在容器内运行：
```bash
[root@izuf6alst6rugagpxl8wizz ~]# sudo docker run hello-world
```

## 交互式容器

进入交互式容器，使用命令`docker run -i -t ubuntu /bin/bash`
```bash
# 启动容器
root@luyaj-virtual-machine:/home/luyaj# docker run -i -t ubuntu /bin/bash
root@1ed36419aeb9:/# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 01:23 ?        00:00:00 /bin/bash
root         9     1  0 01:25 ?        00:00:00 ps -ef
root@1ed36419aeb9:/# exit
exit
root@luyaj-virtual-machine:/home/luyaj# 
```

### 查看容器: 
`docker ps [-a][-l]`  && `docker inspect [id]`

```bash
# 显示进程
root@luyaj-virtual-machine:/home/luyaj# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
root@luyaj-virtual-machine:/home/luyaj# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                          PORTS               NAMES
1ed36419aeb9        ubuntu              "/bin/bash"         3 minutes ago       Exited (0) About a minute ago                       practical_swanson
b934d23e6789        ubuntu              "echo luya"         11 minutes ago      Exited (0) 11 minutes ago                           kind_hawking
9e7de9e1f31b        ubuntu              "echo hello"        13 minutes ago      Exited (0) 13 minutes ago                           peaceful_kowalevski

# 显示配置信息
root@luyaj-virtual-machine:/home/luyaj# docker inspect 9e7de9e1f31b 
[
    {
        "Id": "9e7de9e1f31b4d63573b5dd962c23914085566a3c24fc55550027dd459d82036",
        "Created": "2017-12-12T01:13:35.811132019Z",
        "Path": "echo",
        "Args": [
            "hello"
        ],
        "State": {
            "Status": "exited",
            "Running": false,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 0,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2017-12-12T01:13:38.847825304Z",
            "FinishedAt": "2017-12-12T01:13:38.893957073Z"
        }     
    }
]
```

### 自定义容器名

`docker run --name=自定义名 -i -t IMAGE /bin/bash`

```bash
# 自定义容器名
root@luyaj-virtual-machine:/home/luyaj# docker run --name=container2 -i -t ubuntu /bin/bash
root@9ec00bfc984d:/# exit
exit

# 查看当前容器进程
root@luyaj-virtual-machine:/home/luyaj# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
9ec00bfc984d        ubuntu              "/bin/bash"         25 seconds ago      Exited (0) 9 seconds ago                       container2
1ed36419aeb9        ubuntu              "/bin/bash"         10 hours ago        Exited (0) 10 hours ago                        practical_swanson
b934d23e6789        ubuntu              "echo luya"         10 hours ago        Exited (0) 10 hours ago                        kind_hawking
9e7de9e1f31b        ubuntu              "echo hello"        10 hours ago        Exited (0) 10 hours ago                        peaceful_kowalevski

# 显示配置信息
root@luyaj-virtual-machine:/home/luyaj# docker inspect container2
```

### 重新启动停止的容器

`docker start [-i] 容器名`

```bash
# 重新启动容器
root@luyaj-virtual-machine:/home/luyaj# docker start -i container2
root@9ec00bfc984d:/# exit
exit
root@luyaj-virtual-machine:/home/luyaj# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
9ec00bfc984d        ubuntu              "/bin/bash"         5 minutes ago       Exited (0) 10 seconds ago                       container2
1ed36419aeb9        ubuntu              "/bin/bash"         10 hours ago        Exited (0) 10 hours ago                         practical_swanson
b934d23e6789        ubuntu              "echo luya"         10 hours ago        Exited (0) 10 hours ago                         kind_hawking
9e7de9e1f31b        ubuntu              "echo hello"        10 hours ago        Exited (0) 10 hours ago                         peaceful_kowalevski
```

### 删除停止的容器

删除停止的容器：`docker rm 容器名`。（只能删除已经停止的容器，不能删除运行中的容器）

```bash
root@luyaj-virtual-machine:/home/luyaj# docker rm container2
container2
root@luyaj-virtual-machine:/home/luyaj# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                    PORTS               NAMES
1ed36419aeb9        ubuntu              "/bin/bash"         10 hours ago        Exited (0) 10 hours ago                       practical_swanson
b934d23e6789        ubuntu              "echo luya"         10 hours ago        Exited (0) 10 hours ago                       kind_hawking
9e7de9e1f31b        ubuntu              "echo hello"        10 hours ago        Exited (0) 10 hours ago                       peaceful_kowalevski
```

## 守护式容器

### 以守护形式运行容器

命令：`docker run -i -t IMAGE /bin/bash`，接下来使用`Ctrl+P, Ctrl+Q`。

```bash
root@luyaj-virtual-machine:/home/luyaj# docker run -i -t ubuntu /bin/bash

# Ctrl+P、Ctrl+Q（转到后台）, 换行
root@d7f3a54f0db4:/# root@luyaj-virtual-machine:/home/luyaj# 

# 可以查看到这几个容器当前还在运行
root@luyaj-virtual-machine:/home/luyaj# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
d7f3a54f0db4        ubuntu              "/bin/bash"         About a minute ago   Up About a minute                       affectionate_beaver
5e724ab47af5        ubuntu              "/bin/bash"         4 minutes ago        Up 4 minutes                            amazing_franklin
```

### 附加到运行中的容器

命令：`docker attach 容器名`.

```bash
root@luyaj-virtual-machine:/home/luyaj# docker attach d7f3a54f0db4 
root@d7f3a54f0db4:/# root@luyaj-virtual-machine:/home/luyaj# 

# 可以看到有两个容器在运行
root@luyaj-virtual-machine:/home/luyaj# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
d7f3a54f0db4        ubuntu              "/bin/bash"         6 minutes ago       Up 6 minutes                            affectionate_beaver
5e724ab47af5        ubuntu              "/bin/bash"         10 minutes ago      Up 10 minutes                           amazing_franklin

# 进入此命令
root@luyaj-virtual-machine:/home/luyaj# docker attach d7f3a54f0db4 

# 退出
root@d7f3a54f0db4:/# exit
exit

# 可以看到上面的容器停止了
root@luyaj-virtual-machine:/home/luyaj# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
5e724ab47af5        ubuntu              "/bin/bash"         11 minutes ago      Up 11 minutes                           amazing_franklin
```

### 停止守护式容器

发送一个信号，等待停止：`docker stop 容器名`
直接杀死：`docker kill 容器名`

# 在容器中使用Nginx部署静态网站

Nginx部署流程如下：

* 创建映射80端口的交互式容器
* 安装Nginx
* 安装文本编辑器vim
* 创建静态页面
* 修改Nginx配置文件
* 运行Nginx

```bash
# 创建带端口映射的交互容器，命名为"web"，使用ubuntu为系统。
root@luyaj-virtual-machine:/home/luyaj# docker run -p 80 --name web1 -i -t ubuntu  /bin/bash

# 更新包
root@416f6aee01e7:/# apt-get update

# 安装Nginx
root@416f6aee01e7:/# apt install -y nginx

# 安装vim
root@416f6aee01e7:/# apt intall -y vim

# 创建静态页面（在index.html中填写前端代码）
root@fa38c10136a0:/# mkdir -p /var/www/html
root@fa38c10136a0:/# cd /var/www/html
root@fa38c10136a0:/var/www/html# vim index.html

# 使用whereis查看nginx在哪
root@f3cfa277a97e:/var/www/html# whereis nginx
nginx: /usr/sbin/nginx /etc/nginx /usr/share/nginx

# 打开文件default
root@f3cfa277a97e:/var/www/html# ls /etc/nginx
conf.d        fastcgi_params  koi-win     nginx.conf    scgi_params      sites-enabled  uwsgi_params
fastcgi.conf  koi-utf         mime.types  proxy_params  sites-available  snippets       win-utf
root@f3cfa277a97e:/var/www/html# ls /etc/nginx/sites-enabled 
default
root@f3cfa277a97e:/var/www/html# vim /etc/nginx/sites-enabled/default
```

在default文件，将root值修改为我们创建的静态网站的位置（/var/www/html）,操作如图:

![修改root](http://oxv4lmd01.bkt.clouddn.com/docker_default_root.png)

```bash
# 切换到根目录，运行nginx，使用ps命令在容器查中看当前的进程
root@f3cfa277a97e:/var/www/html# cd /
root@f3cfa277a97e:/# nginx
root@f3cfa277a97e:/# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 12:53 ?        00:00:00 bash
root        18     1  0 13:08 ?        00:00:00 nginx: master process nginx
www-data    19    18  0 13:08 ?        00:00:00 nginx: worker process
root        20     1  0 13:09 ?        00:00:00 ps -ef

# Ctrl+P,Ctrl+Q. “docker ps”或“docker port 容器名”可以看到容器映射的端口
root@f3cfa277a97e:/# [root@izuf6alst6rugagpxl8wizz ~]# 
[root@izuf6alst6rugagpxl8wizz ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                   NAMES
0e10487869ea        ubuntu              "/bin/bash"         30 minutes ago      Up 30 minutes                               silly_bose
f3cfa277a97e        ubuntu              "bash"              35 hours ago        Up 17 minutes       0.0.0.0:32769->80/tcp   web1
8f8c5c20e073        ubuntu              "bash"              35 hours ago        Up 35 hours                                 nostalgic_davinci
[root@izuf6alst6rugagpxl8wizz ~]# docker port web1
80/tcp -> 0.0.0.0:32769

# 运行
[root@izuf6alst6rugagpxl8wizz ~]# curl http://127.0.0.1:32769
<html>
<head>
    <title>nginx in docker</title>
</head>
<body>
    <h1>hello, first website in docker!</h1>
</body>
</html>
```

可以看到，出现了我们编辑的页面。我们也可以在浏览器中访问页面。由于我这里是在云服务器上开启的容器，所以使用云服务器的ip地址+端口号，即可成功访问页面。

上面我们是用宿主机的ip地址来查看的网页，这里我们还可以用容器的ip地址查看页面：
```bash
[root@izuf6alst6rugagpxl8wizz ~]# docker inspect web1
```
运行结果如下，找到ip地址（不需要指定端口号，默认为80端口）：

![代码](http://oxv4lmd01.bkt.clouddn.com/docker_curl.png)

到这里我们就完成了一个静态页面！下面来看看怎么运行nginx。

```bash
# 停用容器
[root@izuf6alst6rugagpxl8wizz ~]# docker stop web1
web1

# 开启容器
[root@izuf6alst6rugagpxl8wizz ~]# docker start -i web1

# 可以看到nginx没有开启
root@f3cfa277a97e:/# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 14:19 ?        00:00:00 bash
root         9     1  0 14:20 ?        00:00:00 ps -ef

# exec命令启动nginx，查看
root@f3cfa277a97e:/# [root@izuf6alst6rugagpxl8wizz ~]# 
[root@izuf6alst6rugagpxl8wizz ~]# docker exec web1 nginx
[root@izuf6alst6rugagpxl8wizz ~]# docker top web1
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                12250               12227               0                   22:19               pts/2               00:00:00            bash
root                12370               1                   0                   22:20               ?                   00:00:00            nginx: master process nginx
33                  12371               12370               0                   22:20               ?                   00:00:00            nginx: worker process

# 运行网站（会发现有问题），使用inspect查看容器，重新输入地址，就成功了。
[root@izuf6alst6rugagpxl8wizz ~]# http://172.17.0.3
-bash: http://172.17.0.3: No such file or directory
[root@izuf6alst6rugagpxl8wizz ~]# docker inspect
/usr/bin/docker-current: "inspect" requires a minimum of 1 argument.
See '/usr/bin/docker-current inspect --help'.

Usage:  docker inspect [OPTIONS] CONTAINER|IMAGE|TASK [CONTAINER|IMAGE|TASK...]

Return low-level information on a container, image or task
[root@izuf6alst6rugagpxl8wizz ~]# docker inspect web1
```

友情链接：[docker上手并部署Ngnix](https://segmentfault.com/a/1190000012161773)

# 构建镜像

