# 1、下载镜像

```python
# 如安装python镜像
docker search python # 查找镜像
docker pull python # 拖取镜像

# 或 
docker run -p 4000:80 --name=python3.5 python # 如果本地不存在会自动下载

```
或 使用Dockerfile 创建
参考：http://www.runoob.com/docker/docker-install-python.html

# 2、更新镜像
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

# 附加

```python
docker images / docker image ls  # 查看本地的镜像
docker ps # 查看后台运行的容器

# -d 表后台启动容器
docker run -d ubuntu:16.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
# 或
docker run -d <Image ID> /bin/sh -c "while true; do echo hello world; sleep 1; done"
```
