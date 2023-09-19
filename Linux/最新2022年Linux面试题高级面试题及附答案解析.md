# 最新2022年Linux面试题高级面试题及附答案解析

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Linux

### 题1：[Linux 中如何快速切换到上 N 级目录？](/docs/Linux/最新2022年Linux面试题高级面试题及附答案解析.md#题1linux-中如何快速切换到上-n-级目录)<br/>
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

### 题2：[Shell 脚本是什么？](/docs/Linux/最新2022年Linux面试题高级面试题及附答案解析.md#题2shell-脚本是什么)<br/>
一个Shell脚本可以理解为一个文本文件，它包含一个或多个命令。

比如作为系统管理员，经常需要使用多个命令来完成一项任务，那么就可以可以将这些命令添加在一个文本文件中（Shell脚本），来完成这些日常的工作任务。

Shell是一种脚本语言，那么就必须有解释器来执行这些脚本，常见的脚本解释器如下：

**bash**

bash脚本解释器是Linux标准默认的shell。

bash由Brian Fox和Chet Ramey共同完成，是BourneAgain Shell的缩写，内部命令一共有40个。

**sh**

sh脚本解释器是由Steve Bourne开发，是Bourne Shell的缩写，sh是Unix标准默认的shell。

另外还有：ash、csh、ksh等。

### 题3：[Linux 中如何进入含有空格的目录？](/docs/Linux/最新2022年Linux面试题高级面试题及附答案解析.md#题3linux-中如何进入含有空格的目录)<br/>
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

### 题4：[Linux 中如何切换到上 N 级目录？](/docs/Linux/最新2022年Linux面试题高级面试题及附答案解析.md#题4linux-中如何切换到上-n-级目录)<br/>
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

### 题5：[Linux 中如何翻页查看大文件内容？](/docs/Linux/最新2022年Linux面试题高级面试题及附答案解析.md#题5linux-中如何翻页查看大文件内容)<br/>
通过管道将命令“cat file_name.txt”和“more”连接在一起可以实现翻页查看大文件内容。

```shell
[root@mrwang ~]# cat file_name.txt | more
```

### 题6：[Linux 中如何查看运行日志？](/docs/Linux/最新2022年Linux面试题高级面试题及附答案解析.md#题6linux-中如何查看运行日志)<br/>
动态打印日志信息：tail –f 日志文件

```shell
[root@mrwang apache-tomcat-blog]# tail -f ./logs/catalina.out 
```

### 题7：[Linux 中如何查看几颗物理 CPU 和每颗 CPU 的核数？](/docs/Linux/最新2022年Linux面试题高级面试题及附答案解析.md#题7linux-中如何查看几颗物理-cpu-和每颗-cpu-的核数)<br/>
```shell
[root@JingXuan-Java ~]# cat /proc/cpuinfo|grep -c 'physical id'
4
[root@JingXuan-Java ~]# cat /proc/cpuinfo|grep -c 'processor'
4
```

### 题8：[Linux 中如何创建软链接？](/docs/Linux/最新2022年Linux面试题高级面试题及附答案解析.md#题8linux-中如何创建软链接)<br/>
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

### 题9：[Linux 中如何让命令后台运行？](/docs/Linux/最新2022年Linux面试题高级面试题及附答案解析.md#题9linux-中如何让命令后台运行)<br/>
一般都是使用&在命令结尾来让程序自动运行。（命令后可以不追加空格）

举例：执行jingxuan.jar包，使其后台运行，退出连接端后不会终止进程。

```shell
java -jar jingxuan.jar &
```


### 题10：[Linux 中如何查看指定目录的大小？](/docs/Linux/最新2022年Linux面试题高级面试题及附答案解析.md#题10linux-中如何查看指定目录的大小)<br/>
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

### 题11：vmstat-命令中-rbsisobibo-这几列表示什么含义<br/>


### 题12：linux-中如何实时查看网卡流量历史网卡流量<br/>


### 题13：linux-中使用什么命令查看磁盘占用情况<br/>


### 题14：linux-中查看文件内容有哪些命令<br/>


### 题15：查看文件内容有哪些命令<br/>


### 题16：linux-中如何查看系统负载数值表示什么含义<br/>


### 题17：linux-中如何查看网络连接状况<br/>


### 题18：linux-中如何启动和关闭防火墙<br/>


### 题19：bash-shell-中-hash-命令有什么作用<br/>


### 题20：linux-中-ll-和-ls-命令有什么区别<br/>


### 题21：linux-中如何创建硬链接<br/>


### 题22：bash-shell-中-hash-命令有什么作用<br/>


### 题23：linux-中-shell-脚本如何增加注释<br/>


### 题24：生产场景如何对-linux-系统进行合理规划分区<br/>


### 题25：根据文件名搜索文件有哪些命令<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")