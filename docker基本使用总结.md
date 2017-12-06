
- [下载镜像](#下载镜像)
- [更新镜像](#更新镜像)
- [设置镜像标签](#设置镜像标签)
- [发布镜像](#发布镜像)
- [从远程存储库中提取并运行镜像](#从远程存储库中提取并运行镜像)
- [发布到阿里镜像](#发布到阿里镜像)
- [Docker容器镜像删除](#Docker容器镜像删除)
- [附加](#附加)

-------
# 下载镜像

```python
# 如安装python镜像
docker search python # 查找镜像
docker pull python # 拖取镜像

# 或 
docker run -p 4000:80 --name=python3.5 python # 如果本地不存在会自动下载

```
或 使用Dockerfile 创建
参考：http://www.runoob.com/docker/docker-install-python.html

# 更新镜像
先使用镜像来创建一个容器

```python
docker run -t -i ubuntu:16.04 /bin/bash
root@84f08a0b8ca8: 
# root@后面的为容器id，每次运行镜像时得到的容器id都是唯一的

# 在该容器id中安装更新等后，可以将容器保存成镜像，后续可以上传该镜像到docker hub
docker commit -m="has update" -a="runoob" 84f08a0b8ca8 runoob/ubuntu:v2
"""
各个参数说明
-m:提交的描述信息
-a:指定镜像作者
84f08a0b8ca8：容器ID （不是镜像ID）
runoob/ubuntu:v2:指定要创建的目标镜像名
"""
```
# 设置镜像标签

```python
docker tag 860c279d2fec 781708249/mydocker:part2
# 860c279d2fec 为镜像id
```

# 发布镜像

```python
# 发布到docker hub
# 连接docker hub账号
$ docker login
# 输入用户名与密码 完成登录

# 或者
docker login -u 用户名 -p 密码
# 退出
docker logout
```

```
docker push username/repository:tag

# 如：我这里是
docker push 781708249/mydocker:part2 # 参考 docker tar 
```

# 从远程存储库中提取并运行镜像
参考：http://www.runoob.com/docker/docker-container-connection.html
```
# docker run -p 4000:80 username/repository:tag
docker run -p 4000:80 781708249/mydocker:part2
```

# 发布到阿里镜像

登录阿里[容器Hub服务](https://cr.console.aliyun.com/?spm=5176.100239.blogcont29941.12.NjxJue#/imageList)的控制台

先设置 镜像加速器

![这里写图片描述](http://img.blog.csdn.net/20171206221230883?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2M3ODE3MDgyNDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

点开管理 查看如何发布镜像到该 阿里镜像上

如：我的是

```python
$ sudo docker login --username=wucong0803@126.com registry.cn-hangzhou.aliyuncs.com # 登录 阿里镜像
$ sudo docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/781708249/ubuntu:[镜像版本号]
$ sudo docker push registry.cn-hangzhou.aliyuncs.com/781708249/ubuntu:[镜像版本号]
```


# Docker容器镜像删除

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



# 附加

```python
docker images / docker image ls  # 查看本地的镜像
docker ps # 查看后台运行的容器

# -d 表后台启动容器
docker run -d ubuntu:16.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
# 或
docker run -d <Image ID> /bin/sh -c "while true; do echo hello world; sleep 1; done"
```
