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

# 更换docker镜像源

阿里：https://market.aliyun.com/products/53366009/?spm=5176.8623419.777960.27.JwMoxm#ymk=%7B%22categoryId%22:57742013,%22pageSize%22:10,%22saleMode%22:0,%22pageIndex%22:1,%22tag%22:%22Ubuntu%22,%22keywords%22:%22%22%7D

网易镜像中心：https://c.163.com/hub#/m/home/ 
daocloud镜像市场：https://hub.daocloud.io/

```
# 查看镜像源
docker images
# 更换网易镜像源
docker pull hub.c.163.com/library/tomcat:latest
# 阿里
sudo sed -i “s|ExecStart=/usr/bin/dockerd|ExecStart=/usr/bin/dockerd –registry-mirror=https://pee6w651.mirror.aliyuncs.com|g” /etc/systemd/system/docker.service

echo "DOCKER_OPTS=\"--registry-mirror=https://pee6w651.mirror.aliyuncs.com\"" | sudo tee -a /etc/default/docker
sudo service docker restart
```
