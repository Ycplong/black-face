# 50 道常见的Linux系统简单面试题附答案

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Linux

### 题1：[Linux 中使用什么命令查看 ip 地址及接口信息？](/docs/Linux/50%20道常见的Linux系统简单面试题附答案.md#题1linux-中使用什么命令查看-ip-地址及接口信息)<br/>
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

### 题2：[Linux 中零拷贝是什么？](/docs/Linux/50%20道常见的Linux系统简单面试题附答案.md#题2linux-中零拷贝是什么)<br/>
零拷贝主要的任务是避免CPU将数据从一块存储拷贝到另外一块存储，利用各种零拷贝技术，避免让CPU做大量的数据拷贝任务，减少不必要的拷贝，或者让别的组件来做这一类简单的数据传输任务，让CPU解脱出来专注于别的任务。这样就可以让系统资源的利用更加有效。

### 题3：[Linux 中如何启动和关闭防火墙？](/docs/Linux/50%20道常见的Linux系统简单面试题附答案.md#题3linux-中如何启动和关闭防火墙)<br/>
CentOS7以上版本的防火墙配置跟以前版本有很大区别，CentOS7以上版本的防火墙默认使用的是firewall，与之前的版本使用iptables不一样。

1、关闭防火墙：

systemctl stop firewalld.service

2、开启防火墙：

systemctl start firewalld.service

3、关闭开机启动：

systemctl disable firewalld.service

4、开启开机启动：

systemctl enable firewalld.service

### 题4：[su root 和 su - root 有什么区别？](/docs/Linux/50%20道常见的Linux系统简单面试题附答案.md#题4su-root-和-su---root-有什么区别)<br/>
su后面不加用户是默认切到root，su是不改变当前变量；su -是改变为切换到用户的变量。

简单来说就是su只能获得root的执行权限，不能获得环境变量，而su -是切换到root并获得root的环境变量及执行权限。

语法：

```shell
$ su [user_name]
```

su命令可以用来更改你的用户ID和组ID。su是switch user或set user id的一个缩写。

此命令是用于开启一个子进程，成为新的用户ID和赋予存取与这个用户ID关联所有文件的存取权限。

出于安全的考虑，在实际转换身份时，会被要求输入这个用户帐号的密码。

如果没有参数，su命令将转换为root（系统管理员）。root帐号有时也被称为超级用户，因为这个用户可以存取系统中的任何文件。

当必须要提供root密码，想要回到原先的用户身份，可以不使用su命令，只需使用exit命令退出使用su命令而生成的新的对话进程。

```shell
$ su – username
```

当使用命令su username时，对话特征和原始的登录身份一样。

如果需要对话进程拥有转换后的用户ID一致的特征，使用短斜杠命令: su – username。

### 题5：[Linux 中如何创建软链接？](/docs/Linux/50%20道常见的Linux系统简单面试题附答案.md#题5linux-中如何创建软链接)<br/>
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

### 题6：[Linux 中查看文件内容有哪些命令？](/docs/Linux/50%20道常见的Linux系统简单面试题附答案.md#题6linux-中查看文件内容有哪些命令)<br/>
vi 文件名 #编辑方式查看，可修改。

cat 文件名 #显示全部文件内容。

more 文件名 #分页显示文件内容。

less 文件名 #与 more 相似，更好的是可以往前翻页。

tail 文件名 #仅查看尾部，还可以指定行数。

head 文件名 #仅查看头部,还可以指定行数。

### 题7：[Linux 中如何翻页查看大文件内容？](/docs/Linux/50%20道常见的Linux系统简单面试题附答案.md#题7linux-中如何翻页查看大文件内容)<br/>
通过管道将命令“cat file_name.txt”和“more”连接在一起可以实现翻页查看大文件内容。

```shell
[root@mrwang ~]# cat file_name.txt | more
```

### 题8：[Linux 中如何创建硬链接？](/docs/Linux/50%20道常见的Linux系统简单面试题附答案.md#题8linux-中如何创建硬链接)<br/>
用法：ln 源文件 目标文件

```shell
[root@mrwang ~]#  ln /jingxuan/titles /home/titles
```
注意如果使用ln –f existingfile newfile，如果newfile已经存在，则无论原来newfile是什么文件，只要当前用户对它有写权限，newfile就成为exisitngfile的硬链接文件。

尽管硬链接节省空间，也是Linux系统整合文件系统的传统方式，但是存在一下不足之处：

1）不可以在不同文件系统的文件间建立链接。

2）不允许给目录创建硬链接。

### 题9：[生产场景如何对 Linux 系统进行合理规划分区？](/docs/Linux/50%20道常见的Linux系统简单面试题附答案.md#题9生产场景如何对-linux-系统进行合理规划分区)<br/>
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

### 题10：[Linux 中如何切换到上 N 级目录？](/docs/Linux/50%20道常见的Linux系统简单面试题附答案.md#题10linux-中如何切换到上-n-级目录)<br/>
执行cd命令可以切换上下级目录，如果切换到上一级目录执行“cd ..”命令：

```shell
[root@iZ256w2hluuZ tomcat]# pwd
/mnt/app/tomcat
[root@iZ256w2hluuZ tomcat]# cd ..
[root@iZ256w2hluuZ app]# pwd
/mnt/app
``

如果切换上两级目录需执行“cd ../..”命令，这里的“../”可以理解成上一级目录，其中“/”最后一个可以省略也可以携带，没有影响。每增加一个“../”即为增加一次上一级目录，举例如下：

```shell
[root@iZ256w2hluuZ apache-tomcat-jingxuan]# pwd
/mnt/app/tomcat/apache-tomcat-jingxuan
[root@iZ256w2hluuZ apache-tomcat-jingxuan]# cd ../../../
[root@iZ256w2hluuZ mnt]# pwd
/mnt
[root@iZ256w2hluuZ mnt]# 
```

### 题11：linux-中如何关闭进程<br/>


### 题12：linux-中使用什么命令查看磁盘占用情况<br/>


### 题13：linux-中-du-和-df-命令有什么区别<br/>


### 题14：linux-中如何查看运行日志<br/>


### 题15：linux-中如何查看某个网卡是否连接着交换机<br/>


### 题16：linux-中如何查看系统负载数值表示什么含义<br/>


### 题17：linux-中-shell-脚本如何增加注释<br/>


### 题18：linux-中-buffer-和-cache-如何区分<br/>


### 题19：linux-中什么是默认登录-shell-<br/>


### 题20：bash-shell-中-hash-命令有什么作用<br/>


### 题21：使用-top-查看系统资源占用情况时哪一列表示内存占用<br/>


### 题22：linux-中如何查看系统都开启了哪些端口<br/>


### 题23：hdfs-一个-block-多大为什么-128m<br/>


### 题24：linux-常见目录结构都有哪些<br/>


### 题25：linux-中什么是硬链接和软链接<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")