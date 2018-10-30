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