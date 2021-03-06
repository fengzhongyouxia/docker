[toc]

- [在主机上列出镜像列表](#在主机上列出镜像列表)
- [获取一个新的镜像](#获取一个新的镜像)
- [查找镜像](#查找镜像)
- [拖取镜像Pull our image](#拖取镜像Pullourimage)
- [创建我们自己的镜像](#创建我们自己的镜像)
- [更新并且提交更改](#更新并且提交更改)
- [使用Dockerfile构建镜像](#使用Dockerfile构建镜像)
- [设置镜像标签](#设置镜像标签)
- [Image Digests](#ImageDigests)
- [推送镜像到 Docker Hub](#推送镜像到DockerHub)
- [主机中移除镜像](#主机中移除镜像)

-------
在了解Docker这部分中，我们知道了 Docker 镜像是容器的基础。在前面的部分我们使用的是已经构建好的 Docker 镜像，例如： ubuntu 镜像和 training/webapp 镜像。

我们还了解了从 Docker 商店下载镜像到本地的 Docker 主机上。如果一个镜像不存在，他就会自动从 Docker 镜像仓库去下载，默认是从 Docker Hub 公共镜像源下载。

在这一节中，我们将探讨更多的关于 Docker 镜像的东西：

 - 管理和使用本地 Docker 主机镜像。
 - 创建基本镜像
 - 上传 Docker 镜像到 Docker Hub Registry。

# 在主机上列出镜像列表
让我们列出本地主机上的镜像。你可以使用 `docker images` 来完成这项任务：

```
$ sudo docker images
REPOSITORY       TAG      IMAGE ID      CREATED      VIRTUAL SIZE
training/webapp  latest   fc77f57ad303  3 weeks ago  280.5 MB
ubuntu           13.10    5e019ab7bf6d  4 weeks ago  180 MB
ubuntu           saucy    5e019ab7bf6d  4 weeks ago  180 MB
ubuntu           12.04    74fe38d11401  4 weeks ago  209.6 MB
ubuntu           precise  74fe38d11401  4 weeks ago  209.6 MB
ubuntu           12.10    a7cf8ae4e998  4 weeks ago  171.3 MB
ubuntu           quantal  a7cf8ae4e998  4 weeks ago  171.3 MB
ubuntu           14.04    99ec81b80c55  4 weeks ago  266 MB
ubuntu           latest   99ec81b80c55  4 weeks ago  266 MB
ubuntu           trusty   99ec81b80c55  4 weeks ago  266 MB
ubuntu           13.04    316b678ddf48  4 weeks ago  169.4 MB
ubuntu           raring   316b678ddf48  4 weeks ago  169.4 MB
ubuntu           10.04    3db9c44f4520  4 weeks ago  183 MB
ubuntu           lucid    3db9c44f4520  4 weeks ago  183 MB
```
我们可以看到之前使用的镜像。当我们每次要使用镜像来启动一个新容器的时候都会从 Docker Hub 下载对应的镜像。

我们在镜像列表中看到三个至关重要的东西。

 - 来自什么镜像源，例如 ubuntu
 - 每个镜像都有标签(tags)，例如 14.04
 - 每个镜像都有镜像ID

镜像源中可能存储着一个镜像源的多个版本。我们会看到 Ubuntu 的多个版本：10.04, 12.04, 12.10, 13.04, 13.10 and 14.04。每个容器有一个唯一的标签(tag)，让我们来识别为不同的镜像，例如：`ubuntu:14.04`

所以我们可以运行一个带标签镜像的容器：

```
$ sudo docker run -t -i ubuntu:14.04 /bin/bash
```
如果我们想要使用 Ubuntu 12.04 的镜像来构建，我们可以这样做

```
$ sudo docker run -t -i ubuntu:12.04 /bin/bash
```
如果你不指定一个镜像的版本标签，例如你只使用 Ubuntu，Docker 将默认使用 `Ubuntu:latest` 镜像。

```
提示：我们建议使用镜像时指定一个标签，例如 ubuntu:12.04 。这样你知道你使用的是一个什么版本的镜像。
```

# 获取一个新的镜像
现在如何获取一个新的镜像？当我们在本地主机上使用一个不存在的镜像时 Docker 就会自动下载这个镜像。但是这需要一段时间来下载这个镜像。如果我们想预先下载这个镜像，我们可以使用 `docker pull` 命令来下载它。这里我们下载 centos 镜像。

```
$ sudo docker pull centos
Pulling repository centos
b7de3133ff98: Pulling dependent layers
5cc9e91966f7: Pulling fs layer
511136ea3c5a: Download complete
ef52fb1fe610: Download complete
. . .
```
我们看到镜像的每一层都被下载下来了，现在我们可以直接使用这个镜像来运行容器，而不需要在下载这个镜像了。

```
$ sudo docker run -t -i centos /bin/bash
bash-4.1#
```
# 查找镜像
Docker 的特点之一是人们创建了各种各样的 Docker 镜像。而且这些镜像已经被上传到了 Docker Hub 。我们可以从 Docker Hub 网站来搜索镜像。
![这里写图片描述](http://www.widuu.com/chinese_docker/images/search.png)

我们也可以使用 `docker search` 命令来搜索镜像。譬如说我们的团队需要一个安装了 Ruby 和 Sinatra 的镜像来做我们的 web 应用程序开发。我们可以通过 `docker search` 命令搜索 `sinatra` 来寻找适合我们的镜像

```
$ sudo docker search sinatra
NAME                                   DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
training/sinatra                       Sinatra training image                          0                    [OK]
marceldegraaf/sinatra                  Sinatra test app                                0
mattwarren/docker-sinatra-demo                                                         0                    [OK]
luisbebop/docker-sinatra-hello-world                                                   0                    [OK]
bmorearty/handson-sinatra              handson-ruby + Sinatra for Hands on with D...   0
subwiz/sinatra                                                                         0
bmorearty/sinatra                                                                      0
. . . 
```

我们看到返回了大量的 sinatra镜像。而列表中有镜像名称、描述、Stars(衡量镜像的流行程度-如果用户喜欢这个镜像他就会点击 stars )和 是否是正式版以及构建状态。官方镜像仓库 是官方精心整理出来服务 Docker 的 Docker 镜像库。自动化构建的镜像仓库是允许你验证镜像的内容和来源。

通过检索镜像，我们决定使用 training/sinatra 镜像。到目前为止，我们已经看到了两种类型的镜像，像`ubuntu` 镜像，我们称它为<font color=#d000 size=5>基础镜像或者根镜像</font>。这些镜像是由 Docker 官方提供构建、验证和支持。这些镜像都可以通过用自己的名字来标记。

我们也可以查找其它用户的公开镜像，像我们选择使用的 training/sinatra 镜像，这属于<font color=#d000 size=5>个人镜像</font>。个人镜像是由 Docker 社区的成员创建和维护的。你可以用用户名称来识别这些镜像，因为这些镜像的前缀都是以用户名来标记的 ，像 training ，就是由 training 用户创建的镜像。

# 拖取镜像(Pull our image)
我们已经确定了要使用的镜像， training/sinatra , 现在我们使用 `docker pull` 命令来下载这个镜像。

```
$ sudo docker pull training/sinatra
```
现在团队成员可以在自己的容器内使用这个镜像了。

```
$ sudo docker run -t -i training/sinatra /bin/bash
root@a8cb6ce02d85:/#
```
# 创建我们自己的镜像
团队成员发现 training/sinatra 镜像对于我们来说是非常有用的，但是它不能满足我们的需求，我们需要针对这个镜像做出更改。这里我们有两种方法，更新镜像或者创建一个新的镜像。

 - 1.我们可以从已经创建的容器中更新镜像，并且提交这个镜像。
 - 2.我们可以使用 Dockerfile 指令来创建一个新的镜像。

# 更新并且提交更改
更新镜像之前，我们需要使用镜像来创建一个容器。

```
$ sudo docker run -t -i training/sinatra /bin/bash
root@0b2616b0e5a8:/#
```

```
注意：已创建容器ID 0b2616b0e5a8,我们在后边还需要使用它。
```
在运行的容器内使用 gem 来安装 json

```
root@0b2616b0e5a8:/# gem install json
```
在完成操作之后，输入 exit命令来退出这个容器。

现在我们有一个根据我们需求做出更改的容器。我们可以使用 `docker commit` 来提交容器副本。

```
$ sudo docker commit -m="Added json gem" -a="Kate Smith" \
0b2616b0e5a8 ouruser/sinatra:v2
4f177bd27a9ff0f6dc2a830403925b5360bfe0b93d476f7fc3231110e7f71b1c
```
这里我们使用了 docker commit 命令。这里我们指定了两个标识(flags) -m 和 -a 。-m 标识我们指定提交的信息，就像你提交一个版本控制。-a 标识允许对我们的更新来指定一个作者。

我们也指定了想要创建的新镜像容器来源 (我们先前记录的ID) 0b2616b0e5a8 和 我们指定要创建的目标镜像：

```
ouruser/sinatra:v2
```
我们分解一下前边的步骤。我们先给这个镜像分配了一个新用户名字 ouruser，接着，未修改镜像名称，保留了原有镜像名称sinatra；最后为镜像指定了标签(tag) v2。
我们可以使用 docker images 命令来查看我们的新镜像 ouruser/sinatra。

```
$ sudo docker images
REPOSITORY          TAG     IMAGE ID       CREATED       VIRTUAL SIZE
training/sinatra    latest  5bc342fa0b91   10 hours ago  446.7 MB
ouruser/sinatra     v2      3c59e02ddd1a   10 hours ago  446.7 MB
ouruser/sinatra     latest  5db5f8471261   10 hours ago  446.7 MB
```
使用我们的新镜像来创建一个容器：

```
$ sudo docker run -t -i ouruser/sinatra:v2 /bin/bash
root@78e82f680994:/#
```


----------
# 使用Dockerfile构建镜像
使用 docker commit 命令能够非常简单的扩展镜像。但是它有点麻烦，并且在一个团队中也不能够轻易的共享它的开发过程。为解决这个问题，我们使用一个新的命令 docker build ， 从零开始来创建一个新的镜像。

为此，我们需要创建一个 Dockerfile 文件，其中包含一组指令来告诉 Docker 如何构建我们的镜像。

现在创建一个目录，并且创建一个 Dockerfile

```
$ mkdir sinatra
$ cd sinatra
$ touch Dockerfile
```
如果你是在 Windows 上使用的 Boot2Docker，你可以通过使用 cd 命令来访问你的本地主机目录 /c/Users/your_user_name

每一个指令都会在镜像上创建一个新的层，来看一个简单的例子，我们的开发团建来构建一个自己的 Sinatra镜像：

```
# This is a comment
FROM ubuntu:14.04
MAINTAINER Kate Smith <ksmith@example.com>
RUN apt-get update && apt-get install -y ruby ruby-dev
RUN gem install sinatra
```
让我们看一下我们的 `Dockerfile` 做了什么。每一个指令的前缀都必须是大写的。

```
INSTRUCTION statement
```
提示：我们可以使用 # 来注释

第一个指令 FROM 是告诉 Docker 使用的哪个镜像源，在这个案例中，我们使用的是 `Ubuntu 14.04` 基础镜像。

下一步，我们使用 `MAINTAINER` 指令来指定谁在维护这个新镜像。

最后，我们指定了两个 RUN 指令。 RUN 指令在镜像内执行一条命令，例如：安装一个包。这里我们更新了 APT 的缓存，并且安装 Ruby 和 RubyGems ，然后使用 gem 安装 Sinatra 。

注意：我们还提供了更多的 Dockerfile 指令参数。

现在，我们使用 Dockerfile 文件，通过 docker build 命令来构建一个镜像。

```
$ docker build -t ouruser/sinatra:v2 .
```
我们使用 docker build 命令并指定 -t 标识(flag)来标示属于 ouruser ，镜像名称为 sinatra,标签是 v2。

如果 Dockerfile 在我们当前目录下，我们可以使用 `.` 来指定 Dockerfile

```
提示：你也可以指定 Dockerfile 路径
```
现在我们可以看到构建过程。Docker 做的第一件事是通过你的上下文进行构建，基本上是目录的内容构建。这样做是因为 Docker 进程构建镜像是实时构建的，并且是需要本地的上下文来做这些工作的。（这里上下文是指Context）

下一步，Dockerfile 中的每一条命令都一步一步的被执行。我们会看到每一步都会创建一个新的容器，在容器内部运行指令并且提交更改 - 就像我们之前使用的 docker commit 一样。当所有的指令执行完成之后，我们会得到97feabe5d2ed 镜像（也帮助标记为 ouruser/sinatra:v2）, 然后所有中间容器会被清除。

```
注意：与存储程序驱动无关，镜像不能超过127层。这是一种全局设置，为了是从整体上来优化镜像的大小。
```
我们可以使用新的镜像来创建容器：

```
$ docker run -t -i ouruser/sinatra:v2 /bin/bash
root@8196968dac35:/#
```
注意：这里只是简单的介绍一下如何创建镜像。我们跳过了很多你可以使用的其它指令。你会从后边的章节看到更多的指令或者你可以参考 Dockerfile 的详细说明和每一条指令的例子。为了帮助你编写清晰、易读、易维护的 Dockerfile ，我们也编写了 Dockerfile 最佳实践指南
https://github.com/widuu/Dockerfile/blob/master/Dockerfile

# 设置镜像标签
你可以在提交更改和构建之后为镜像来添加标签(tag)。我们可以使用 docker tag 命令。为我们的 ouruser/sinatra 镜像添加一个新的标签。

```
$ docker tag 5db5f8471261 ouruser/sinatra:devel
```
docker tag 需要使用镜像ID，这里是 5db5f8471261 ,用户名称、镜像源名(repository name)和新的标签名(tag)。

```
$ docker images ouruser/sinatra
REPOSITORY          TAG     IMAGE ID      CREATED        VIRTUAL SIZE
ouruser/sinatra     latest  5db5f8471261  11 hours ago   446.7 MB
ouruser/sinatra     devel   5db5f8471261  11 hours ago   446.7 MB
ouruser/sinatra     v2      5db5f8471261  11 hours ago   446.7 MB
```

# Image Digests
v2 或后续版本格式的镜像会有内容定位标示符叫做 digest。只要用于生成镜像的镜像源不更改， digests 值就是可以预料的。使用 --digests 标识来列出镜像 digests 的值

```
$ docker images --digests | head
REPOSITORY                         TAG                 DIGEST                                                                     IMAGE ID            CREATED             VIRTUAL SIZE
ouruser/sinatra                    latest              sha256:cbbf2f9a99b47fc460d422812b6a5adff7dfee951d8fa2e4a98caa0382cfbdbf    5db5f8471261        11 hours ago        446.7 MB
```
当我们从 v2 版本的镜像仓库来推送或者拉取镜像的时候，pull 和 push 命令包含了镜像 digests 。 你可以使用 digests 值来拉取镜像。

```
$ docker pull ouruser/sinatra@cbbf2f9a99b47fc460d422812b6a5adff7dfee951d8fa2e4a98caa0382cfbdbf
```
你可以参考 digest 的 create、run 和 rmi 命令，以及 Dockerfile 中的 FROM 镜像。

# 推送镜像到 Docker Hub
一旦你构建或创建了一个新的镜像，你可以使用 docker push 命令将镜像推送到 Docker Hub 。这样你就可以分享你的镜像了，镜像可以是公开的，或者你可以把镜像添加到你的私有仓库中。

```
$ docker push ouruser/sinatra
The push refers to a repository [ouruser/sinatra] (len: 1)
Sending image list
Pushing repository ouruser/sinatra (3 tags)
. . .
```
# 主机中移除镜像

你可以删除你主机上的镜像，类似于删处容器的方法，这里我们使用 docker rmi 命令。

让我们删除这个不需要使用的容器：training/sinatra。

```
$ docker rmi training/sinatra
```
提示：为了能够从主机上删除镜像，请确保没有基于此镜像的容器。


现在，我们已经看到如何在容器中构建单独的应用程序。接下来，我们要学习如何把多个 Docker 容器连接在一起构建一个完整的应用程序。
