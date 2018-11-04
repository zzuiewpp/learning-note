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