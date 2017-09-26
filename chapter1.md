# Docker File的编写

Dockerfile包含了创建镜像所需要的全部命令，是用来支持docker自动化的一种手段。编写完dockerfile可以使用docker build命令来自动化创建docker镜像。

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





















