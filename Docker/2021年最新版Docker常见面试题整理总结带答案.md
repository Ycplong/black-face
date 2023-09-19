# 2021年最新版Docker常见面试题整理总结带答案

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Docker

### 题1：[Docker 中一个容器可以同时运行多个应用进程吗？](/docs/Docker/2021年最新版Docker常见面试题整理总结带答案.md#题1docker-中一个容器可以同时运行多个应用进程吗)<br/>
一般不推荐在用以容器内运行多个应用进程，如果有类似需求，可以用过额外的进程管理机制，比如supervisord来管理所运行的进程。

### 题2：[DockerFile中 COPY 和 ADD 命令有什么区别？](/docs/Docker/2021年最新版Docker常见面试题整理总结带答案.md#题2dockerfile中-copy-和-add-命令有什么区别)<br/>
COPY指令和ADD指令的唯一区别在于是否支持从远程URL获取资源。

COPY指令只能从执行docker build所在的主机上读取资源并复制到镜像中，而ADD指令还支持通过URL从远程服务器读取资源并复制到镜像中。

### 题3：[解释一下 dockerfile 的 ONBUILD 指令？](/docs/Docker/2021年最新版Docker常见面试题整理总结带答案.md#题3解释一下-dockerfile-的-onbuild-指令)<br/>
当镜像用作另一个镜像构建的基础时，ONBUILD指令向镜像添加将在稍后执行的触发指令。如果要构建将用作构建其他镜像的基础的镜像，这将非常有用。

例如，可以使用特定于用户的配置自定义的应用程序构建环境或守护程序。

### 题4：[非官方仓库下载镜像时，可能提示“Error：Invaild registry endpoint https://dl.docker.com:5000/v1/…”？](/docs/Docker/2021年最新版Docker常见面试题整理总结带答案.md#题4非官方仓库下载镜像时可能提示“error-invaild-registry-endpoint-https://dl.docker.com:5000/v1/…”)<br/>
非官方地址，例如：dl.dockerpool.com。

Docker自1.3.0版本之后，加强了对镜像安全性的验证，需要手动添加对非官方仓库的信任。 

```shell
DOCKER_OPTS=”–insecure-registry dl.dockerpool.com:5000” 
```
重启docker服务即可。

### 题5：[Docker 和 LXC 有什么区别？](/docs/Docker/2021年最新版Docker常见面试题整理总结带答案.md#题5docker-和-lxc-有什么区别)<br/>
LXC是在Linux上相关技术实现的容器，docker则在如下的几个方面进行了改进：

1、移植性：通过抽象容器配置，容器可以实现一个平台移植到另一个平台。

2、镜像系统：基于AUFS的镜像系统为容器的分发带来了很多的便利，通是共同的镜像层只需要存储一份，实现高效率的存储。

3、版本管理：类似于GIT的版本管理理念，用户可以更方便的创建、管理镜像文件。

4、仓库系统:仓库系统大大降低了镜像的分发和管理的成本。

5、周边工具：各种现有的工具（配置管理、云平台）对docker的支持，以及基于docker的pass、Cl等系统，让docker的应用更加方便和多样。

### 题6：[Docker 中什么是 Image？](/docs/Docker/2021年最新版Docker常见面试题整理总结带答案.md#题6docker-中什么是-image)<br/>
image即镜像。image相当于container的模板，container创建后里面有什么软件完全取决于它使用什么image。

image可以通过container创建，相当于把此时container的状态保存成快照，也可以通过Dockerfile来创建。其中通过Dockerfile创建的方法能让环境配置和代码一起被版本库一起管理。

### 题7：[Docker 环境如何迁移到另外宿主机？](/docs/Docker/2021年最新版Docker常见面试题整理总结带答案.md#题7docker-环境如何迁移到另外宿主机)<br/>
停止docker服务，之后将整个docker存储文件复制到另外一台宿主机上，然后调整另外一台宿主机的配置即可。

### 题8：[Docker 中什么是 Dockerfile？](/docs/Docker/2021年最新版Docker常见面试题整理总结带答案.md#题8docker-中什么是-dockerfile)<br/>
Dockerfile：用于创建image镜像的模板文件，出于管理和安全的考虑，docker官方建议所有的镜像文件应该由dockerfile来创建，而当前不少用户把docker当虚拟机来使用，甚至容器中安装SSH，从安全的角度，这是不恰当的。

### 题9：[Docker 中什么是 Registry？](/docs/Docker/2021年最新版Docker常见面试题整理总结带答案.md#题9docker-中什么是-registry)<br/>
registry即存放镜像的仓库。只要能连接到registry每个人都可以很方便地通过pull命令从仓库中获取镜像。

registry在github上有两份代码：老代码库和新代码库。老代码是采用python编写的，存在pull和push的性能问题，出到0.9.1版本之后就标志为deprecated，不再继续开发。从2.0版本开始就到在新代码库进行开发，新代码库是采用go语言编写，修改了镜像id的生成算法、registry上镜像的保存结构，大大优化了pull和push镜像的效率。

官方在Docker hub上提供了registry的镜像，可以直接使用registry镜像来构建一个容器，搭建自己的私有仓库服务。

docker默认使用的仓库是docker hub，国内可以使用DaoCloud来建立Mirror连接到docker hub，进而加快获取image的速度。

### 题10：[Docker 中本地镜像文件一般存放在什么位置？](/docs/Docker/2021年最新版Docker常见面试题整理总结带答案.md#题10docker-中本地镜像文件一般存放在什么位置)<br/>
Docker相关的本地资源存在/var/lib/docker/目录下。

其中container目录存放容器信息，graph目录存放镜像信息，aufs目录下存放具体的镜像底层文件。

### 题11：docker-需要查询日志应该使用什么命令<br/>


### 题12：docker-中如何控制容器占用系统资源情况<br/>


### 题13：开发环境中-docker-与-vagrant-该如何选择<br/>


### 题14：如何临时退出一个正在交互的容器的终端-而不终止它<br/>


### 题15：docker-中如何查看镜像支持环境变量<br/>


### 题16：什么是docker-hub<br/>


### 题17：docker-容器如何运行在非linux系统<br/>


### 题18：docker-安全吗-<br/>


### 题19：docker-和-vagrant-有什么区别<br/>


### 题20：什么是-docker-镜像<br/>


### 题21：docker-容器和主机之间如何复制数据<br/>


### 题22：docker-如何临时退出正在交互容器终端<br/>


### 题23：如何停止所有正在运行的容器<br/>


### 题24：ci持续集成服务器的功能是什么<br/>


### 题25：如何控制容器占用系统资源cpu内存的份额<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")