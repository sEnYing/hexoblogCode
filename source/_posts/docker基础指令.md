---
title: docker基础指令
date: 2023-09-26 15:53:31
description: docker基础指令,省的忘记了总去百度
tags:
- docker
categories:
- docker
---

```text
Docker环境信息 — docker [info|version]
容器生命周期管理 — docker
[create|exec|run|start|stop|restart|kill|rm|pause|unpause]
容器操作管理 — docker [ps|inspect|top|attach|wait|export|port|rename|stat]
容器rootfs命令 — docker [commit|cp|diff]
镜像仓库 — docker [login|pull|push|search]
本地镜像管理 — docker [build|images|rmi|tag|save|import|load]
容器资源管理 — docker [volume|network]
系统日志信息 — docker [events|history|logs]
```

```text
pull命令:
-a, --all-tags=true|false : 是否获取仓库中所有镜像，默认为否；
--disable-content-trust : 跳过镜像内容的校验，默认为 true;

images命令,列出本机已有的镜像:
docker images
docker image ls

各个选项说明:
REPOSITORY：表示镜像的仓库源
TAG：镜像的标签
IMAGE ID：镜像ID
CREATED：镜像创建时间
SIZE：镜像大小

save命令: 备份镜像
一个镜像
docker save xxx.tar
多个镜像
docker save \
ubuntu:20.04 \
alpine:3.12.1 \
debian:10.6-slim \
centos:7.8.2003 \
-o linux.tar

load命令: 导入镜像
docker load -i linux.tar

--input , -i : 指定导入的文件。
--quiet , -q : 精简输出信息。

search命令:
docker search tomcat

-f, --filter filter : 过滤输出的内容；
--limit int ：指定搜索内容展示个数;
--no-index : 不截断输出内容；
--no-trunc ：不截断输出内容；

inspect命令:
docker inspect tomcat:9.0.20-jre8-alpine

history命令:
docker history tomcat:9.0.20-jre8-alpine

tag命令:
docker tag tomcat:9.0.20-jre8-alpine lagou/tomcat:9

rmi命令:
docker rmi tomcat:9.0.20-jre8-alpine 
或者
docker image rm tomcat:9.0.20-jre8-alpine

-f, -force : 强制删除镜像，即便有容器引用该镜像；
-no-prune : 不要删除未带标签的父镜像；

清理镜像:
docker image prune

-a, --all : 删除所有没有用的镜像，而不仅仅是临时文件；
-f, --force ：强制删除镜像文件，无需弹出提示确认；

新建并启动容器:
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
docker run -it --rm -p 8080:8080 tomcat:9.0.20-jre8-alpine

-d, --detach=false: 后台运行容器，并返回容器ID
-i, --interactive=false: 以交互模式运行容器，通常与 -t 同时使用
-P, --publish-all=false: 随机端口映射，容器内部端口随机映射到主机的端口。不推荐各位小伙伴使用该参数
-p, --publish=[]: 指定端口映射，格式为：主机(宿主)端口:容器端口，推荐各位小伙伴们使用
-t, --tty=false: 为容器重新分配一个伪输入终端，通常与 -i 同时使用
--name="nginx-lb": 为容器指定一个名称
-h , --hostname="laosiji": 指定容器的hostname
-e , --env=[]: 设置环境变量，容器中可以使用该环境变量
--net="bridge": 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型
--link=[]: 添加链接到另一个容器；不推荐各位小伙伴使用该参数
-v, --volume : 绑定一个卷
--privileged=false: 指定容器是否为特权容器，特权容器拥有所有的capabilities 
--restart=no：指定容器停止后的重启策略
no：容器退出时不重启 
on-failure：容器故障退出（返回值非零）时重启 
always：容器退出时总是重启,推荐各位小伙伴们使用 
--rm=false: 指定容器停止后自动删除容器,不能以docker run -d启动的容器

容器日志:
docker logs [OPTIONS] CONTAINER

-f : 跟踪日志输出
--tail :仅列出最新N条容器日志

删除容器:
docker rm [OPTIONS] CONTAINER [CONTAINER...]

-f :通过 SIGKILL 信号强制删除一个运行中的容器。
-l :移除容器间的网络连接，而非容器本身。
-v :删除与容器关联的卷

列出容器:
docker ps [OPTIONS]

-a :显示所有的容器，包括未运行的。
-q :只显示容器编号。

创建容器:
docker create [OPTIONS] IMAGE [COMMAND] [ARG...]
docker create -it --name tomcat9 -p 8080:8080 9.0.20-jre8-alpine

启动、重启、终止容器:
docker start [OPTIONS] CONTAINER [CONTAINER...]
docker stop [OPTIONS] CONTAINER [CONTAINER...]
docker restart [OPTIONS] CONTAINER [CONTAINER...]

进入容器:
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

-i :即使没有附加也保持STDIN 打开
-t :分配一个伪终端

查看容器:
docker inspect [OPTIONS] NAME|ID [NAME|ID...]

-f :指定返回值的模板文件。
-s :显示总的文件大小。
--type :为指定类型返回JSON。

更新容器:
docker update [OPTIONS] CONTAINER [CONTAINER...]

杀掉容器:
docker kill [OPTIONS] CONTAINER [CONTAINER...]

-s :向容器发送一个信号

cp命令: 用于容器与主机之间的数据拷贝。
宿主机文件复制到容器内
docker cp [OPTIONS] SRC_PATH CONTAINER:DEST_PATH
容器内文件复制到宿主机
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH

-L :保持源目标中的链接
```
