参考：https://docs.docker.com/compose/install/

----------

您可以在macOS，Windows和64位Linux上运行Compose。

# 前提条件（安装docker）
Docker Compose依靠Docker Engine进行任何有意义的工作，因此请确保您已经安装了本地或远程的Docker Engine，具体取决于您的设置。

 - 在Docker for Mac和Windows等桌面系统上，Docker Compose作为桌面安装的一部分。
 - 在Linux系统上，首先按照Get Docker页面上的描述为您的操作系统安装Docker，然后回到这里以获取在Linux系统上安装Compose的说明。

----------
# 安装 Compose
按照下面的说明在Mac，Windows，Windows Server 2016或Linux系统上安装Compose，或者查找使用pip Python包管理器或将Compose作为容器安装的替代方法。

## 在Linux系统上安装Compose
在Linux上，您可以从GitHub上的[Compose存储库版本页面](https://github.com/docker/compose/releases)下载Docker Compose二进制文件。 按照链接中的说明进行操作，即在终端中运行curl命令以下载二进制文件。 这些一步一步的说明也包括在下面。

1、运行此命令下载最新版本的Docker Compose：

```
sudo curl -L https://github.com/docker/compose/releases/download/1.17.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```
2、对二进制文件应用可执行权限：

```
sudo chmod +x /usr/local/bin/docker-compose
```
3、（可选）为bash和zsh shell安装[命令完成](https://docs.docker.com/compose/completion/)。

4、测试安装。

```
$ docker-compose --version
docker-compose version 1.17.1, build 1719ceb
```

# 更新
如果你使用的是 Compose 1.2或者早期版本，当你升级完成后，你需要删除或者迁移你现有的容器。这是因为，1.3版本， Composer 使用 Docker 标签来对容器进行检测，所以它们需要重新创建索引标记。

如果 Composer 检测到创建的容器没有标签，它将拒绝运行，这样你就不会有两组容器。如果你想要保留已经存在的容器（举例：这里有容器的数据卷上保留这非常重要的数据），你可以使用下边的命令来迁移：

```
docker-compose migrate-to-labels
```
或者，如果这些容器是不必要的，你可以删除它们 - Composer 会重新创建一个新的。

```
docker rm -f myapp_web_1 myapp_db_1 ...
```

# 卸载
如果使用curl安装，请卸载Docker Compose：

```
sudo rm /usr/local/bin/docker-compose
```
如果使用pip安装，请卸载Docker Compose：

```
pip uninstall docker-compose
```

# Compose 文档

[用户指南](https://docs.docker.com/compose/)
[入门](https://docs.docker.com/compose/gettingstarted/)
开始使用Django
开始使用Rails
开始使用WordPress
命令行参考
Compose文件参考
[环境文件](https://docs.docker.com/compose/env-file/)
