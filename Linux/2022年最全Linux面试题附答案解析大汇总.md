# 2022年最全Linux面试题附答案解析大汇总

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Linux

### 题1：[Linux 中如何创建硬链接？](/docs/Linux/2022年最全Linux面试题附答案解析大汇总.md#题1linux-中如何创建硬链接)<br/>
用法：ln 源文件 目标文件

```shell
[root@mrwang ~]#  ln /jingxuan/titles /home/titles
```
注意如果使用ln –f existingfile newfile，如果newfile已经存在，则无论原来newfile是什么文件，只要当前用户对它有写权限，newfile就成为exisitngfile的硬链接文件。

尽管硬链接节省空间，也是Linux系统整合文件系统的传统方式，但是存在一下不足之处：

1）不可以在不同文件系统的文件间建立链接。

2）不允许给目录创建硬链接。

### 题2：[Linux 中如何快速切换到上 N 级目录？](/docs/Linux/2022年最全Linux面试题附答案解析大汇总.md#题2linux-中如何快速切换到上-n-级目录)<br/>
**编辑文件**

使修改在当前用户下有效，执行命令如下：

```shell
[root@iZ256w2hluuZ conf]# vim .bashrc
```

使修改在所有用户下有效，需切换root用户下，执行命令如下：

```shell
[root@iZ256w2hluuZ conf]# vim /etc/profile
```

打开文件后，在文件结尾添加别名如下：
```shell
alias cd1 = 'cd ..'
alias cd2 = 'cd ../..'
alias cd3 = 'cd ../../..'
alias cd4 = 'cd ../../../..'
alias cd5 = 'cd ../../../../..'
alias cd6 = 'cd ../../../../../..'
```
执行wq命令，保存文件并退出。

**为了使修改立即生效**

.bashrc文件执行命令：
```shell
source .bashrc
```
profile文件，执行命令：

```shell
source /etc/profile
```
下面就可以执行对应不同级别目录的命令切换目录了，举例如下：
```shell
[root@iZ256w2hluuZ tomcat]# pwd
/mnt/app/tomcat
[root@iZ256w2hluuZ tomcat]# cd2
[root@iZ256w2hluuZ mnt]# pwd
/mnt
```

### 题3：[vmstat 命令中 r、b、si、so、bi、bo 这几列表示什么含义？](/docs/Linux/2022年最全Linux面试题附答案解析大汇总.md#题3vmstat-命令中-rbsisobibo-这几列表示什么含义)<br/>
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

### 题4：[bash shell 中 hash 命令有什么作用？](/docs/Linux/2022年最全Linux面试题附答案解析大汇总.md#题4bash-shell-中-hash-命令有什么作用)<br/>
Linux中hash命令管理着一个内置的哈希表，记录了已执行过命令的完整路径, 用该命令可以打印出所使用过的命令以及执行的次数。

### 题5：[Linux 中 du 和 df 命令有什么区别？](/docs/Linux/2022年最全Linux面试题附答案解析大汇总.md#题5linux-中-du-和-df-命令有什么区别)<br/>
du显示目录或文件的大小。

df显示每个<文件>所在的文件系统的信息，默认是显示所有文件系统。

文件系统分配其中的一些磁盘块用来记录它自身的一些数据，如i节点，磁盘分布图，间接块，超级块等。这些数据对大多数用户级的程序来说是不可见的，通常称为Meta Data。

du命令是用户级的程序，它不考虑Meta Data，而df命令则查看文件系统的磁盘分配图并考虑Meta Data。

df命令获得真正的文件系统数据，而du命令只查看文件系统的部分情况。


### 题6：[Linux 中如何查看几颗物理 CPU 和每颗 CPU 的核数？](/docs/Linux/2022年最全Linux面试题附答案解析大汇总.md#题6linux-中如何查看几颗物理-cpu-和每颗-cpu-的核数)<br/>
```shell
[root@JingXuan-Java ~]# cat /proc/cpuinfo|grep -c 'physical id'
4
[root@JingXuan-Java ~]# cat /proc/cpuinfo|grep -c 'processor'
4
```

### 题7：[Linux 如何统计系统当前进程连接数？](/docs/Linux/2022年最全Linux面试题附答案解析大汇总.md#题7linux-如何统计系统当前进程连接数)<br/>
Linux 统计系统当前进程连接数，输入命令如下：

```shell
netstat -an | grep ESTABLISHED | wc -l
```

输出结果33。意思就是目前一共有33个连接数。

### 题8：[Linux 中如何进入含有空格的目录？](/docs/Linux/2022年最全Linux面试题附答案解析大汇总.md#题8linux-中如何进入含有空格的目录)<br/>
Linux中有时需要进入带有空格的目录，比如“jing xuan”目录。

可以将整个目录用英文半角的双引号""包过起来执行cd命令，举例如下：

```shell
[root@iZ256w2hluuZ tomcat]# ls
jing xuan
[root@iZ256w2hluuZ tomcat]# cd jing xuan
-bash: cd: jing: No such file or directory
[root@iZ256w2hluuZ tomcat]# cd "jing xuan"
[root@iZ256w2hluuZ jing xuan]# 
```

可以使用转义的方法\将空格转义执行cd命令，举例如下：
```shell
[root@iZ256w2hluuZ tomcat]# ls
jing xuan  
[root@iZ256w2hluuZ tomcat]# cd jing\ xuan
[root@iZ256w2hluuZ jing xuan]# 
```

### 题9：[hdfs 一个 block 多大，为什么 128M？](/docs/Linux/2022年最全Linux面试题附答案解析大汇总.md#题9hdfs-一个-block-多大为什么-128m)<br/>
1、不能远小于128M：减少硬盘寻道时间、减少Namenode内存消耗。

2、不能远大于128M：

1）Map崩溃问题 （数据块大，重新加载时间长）。

2）预设时间间隔问题（从数据块的角度大概估算，数据块越大，时间越长）。

3）问题分解问题：数据量大小和问题解决的复杂度成线性关系。

4）约束map输出：map之后的数据需要排序后再执行reduce，大文件不利于归并排序的思想。

### 题10：[Linux 中如何重启网卡，使配置生效？](/docs/Linux/2022年最全Linux面试题附答案解析大汇总.md#题10linux-中如何重启网卡使配置生效)<br/>
使用vi或者vim编辑器编辑网卡配置文件/etc/sysconfig/network-scripts/ifcft-eth0（如果是eth1文件名为ifcft-eth1），内容如下：

```shell
DEVICE=eth0
HWADDR=00:0C:29:06:37:BA
TYPE=Ethernet
UUID=0eea1820-1fe8-4a80-a6f0-39b3d314f8da
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
IPADDR=192.168.147.130
NETMASK=255.255.255.0
GATEWAY=192.168.147.2
DNS1=192.168.147.2
DNS2=8.8.8.8
```

修改网卡后，使用命令重启网卡：

```shell
ifdown eth0
ifup eth0
```

也可以重启网络服务：

```shell
service network restart
```

### 题11：linux-中如何让命令后台运行<br/>


### 题12：shell-脚本是什么<br/>


### 题13：linux-中如何查看指定目录的大小<br/>


### 题14：su-root-和-su---root-有什么区别<br/>


### 题15：linux-中如何关闭进程<br/>


### 题16：linux-中如何切换到上-n-级目录<br/>


### 题17：linux-中如何翻页查看大文件内容<br/>


### 题18：linux-中如何查看网络连接状况<br/>


### 题19：命令中可以使用哪几种通配符<br/>


### 题20：什么是-linux-操作系统<br/>


### 题21：linux-中进程有哪几种状态<br/>


### 题22：bash-shell-中-hash-命令有什么作用<br/>


### 题23：linux-中-ll-和-ls-命令有什么区别<br/>


### 题24：使用-top-查看系统资源占用情况时哪一列表示内存占用<br/>


### 题25：linux-中如何查看系统都开启了哪些端口<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")