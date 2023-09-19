# 常见 Docker 面试题整理汇总附答案

### 全部面试题答案，更新日期：12月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Docker

### 题1：[Docker 中什么是 Image？](/docs/Docker/常见%20Docker%20面试题整理汇总附答案.md#题1docker-中什么是-image)<br/>
image即镜像。image相当于container的模板，container创建后里面有什么软件完全取决于它使用什么image。

image可以通过container创建，相当于把此时container的状态保存成快照，也可以通过Dockerfile来创建。其中通过Dockerfile创建的方法能让环境配置和代码一起被版本库一起管理。

### 题2：[生产环境中如何监控 Docker？](/docs/Docker/常见%20Docker%20面试题整理汇总附答案.md#题2生产环境中如何监控-docker)<br/>
1、Docker提供docker:stats和docker事件等工具来监控生产中的docker。我们可以使用这些命令获取重要统计数据的报告。

2、Docker统计数据：当我们使用容器ID调用docker stats时，我们获得容器的CPU，内存使用情况等。它类似于Linux中的top命令。

3、Docker事件：docker事件是一个命令，用于查看docker守护程序中正在进行的活动流。

一些常见的docker事件：attach、commit、die、detach、rename、destroy等。还可以使用各种选项来限制或过滤我们关注的事情。

### 题3：[Docker 如何临时退出正在交互容器终端？](/docs/Docker/常见%20Docker%20面试题整理汇总附答案.md#题3docker-如何临时退出正在交互容器终端)<br/>
使用快捷键Ctrl+p，后Ctrl+q，如果按Ctrl+c会使容器内的应用进程终止，进而会使容器终止。

### 题4：[构建 Docker 镜像应该遵循哪些原则？](/docs/Docker/常见%20Docker%20面试题整理汇总附答案.md#题4构建-docker-镜像应该遵循哪些原则)<br/>
整体原则上， 尽量保待镜像功能的明确和内容的精简， 避免添加额外文件和操作步骤， 要点包括：

- 尽量选取满足需求但较小的基础系统镜像， 例如大部分时候可以选择 debian:wheezy或debian:jessie镜像， 仅有不足百兆大小；

- 清理编译生成文件、 安装包的缓存等临时文件；

- 安装各个软件时候要指定准确的版本号， 并避免引入不需要的依赖；

- 从安全角度考虑， 应用要尽量使用系统的库和依赖；

- 如果安装应用时候需要配置一些特殊的环境变量， 在安装后要还原不需要保持的变量值；

- 使用Dockerfile创建镜像时候要添加.dockerignore文件或使用干净的工作目录；

- 区分编译环境容器和运行时环境容器， 使用多阶段镜像创建。

### 题5：[Docker 容器中如何启动 Nginx 服务？](/docs/Docker/常见%20Docker%20面试题整理汇总附答案.md#题5docker-容器中如何启动-nginx-服务)<br/>
启动nginx服务，端口映射随机，并挂载本地文件目录到容器html的命令。

```shell
docker run -d -P --name nginx2 -v /home/nginx:/usr/share/nginx/html nginx
```

### 题6：[什么是Docker Hub？](/docs/Docker/常见%20Docker%20面试题整理汇总附答案.md#题6什么是docker-hub)<br/>
Docker hub是一个基于云的注册表服务，允许您链接到代码存储库，构建镜像并测试它们，存储手动推送的镜像以及指向Docker云的链接，以便可以将镜像部署到主机。它为整个开发流程中的容器镜像发现，分发和变更管理，用户和团队协作以及工作流自动化提供了集中资源。

### 题7：[如何控制容器占用系统资源（CPU、内存）的份额？](/docs/Docker/常见%20Docker%20面试题整理汇总附答案.md#题7如何控制容器占用系统资源cpu内存的份额)<br/>
使用<code>docker [container] create</code>命令创建容器或使用<code>docker [con­tainer] run</code>创建并启动容器的时候，可以使用<code>-c | - cpu -shares[=O]</code>参数来调整容器使用CPU的权重；使用<code>-ml-memory[=MEMORY]</code>参数来调整容器使用内存的大小。


### 题8：[DockerFile中 COPY 和 ADD 命令有什么区别？](/docs/Docker/常见%20Docker%20面试题整理汇总附答案.md#题8dockerfile中-copy-和-add-命令有什么区别)<br/>
COPY指令和ADD指令的唯一区别在于是否支持从远程URL获取资源。

COPY指令只能从执行docker build所在的主机上读取资源并复制到镜像中，而ADD指令还支持通过URL从远程服务器读取资源并复制到镜像中。

### 题9：[Docker 中如何查看镜像支持环境变量？](/docs/Docker/常见%20Docker%20面试题整理汇总附答案.md#题9docker-中如何查看镜像支持环境变量)<br/>
**方式一：docker inspect**

通过docker inspect命令不仅能查看环境变量，还能查看容器其它相关信息，非常丰富，以Json格式输出。
```shell
docker inspect centos
```

可读性还可以，但也不算很高，可以通过grep命令过滤一下：
```shell
$ docker inspect centos | grep SERVER "SERVER_PORT=80"
```

或者可以解析一下Json文本：

```shell
$ docker inspect -f '{{range $index, $value := .Config.Env}}{{println $value}}{{end}}' centosPATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

**方式二：doecker exec env**

这种方式获取的环境变量就跟我们平时获取linux环境变量是一样的了。只是在容器跑了个env命令而已。如下：
```shell
$ docker exec centos envPATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/binHOSTNAME=f8b489603f31HOME=/root
```

### 题10：[Docker 容器和虚拟机有什么区别？](/docs/Docker/常见%20Docker%20面试题整理汇总附答案.md#题10docker-容器和虚拟机有什么区别)<br/>
**相同点**

1、容器和虚拟机一样，都会对物理硬件资源进行共享使用。

2、容器和虚拟机的生命周期比较相似（创建、运行、暂停、关闭等等）。

3、容器中或虚拟机中都可以安装各种应用，如redis、mysql、nginx等。也就是说，在容器中的操作，如同在一个虚拟机(操作系统)中操作一样。

4、同虚拟机一样，容器创建后，会存储在宿主机上：linux上位于/var/lib/docker/containers下

**不同点**

1、虚拟机的创建、启动和关闭都是基于一个完整的操作系统。一个虚拟机就是一个完整的操作系统。而容器直接运行在宿主机的内核上，其本质上以一系列进程的结合。

2、容器是轻量级的，虚拟机是重量级的。首先容器不需要额外的资源来管理(不需要Hypervisor、Guest OS)，虚拟机额外更多的性能消耗；其次创建、启动或关闭容器，如同创建、启动或者关闭进程那么轻松，而创建、启动、关闭一个操作系统就没那么方便了。

也因此，意味着在给定的硬件上能运行更多数量的容器，甚至可以直接把Docker运行在虚拟机上。

![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")