# 最新2021年Linux面试题及答案汇总版

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Linux

### 题1：[Linux 中如何查看系统都开启了哪些端口？](/docs/Linux/最新2021年Linux面试题及答案汇总版.md#题1linux-中如何查看系统都开启了哪些端口)<br/>
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

### 题2：[Linux 中 ll 和 ls 命令有什么区别？](/docs/Linux/最新2021年Linux面试题及答案汇总版.md#题2linux-中-ll-和-ls-命令有什么区别)<br/>
ll命令可以列出当前文件或目录的详细信息，含有时间、读写权限、大小、时间等信息 ，类似Windows显示的详细信息。而ls命令只可以列出文件名或目录名，类似windows的列表。

ll命令是ls -l命令的别名。相当于Windows里的快捷方式，也就是可以理解为ll和ls -l命令功能是相同的。

**ls [-参数]**

```shell
-a 列出目录下的所有文件，包括以 . 开头的隐含文件。
-b 把文件名中不可输出的字符用反斜杠加字符编号(就象在C语言里一样)的形式列出。
-c 输出文件的 i 节点的修改时间，并以此排序。
-d 将目录象文件一样显示，而不是显示其下的文件。
-e 输出时间的全部信息，而不是输出简略信息。
-f -U 对输出的文件不排序。
-g 无用。
-i 输出文件的 i 节点的索引信息。
-k 以 k 字节的形式表示文件的大小。
-l 列出文件的详细信息。
-m 横向输出文件名，并以“，”作分格符。
-n 用数字的 UID,GID 代替名称。
-o 显示文件的除组信息外的详细信息。
-p -F 在每个文件名后附上一个字符以说明该文件的类型，“*”表示可执行的普通
文件；“/”表示目录；“@”表示符号链接；“|”表示FIFOs；“=”表示套接字(sockets)。
-q 用?代替不可输出的字符。
-r 对目录反向排序。
-s 在每个文件名后输出该文件的大小。
-t 以时间排序。
-u 以文件上次被访问的时间排序。
-x 按列输出，横向排序。
-A 显示除 “.”和“..”外的所有文件。
-B 不输出以 “~”结尾的备份文件。
-C 按列输出，纵向排序。
-G 输出文件的组的信息。
-L 列出链接文件名而不是链接到的文件。
-N 不限制文件长度。
-Q 把输出的文件名用双引号括起来。
-R 列出所有子目录下的文件。
-S 以文件大小排序。
-X 以文件的扩展名(最后一个 . 后的字符)排序。
-1 一行只输出一个文件。
-–color=no 不显示彩色文件名
-–help 在标准输出上显示帮助信息。
-–version 在标准输出上输出版本信息并退出。
```

查看当前目录下文件数和目录数（包含子目录中的），命令如下：
```shell
[root@iZ256w2hluuZ tomcat]# ls -l * |grep "^-"|wc -l
247
[root@iZ256w2hluuZ tomcat]# ls -l * |grep "^d"|wc -l
29
```

去掉“|wc -l”命令可查看当前目录下文件和目录（包含子目录中的）。

### 题3：[查看文件内容有哪些命令？](/docs/Linux/最新2021年Linux面试题及答案汇总版.md#题3查看文件内容有哪些命令)<br/>
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

### 题4：[Linux 中什么是硬链接和软链接？](/docs/Linux/最新2021年Linux面试题及答案汇总版.md#题4linux-中什么是硬链接和软链接)<br/>
**1、硬链接**

由于Linux下的文件是通过索引节点(inode)来识别文件，硬链接可以认为是一个指针，指向文件索引节点的指针，系统并不为它重新分配inode。每添加一个一个硬链接，文件的链接数就加1。

硬链接不足是不可以在不同文件系统的文件间建立链接；只有超级用户才可以为目录创建硬链接。

**2、软链接**

软链接克服了硬链接的不足，没有任何文件系统的限制，任何用户可以创建指向目录的符号链接。因而现在更为广泛使用，它具有更大的灵活性，甚至可以跨越不同机器、不同网络对文件进行链接。

软链接不足之处是链接文件包含有原文件的路径信息，所以当原文件从一个目录下移到其他目录中，再访问链接文件，系统就找不到了文件了，而硬链接就没有这个缺陷，想怎么移就怎么移；还有它要系统分配额外的空间用于建立新的索引节点和保存原文件的路径。

### 题5：[Linux 中如何返回到切换目录之前的目录？](/docs/Linux/最新2021年Linux面试题及答案汇总版.md#题5linux-中如何返回到切换目录之前的目录)<br/>
Linux系统上切换目录使用cd（change directory）命令，查看当前目录使用pwd （print working directory）命令。

例如当前目录为:/home/Jingxuan/Tomcat

```shell
[root@iZ256w2hluuZ Tomcat]#  pwd
/home/Jingxuan/Tomcat
[root@iZ256w2hluuZ Tomcat]#  
```

进入到根目录，执行cd /命令。

```shell
[root@iZ256w2hluuZ Tomcat]#  cd /
[root@iZ256w2hluuZ /]# pwd
/
[root@iZ256w2hluuZ /]#
```

如果需要返回到/home/Jingxuan/Tomcat目录，则执行cd -命令。

```shell
[root@iZ256w2hluuZ /]# cd -
/home/Jingxuan/Tomcat
[root@iZ256w2hluuZ Tomcat]#  pwd
/home/Jingxuan/Tomcat
[root@iZ256w2hluuZ Tomcat]#  
```

### 题6：[Linux 中如何翻页查看大文件内容？](/docs/Linux/最新2021年Linux面试题及答案汇总版.md#题6linux-中如何翻页查看大文件内容)<br/>
通过管道将命令“cat file_name.txt”和“more”连接在一起可以实现翻页查看大文件内容。

```shell
[root@mrwang ~]# cat file_name.txt | more
```

### 题7：[Linux 如何切换用户？](/docs/Linux/最新2021年Linux面试题及答案汇总版.md#题7linux-如何切换用户)<br/>
Linux系统切换用户的命令是su

su的含义是(switch user)切换用户的缩写。

通过su命令，可以从普通用户切换到root用户，也可以从root用户切换到普通用户。

<font color="#dd0000">注：从普通用户切换到root用户需要密码，从root用户切换到普通用户不需要密码。</font>

使用SecureCRT工具连接终端，由普通用户切换到root用户。

在终端输入su命令然后回车，要求输入密码（linux终端输入的密码不显示）输入密码后回车进入root用户。

在终端输入su  root然后回车，也可以进入root用户或su - root回车，也可以切换root用户。针对su root与su - root区别之后的篇幅会阐述，欢迎关注微信公众号“Java精选”，送免费资料。

### 题8：[Linux 中如何查看指定目录的大小？](/docs/Linux/最新2021年Linux面试题及答案汇总版.md#题8linux-中如何查看指定目录的大小)<br/>
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

### 题9：[Linux 中如何查看运行日志？](/docs/Linux/最新2021年Linux面试题及答案汇总版.md#题9linux-中如何查看运行日志)<br/>
动态打印日志信息：tail –f 日志文件

```shell
[root@mrwang apache-tomcat-blog]# tail -f ./logs/catalina.out 
```

### 题10：[根据文件名搜索文件有哪些命令？](/docs/Linux/最新2021年Linux面试题及答案汇总版.md#题10根据文件名搜索文件有哪些命令)<br/>
find <指定目录> <指定条件> <指定动作>，

whereis 加参数与文件名

locate 只加文件名

### 题11：linux-中如何查看几颗物理-cpu-和每颗-cpu-的核数<br/>


### 题12：描述-linux-系统从开机到登陆界面的启动过程。<br/>


### 题13：linux-中如何重启网卡使配置生效<br/>


### 题14：linux-中如何实时查看网卡流量历史网卡流量<br/>


### 题15：linux-中如何启动和关闭防火墙<br/>


### 题16：linux-中什么是默认登录-shell-<br/>


### 题17：linux-中如何让命令后台运行<br/>


### 题18：linux-中使用什么命令查看磁盘占用情况<br/>


### 题19：bash-shell-中-hash-命令有什么作用<br/>


### 题20：linux-中为什么需要零拷贝<br/>


### 题21：linux-中如何查看网络连接状况<br/>


### 题22：命令中可以使用哪几种通配符<br/>


### 题23：linux-中如何关闭进程<br/>


### 题24：linux-中-shell-脚本如何增加注释<br/>


### 题25：linux-如何统计系统当前进程连接数<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")