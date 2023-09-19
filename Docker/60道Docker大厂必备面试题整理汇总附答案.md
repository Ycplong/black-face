# 60道Docker大厂必备面试题整理汇总附答案

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Docker

### 题1：[Docker 安全吗？ ](/docs/Docker/60道Docker大厂必备面试题整理汇总附答案.md#题1docker-安全吗-)<br/>
Docker利用Linux内核中很多安全特性来保证不同容器之间的隔离，并且通过签名机制来对镜像进行验证。

大量生产环境的部署证明，Docker虽然隔离性无法与虚拟机相比，但仍然具有极高的安全性。

### 题2：[什么是 Docker？](/docs/Docker/60道Docker大厂必备面试题整理汇总附答案.md#题2什么是-docker)<br/>
Docker是一个容器化平台，它以容器的形式将你的应用程序及所有的依赖项打包在一起，以确保你的应用程序在任何环境中无缝运行。

到目前为止，Docker看起来还很像一个典型的Linux虚拟化栈。实际上，Docker镜像的第二层是root文件系统rootfs，它位于引导文件系统之上。rootfs可以是一种或多种操作系统（如Debian或者 Ubuntu文件系统）。

在传统的Linux 引导过程中，root文件系统会最先以只读的方式加载，当引导结束并完成了完整性检查之后，它才会被切换为读写模式。但是在Docker中root文件系统永远只能是只读状态，并且Docker利用联合加载（union mount）技术又会在root文件系统层加载更多的只读文件系统。联合加载指的是一次同时加载多个文件系统，但是在外面看起来只能看到一个文件系统。联合加载会将各层文件系统叠加到一起，这样最终的文件系统会包含所有底层的文件和目录。

Docker将这样的文件系统称为镜像。一个镜像可以放到另一个镜像的顶部。位于下面的镜像称为父镜像（parent image），可以依次类推，直到镜像栈的最底部，最底部的镜像称为基础镜像（base image）。最后，当从一个镜像启动容器时，Docker会在该镜像的最顶层加载一个读写文件系统。如果想在Docker中运行的程序就是在这个读写层中执行的。

### 题3：[Docker 如何临时退出正在交互容器终端？](/docs/Docker/60道Docker大厂必备面试题整理汇总附答案.md#题3docker-如何临时退出正在交互容器终端)<br/>
使用快捷键Ctrl+p，后Ctrl+q，如果按Ctrl+c会使容器内的应用进程终止，进而会使容器终止。

### 题4：[Docker 中一个容器可以同时运行多个应用进程吗？](/docs/Docker/60道Docker大厂必备面试题整理汇总附答案.md#题4docker-中一个容器可以同时运行多个应用进程吗)<br/>
一般不推荐在用以容器内运行多个应用进程，如果有类似需求，可以用过额外的进程管理机制，比如supervisord来管理所运行的进程。

### 题5：[如何备份系统中所有的镜像？](/docs/Docker/60道Docker大厂必备面试题整理汇总附答案.md#题5如何备份系统中所有的镜像)<br/>
备份镜像列表可以使用命令：
```shell
docker images | awk 'NR>l{prin七$1":"$2} ' | sort > images.list
```

导出所有镜像为当前目录下文件， 可以使用如下命令：

```shell
while read img; do 
echo $img 
file="${img/\//-}" 
sudo docker save --output $file. tar $img 
done< images.list
```

将本地镜像文件导入为Docker镜像：

```shell
while read img; do 
echo $img 
file="${img/\//-}" 
docker load< $file.tar 
done< images.list
```

### 题6：[如何控制容器占用系统资源（CPU、内存）的份额？](/docs/Docker/60道Docker大厂必备面试题整理汇总附答案.md#题6如何控制容器占用系统资源cpu内存的份额)<br/>
使用<code>docker [container] create</code>命令创建容器或使用<code>docker [con­tainer] run</code>创建并启动容器的时候，可以使用<code>-c | - cpu -shares[=O]</code>参数来调整容器使用CPU的权重；使用<code>-ml-memory[=MEMORY]</code>参数来调整容器使用内存的大小。


### 题7：[什么是 Docker 容器？](/docs/Docker/60道Docker大厂必备面试题整理汇总附答案.md#题7什么是-docker-容器)<br/>
Docker容器是一个开源的应用容器引擎，让开发者可以以统一的方式打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何安装了docker引擎的服务器上（包括流行的Linux机器、windows机器），也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口（类似iPhone的app）。

几乎没有性能开销,可以很容易地在机器和数据中心中运行。最重要的是,他们不依赖于任何语言、框架包括系统。

### 题8：[Docker 环境如何迁移到另外宿主机？](/docs/Docker/60道Docker大厂必备面试题整理汇总附答案.md#题8docker-环境如何迁移到另外宿主机)<br/>
停止docker服务，之后将整个docker存储文件复制到另外一台宿主机上，然后调整另外一台宿主机的配置即可。

### 题9：[什么是Docker Hub？](/docs/Docker/60道Docker大厂必备面试题整理汇总附答案.md#题9什么是docker-hub)<br/>
Docker hub是一个基于云的注册表服务，允许您链接到代码存储库，构建镜像并测试它们，存储手动推送的镜像以及指向Docker云的链接，以便可以将镜像部署到主机。它为整个开发流程中的容器镜像发现，分发和变更管理，用户和团队协作以及工作流自动化提供了集中资源。

### 题10：[生产环境中如何监控 Docker？](/docs/Docker/60道Docker大厂必备面试题整理汇总附答案.md#题10生产环境中如何监控-docker)<br/>
1、Docker提供docker:stats和docker事件等工具来监控生产中的docker。我们可以使用这些命令获取重要统计数据的报告。

2、Docker统计数据：当我们使用容器ID调用docker stats时，我们获得容器的CPU，内存使用情况等。它类似于Linux中的top命令。

3、Docker事件：docker事件是一个命令，用于查看docker守护程序中正在进行的活动流。

一些常见的docker事件：attach、commit、die、detach、rename、destroy等。还可以使用各种选项来限制或过滤我们关注的事情。

### 题11：如何停止所有正在运行的容器<br/>


### 题12：非官方仓库下载镜像时可能提示“error-invaild-registry-endpoint-https://dl.docker.com:5000/v1/…”<br/>


### 题13：docker-容器和主机之间如何复制数据<br/>


### 题14：如何获取某个容器的-pio-信息<br/>


### 题15：dockerfile-中最常见指令有哪些?<br/>


### 题16：docker-中什么是-image<br/>


### 题17：docker-中仓库注册服务器注册索引有什么联系<br/>


### 题18：docker-中如何控制容器占用系统资源情况<br/>


### 题19：docker的配置文件放在什么位置如何修改配置<br/>


### 题20：什么是容器什么是-docker<br/>


### 题21：容器与主机之间的数据拷贝命令是什么<br/>


### 题22：docker-中如何批量清理容器和镜像文件<br/>


### 题23：如何更改-docker-的默认存储设置<br/>


### 题24：ci持续集成服务器的功能是什么<br/>


### 题25：如何删除所有本地的镜像<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")