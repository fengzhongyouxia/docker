参考：
1、https://docs.docker.com/
2、http://wiki.jikexueyuan.com/project/docker/
3、http://www.runoob.com/docker/docker-tutorial.html

----------
[toc]

# 第1部分：定位和设置
Docker的价值在于它如何构建，发布和运行应用程序; 对于你的应用程序实际上做什么是完全不可知的。

## Prerequisites
IP地址和端口
虚拟机
编辑配置文件
基本熟悉代码依赖性和构建的思想
机器资源使用条款，如CPU百分比，RAM使用的字节数等

## 容器的简要说明
**image(镜像)** 是一个轻量级的，独立的可执行程序包，包含运行一个软件所需的所有东西，包括代码，运行时，库，环境变量和配置文件。

**container （容器）**是镜像的运行时实例 - image在实际执行时在内存中变成的内容。 默认情况下，它与主机环境完全隔离，只有在配置时才访问主机文件和端口。

容器在主机的内核上本地运行应用程序。 它们比只能通过管理程序虚拟访问主机资源的虚拟机具有更好的性能特征。 容器可以获得本地访问权限，每个容器都以独立的进程运行，不会比其他可执行文件更多的内存。

## 容器与虚拟机器
考虑将虚拟机与容器进行比较的图表：

虚拟机图

![这里写图片描述](https://www.docker.com/sites/default/files/VM@2x.png)

虚拟机运行客户操作系统 - 注意每个框中的操作系统层。 这是资源密集型的，产生的磁盘映像和应用程序状态是操作系统设置，系统安装的依赖关系，操作系统安全补丁和其他易于丢失，难以复制的ep ent的纠缠。

容器图
![这里写图片描述](https://www.docker.com/sites/default/files/Container@2x.png)

容器可以共享一个内核，而唯一需要在容器镜像中的信息是可执行文件及其包依赖关系，而这些依赖关系永远不需要安装在主机系统上。 这些进程像本地进程一样运行，你可以像docker ps一样运行命令来单独管理它们，就像在Linux上运行ps来查看活动进程一样。 最后，因为它们包含了所有的依赖关系，所以没有配置纠缠。 一个集装箱化的应用程序“随处运行”。

## Setup
在我们开始之前，请确保您的系统安装了最新版本的Docker。
[Install Docker](https://docs.docker.com/engine/installation/)

注意：您可能需要将您的用户添加到`docker `组，以便在不使用sudo的情况下调用此命令。 [阅读更多](https://docs.docker.com/engine/installation/linux/linux-postinstall/)


注意：如果您的设置中存在网络问题，`docker run hello-world`可能无法成功执行。 如果您在代理服务器后面，并且怀疑它阻止了连接，请检查教程的[下一部分](https://docs.docker.com/get-started/part2/)。

```python
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
...(snipped)...
```

现在也是确保使用1.13或更高版本的好时机。 运行`docker --version`检查它。

```
$ docker --version
Docker version 17.05.0-ce-rc1, build 2878a85
```

## 结论

规模单位是一个个体，可移植的可执行文件具有广泛的含义。 这意味着CI / CD可以将更新推送到分布式应用程序的任何部分，系统依赖性不成问题，资源密度也会增加。 缩放行为的编排是关键在于新的可执行文件，而不是新的VM主机。

我们将学习所有这些东西，但首先让我们学会走路。


----------
# 第2部分：容器

## Prerequisites
安装Docker版本1.13或更高版本。
阅读第一部分的方向。
给你的环境一个快速的测试运行，以确保你所有的设置：

```
docker run hello-world
```

提示：请使用最新的Docker版本和客户端来帮助教程顺利进行。 例如，像Docker Toolbox这样的传统应用程序可能会为本地IP地址带来意想不到的结果，而较早版本的Docker并不支持这里演示的所有功能。

## Introduction

## 你的新开发环境
在过去，如果你要开始编写一个Python应用程序，你的第一步就是在你的机器上安装一个Python运行库。 但是，这会造成您的机器上的环境必须如此以使您的应用程序按预期运行。 同上运行你的应用程序的服务器。

使用Docker，您可以将一个可移植的Python运行时作为一个映像来获取，无需安装。 然后，您的构建可以将基础Python图像与应用程序代码一起包括在内，确保您的应用程序，依赖项和运行时都一起旅行。

这些可移植的图像是由一个叫做Dockerfile的东西来定义的。

## 用Dockerfile定义一个容器
Dockerfile将定义你的容器内的环境中发生了什么。 访问像网络接口和磁盘驱动器这样的资源是在这个环境中虚拟化的，这个环境与系统的其他部分是隔离的，所以你必须把端口映射到外部世界，并且明确你要“复制”到哪个文件 那个环境。 但是，这样做后，你可以期望在这个Dockerfile中定义的应用程序的构建在任何运行的地方都将表现完全相同。

### Dockerfile
创建一个空目录。 将目录（cd）更改为新目录，创建一个名为`Dockerfile`的文件，将以下内容复制并粘贴到该文件中并保存。 记下解释新Dockerfile中每条语句的注释。

```python
# Use an official Python runtime as a parent image
# 使用官方的Python运行时作为父镜像
FROM python:2.7-slim

# Set the working directory to /app
# 将工作目录设置为/app
WORKDIR /app

# Copy the current directory contents into the container at /app
# 将当前目录内容复制到/app的容器中
ADD . /app

# Install any needed packages specified in requirements.txt
# 安装requirements.txt中指定的任何所需的软件包
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
# 在此容器外面向全球提供端口80
EXPOSE 80

# Define environment variable
# 定义环境变量
ENV NAME World

# Run app.py when the container launches
# 容器启动时运行app.py
CMD ["python", "app.py"]

ENV http_proxy 192.168.31.89:22
ENV https_proxy 192.168.31.89:22
```
**你在代理服务器后面吗？**

代理服务器启动并运行后，可以阻止与您的网络应用程序的连接。 如果您位于代理服务器的后面，请使用ENV命令为您的代理服务器指定主机和端口，将以下行添加到`Dockerfile`中：

```python
# Set proxy server, replace host:port with values for your servers
# 设置代理服务器，将host：port替换为服务器的值（换成本机对应的ip与端口）
ENV http_proxy host:port
ENV https_proxy host:port
```

这个Dockerfile引用了一些我们还没有创建的文件，分别是app.py和requirements.txt。 接下来创建这些。

## 应用程序本身
创建另外两个文件，`requirements.txt`和`app.py`，并将它们放在与Dockerfile相同的文件夹中。 这完成了我们的应用程序，你可以看到很简单。 当上面的Dockerfile被内置到镜像中时，由于Dockerfile的ADD命令，app.py和requirements.txt将会出现，并且app.py的输出将可以通过HTTP访问，这要归功于EXPOSE命令。
### requirements.txt

```
Flask
Redis
```
### app.py

```
from flask import Flask
from redis import Redis, RedisError
import os
import socket

# Connect to Redis
redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

app = Flask(__name__)

@app.route("/")
def hello():
    try:
        visits = redis.incr("counter")
    except RedisError:
        visits = "<i>cannot connect to Redis, counter disabled</i>"

    html = "<h3>Hello {name}!</h3>""<b>Hostname:</b> {hostname}<br/>""<b>Visits:</b> {visits}"
    return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```
现在我们看到`pip install -r requirements.txt`安装了Python的Flask和Redis库，应用程序打印环境变量NAME以及调用socket.gethostname（）的输出。 最后，由于Redis没有运行（因为我们只安装了Python库，而不是Redis本身），所以我们应该期望在这里使用它的尝试将失败并产生错误消息。

注意：在容器中访问主机的名称将检索容器ID，这与正在运行的可执行文件的进程ID类似。

而已！ 您的系统上不需要Python或任何requirements.txt文件，也不会在您的系统上安装或运行此映像。 似乎你并没有真正用Python和Flask建立一个环境，但是你已经拥有了。

## 构建应用程序
我们准备构建应用程序。 确保你仍然在你的新目录的顶层。 这是什么ls应该显示：

```
$ ls
Dockerfile		app.py			requirements.txt
```
现在运行build命令。 这会创建一个Docker镜像，我们将使用`-t`标记它，因此它有一个友好的名字。

```
docker build -t friendlyhello .
```
你的构建的镜像在哪里？ 它在你的机器的本地`Docker images`注册表中：

```
$ docker images

REPOSITORY            TAG                 IMAGE ID
friendlyhello         latest              326387cea398
```

提示：您可以使用命令`docker images`或较新的`docker image ls` 列举镜像。 他们给你相同的输出。

## 运行应用程序
运行应用程序，使用`-p`将您的机器的端口4000映射到容器的已发布端口80：

```
docker run -p 4000:80 friendlyhello
```
你应该看到一条消息，Python在http://0.0.0.0:80上提供你的应用程序。 但是，这个消息来自容器内部，它不知道你将该容器的端口80映射到4000，使得正确的URL为http://localhost:4000。

在Web浏览器中转到该URL以查看网页上显示的显示内容，包括“Hello World”文本，容器标识和Redis错误消息。

注意：如果您在Windows 7上使用Docker Toolbox，请使用Docker Machine IP而不是本地主机。 例如，http://192.168.99.100:4000/。 要查找IP地址，请使用命令`docker-machine ip`。

您也可以在shell中使用curl命令来查看相同的内容。

```
$ curl http://localhost:4000

<h3>Hello World!</h3><b>Hostname:</b> 8fc990912a14<br/><b>Visits:</b> <i>cannot connect to Redis, counter disabled</i>
```
这个4000：80的端口重映射是为了演示Dockerfile中的EXPOSE与使用`docker run -p`发布的内容之间的区别。 在后面的步骤中，我们将主机上的端口80映射到容器中的端口80，并使用http：// localhost。

在终端中点击CTRL + C退出。

**在Windows上，显式停止容器**

在Windows系统上，CTRL + C不会停止容器。 因此，首先键入CTRL + C来获取提示（或打开另一个shell），然后键入`docker container ls`列出正在运行的容器，接着是`docker container stop <Container NAME或ID>`以停止容器。 否则，当您尝试在下一步中重新运行容器时，将从守护程序中收到错误响应。

现在让我们以分离模式在后台运行应用程序：

```
docker run -d -p 4000:80 friendlyhello
```
你得到你的应用程序的长容器ID，然后被踢回你的终端。 您的容器正在后台运行。 您还可以使用`docker container ls`查看缩写的容器ID（在运行命令时两者可以互换使用）：

```
$ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED
1fa4ab2cf395        friendlyhello       "python app.py"     28 seconds ago
```

您将看到CONTAINER ID与http：// localhost：4000上的内容匹配。

现在使用docker container stop来结束进程，使用CONTAINER ID，如下所示：

```
docker container stop 1fa4ab2cf395
```

## Share your image

为了演示我们刚刚创建的可移植性，我们上传我们构建的镜像，并在其他地方运行它。 毕竟，当你想将容器部署到生产环境中时，你需要学习如何推送注册表。

一个注册表是一个存储库的集合，一个存储库是一个镜像集合 - 有点像GitHub存储库，除了代码已经建立。 注册表上的帐户可以创建许多存储库。 Docker CLI默认使用Docker的公共注册表。

注意：我们将在这里使用<font color=#d000 size=5>Docker的公共注册表</font>，因为它是免费的，并且是预配置的，但是有许多公共选项可供选择，甚至可以使用[Docker Trusted Registry](https://docs.docker.com/datacenter/dtr/2.2/guides/)设置您自己的私有注册表。

### 使用您的Docker ID登录
如果您没有Docker帐户，请在[cloud.docker.com](https://cloud.docker.com/)注册一个。 记下你的用户名。

**登录到本地机器上的Docker公共注册表。**

```python
$ docker login
# 输入用户名与密码 完成登录

# 或者
docker login -u 用户名 -p 密码
# 退出
docker logout
```
### 标记镜像
将本地镜像与注册表中的存储库相关联的符号是username / repository：tag。 该标签是可选的，但推荐使用，因为这是注册管理机构为Docker镜像提供版本的机制。 给存储库并标记上下文的有意义的名字，比如get-started：part2。 这会将图像放入`get-started`存储库中，并将其标记为part2。

现在，把它们放在一起来标记镜像。 使用您的用户名，存储库和标签名称运行`docker tag image`，以便镜像将上传到您想要的目的地。 该命令的语法是：

```
docker tag image username/repository:tag
```
For example:

```python
docker tag friendlyhello john/get-started:part2

# 由于我的docker hub 目录为 781708249/mydocker
docker tag friendlyhello 781708249/mydocker:part2
```

运行` docker images `来查看新标记的镜像。 （您也可以使用`docker image ls`。）

```
$ docker images
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
friendlyhello            latest              d9e555c53008        3 minutes ago       195MB
john/get-started         part2               d9e555c53008        3 minutes ago       195MB
python                   2.7-slim            1c7128a655f6        5 days ago          183MB
...
```
### 发布镜像
将您的标记镜像上传到存储库：

```python
docker push username/repository:tag

# 如：我这里是
docker push 781708249/mydocker:part2
```
一旦完成，这个上传的结果是公开的。 如果你登录到`[Docker Hub](https://hub.docker.com/)`，你将会看到那个新的镜像和它的pull命令。

![这里写图片描述](http://img.blog.csdn.net/20171129135553272?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20171129135834253?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 从远程存储库中提取并运行映像
从现在开始，您可以使用docker run并使用此命令在任何机器上运行您的应用程序：

```python
# docker run -p 4000:80 username/repository:tag
docker run -p 4000:80 781708249/mydocker:part2
```
如果镜像在机器本地不可用，Docker将从存储库中取出。

```
$ docker run -p 4000:80 781708249/mydocker:part2
Unable to find image 'john/get-started:part2' locally
part2: Pulling from john/get-started
10a267c67f42: Already exists
f68a39a6a5e4: Already exists
9beaffc0cf19: Already exists
3c1fe835fb6b: Already exists
4c9f1fa8fcb8: Already exists
ee7d8f576a14: Already exists
fbccdcced46e: Already exists
Digest: sha256:0601c866aab2adcc6498200efd0f754037e909e5fd42069adeff72d1e2439068
Status: Downloaded newer image for john/get-started:part2
 * Running on http://0.0.0.0:80/ (Press CTRL+C to quit)
```

注意：如果你没有指定这些命令的`:tag`，那么将会假定`:latest`的标签，无论是在构建时还是运行镜像时。 Docker将使用没有指定标签（不一定是最新的镜像）运行的镜像的最后一个版本。

无论`docker run`在哪里，它都会将您的镜像以及Python和requirements.txt中的所有依赖关系一起拉出来，并运行您的代码。 它们一起旅行在一个整洁的小包里，主机不需要安装任何东西，除了Docker来运行它。

## Recap and cheat sheet (optional)
以下是本页所涵盖内容的终端录像：

下面是这个页面的基本Docker命令列表，以及一些相关的命令，如果你想在继续之前探索一下。

```
docker build -t friendlyname .  # Create image using this directory's Dockerfile（Dockerfile创建镜像 friendlyname为镜像名）
docker run -p 4000:80 friendlyname  # Run "friendlyname" mapping port 4000 to 80（映射端口4000到80）
docker run -d -p 4000:80 friendlyname         # Same thing, but in detached mode（端口映射，但在分离模式）
docker container ls                                # List all running containers（列出所有运行的容器）
docker container ls -a             # List all containers, even those not running（列出所有容器，甚至那些没有运行的容器）
docker container stop <hash>           # Gracefully stop the specified container（停止指定的容器）
docker container kill <hash>         # Force shutdown of the specified container（强制关闭指定的容器）
docker container rm <hash>        # Remove specified container from this machine（从此机器中移除指定的容器）
docker container rm $(docker container ls -a -q)         # Remove all containers（移除所有容器）
docker image ls -a                             # List all images on this machine（列出机器上所有镜像）
docker image rm <image id>            # Remove specified image from this machine（移除指定的镜像）
docker image rm $(docker image ls -a -q)   # Remove all images from this machine（移除所有镜像）
docker login             # Log in this CLI session using your Docker credentials（使用您的Docker凭据登录此CLI会话）
docker tag <image> username/repository:tag  # Tag <image> for upload to registry（标签<image>上传到注册表）
docker push username/repository:tag            # Upload tagged image to registry（上传标记的镜像到注册表）
docker run username/repository:tag                   # Run image from a registry（从注册表运行镜像）
```


----------


# 第3部分：服务
## Prerequisites
安装Docker版本1.13或更高版本。

获取Docker撰写。 在Docker for Mac和Docker for Windows上，它已经预装了，所以你很好用。 在Linux系统上，您需要直接安装它。 在没有Hyper-V的Windows 10系统之前，使用Docker Toolbox。

阅读第一部分的方向。

了解如何在第2部分中创建容器。

确保您已经发布了通过推送到注册表创建的friendlyhello镜像。 我们将在这里使用该共享镜像。

确保你的镜像作为一个部署的容器。 运行这个命令，在你的信息中插入用户名，仓库和标签：`docker run -p 80:80 username/repo:tag`，然后访问
http://localhost/。


----------


## 关于服务
在分布式应用程序中，应用程序的不同部分被称为“服务”。例如，如果您想象一个视频共享站点，它可能包括一个用于将应用程序数据存储在数据库中的服务，后面的视频转码服务 用户上传东西，为前端服务等等。

服务实际上只是“生产中的容器”。服务只运行一个镜像，但它编码图像运行的方式 - 应该使用哪个端口，容器应该运行多少个副本，以便服务具有所需的容量，以及 等等。 缩放服务会更改运行该软件的容器实例的数量，从而为流程中的服务分配更多的计算资源。

幸运的是，使用Docker平台定义，运行和扩展服务非常简单 - 只需编写一个`docker-compose.yml`文件即可。

## Your first docker-compose.yml file
`docker-compose.yml`文件是一个YAML文件，它定义了Docker容器在生产中的行为方式。
###`docker-compose.yml`

将这个文件保存为`docker-compose.yml`，无论你在哪里。 确保您已将第2部分中创建的镜像推送到注册表中，并通过用您的镜像细节替换`username/repo:tag`来更新此`.yml`。

```
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: 781708249/mydocker:part2
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
      - "80:80"
    networks:
      - webnet
networks:
  webnet:
```
这个`docker-compose.yml`文件告诉Docker执行以下操作：

 - 从注册表中取出我们在步骤2中上传的镜像。
 - 运行该镜像的5个实例作为一个名为web的服务，限制每个服务器最多使用10％的CPU（跨所有核心）和50MB的RAM。
 - 如果一个失败，立即重新启动容器。
 - 将主机上的端口80映射到Web的端口80。
 - 指示Web容器通过称为webnet的负载平衡网络共享端口80。 （在内部，容器本身将在临时端口上发布到网站的端口80）。
 - 使用默认设置（这是一个负载平衡覆盖网络）定义webnet网络。

## Run your new load-balanced app
在我们使用`docker stack deploy`命令之前，我们先运行：

```
docker swarm init
```
注意：我们将在第4部分中介绍该命令的含义。如果您不运行`docker swarm init`，则会出现“此节点不是swarm manager”的错误。

现在让我们来运行它。 你必须给你的应用程序的名称。 在这里，它被设置为`getstartedlab`：

```
docker stack deploy -c docker-compose.yml getstartedlab
```

我们的单个服务堆栈在一台主机上运行了5个部署镜像的容器实例。 让我们来调查。

在我们的应用程序中获取一项服务的服务ID：

```
docker service ls
```
您将看到Web服务的输出，并以您的应用程序名称作为前缀。 如果您将其命名为与此示例中所示的相同，则名称将为getstartedlab_web。 还列出了服务ID以及副本数量，映像名称和暴露端口。

在服务中运行的单个容器称为任务。 任务会被赋予唯一的数字增加的ID，最多可以在`docker-compose.yml`中定义的副本数量。 列出您的服务的任务：

```
docker service ps getstartedlab_web
```
如果您只列出系统中的所有容器，也会显示任务，但不会被服务过滤：

```
docker container ls -q
```
您可以连续多次运行`curl -4 http://localhost`，或者在浏览器中转到该URL并点击几次刷新。

无论哪种方式，您将看到容器ID更改，演示负载平衡; 在每个请求中，以循环方式选择5个任务中的一个来响应。 容器ID将匹配您以前的命令（`docker container ls -q`）的输出。

**运行Windows 10？**

Windows 10 PowerShell应该已经有curl可用了，但是如果没有的话，你可以抓一个像[Git BASH](https://git-for-windows.github.io/)这样的Linux终端仿真器，或者下载非常相似的[wget for Windows](http://gnuwin32.sourceforge.net/packages/wget.htm)。

**响应时间慢？**

根据您的环境的网络配置，容器可能需要长达30秒才能响应HTTP请求。 这并不代表Docker或Swarm的性能，而是一个未得到满足的Redis依赖，我们将在后面的教程中解决。 就目前而言，访问者柜台不是出于同样的原因; 我们还没有添加一个服务来保存数据。

## 缩放应用程序
您可以通过更改`docker-compose.yml`中的副本值，保存更改并重新运行`docker stack deploy`命令来扩展应用程序：

```
docker stack deploy -c docker-compose.yml getstartedlab
```
Docker会做一个就地更新，不需要先撕下堆栈或杀死任何容器。

现在，重新运行`docker container ls -q`来查看重新配置的已部署实例。 如果你扩大了副本，更多的任务，因此，更多的容器，开始。
### Take down the app and the swarm

 - 取消掉应用程序`docker stack rm`：

```
docker stack rm getstartedlab
```

 - Take down the swarm.

```
docker swarm leave --force
```
这很容易，站起来和扩展您的应用程序与Docker。 你已经朝着学习如何在生产中运行容器迈出了一大步。 接下来，您将学习如何将这个应用程序作为Docker机器集群上的真正群体运行。

注意：像这样编写文件用于定义Docker应用程序，可以使用Docker Cloud上传到云提供程序，也可以通过Docker Enterprise Edition选择任何硬件或云提供程序。

## Recap and cheat sheet (optional)
总而言之，在输入docker run的过程中很简单，生产中容器的真正实现就是将其作为服务运行。 服务在一个Compose文件中编写一个容器的行为，这个文件可以用来缩放，限制和重新部署我们的应用程序。 对服务的更改可以在运行时使用相同的命令启动服务：`docker stack deploy`。

一些在这个阶段探索的命令：

```python
docker stack ls # List stacks or apps（列表堆栈或应用程序）
docker stack deploy -c <composefile> <appname>  # Run the specified Compose file（运行指定的Compose文件）
docker service ls   # List running services associated with an app（列出与应用关联的正在运行的服务）
docker service ps <service>  # List tasks associated with an app（列出与应用程序关联的任务）
docker inspect <task or container>   # Inspect task or container（检查任务或容器）
docker container ls -q  # List container IDs（列出容器ID）
docker stack rm <appname> # Tear down an application（撕下一个应用程序）
docker swarm leave --force  # Take down a single node swarm from the manager（从manager上取下一个节点群）
```
# 第4部分：群集

## 了解群集
swarm是运行Docker并加入到一个集群中的一组机器。发生这种情况之后，您将继续运行您习惯的Docker命令，但是现在它们将由群集管理器在群集上执行。群体中的机器可以是物理的或虚拟的。加入群体后，他们被称为节点。

Swarm管理人员可以使用多种策略来运行容器，比如“最空的节点”（emptiest node） - 它使用容器填充最少使用的机器。或“全局”，这确保了每台机器只能得到指定容器的一个实例。您可以指示swarm manager在Compose文件中使用这些策略，就像您已经使用的策略一样。

群体管理者是群体中唯一可以执行你的命令的机器，或者授权其他机器作为工作者加入群体。工人只是在那里提供能力，并没有权力告诉任何其他机器可以做什么和不可以做什么。

到目前为止，您已经在本地机器上以单主机模式使用Docker。但是Docker也可以切换到群集模式，这就是使用群集的原因。立即启用群模式使当前机器成为群管理器。从那时起，Docker将运行您正在管理的群集上执行的命令，而不仅仅是在当前的机器上。

## 建立你的群
一个群由多个节点组成，可以是物理机或虚拟机。 基本的概念很简单：运行`docker swarm init`来启用swarm模式，并让你的当前机器成为swarm manager，然后在其他机器上运行`docker swarm join`使它们作为worker加入swarm。 选择下面的选项卡，看看这是如何在不同的情况下发挥。 我们将使用虚拟机快速创建一个双机群集，并将其变成群集。

### 创建一个集群
#### VMS在本地机器上（MAC，LINUX，WINDOWS 7和8）
首先，您需要一个可以创建虚拟机（VM）的虚拟机管理程序，因此为您的机器的操作系统安装[Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)。

注意：如果您在安装了Hyper-V的Windows系统（如Windows 10）上，则无需安装VirtualBox，而应该使用Hyper-V。 通过单击上面的Hyper-V选项卡查看Hyper-V系统的说明。 如果你使用的是Docker Toolbox，你应该已经安装了VirtualBox作为其一部分，所以你很好。

现在，使用Docker-machine创建几个虚拟机，使用VirtualBox驱动程序：

```
docker-machine create --driver virtualbox myvm1
docker-machine create --driver virtualbox myvm2
```
### 列出VMS并获取他们的IP地址
