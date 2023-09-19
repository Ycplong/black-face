# 最新面试题2021年Docker面试题及答案汇总

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Docker

### 题1：[Docker 中一个容器可以同时运行多个应用进程吗？](/docs/Docker/最新面试题2021年Docker面试题及答案汇总.md#题1docker-中一个容器可以同时运行多个应用进程吗)<br/>
一般不推荐在用以容器内运行多个应用进程，如果有类似需求，可以用过额外的进程管理机制，比如supervisord来管理所运行的进程。

### 题2：[如何停止所有正在运行的容器？](/docs/Docker/最新面试题2021年Docker面试题及答案汇总.md#题2如何停止所有正在运行的容器)<br/>
可以使用<code>docker [container] stop $(docker ps -q)</code>命令。

### 题3：[如何清理 Docker 系统中的无用数据？](/docs/Docker/最新面试题2021年Docker面试题及答案汇总.md#题3如何清理-docker-系统中的无用数据)<br/>
可以使用<code>docker system prune --volumes -f</code>命令， 这个命令会自动清理处于停止状态的容器、 无用的网络和挂载卷、 临时镜像和创建镜像缓存。

### 题4：[Docker 中什么是 Container？](/docs/Docker/最新面试题2021年Docker面试题及答案汇总.md#题4docker-中什么是-container)<br/>
container即容器。可以把每个container看做是一个独立的主机。 

container的创建通常有一个image作为其模板。类比成虚拟机的话可以理解为image就是虚拟机的镜像，而container就是一个个正在运行的虚拟机。一个虚拟机镜像可以创建出多个运行的虚拟主机且相互独立。

注意：container一旦创建如果没有用rm命令移除，将会一直存在，因此在不使用的情况下需要手动删除。

### 题5：[Docker 和 Vagrant 有什么区别？](/docs/Docker/最新面试题2021年Docker面试题及答案汇总.md#题5docker-和-vagrant-有什么区别)<br/>
Docker和Vagrant的定位完全不同。

Vagrant类似于Boot2Docker（一款运行Docker的最小内核），是一套虚拟机的管理环境，Vagrant可以在多种系统上和虚拟机软件中运行，可以在Windows、Mac等非Linux平台上为Docker支持，自身具有较好的包装性和移植性。

原生Docker自身只能运行在Linux平台上，但启动和运行的性能比虚拟机要快，往往更适合快速开发和部署应用的场景。


### 题6：[Docker 需要查询日志应该使用什么命令？](/docs/Docker/最新面试题2021年Docker面试题及答案汇总.md#题6docker-需要查询日志应该使用什么命令)<br/>
```shell
docker logs
```

### 题7：[DevOps 有哪些优势？](/docs/Docker/最新面试题2021年Docker面试题及答案汇总.md#题7devops-有哪些优势)<br/>
技术优势：

- 持续的软件交付
- 修复不太复杂的问题
- 更快地解决问题

商业利益：

- 更快速地传递功能
- 更稳定的操作环境
- 有更多时间可以增加价值（而不是修复/维护）

### 题8：[如何获取某个容器的 PIO 信息？](/docs/Docker/最新面试题2021年Docker面试题及答案汇总.md#题8如何获取某个容器的-pio-信息)<br/>
可以使用<code>docker [container] inspect --format ' {{ . State.Pid }} '< CONTAINER ID or NAME></code>命令。

### 题9：[Docker 中如何批量清理容器和镜像文件？](/docs/Docker/最新面试题2021年Docker面试题及答案汇总.md#题9docker-中如何批量清理容器和镜像文件)<br/>
1、清理所有已经停止的容器
```shell
docker rm $(docker ps -a -q)
```
2、清理所有镜像
```shell
docker rmi $(docker images -q)
```
3、强制清理所有镜像
```shell
docker rmi -f $(docker images -q)
```
4、清理过滤出来的镜像
```shell
docker rmi $(docker images | grep "关键字" | awk '{print $3}')

### 题10：[Docker 容器和虚拟机有什么区别？](/docs/Docker/最新面试题2021年Docker面试题及答案汇总.md#题10docker-容器和虚拟机有什么区别)<br/>
**相同点**

1、容器和虚拟机一样，都会对物理硬件资源进行共享使用。

2、容器和虚拟机的生命周期比较相似（创建、运行、暂停、关闭等等）。

3、容器中或虚拟机中都可以安装各种应用，如redis、mysql、nginx等。也就是说，在容器中的操作，如同在一个虚拟机(操作系统)中操作一样。

4、同虚拟机一样，容器创建后，会存储在宿主机上：linux上位于/var/lib/docker/containers下

**不同点**

1、虚拟机的创建、启动和关闭都是基于一个完整的操作系统。一个虚拟机就是一个完整的操作系统。而容器直接运行在宿主机的内核上，其本质上以一系列进程的结合。

2、容器是轻量级的，虚拟机是重量级的。首先容器不需要额外的资源来管理(不需要Hypervisor、Guest OS)，虚拟机额外更多的性能消耗；其次创建、启动或关闭容器，如同创建、启动或者关闭进程那么轻松，而创建、启动、关闭一个操作系统就没那么方便了。

也因此，意味着在给定的硬件上能运行更多数量的容器，甚至可以直接把Docker运行在虚拟机上。

### 题11：docker-中什么是-image<br/>


### 题12：docker-环境如何迁移到另外宿主机<br/>


### 题13：docker-如何临时退出正在交互容器终端<br/>


### 题14：docker-中本地镜像文件一般存放在什么位置<br/>


### 题15：什么是-docker<br/>


### 题16：如何备份系统中所有的镜像<br/>


### 题17：docker-中如何查看镜像支持环境变量<br/>


### 题18：构建-docker-镜像应该遵循哪些原则<br/>


### 题19：docker-和-lxc-有什么区别<br/>


### 题20：ci持续集成服务器的功能是什么<br/>


### 题21：docker-容器中如何启动-nginx-服务<br/>


### 题22：如何更改-docker-的默认存储设置<br/>


### 题23：容器与主机之间的数据拷贝命令是什么<br/>


### 题24：docker-容器有几种状态<br/>


### 题25：docker的配置文件放在什么位置如何修改配置<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")