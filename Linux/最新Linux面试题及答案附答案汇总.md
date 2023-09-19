# 最新Linux面试题及答案附答案汇总

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Linux

### 题1：[Linux 中零拷贝是什么？](/docs/Linux/最新Linux面试题及答案附答案汇总.md#题1linux-中零拷贝是什么)<br/>
零拷贝主要的任务是避免CPU将数据从一块存储拷贝到另外一块存储，利用各种零拷贝技术，避免让CPU做大量的数据拷贝任务，减少不必要的拷贝，或者让别的组件来做这一类简单的数据传输任务，让CPU解脱出来专注于别的任务。这样就可以让系统资源的利用更加有效。

### 题2：[Linux 中使用什么命令搜索文件？](/docs/Linux/最新Linux面试题及答案附答案汇总.md#题2linux-中使用什么命令搜索文件)<br/>
find <指定目录> <指定条件> <指定动作>

whereis 加参数与文件名

locate 只加文件名

find 直接搜索磁盘，较慢。

举例：

```shell
find / -name "string*"
```

### 题3：[Linux 中查看文件内容有哪些命令？](/docs/Linux/最新Linux面试题及答案附答案汇总.md#题3linux-中查看文件内容有哪些命令)<br/>
vi 文件名 #编辑方式查看，可修改。

cat 文件名 #显示全部文件内容。

more 文件名 #分页显示文件内容。

less 文件名 #与 more 相似，更好的是可以往前翻页。

tail 文件名 #仅查看尾部，还可以指定行数。

head 文件名 #仅查看头部,还可以指定行数。

### 题4：[Linux 中如何查看系统负载？数值表示什么含义？](/docs/Linux/最新Linux面试题及答案附答案汇总.md#题4linux-中如何查看系统负载数值表示什么含义)<br/>
```shell
[root@JingXuan-Java ~]# w
 11:35:35 up 155 days,  1:22,  1 user,  load average: 0.04, 0.04, 0.01
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/1    124.200.36.106   11:33    0.00s  0.01s  0.00s w
[root@JingXuan-Java ~]# uptime
 11:35:43 up 155 days,  1:22,  1 user,  load average: 0.04, 0.04, 0.00
```

其中load average即系统负载，三个数值分别表示一分钟、五分钟、十五分钟内系统的平均负载，即平均任务数。

### 题5：[查看文件内容有哪些命令？](/docs/Linux/最新Linux面试题及答案附答案汇总.md#题5查看文件内容有哪些命令)<br/>
vi 文件名 

编辑方式查看，可以修改文件

cat 文件名 

显示全部文件内容


tail 文件名 

仅查看文件尾部信息，可以指定查看行数

head 文件名 

仅查看头部,可以指定查看行数

more 文件名 

分页显示文件内容

less 文件名 

与more相似，可以往前翻页

### 题6：[Linux 中如何查看系统都开启了哪些端口？](/docs/Linux/最新Linux面试题及答案附答案汇总.md#题6linux-中如何查看系统都开启了哪些端口)<br/>
```shell
[root@JingXuan-Java ~]# netstat -lnp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:5355            0.0.0.0:*               LISTEN      1035/systemd-resolv 
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      317/nginx: worker p 
tcp        0      0 0.0.0.0:6386            0.0.0.0:*               LISTEN      1718/./src/redis-se 
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      317/nginx: worker p 
tcp        0      0 0.0.0.0:1022            0.0.0.0:*               LISTEN      1089/sshd           
tcp6       0      0 :::5355                 :::*                    LISTEN      1035/systemd-resolv 
tcp6       0      0 :::8080                 :::*                    LISTEN      27423/java          
```

### 题7：[生产场景如何对 Linux 系统进行合理规划分区？](/docs/Linux/最新Linux面试题及答案附答案汇总.md#题7生产场景如何对-linux-系统进行合理规划分区)<br/>
分区的根本原则是简单、易用、方便批量管理。根据服务器角色定位建议如下：

①单机服务器：如8G内存，300G硬盘

分区：  /boot 100-200M，swap 16G，内存大小8G*2，/ 80G，/var 20G（也可不分），/data 180G（存放web及db数据）

优点：数据盘和系统盘分开，有利于出问题时维护。

RAID方案：视数据及性能要求，一般可采用raid5折中。 

②负载均衡器（如LVS等） 

分区：/boot 100-200M，swap 内存的1-2倍，/  ，

优点：简单方便，只做转发数据量很少。 

RAID方案：数据量小，重要性高，可采用RAID1 

③负载均衡下的RS server

分区： /boot 100-200M，swap 内存的1-2倍，/  

优点：简单方便，因为有多机，对数据要求低。 

RAID方案：数据量大，重要性不高，有性能要求，数据要求低，可采用RAID0 

④数据库服务器mysql及oracle如16/32G内存

分区：/boot 100-200M，swap 16G，内存的1倍，/ 100G，/data 剩余（存放db数据） 

优点：数据盘和系统盘分开，有利于出问题时维护,及保持数据完整。 

RAID方案：视数据及性能要求主库可采取raid10/raid5，从库可采用raid0提高性能（读写分离的情况下。）

⑤存储服务器

分区：/boot 100-200M，swap 内存的1-2倍，/ 100G，/data(存放数据) 

优点：此服务器不要分区太多。只做备份，性能要求低。容量要大。 

RAID方案：可采取sata盘，raid5 

⑥共享存储服务器（如NFS） 

分区：/boot 100-200M，swap 内存的1-2倍，/ 100G，/data(存放数据) 

优点：此服务器不要分区太多。NFS共享比存储多的要求就是性能要求。 

RAID方案：视性能及访问要求可以raid5,raid10,甚至raid0（要有高可用或双写方案） 

⑦监控服务器cacti,nagios 

分区：/boot 100-200M，swap 内存的1-2倍，/ 

优点：重要性一般，数据要求也一般。 

RAID方案：单盘或双盘raid1即可。三盘就RAID5，看容量要求加盘即可。

### 题8：[Shell 脚本是什么？](/docs/Linux/最新Linux面试题及答案附答案汇总.md#题8shell-脚本是什么)<br/>
一个Shell脚本可以理解为一个文本文件，它包含一个或多个命令。

比如作为系统管理员，经常需要使用多个命令来完成一项任务，那么就可以可以将这些命令添加在一个文本文件中（Shell脚本），来完成这些日常的工作任务。

Shell是一种脚本语言，那么就必须有解释器来执行这些脚本，常见的脚本解释器如下：

**bash**

bash脚本解释器是Linux标准默认的shell。

bash由Brian Fox和Chet Ramey共同完成，是BourneAgain Shell的缩写，内部命令一共有40个。

**sh**

sh脚本解释器是由Steve Bourne开发，是Bourne Shell的缩写，sh是Unix标准默认的shell。

另外还有：ash、csh、ksh等。

### 题9：[Linux 中如何查看几颗物理 CPU 和每颗 CPU 的核数？](/docs/Linux/最新Linux面试题及答案附答案汇总.md#题9linux-中如何查看几颗物理-cpu-和每颗-cpu-的核数)<br/>
```shell
[root@JingXuan-Java ~]# cat /proc/cpuinfo|grep -c 'physical id'
4
[root@JingXuan-Java ~]# cat /proc/cpuinfo|grep -c 'processor'
4
```

### 题10：[什么是 Linux 操作系统？](/docs/Linux/最新Linux面试题及答案附答案汇总.md#题10什么是-linux-操作系统)<br/>
Linux全称GNU/Linux，是一套免费使用和自由传播的类Unix操作系统，是一个基于POSIX的多用户、多任务、支持多线程和多CPU的操作系统。

伴随着互联网的发展，Linux得到了来自全世界软件爱好者、组织、公司的支持。它除了在服务器方面保持着强劲的发展势头以外，在个人电脑、嵌入式系统上都有着长足的进步。使用者不仅可以直观地获取该操作系统的实现机制，而且可以根据自身的需要来修改完善Linux，使其最大化地适应用户的需要。&nbsp;

Linux不仅系统性能稳定，而且是开源软件。其核心防火墙组件性能高效、配置简单，保证了系统的安全。

在很多企业网络中，为了追求速度和安全，Linux不仅仅是被网络运维人员当作服务器使用，甚至当作网络防火墙，这是Linux的一大亮点。

Linux具有开放源码、没有版权、技术社区用户多等特点，开放源码使得用户可以自由裁剪，灵活性高，功能强大，成本低。尤其系统中内嵌网络协议栈，经过适当的配置就可实现路由器的功能。这些特点使得Linux成为开发路由交换设备的理想开发平台。

### 题11：linux-中如何创建软链接<br/>


### 题12：linux-中如何创建硬链接<br/>


### 题13：linux-中如何让命令后台运行<br/>


### 题14：使用-top-查看系统资源占用情况时哪一列表示内存占用<br/>


### 题15：bash-shell-中-hash-命令有什么作用<br/>


### 题16：linux-中为什么需要零拷贝<br/>


### 题17：linux-如何切换用户<br/>


### 题18：linux-中如何切换到上-n-级目录<br/>


### 题19：linux-中如何查看运行日志<br/>


### 题20：linux-中使用什么命令查看-ip-地址及接口信息<br/>


### 题21：shell-脚本中-$?-标记有什么用途<br/>


### 题22：hdfs-一个-block-多大为什么-128m<br/>


### 题23：linux-中使用什么命令查看磁盘占用情况<br/>


### 题24：linux-中进程有哪几种状态<br/>


### 题25：命令中可以使用哪几种通配符<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")