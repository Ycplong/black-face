# 最新面试题2021年常见Docker面试题及答案汇总

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Docker

### 题1：[Docker 镜像和层有什么区别？](/docs/Docker/最新面试题2021年常见Docker面试题及答案汇总.md#题1docker-镜像和层有什么区别)<br/>
Docker镜像是由一系列只读层构建的，而每个层代表Dockerfile中的一条指令。

### 题2：[什么是 Docker Swarm？](/docs/Docker/最新面试题2021年常见Docker面试题及答案汇总.md#题2什么是-docker-swarm)<br/>
Docker Swarm是docker的本地群集。

Docker Swarm将docker主机池转变为单个虚拟docker主机。

Docjer Swarm提供标准的docker API，任何已经与docker守护进程通信的工具都可以使用Swarm透明地扩展到多个主机。

### 题3：[Docker 中什么是 Dockerfile？](/docs/Docker/最新面试题2021年常见Docker面试题及答案汇总.md#题3docker-中什么是-dockerfile)<br/>
Dockerfile：用于创建image镜像的模板文件，出于管理和安全的考虑，docker官方建议所有的镜像文件应该由dockerfile来创建，而当前不少用户把docker当虚拟机来使用，甚至容器中安装SSH，从安全的角度，这是不恰当的。

### 题4：[Docker 中仓库、注册服务器、注册索引有什么联系？](/docs/Docker/最新面试题2021年常见Docker面试题及答案汇总.md#题4docker-中仓库注册服务器注册索引有什么联系)<br/>
仓库（Repository）是存放一组关联镜像的集合，比如同一个应用的不同版本的镜像。

注册服务器（Registry）是存放实际的镜像的地方。

注册索引（Index）则负责维护用户的账号、权限、搜索、标签等管理。

注册服务器利用注册索引来实现认证等管理。

### 题5：[非官方仓库下载镜像时，可能提示“Error：Invaild registry endpoint https://dl.docker.com:5000/v1/…”？](/docs/Docker/最新面试题2021年常见Docker面试题及答案汇总.md#题5非官方仓库下载镜像时可能提示“error-invaild-registry-endpoint-https://dl.docker.com:5000/v1/…”)<br/>
非官方地址，例如：dl.dockerpool.com。

Docker自1.3.0版本之后，加强了对镜像安全性的验证，需要手动添加对非官方仓库的信任。 

```shell
DOCKER_OPTS=”–insecure-registry dl.dockerpool.com:5000” 
```
重启docker服务即可。

### 题6：[如何停止所有正在运行的容器？](/docs/Docker/最新面试题2021年常见Docker面试题及答案汇总.md#题6如何停止所有正在运行的容器)<br/>
可以使用<code>docker [container] stop $(docker ps -q)</code>命令。

### 题7：[Docker 和 LXC 有什么区别？](/docs/Docker/最新面试题2021年常见Docker面试题及答案汇总.md#题7docker-和-lxc-有什么区别)<br/>
LXC是在Linux上相关技术实现的容器，docker则在如下的几个方面进行了改进：

1、移植性：通过抽象容器配置，容器可以实现一个平台移植到另一个平台。

2、镜像系统：基于AUFS的镜像系统为容器的分发带来了很多的便利，通是共同的镜像层只需要存储一份，实现高效率的存储。

3、版本管理：类似于GIT的版本管理理念，用户可以更方便的创建、管理镜像文件。

4、仓库系统:仓库系统大大降低了镜像的分发和管理的成本。

5、周边工具：各种现有的工具（配置管理、云平台）对docker的支持，以及基于docker的pass、Cl等系统，让docker的应用更加方便和多样。

### 题8：[什么是容器？什么是 Docker？](/docs/Docker/最新面试题2021年常见Docker面试题及答案汇总.md#题8什么是容器什么是-docker)<br/>
容器是一种轻量级、可移植、自包含的软件打包技术，使应用程序可以在几乎任何地方以相同的方式运行。开发人员在自己笔记本上创建并测试好的容器，无需任何修改就能够在生产系统的虚拟机、物理服务器或公有云主机上运行。

Docker容器包括应用程序及所有的依赖项，作为操作系统的独立进程运行。Docker是容器的一种，还有其他容器，比如CoreOS rkt等。

### 题9：[DockerFile 中最常见指令有哪些?](/docs/Docker/最新面试题2021年常见Docker面试题及答案汇总.md#题9dockerfile-中最常见指令有哪些?)<br/>
|指令|说明|
|-|-|
|FROM|指定基础镜像|
|LABEL|功能为镜像指定标签|
|RUN|运行指定命令|
|CMD|容器启动时要运行的命令|

### 题10：[如何临时退出一个正在交互的容器的终端， 而不终止它？](/docs/Docker/最新面试题2021年常见Docker面试题及答案汇总.md#题10如何临时退出一个正在交互的容器的终端-而不终止它)<br/>
按<code>ctrl-p Ctrl-q</code>。

如果按<code>ctrl-c</code>往往会让容器内应用进程终止， 进而会终止容器。

### 题11：如何批量清理临时镜像文件<br/>


### 题12：什么是docker-hub<br/>


### 题13：什么是-docker-镜像<br/>


### 题14：如何删除所有本地的镜像<br/>


### 题15：docker-如何临时退出正在交互容器终端<br/>


### 题16：docker-和-vagrant-有什么区别<br/>


### 题17：docker-中如何批量清理容器和镜像文件<br/>


### 题18：docker-中什么是-image<br/>


### 题19：什么是-docker-容器<br/>


### 题20：docker-容器和虚拟机有什么区别<br/>


### 题21：docker-中一个容器可以同时运行多个应用进程吗<br/>


### 题22：生产环境中如何监控-docker<br/>


### 题23：docker-中都有哪些常用命令<br/>


### 题24：docker-容器和主机之间如何复制数据<br/>


### 题25：docker-中什么是-registry<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")