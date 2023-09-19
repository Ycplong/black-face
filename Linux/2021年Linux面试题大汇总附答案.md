# 2021年Linux面试题大汇总附答案

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Linux

### 题1：[查看文件内容有哪些命令？](/docs/Linux/2021年Linux面试题大汇总附答案.md#题1查看文件内容有哪些命令)<br/>
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

### 题2：[Linux 中如何查看运行日志？](/docs/Linux/2021年Linux面试题大汇总附答案.md#题2linux-中如何查看运行日志)<br/>
动态打印日志信息：tail –f 日志文件

```shell
[root@mrwang apache-tomcat-blog]# tail -f ./logs/catalina.out 
```

### 题3：[Linux 中如何快速切换到上 N 级目录？](/docs/Linux/2021年Linux面试题大汇总附答案.md#题3linux-中如何快速切换到上-n-级目录)<br/>
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

### 题4：[Linux 中如何查看几颗物理 CPU 和每颗 CPU 的核数？](/docs/Linux/2021年Linux面试题大汇总附答案.md#题4linux-中如何查看几颗物理-cpu-和每颗-cpu-的核数)<br/>
```shell
[root@JingXuan-Java ~]# cat /proc/cpuinfo|grep -c 'physical id'
4
[root@JingXuan-Java ~]# cat /proc/cpuinfo|grep -c 'processor'
4
```

### 题5：[bash shell 中 hash 命令有什么作用？](/docs/Linux/2021年Linux面试题大汇总附答案.md#题5bash-shell-中-hash-命令有什么作用)<br/>
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


### 题6：[su root 和 su - root 有什么区别？](/docs/Linux/2021年Linux面试题大汇总附答案.md#题6su-root-和-su---root-有什么区别)<br/>
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

### 题7：[Linux 中如何查看系统负载？数值表示什么含义？](/docs/Linux/2021年Linux面试题大汇总附答案.md#题7linux-中如何查看系统负载数值表示什么含义)<br/>
```shell
[root@JingXuan-Java ~]# w
 11:35:35 up 155 days,  1:22,  1 user,  load average: 0.04, 0.04, 0.01
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/1    124.200.36.106   11:33    0.00s  0.01s  0.00s w
[root@JingXuan-Java ~]# uptime
 11:35:43 up 155 days,  1:22,  1 user,  load average: 0.04, 0.04, 0.00
```

其中load average即系统负载，三个数值分别表示一分钟、五分钟、十五分钟内系统的平均负载，即平均任务数。

### 题8：[Linux 中如何修改文件权限？](/docs/Linux/2021年Linux面试题大汇总附答案.md#题8linux-中如何修改文件权限)<br/>
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

### 题9：[Linux 中使用什么命令查看 ip 地址及接口信息？](/docs/Linux/2021年Linux面试题大汇总附答案.md#题9linux-中使用什么命令查看-ip-地址及接口信息)<br/>
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

### 题10：[Linux 中如何查看某个网卡是否连接着交换机？](/docs/Linux/2021年Linux面试题大汇总附答案.md#题10linux-中如何查看某个网卡是否连接着交换机)<br/>
使用mii-tool eth0或者mii-tool eth1命令，查看某个网卡是否连接着交换机。

### 题11：生产场景如何对-linux-系统进行合理规划分区<br/>


### 题12：shell-脚本是什么<br/>


### 题13：linux-如何切换用户<br/>


### 题14：linux-如何统计系统当前进程连接数<br/>


### 题15：linux-中如何查看网络连接状况<br/>


### 题16：linux-中如何关闭进程<br/>


### 题17：描述-linux-系统从开机到登陆界面的启动过程。<br/>


### 题18：linux-中如何切换到上-n-级目录<br/>


### 题19：根据文件名搜索文件有哪些命令<br/>


### 题20：linux-中如何返回到切换目录之前的目录<br/>


### 题21：linux-中如何重启网卡使配置生效<br/>


### 题22：什么是-linux-操作系统<br/>


### 题23：vmstat-命令中-rbsisobibo-这几列表示什么含义<br/>


### 题24：linux-中什么是硬链接和软链接<br/>


### 题25：linux-中如何启动和关闭防火墙<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")