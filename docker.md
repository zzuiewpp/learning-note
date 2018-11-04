# 特性

+ 镜像
+ 容器
+ 仓库

# 安装

```
brew cask install docker
```

或直接下载安装包，双击安装即可

测试第一个示例

```
docker run hello-world
```

# 基本命令

+ docker images：查看本地所有镜像
+ docker search XXX：在docker HUB上查找镜像
+ docker pull XXX：从hub获取镜像到本地
+ docker rmi xxx：从本地移除镜像
+ docker rmi -f xxx：强制移除镜像
+ docker run -it xxx/或image ID：创建并运行该容器，-t表示运行后进入到容器终端
+ exit：关闭并退出当前运行的容器
+ Ctrl+p+q：退出容器，但不停止运行
+ docker stop 容器ID：停止容器运行
+ docker ps：查看与运行过的容器的状态（包含当前正在运行的和已经被关闭的）
+ docker start 容器ID或容器名
+ docker restart 容器ID或容器名
+ docker kill 容器ID或容器名：强制停止容器
+ docker logs -f -t --tail 显示的数目 容器ID：查看日志
+ docker run -d image：启动守护进程容器
+ docker top 容器ID：查看容器内运行的进程
+ docker exec -it 容器ID baseShell：进入指定容器，并执行BashShell命令
+ docker attach 容器ID：重新进入指定容器
+ docker cp 容器ID:/tmp/yum.log ~/XXX：将容器中某个文件拷贝到宿主机指定目下

# Docker镜像Commit

HUB上pull下来的镜像，自己可以个性化配置，然后再提交成为一个新的image

docker commit提交容器副本使之成为一个新的镜像

```
docker commit -m="提交说明" -a="作者" 容器ID 新的镜像名:[tag标签名]
```

+ 启动Tomcat容器

```
docker run -d -p docker容器的端口号:镜像Tomcat内置端口(8080) 容器名/容器ID
```

+ 提交Tomcat副本

```
docker exec -it 容器ID /bin/bash	交互启动容器
ls -al
cd webapps/
rm -rf docs/	移除Tomcat中的doc文档
dokcer commit -m="delete docs" -a="walker" 容器ID avatar/tomcat01:1.0.0
docker run -d -p 9999:8080 avatar/tomcat01:1.0.0	启动容器副本
#和HUB上的Tomcat对比
```

# 数据卷

## 简介

***容器和宿主机之间数据共享，持久化***

```
#挂载，使得宿主机和容器中对应目录下的文件数据可以被共享
#注意，宿主机中文件夹的权限问题
docker run -it -v ~/宿主机文件夹:/容器文件夹 imageId
```

## 数据共享

**执行完上命令后，当宿主机或者容器更新了对应文件夹下的文件后了，两者会同步数据**

**当容器关闭，下次再重新进入该容器（同一个容器ID）后，宿主机更新过的文件也会被同步到容器**

```
#重新进入某个容器
docker ps -l		查看所有运行和运行过的容器
docker start 容器ID
docker attach 容器ID	重新进入该容器
```

## 设置容器权限

```
docker run -it -v ~/宿主机文件夹:/容器文件夹 -ro imageId	表示容器只有读权限
```

容器中该目录下只有读操作权限，数据的更新只能由宿主机完成