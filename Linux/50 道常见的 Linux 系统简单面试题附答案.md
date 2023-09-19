# 50 道常见的 Linux 系统简单面试题附答案

### 全部面试题答案，更新日期：12月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Linux

### 题1：[Linux 中如何查看某个网卡是否连接着交换机？](/docs/Linux/50%20道常见的%20Linux%20系统简单面试题附答案.md#题1linux-中如何查看某个网卡是否连接着交换机)<br/>
使用mii-tool eth0或者mii-tool eth1命令，查看某个网卡是否连接着交换机。

### 题2：[bash shell 中 hash 命令有什么作用？](/docs/Linux/50%20道常见的%20Linux%20系统简单面试题附答案.md#题2bash-shell-中-hash-命令有什么作用)<br/>
Linux中hash命令管理着一个内置的哈希表，记录了已执行过命令的完整路径, 用该命令可以打印出所使用过的命令以及执行的次数。

### 题3：[Linux 如何切换用户？](/docs/Linux/50%20道常见的%20Linux%20系统简单面试题附答案.md#题3linux-如何切换用户)<br/>
Linux系统切换用户的命令是su

su的含义是(switch user)切换用户的缩写。

通过su命令，可以从普通用户切换到root用户，也可以从root用户切换到普通用户。

<font color="#dd0000">注：从普通用户切换到root用户需要密码，从root用户切换到普通用户不需要密码。</font>

使用SecureCRT工具连接终端，由普通用户切换到root用户。

在终端输入su命令然后回车，要求输入密码（linux终端输入的密码不显示）输入密码后回车进入root用户。

在终端输入su  root然后回车，也可以进入root用户或su - root回车，也可以切换root用户。针对su root与su - root区别之后的篇幅会阐述，欢迎关注微信公众号“Java精选”，送免费资料。

### 题4：[Linux 中如何翻页查看大文件内容？](/docs/Linux/50%20道常见的%20Linux%20系统简单面试题附答案.md#题4linux-中如何翻页查看大文件内容)<br/>
通过管道将命令“cat file_name.txt”和“more”连接在一起可以实现翻页查看大文件内容。

```shell
[root@mrwang ~]# cat file_name.txt | more
```

### 题5：[Shell 脚本中 $? 标记有什么用途？](/docs/Linux/50%20道常见的%20Linux%20系统简单面试题附答案.md#题5shell-脚本中-$?-标记有什么用途)<br/>
编写一个Shell脚本时，如果想要检查前一命令是否执行成功，可以在if条件中使用“$?”来检查前一命令的结束状态。

如果结束状态是0，说明前一个命令执行成功。举例如下：

```shell
[root@iZ256w2hluuZ ~]# ls /mnt/app/tomcat/
apache-tomcat-blog    apache-tomcat-jingxuan  jingxuan-admin.jar  logs
apache-tomcat-client  apache-tomcat-server    jingxuan.log        zs-jingxuan-admin.jar
[root@iZ256w2hluuZ ~]# echo $?
0
[root@iZ256w2hluuZ ~]# 
```

如果结束状态不是0，说明命令执行失败。举例如下：

```shell
[root@iZ256w2hluuZ ~]# ls /mnt/app/nginx
ls: cannot access /mnt/app/nginx: No such file or directory
[root@iZ256w2hluuZ ~]# echo $?          
2
[root@iZ256w2hluuZ ~]# 
```

### 题6：[什么是 Linux 操作系统？](/docs/Linux/50%20道常见的%20Linux%20系统简单面试题附答案.md#题6什么是-linux-操作系统)<br/>
Linux全称GNU/Linux，是一套免费使用和自由传播的类Unix操作系统，是一个基于POSIX的多用户、多任务、支持多线程和多CPU的操作系统。

伴随着互联网的发展，Linux得到了来自全世界软件爱好者、组织、公司的支持。它除了在服务器方面保持着强劲的发展势头以外，在个人电脑、嵌入式系统上都有着长足的进步。使用者不仅可以直观地获取该操作系统的实现机制，而且可以根据自身的需要来修改完善Linux，使其最大化地适应用户的需要。&nbsp;

Linux不仅系统性能稳定，而且是开源软件。其核心防火墙组件性能高效、配置简单，保证了系统的安全。

在很多企业网络中，为了追求速度和安全，Linux不仅仅是被网络运维人员当作服务器使用，甚至当作网络防火墙，这是Linux的一大亮点。

Linux具有开放源码、没有版权、技术社区用户多等特点，开放源码使得用户可以自由裁剪，灵活性高，功能强大，成本低。尤其系统中内嵌网络协议栈，经过适当的配置就可实现路由器的功能。这些特点使得Linux成为开发路由交换设备的理想开发平台。

### 题7：[Linux 中如何快速切换到上 N 级目录？](/docs/Linux/50%20道常见的%20Linux%20系统简单面试题附答案.md#题7linux-中如何快速切换到上-n-级目录)<br/>
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

### 题8：[Linux 中 buffer 和 cache 如何区分？](/docs/Linux/50%20道常见的%20Linux%20系统简单面试题附答案.md#题8linux-中-buffer-和-cache-如何区分)<br/>
buffer和cache都是内存中的一块区域，当CPU需要写数据到磁盘时，由于磁盘速度比较慢，所以CPU先把数据存进buffer，然后CPU去执行其他任务，buffer中的数据会定期写入磁盘；当CPU需要从磁盘读入数据时，由于磁盘速度比较慢，可以把即将用到的数据提前存入cache，CPU直接从Cache中拿数据要快的多。

### 题9：[Linux 中如何查看指定目录的大小？](/docs/Linux/50%20道常见的%20Linux%20系统简单面试题附答案.md#题9linux-中如何查看指定目录的大小)<br/>
查询当前目录总大小可以使用du -sh，其中s代表统计汇总的意思，即只输出一个总和大小。

```shell
[root@mrwang jingxuan]# du -sh
6.1M    .
```

linux查看指定目录的的大小，可以使用“du -sh 目录名称”命令。

```shell
[root@mrwang jingxuan]# du -sh logs         
6.1M    logs
```

### 题10：[Linux 中使用什么命令查看 ip 地址及接口信息？](/docs/Linux/50%20道常见的%20Linux%20系统简单面试题附答案.md#题10linux-中使用什么命令查看-ip-地址及接口信息)<br/>
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

![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")