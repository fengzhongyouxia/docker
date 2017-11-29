参考：https://docs.docker.com/


----------
[toc]
# Install using the repository
## SET UP THE REPOSITORY
```
$ sudo apt-get update

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

$ sudo apt-key fingerprint 0EBFCD88
```
**amd64:**

```
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
**armhf:**

```
$ sudo add-apt-repository \
   "deb [arch=armhf] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
**s390x:**

```
$ sudo add-apt-repository \
   "deb [arch=s390x] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
## INSTALL DOCKER CE

```python
sudo apt-get update

sudo apt-get install docker-ce # 默认安装最新版本的docker

# 或者 指定docker版本
$ apt-cache madison docker-ce

docker-ce | 17.09.0~ce-0~ubuntu | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages

$ sudo apt-get install docker-ce=<VERSION> # 如 17.09.0~ce-0~ubuntu

# 验证是否安装成功
$ sudo docker run hello-world

```

## UPGRADE DOCKER CE

```
sudo apt-get update
```


----------
# Install from a package
如果你不能使用Docker的版本库来安装Docker CE，你可以下载你的版本的.deb文件并手动安装。 每次要升级Docker CE时，都需要下载一个新文件。
 https://download.docker.com/linux/ubuntu/dists/ 下载

```
sudo dpkg -i /path/to/package.deb
sudo docker run hello-world
```

# Uninstall Docker CE

```python
$ sudo apt-get purge docker-ce
$ sudo rm -rf /var/lib/docker
```


# Docker用户权限

```python
###以下操作都是在根用户下进行的

###增加一个用户组docker
groupadd docker
###把用户${USER}加入docker用户组
usermod -aG docker ${USER}
###重启docker服务(可不执行)
service docker restart
###新窗口登录${USER}
```

# Docker 容器镜像删除

1.停止所有的container，这样才能够删除其中的images：

docker stop $(docker ps -a -q)

如果想要删除所有container的话再加一个指令：

docker rm $(docker ps -a -q)

2.查看当前有些什么images

docker images

3.删除images，通过image的id来指定删除谁

docker rmi <image id>

想要删除untagged images，也就是那些id为<None>的image的话可以用

docker rmi $(docker images | grep "^<none>" | awk "{print $3}")

要删除全部image的话

docker rmi $(docker images -q)
