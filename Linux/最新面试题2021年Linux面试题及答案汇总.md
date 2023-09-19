# 最新面试题2021年Linux面试题及答案汇总

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Linux

### 题1：[bash shell 中 hash 命令有什么作用？](/docs/Linux/最新面试题2021年Linux面试题及答案汇总.md#题1bash-shell-中-hash-命令有什么作用)<br/>
linux中hash命令管理着一个内置的哈希表，记录了已执行过的命令的完整路径，用该命令可以打印出你所使用过的命令以及执行的次数。

```shell
[root@mrwang jingxuan]# hash
hits    command
   1    /usr/bin/tail
   5    /usr/bin/df
   1    /usr/sbin/ifconfig
   5    /usr/bin/du
   1    /usr/bin/netstat
   2    /usr/bin/ls
```


### 题2：[Linux 中 du 和 df 命令有什么区别？](/docs/Linux/最新面试题2021年Linux面试题及答案汇总.md#题2linux-中-du-和-df-命令有什么区别)<br/>
du显示目录或文件的大小。

df显示每个<文件>所在的文件系统的信息，默认是显示所有文件系统。

文件系统分配其中的一些磁盘块用来记录它自身的一些数据，如i节点，磁盘分布图，间接块，超级块等。这些数据对大多数用户级的程序来说是不可见的，通常称为Meta Data。

du命令是用户级的程序，它不考虑Meta Data，而df命令则查看文件系统的磁盘分配图并考虑Meta Data。

df命令获得真正的文件系统数据，而du命令只查看文件系统的部分情况。


### 题3：[Linux 中如何修改文件权限？](/docs/Linux/最新面试题2021年Linux面试题及答案汇总.md#题3linux-中如何修改文件权限)<br/>
**语法**

```shell
chmod [-cfvR] [--help] [--version] mode file...
```

**参数说明**

mode : 权限设定字串，格式如下 :

[ugoa...][[+-=][rwxX]...][,...]

其中：

u表示该文件的拥有者，g表示与该文件的拥有者属于同一个群体(group)者，o表示其他以外的用户，a表示这三者皆是。

\+ 表示增加权限、-表示取消权限、=表示唯一设定权限。

r表示可读取，w表示可写入，x表示可执行，X表示只有当该文件是个子目录或者该文件已经被设定过为可执行。

**其他参数说明**

-c若该文件权限确实已经更改，才显示其更改动作

-f若该文件权限无法被更改也不要显示错误讯息

-v显示权限变更的详细资料

-R对目前目录下的所有文件与子目录进行相同的权限变更，即以递回的方式逐个变更。

--help显示辅助说明

--version显示版本

**chmod命令使用数字方式修改文件权限**

文件的基本权限由9个字符组成，以rwxrw-r-x为例，可以使用数字来代表各个权限，各个权限与数字的对应关系如下：
>r --> 4
w --> 2
x --> 1

这9个字符分属3类用户，每种用户身份包含3个权限（r、w、x），通过将3个权限对应的数字累加，最终得到的值即可作为每种用户所具有的权限。

比如rwxrw-r-x，所有者、所属组和其他人分别对应的权限值为：
>所有者 = rwx = 4+2+1 = 7
所属组 = rw- = 4+2 = 6
其他人 = r-x = 4+1 = 5

因此权限对应的权限值就是765。

使用数字方式修改文件权限的chmod命令基本格式为：
```shell
chmod [-R] 权限值 文件名
```
举例修改微信小程序“Java精选面试题”，后台部署的jingxuan-admin.jar包文件的权限：

```shell
[root@iZ256w2hluuZ tomcat]# ll
total 100
-rw-r--r--  1 root root 75504722 May 29 13:29 jingxuan-admin.jar
[root@iZ256w2hluuZ tomcat]# chmod 765 jingxuan-admin.jar 
[root@iZ256w2hluuZ tomcat]# ll
total 100
-rwxrw-r-x  1 root root 75504722 May 29 13:29 jingxuan-admin.jar
```

### 题4：[Linux 中为什么需要零拷贝？](/docs/Linux/最新面试题2021年Linux面试题及答案汇总.md#题4linux-中为什么需要零拷贝)<br/>
传统的Linux系统的标准I/O接口（read、write）是基于数据拷贝的，也就是数据都是copy_to_user或者copy_from_user。

好处：通过中间缓存的机制，减少磁盘I/O的操作。

坏处：大量数据的拷贝，用户态和内核态的频繁切换，会消耗大量的CPU资源，严重影响数据传输的性能。

在网络速度比较慢的时代（56K猫、10/100MB以太网）其实不需要零拷贝技术，因为内部再快也会被网络速率卡住，木桶效应。但是当网路速度大幅提升出现1Gb、10Gb甚至100Gb网速的时候这种零拷贝技术就迫切需要，因为网络传输速度已经远远大于计算机内部的数据流转速度。所以有必要提速，那么这时候人们就关注如何优化计算机内部数据流。

### 题5：[使用 top 查看系统资源占用情况时，哪一列表示内存占用？](/docs/Linux/最新面试题2021年Linux面试题及答案汇总.md#题5使用-top-查看系统资源占用情况时哪一列表示内存占用)<br/>
```shell
[root@JingXuan-Java ~]# top
top - 11:41:17 up 155 days,  1:28,  1 user,  load average: 0.01, 0.02, 0.00
Tasks: 110 total,   1 running, 109 sleeping,   0 stopped,   0 zombie
%Cpu(s):  1.0 us,  0.5 sy,  0.0 ni, 98.2 id,  0.0 wa,  0.2 hi,  0.2 si,  0.0 st
MiB Mem :   3637.6 total,    130.2 free,   2901.4 used,    606.0 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.    509.8 avail Mem 

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                                                                                             
  803 polkitd   20   0 1608468   5104      0 S   0.3   0.1  43:09.20 polkitd                                                                                                             
 8562 root      20   0  140676   8596   7408 S   0.3   0.2   0:00.01 sshd                                                                                                                
 8563 root      20   0  143076   8916   7736 S   0.3   0.2   0:00.01 sshd        
```

VIRT表示虚拟内存用量。

RES表示物理内存用量。

SHR表示共享内存用量。

%MEM表示内存用量。

### 题6：[vmstat 命令中 r、b、si、so、bi、bo 这几列表示什么含义？](/docs/Linux/最新面试题2021年Linux面试题及答案汇总.md#题6vmstat-命令中-rbsisobibo-这几列表示什么含义)<br/>
```shell
 [root@JingXuan-Java ~]# vmstat
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 112896      0 641456    0    0    13    18    1    2  0  0 99  0  0
```

r即running，表示正在跑的任务数。

b即blocked，表示被阻塞的任务数。

si表示有多少数据从交换分区读入内存。

so表示有多少数据从内存写入交换分区。

bi表示有多少数据从磁盘读入内存。

bo表示有多少数据从内存写入磁盘。

其他：

i --input，进入内存。

o --output，从内存出去。

s --swap，交换分区。

b --block，块设备，磁盘。

注：单位都是KB

### 题7：[Linux 中如何创建软链接？](/docs/Linux/最新面试题2021年Linux面试题及答案汇总.md#题7linux-中如何创建软链接)<br/>
用法：ln -s 源文件 目标文件

```shell
[root@mrwang ~]#  ln -s /jingxuan/titles /home/titles
```

其中/jingxuan/titles目录中titles是源文件，/home/titles目录中titles是目标文件，实际链接的是/jingxuan/titles文件。

删除软连接命令

```shell
[root@mrwang ~]# rm -rf /home/titles
```
这样只会删除目标文件，不会删除源文件。

### 题8：[Linux 中如何查看某个网卡是否连接着交换机？](/docs/Linux/最新面试题2021年Linux面试题及答案汇总.md#题8linux-中如何查看某个网卡是否连接着交换机)<br/>
使用mii-tool eth0或者mii-tool eth1命令，查看某个网卡是否连接着交换机。

### 题9：[Linux 设置 DNS 修改哪个配置文件？](/docs/Linux/最新面试题2021年Linux面试题及答案汇总.md#题9linux-设置-dns-修改哪个配置文件)<br/>
全局的配置可以在/etc/resolv.conf文件中配置。

指定网卡的配置可以在/etc/sysconfig/network-scripts/ifcfg-eth0文件中配置。

一般来说，肯定是全局配置即可。

### 题10：[Linux 中使用什么命令搜索文件？](/docs/Linux/最新面试题2021年Linux面试题及答案汇总.md#题10linux-中使用什么命令搜索文件)<br/>
find <指定目录> <指定条件> <指定动作>

whereis 加参数与文件名

locate 只加文件名

find 直接搜索磁盘，较慢。

举例：

```shell
find / -name "string*"
```

### 题11：linux-常见服务占用端口都有哪些<br/>


### 题12：linux-中什么是默认登录-shell-<br/>


### 题13：描述-linux-系统从开机到登陆界面的启动过程。<br/>


### 题14：linux-中查看文件内容有哪些命令<br/>


### 题15：linux-中如何快速切换到上-n-级目录<br/>


### 题16：linux-中如何查看网络连接状况<br/>


### 题17：linux-中使用什么命令查看-ip-地址及接口信息<br/>


### 题18：linux-中如何进入含有空格的目录<br/>


### 题19：linux-中进程有哪几种状态<br/>


### 题20：bash-shell-中-hash-命令有什么作用<br/>


### 题21：linux-中如何切换到上-n-级目录<br/>


### 题22：linux-中如何创建硬链接<br/>


### 题23：linux-如何切换用户<br/>


### 题24：linux-中如何让命令后台运行<br/>


### 题25：su-root-和-su---root-有什么区别<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")