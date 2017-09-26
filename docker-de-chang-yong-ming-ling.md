# Docker的常用命令

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
docker push new_image_name
```





























