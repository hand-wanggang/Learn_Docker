# 使用Docker发布工程

## chapter1: Ubuntu安装Docker

##### 一、ubuntu 安装Docker

1、在ubuntu中打开一个终端，并执行如下命令

```
#更新软件源列表
sudo apt-get update
```

2、安装docker

```
#安装docker
sudo apt-get install docker.io
```

3、判断是否安装正确

```
sudo docker info
```

##### 二、不使用sudo的情况下，使用docker命令

默认安装完docker后，每次执行docker命令都需要sudo，否则就会失败！下面来解决如何不使用sudo的情况下使用docker

注意：需要将用户加入docker用户组即可

```
#如果还没有docker group就添加一个
sudo groupadd docker
#将用户加入该group内，然后退出并重新登录即可
sudo gpasswd -a ${USER} docker
#重启daocker服务
sudo service docker restart
#切换当前会话到新group
newgrp - docker
```

# chapter2:Docker的常用命令

##### 一、查看docker信息

1、查看docker版本： docker version

2、显示docker系统信息： docker info

##### 二、对image的操作

1、检索image

```
docker search image_name
```

2、下载image

```
docker pull image_name
```

3、列出镜像列表

```
docker images
```

4、删除镜像

```
docker rmi image_name
```

5、显示一个镜像历史

`docker history image_name`

##### 三、启动容器

```
#在容器中运行"echo"命令 ，输出“hello world”
docker run image_name echo "hello world"

#交互式进入容器
docker run -i -t image_name /bin/bash

#在容器中安装新的程序
docker run image_name apt-get install -y app_name
```

##### 四、查看容器

```
# 列出当前所有运行的container
docker ps

# 列出所有的container
docker ps -a

#列出最近一次启动的container
docker ps -1
```

##### 五、保存对容器的修改

```
docker commit ID new_image_name
```

##### 六、对容器的操作

```
#删除所有容器
docker rm 'docker ps -a -q'

#删除单个容器
docker rm name/ID

#停止、启动、杀死一个容器、重启一个容器
docker stop Name/ID
docker start Name/ID
docker kill Name/ID
docker restart Name/ID

#从容器中取日志
docker logs Name/ID

#列出容器中被改变的文件或者目录
docker diff Name/ID

#显示一个运行的容器里面的进程信息
docker top Name/ID

#从容器拷贝文件、目录到本地的一个路径
docker cp Name:/container_path to_path
docker cp ID:/container_path to_path
```

##### 七、登录registry server

```
#登录registry server -e/--email=;-p/--password="";-u /--username=""
docker login
```

##### 八、docker镜像的发布

```
# docker push 镜像名称（'/'之前应该是docker上注册的用户名）
docker push new_image_name
```

##### 九、设置镜像标签

```
# 设置镜像标签
docker tag 镜像id 用户名称/镜像源:新的标签名
#exm
docker tag 5db5f8471261 wg/eureka-server:devel
```

# chapter3:Docker File的编写

Dockerfile包含了创建镜像所需要的全部命令，是用来支持docker自动化的一种手段。编写完dockerfile可以使用docker build命令来自动化创建docker镜像。

##### 一、常用命令

1、所有dockerfile中的命令以 FROM命令开始。表示镜像是基于哪个基础镜像创建的。

```
FROM <image name>
```

2、MAINTAINER,设置镜像的作者

```
MAINTAINER <author name>
```

3、RUN 在shell或者exec环境中执行的命令。

```
RUN 《command》
```

4、ADD复制文件指令两个参数&lt;source&gt; 和&lt;destination&gt;。destination是容器的路径，source可以是url或者启动配置上下文中的一个文件。

```
ADD <source> <destination>
```

5、CMD 提供容器默认执行的命令，Dockerfile只允许使用一次CMD命令。使用多个CMD命令只有最后一个指令生效，之前的都会被抵消。

```
CMD ["executable","param1","param2"]
CMD ["param1","param2"]
CMD comand param1 param2
```

6、EXPOSE 指定容器运行时监听的端口

```
EXPORT <port>
```

7、ENTRYPOINT 配置给容器一个可执行的命令，每次镜像被调用时，仅能运行指定的应用。类似于CMD,Docker只允许一个ENTYPOINT命令

```
ENTRYPOINT ["execuable","param1","param2"]
ENTRYPOINT command param1 param2
```

8、workdir 指定run \cmd \entrypoint 命令的工作目录

```
WORKDIR /usr/bin/workdir
```

9、ENV 设置环境变量。

```
EMV <key> <value>
```

10、USER 镜像正在运行时设置一个UUID

```
USER <uid>
```

11、VOLUME 授权访问从容器到主机目录

```
VOLUME ["/data"]
```

##### 二、Dockerfile的最佳实践

1、使用缓存

为了有效的利用缓存，需要保持Dockerfiles的一致性，并且只在最后添加修改，所有的Dockfiles都以相同的5行开始

```
FROM ubuntu
MAINTAINER Michael Crosby <michael@crosbymichael.com>

RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
```

2、编译镜像时使用标签

```
docker build -t="crosbymichael/sentry"
```

3、端口

docker的两个核心概念是可重复性和可移植性。通过Dockerfiles我们可以映射私有端口和公共的端口。但是我们应尽量不使用其来映射公共端口，否则docker只能运行一个实例。

```
#私有端口和开发端口映射
EXPOSE 80：8080
#只有私有端口
EXPOSE 80
```



