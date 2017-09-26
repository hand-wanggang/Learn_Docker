# Ubuntu安装Docker

##### 一、不使用sudo的情况下，使用docker命令

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



