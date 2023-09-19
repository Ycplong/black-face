# Linux经典面试题集锦，学会再也不怕面试了！

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Linux

### 题1：[Linux 中 du 和 df 命令有什么区别？](/docs/Linux/Linux经典面试题集锦，学会再也不怕面试了！.md#题1linux-中-du-和-df-命令有什么区别)<br/>
du显示目录或文件的大小。

df显示每个<文件>所在的文件系统的信息，默认是显示所有文件系统。

文件系统分配其中的一些磁盘块用来记录它自身的一些数据，如i节点，磁盘分布图，间接块，超级块等。这些数据对大多数用户级的程序来说是不可见的，通常称为Meta Data。

du命令是用户级的程序，它不考虑Meta Data，而df命令则查看文件系统的磁盘分配图并考虑Meta Data。

df命令获得真正的文件系统数据，而du命令只查看文件系统的部分情况。


### 题2：[Linux 常见服务占用端口都有哪些？](/docs/Linux/Linux经典面试题集锦，学会再也不怕面试了！.md#题2linux-常见服务占用端口都有哪些)<br/>
80 8080 443

20 21 22 23 25 53

135（RPC）137（NetBIOS/UDP） 138（UDP） 139 （samba）

161 SNMP

1080 Socket代理

3306 11211 8080 jboss tomcat 50170

### 题3：[Linux 中 buffer 和 cache 如何区分？](/docs/Linux/Linux经典面试题集锦，学会再也不怕面试了！.md#题3linux-中-buffer-和-cache-如何区分)<br/>
buffer和cache都是内存中的一块区域，当CPU需要写数据到磁盘时，由于磁盘速度比较慢，所以CPU先把数据存进buffer，然后CPU去执行其他任务，buffer中的数据会定期写入磁盘；当CPU需要从磁盘读入数据时，由于磁盘速度比较慢，可以把即将用到的数据提前存入cache，CPU直接从Cache中拿数据要快的多。

### 题4：[Shell 脚本是什么？](/docs/Linux/Linux经典面试题集锦，学会再也不怕面试了！.md#题4shell-脚本是什么)<br/>
一个Shell脚本可以理解为一个文本文件，它包含一个或多个命令。

比如作为系统管理员，经常需要使用多个命令来完成一项任务，那么就可以可以将这些命令添加在一个文本文件中（Shell脚本），来完成这些日常的工作任务。

Shell是一种脚本语言，那么就必须有解释器来执行这些脚本，常见的脚本解释器如下：

**bash**

bash脚本解释器是Linux标准默认的shell。

bash由Brian Fox和Chet Ramey共同完成，是BourneAgain Shell的缩写，内部命令一共有40个。

**sh**

sh脚本解释器是由Steve Bourne开发，是Bourne Shell的缩写，sh是Unix标准默认的shell。

另外还有：ash、csh、ksh等。

### 题5：[Linux 中零拷贝是什么？](/docs/Linux/Linux经典面试题集锦，学会再也不怕面试了！.md#题5linux-中零拷贝是什么)<br/>
零拷贝主要的任务是避免CPU将数据从一块存储拷贝到另外一块存储，利用各种零拷贝技术，避免让CPU做大量的数据拷贝任务，减少不必要的拷贝，或者让别的组件来做这一类简单的数据传输任务，让CPU解脱出来专注于别的任务。这样就可以让系统资源的利用更加有效。

### 题6：[Linux 中如何翻页查看大文件内容？](/docs/Linux/Linux经典面试题集锦，学会再也不怕面试了！.md#题6linux-中如何翻页查看大文件内容)<br/>
通过管道将命令“cat file_name.txt”和“more”连接在一起可以实现翻页查看大文件内容。

```shell
[root@mrwang ~]# cat file_name.txt | more
```

### 题7：[使用 top 查看系统资源占用情况时，哪一列表示内存占用？](/docs/Linux/Linux经典面试题集锦，学会再也不怕面试了！.md#题7使用-top-查看系统资源占用情况时哪一列表示内存占用)<br/>
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

### 题8：[Linux 中如何快速切换到上 N 级目录？](/docs/Linux/Linux经典面试题集锦，学会再也不怕面试了！.md#题8linux-中如何快速切换到上-n-级目录)<br/>
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

### 题9：[Linux 中什么是硬链接和软链接？](/docs/Linux/Linux经典面试题集锦，学会再也不怕面试了！.md#题9linux-中什么是硬链接和软链接)<br/>
**1、硬链接**

由于Linux下的文件是通过索引节点(inode)来识别文件，硬链接可以认为是一个指针，指向文件索引节点的指针，系统并不为它重新分配inode。每添加一个一个硬链接，文件的链接数就加1。

硬链接不足是不可以在不同文件系统的文件间建立链接；只有超级用户才可以为目录创建硬链接。

**2、软链接**

软链接克服了硬链接的不足，没有任何文件系统的限制，任何用户可以创建指向目录的符号链接。因而现在更为广泛使用，它具有更大的灵活性，甚至可以跨越不同机器、不同网络对文件进行链接。

软链接不足之处是链接文件包含有原文件的路径信息，所以当原文件从一个目录下移到其他目录中，再访问链接文件，系统就找不到了文件了，而硬链接就没有这个缺陷，想怎么移就怎么移；还有它要系统分配额外的空间用于建立新的索引节点和保存原文件的路径。

### 题10：[Linux 中使用什么命令查看 ip 地址及接口信息？](/docs/Linux/Linux经典面试题集锦，学会再也不怕面试了！.md#题10linux-中使用什么命令查看-ip-地址及接口信息)<br/>
Linux中使用ifconfig命令查看ip地址及接口信息。

```shell
[root@mrwang jingxuan]# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.44.191  netmask 255.255.240.0  broadcast 172.17.47.255
        inet6 fe80::216:3eff:fe35:66a2  prefixlen 64  scopeid 0x20<link>
        ether 00:16:3e:35:66:a2  txqueuelen 1000  (Ethernet)
        RX packets 44392695  bytes 23482900900 (21.8 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 40107204  bytes 48480485174 (45.1 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 19128710  bytes 33464900312 (31.1 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 19128710  bytes 33464900312 (31.1 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

### 题11：su-root-和-su---root-有什么区别<br/>


### 题12：linux-中如何修改文件权限<br/>


### 题13：linux-中如何查看运行日志<br/>


### 题14：linux-中-shell-脚本如何增加注释<br/>


### 题15：linux-中如何查看系统负载数值表示什么含义<br/>


### 题16：linux-中什么是默认登录-shell-<br/>


### 题17：linux-中如何切换到上-n-级目录<br/>


### 题18：linux-中如何查看几颗物理-cpu-和每颗-cpu-的核数<br/>


### 题19：linux-中如何创建硬链接<br/>


### 题20：linux-中如何查看指定目录的大小<br/>


### 题21：bash-shell-中-hash-命令有什么作用<br/>


### 题22：vmstat-命令中-rbsisobibo-这几列表示什么含义<br/>


### 题23：linux-常见目录结构都有哪些<br/>


### 题24：linux-中使用什么命令搜索文件<br/>


### 题25：shell-脚本中-$?-标记有什么用途<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")