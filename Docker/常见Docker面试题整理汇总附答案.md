# 常见Docker面试题整理汇总附答案

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Docker

### 题1：[Docker 中如何批量清理容器和镜像文件？](/docs/Docker/常见Docker面试题整理汇总附答案.md#题1docker-中如何批量清理容器和镜像文件)<br/>
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

### 题2：[Docker 镜像和层有什么区别？](/docs/Docker/常见Docker面试题整理汇总附答案.md#题2docker-镜像和层有什么区别)<br/>
Docker镜像是由一系列只读层构建的，而每个层代表Dockerfile中的一条指令。

### 题3：[Docker 中什么是 Dockerfile？](/docs/Docker/常见Docker面试题整理汇总附答案.md#题3docker-中什么是-dockerfile)<br/>
Dockerfile：用于创建image镜像的模板文件，出于管理和安全的考虑，docker官方建议所有的镜像文件应该由dockerfile来创建，而当前不少用户把docker当虚拟机来使用，甚至容器中安装SSH，从安全的角度，这是不恰当的。

### 题4：[Docker 需要查询日志应该使用什么命令？](/docs/Docker/常见Docker面试题整理汇总附答案.md#题4docker-需要查询日志应该使用什么命令)<br/>
```shell
docker logs
```

### 题5：[如何清理 Docker 系统中的无用数据？](/docs/Docker/常见Docker面试题整理汇总附答案.md#题5如何清理-docker-系统中的无用数据)<br/>
可以使用<code>docker system prune --volumes -f</code>命令， 这个命令会自动清理处于停止状态的容器、 无用的网络和挂载卷、 临时镜像和创建镜像缓存。

### 题6：[非官方仓库下载镜像时，可能提示“Error：Invaild registry endpoint https://dl.docker.com:5000/v1/…”？](/docs/Docker/常见Docker面试题整理汇总附答案.md#题6非官方仓库下载镜像时可能提示“error-invaild-registry-endpoint-https://dl.docker.com:5000/v1/…”)<br/>
非官方地址，例如：dl.dockerpool.com。

Docker自1.3.0版本之后，加强了对镜像安全性的验证，需要手动添加对非官方仓库的信任。 

```shell
DOCKER_OPTS=”–insecure-registry dl.dockerpool.com:5000” 
```
重启docker服务即可。

### 题7：[Docker 中如何查看输出和日志信息？](/docs/Docker/常见Docker面试题整理汇总附答案.md#题7docker-中如何查看输出和日志信息)<br/>
当要查看一个docker容器的日志时，可以直接使用
```shell
docker logs 容器名字或者ID。
```
如果需要找其中包含某些内容（如xxx）的所有行，可以使用
```shell
docker logs 容器名字或者 ID 2>&1 | grep xxx
```
这里的2>&1代表把标准错误（文件描述符2）重定向（>）到标准输出（文件描述符 1）的位置（&）。

如果需要导出日志文件，可以使用
```java
# grep 的 -i 表示不区分大小写 
docker inspect 容器名字或者 ID | grep -i logpath
```

### 题8：[什么是Docker Hub？](/docs/Docker/常见Docker面试题整理汇总附答案.md#题8什么是docker-hub)<br/>
Docker hub是一个基于云的注册表服务，允许您链接到代码存储库，构建镜像并测试它们，存储手动推送的镜像以及指向Docker云的链接，以便可以将镜像部署到主机。它为整个开发流程中的容器镜像发现，分发和变更管理，用户和团队协作以及工作流自动化提供了集中资源。

### 题9：[如何更改 Docker 的默认存储设置？](/docs/Docker/常见Docker面试题整理汇总附答案.md#题9如何更改-docker-的默认存储设置)<br/>
Docker的默认存放位置是/var/lib/docker，如果希望将Docker的本地文件存储到其他分区，可以使用Linux软连接的方式来实现。

### 题10：[Docker 如何临时退出正在交互容器终端？](/docs/Docker/常见Docker面试题整理汇总附答案.md#题10docker-如何临时退出正在交互容器终端)<br/>
使用快捷键Ctrl+p，后Ctrl+q，如果按Ctrl+c会使容器内的应用进程终止，进而会使容器终止。

### 题11：docker-环境如何迁移到另外宿主机<br/>


### 题12：docker-容器和主机之间如何复制数据<br/>


### 题13：容器与主机之间的数据拷贝命令是什么<br/>


### 题14：什么是-docker-容器<br/>


### 题15：解释一下-dockerfile-的-onbuild-指令<br/>


### 题16：docker-安全吗-<br/>


### 题17：docker-容器和虚拟机有什么区别<br/>


### 题18：什么是-docker-swarm<br/>


### 题19：docker-容器有几种状态<br/>


### 题20：docker-中什么是-image<br/>


### 题21：容器退出后-通过-docker-ps-命令查看不到数据会丢失么<br/>


### 题22：docker-中都有哪些常用命令<br/>


### 题23：docker-容器如何运行在非linux系统<br/>


### 题24：什么是容器什么是-docker<br/>


### 题25：docker-中什么是-registry<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")