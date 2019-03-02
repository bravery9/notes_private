
>Docker学习笔记 对官方文档的学习 https://docs.docker.com/get-started/


### Docker国内镜像源

```
https://docker.mirrors.ustc.edu.cn
https://hub-mirror.c.163.com
```

### 基本命令

```
# 列出Docker命令行命令 List Docker CLI commands
docker
docker container --help

# Display Docker version and info
docker --version
docker version
docker info

# Execute Docker image
docker run hello-world

##列出所有镜像 List Docker images
docker image ls

## 列出容器 List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -aq
```

### Dockerfile

https://docs.docker.com/get-started/part2/


images被Dockerfile定义。


### 建立与某容器的交互shell

使用`docker attach`命令进入container有一个缺点：每次从container中输入exit则container也跟着退出了。

所以推荐使用命令`docker exec -it`进入container，当退出container后，container仍然在后台运行，命令使用方法如下：

```
docker exec -it goofy_almeida /bin/bash
```
goofy_almeida 为要启动的container的名称

/bin/bash 表示在container中启动一个bash shell


这样的情况下在交互式shell输入`exit`或者按键`Ctrl + C`退出container时，这个container仍然在后台运行。

docker ps
可以看见。
