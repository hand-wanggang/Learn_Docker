## 在Docker中启动一个服务

1、下载本示例中的代码（这是之前学习spring cloud的一个Demo，使用gradle构建）

[https://github.com/hand-wanggang/erueka-demo/tree/master/eureka-server1](https://github.com/hand-wanggang/erueka-demo/tree/master/eureka-server1)

2、确保linux上安装了gradle

```
#查看是否安装了gradle
gradle -version

#如果没安装，建议去官网下载gradle，并进行安装（官网有详细的安装过程）
#ubuntu软件源中的gradle版本较低，建议使用版本4.1/4.2(不低于3.1即可)
```

3、Docker要运行早内核版本大于3.1的linux系统中

```
#查看内核版本
$ uname -v
```

4、使用git将代码拉取到本地

```
#创建一个文件夹
mkdir Demo

#进入该文件夹
cd Demo

#克隆git代码
git clone https://github.com/hand-wanggang/erueka-demo.git
```

执行以上命令：会发现Demo文件下存在文件夹eureka-demo

5、使用gradle将eureka-server1项目打包成jar包

```
#进入工作目录
cd eureka-demo/eure-server1
#gradle进行打包
gradle bootRepackage
```

执行完以上命令后，会生成一个build目录，build/libs下会有一个文件eureka-server1-0.0.1-SNAPSHOT.jar

6、在src/main/sh/下添加文件entrypoint.sh，内容如下（主要是内核参数和联网设置）

```
#!/bin/bash

sysctl -w fs.aio-max-nr=1048576
sysctl -w fs.file-max=6815744
sysctl -w kernel.shmall=6815744
sysctl -w kernel.shmmax=6815744
sysctl -w kernel.shmmni=6815744
sysctl -w kernel.sem=250 32000 100 128
sysctl -w net.ipv4.ip_local_port_range=9000 65500
sysctl -w net.core.rmem_default=262144
sysctl -w net.core.rmem_max=6815744
sysctl -w net.core.wmem_default=262144
sysctl -w net.core.wmem_max=6815744
sysctl -w net.ipv4.tcp_keepalive_time=30
sysctl -w net.ipv4.tcp_keepalive_intvl=1
sysctl -w net.ipv4.tcp_keepalive_probes=10
```

7、编写Dockerfile，在项目的根目录下即可eureka-demo/eure-server1/

```
#设置基础镜像
FROM anapsix/alpine-java:8_jdk
#copy文件到app.jar
ADD build/libs/*.jar /app.jar
#拷贝文件
ADD src/main/sh/entrypoint.sh /entrypoint.sh
#给脚本添加执行权限
RUN chmod +x /entrypoint.sh
#设置环境变量，因为使用了spring的profile
ENV spring.profiles.active="server1"
#设置虚拟机参数
ENV JAVA_OPTS="-Xmx256m -Xms64m -XX:MinHeapDeltaBytes=65536 -Dcom.sun.management.jmxremote=true -Dcom.sun.management.jmxremote.port=11601 -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false"
#docker启动时执行的命令
ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar /app.jar" ]
```

8、生成docker镜像

```
#生成docker镜像,不要忘记最后一个点，这表明dockerfile在当前目录下，如果dockerfile不在当前目录，需要指定
docker build -t demo/erueka-server-v1.0 .

#执行上面的命令后，查看镜像是否生成成功
docker images | grep eureka-server-v1.0

```

![](/assets/import-docker-1.png)

9、运行一个docker实例

```
#运行生成的镜像，并进行端口映射
docker run -p 8761:8761 demo/eureka-server-v1.0
```

```
#查看当前正在运行的docker实例
docker ps
```

![](/assets/import.png)

