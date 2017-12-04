参考：http://www.runoob.com/docker/docker-hello-world.html


----------
![这里写图片描述](http://img.blog.csdn.net/20171204100434318?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
```
docker run ubuntu:16.04 /bin/echo "Hello world"
# 或
docker run ubuntu:16.04 echo "Hello world"

# 结果
Hello world
```
各个参数解析：

 - docker: Docker 的二进制执行文件。
 
 - run:与前面的 docker 组合来运行一个容器。
 
 - ubuntu:16.04指定要运行的镜像，Docker首先从本地主机上查找镜像是否存在，如果不存在，Docker 就会从镜像仓库Docker Hub 下载公共镜像。
 
 - /bin/echo "Hello world": 在启动的容器里执行的命令

以上命令完整的意思可以解释为：Docker 以 ubuntu16.04 镜像创建一个新容器，然后在容器里执行 bin/echo "Hello world"，然后输出结果。


## 运行交互式的容器
我们通过docker的两个参数 -i -t，让docker运行的容器实现"对话"的能力

```
docker run -i -t ubuntu:16.04 /bin/bash
# 或
docker run -i -t ubuntu:16.04
```
各个参数解析：

 - -t:在新容器内指定一个伪终端或终端。
 - -i:允许你对容器内的标准输入 (STDIN) 进行交互。

此时我们已进入一个 ubuntu16.04系统的容器
我们尝试在容器中运行命令 cat /proc/version和ls分别查看当前系统的版本信息和当前目录下的文件列表

我们可以通过运行exit命令或者使用`CTRL+D`来退出容器。

## 启动容器（后台模式）

使用以下命令创建一个以进程方式运行的容器

```
docker run -d ubuntu:16.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
# 665ee7dd694f1a5e6d363ffeb9c9405131c0556b6ca4cc550b071975b7676956
```

在输出中，我们没有看到期望的"hello world"，而是一串长字符
665ee7dd694f1a5e6d363ffeb9c9405131c0556b6ca4cc550b071975b7676956
这个长字符串叫做容器ID，对每个容器来说都是唯一的，我们可以通过容器ID来查看对应的容器发生了什么。
首先，我们需要确认容器有在运行，可以通过 `docker ps` 来查看

```
docker ps # 查看正在运行的容器
```

![这里写图片描述](http://img.blog.csdn.net/20171204101412103?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 - CONTAINER ID:容器ID
 
 - NAMES:自动分配的容器名称 这里为`relaxed_pare`

在容器内使用`docker logs`命令，查看容器内的标准输出

```python
docker logs 665ee7dd694f
# 或
docker logs relaxed_pare
```
![这里写图片描述](http://www.runoob.com/wp-content/uploads/2016/05/docker23.png)

----------


## 停止容器
我们使用 `docker stop` 命令来停止容器:
```python
docker stop 665ee7dd694f
# 或者 
docker stop relaxed_pare
```
通过`docker ps`查看，容器已经停止工作:
