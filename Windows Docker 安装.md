参考：http://www.runoob.com/docker/windows-docker-install.html


----------


# Windows Docker 安装
Docker 引擎使用的是 Linux 内核特性，所以我们需要在 Windows 上使用一个轻量级的虚拟机 (VM) 来运行 Docker。
我们通过 Boot2Docker 来安装虚拟机和运行 Docker


----------


## 安装
### 1、安装Boot2Docker 
最新版 Boot2Docker 下载地址： https://github.com/boot2docker/windows-installer/releases/latest
目前最新版为v1.8.0, 下载地址为： https://github.com/boot2docker/windows-installer/releases/download/v1.8.0/docker-install.exe
### 2、运行安装文件
运行安装文件，它将会安装 virtualbox、MSYS-git boot2docker Linux 镜像和 Boot2Docker 的管理工具。

![这里写图片描述](http://www.runoob.com/wp-content/uploads/2016/05/docker13.png)

![这里写图片描述](http://www.runoob.com/wp-content/uploads/2016/05/docker14.png)

![这里写图片描述](http://www.runoob.com/wp-content/uploads/2016/05/docker15.png)

从桌面上或者 Program Files 中运行 Boot2Docker Start。
Boot2Docker Start 将启动一个 Unix shell 来配置和管理运行在虚拟主机中的 Docker，我们可以通过运行 docker version 来查看它是否正常工作。

![这里写图片描述](http://www.runoob.com/wp-content/uploads/2016/05/docker36.png)

## 运行 Docker
使用boot2docker.exe ssh 连接到虚拟主机上，然后执行docker run hello-world
![这里写图片描述](http://www.runoob.com/wp-content/uploads/2016/05/docker37.png)

![这里写图片描述](http://www.runoob.com/wp-content/uploads/2016/05/docker38.jpg)

到此，docker在Windows系统的安装完成。
