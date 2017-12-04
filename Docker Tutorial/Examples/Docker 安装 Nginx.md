参考：http://www.runoob.com/docker/docker-install-nginx.html


----------
# 方法一、通过 Dockerfile构建
创建Dockerfile
首先，创建目录nginx,用于存放后面的相关东西。

```
runoob@runoob:~$ mkdir -p ~/nginx/www ~/nginx/logs ~/nginx/conf
```
www目录将映射为nginx容器配置的虚拟目录
logs目录将映射为nginx容器的日志目录
conf目录里的配置文件将映射为nginx容器的配置文件
进入创建的nginx目录，创建`Dockerfile`

```
vim Dockerfile
# 输入以下内容
FROM debian:jessie

MAINTAINER NGINX Docker Maintainers "docker-maint@nginx.com"

ENV NGINX_VERSION 1.10.1-1~jessie

RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 \
        && echo "deb http://nginx.org/packages/debian/ jessie nginx" >> /etc/apt/sources.list \
        && apt-get update \
        && apt-get install --no-install-recommends --no-install-suggests -y \
                                                ca-certificates \
                                                nginx=${NGINX_VERSION} \
                                                nginx-module-xslt \
                                                nginx-module-geoip \
                                                nginx-module-image-filter \
                                                nginx-module-perl \
                                                nginx-module-njs \
                                                gettext-base \
        && rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
        && ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
```

通过Dockerfile创建一个镜像，替换成你自己的名字

```
docker build -t nginx .
```
创建完成后，我们可以在本地的镜像列表里查找到刚刚创建的镜像

```
runoob@runoob:~/nginx$ docker images nginx
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               latest              555bbd91e13c        3 days ago          182.8 MB
```


----------


# 方法二、docker pull nginx
查找Docker Hub上的nginx镜像

```
docker search nginx
 docker pull nginx
```


----------
# 使用nginx镜像
## 运行容器

```
docker run -p 80:80 --name mynginx -v $PWD/www:/www -v $PWD/conf/nginx.con
```
命令说明：

 - -p 80:80：将容器的80端口映射到主机的80端口
 - --name mynginx：将容器命名为mynginx
 - -v $PWD/www:/www：将主机中当前目录下的www挂载到容器的/www
 - -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf：将主机中当前目录下的nginx.conf挂载到容器的/etc/nginx/nginx.conf
 - -v $PWD/logs:/wwwlogs：将主机中当前目录下的logs挂载到容器的/wwwlogs

## 查看容器启动情况

```
runoob@runoob:~/nginx$ docker ps
CONTAINER ID        IMAGE        COMMAND                      PORTS                         NAMES
45c89fab0bf9        nginx        "nginx -g 'daemon off"  ...  0.0.0.0:80->80/tcp, 443/tcp   mynginx
f2fa96138d71        tomcat       "catalina.sh run"       ...  0.0.0.0:81->8080/tcp          tomcat
```
通过浏览器访问

![这里写图片描述](http://www.runoob.com/wp-content/uploads/2016/06/nginx.png)
