---
date: 2017-05-07 22:28:31+00:00
layout: post
title: dotnet Core 在Docker的部署
categories: 文档
tags: 问题 .net
---

## Docker 常用命令

`docker info` 检查Docker的安装  

`docker pull image` 拉取一个预建的镜像  

`docker help` 所有Docker命令  

`sample_job=$(docker run -d busybox /bin/sh -c "while true; do echo Docker; sleep 1; done")`
以后台进程的方式运行hello docker
sample_job命令会隔一秒打印一次Docker，使用Docker logs可以查看输出。如果没有起名字，那这个job会被分配一个id，以后使用命令例如Docker logs查看日志会变得比较麻烦。  

`docker stop $sample_job` 停止名为sample_job的容器  

`docker restart $sample_job` 重新启动该容器  

`docker stop $sample_job docker rm $sample_job` 如果要完全移除容器，需要将该容器停止，然后才能移除  

`docker commit $sample_job job1` 将容器的状态保存为镜像  

`docker images` 查看所有镜像的列表  

`docker rmi -f <image id>` 清除单个镜像  

## Docker安装dotnet core镜像
### 镜像
`docker pull microsoft/dotnet`下载镜像  
`docker images`检查镜像
### Dockerfile部署
1. 程序命令行切换到publish文件目录中。
2. touch Dockerfile：新建一个Dockerfile文件。
3. vim Dockerfile：使用Vim来编辑Dockerfile。
  输入以下内容：
  ```
    #基于 `microsoft/dotnet:1.0.0-core` 来构建我们的镜像
    FROM microsoft/dotnet:1.0.0-core

    #拷贝项目publish文件夹中的所有文件到 docker容器中的publish文件夹中  
    COPY . /publish

    #设置工作目录为 `/publish` 文件夹，即容器启动默认的文件夹
    WORKDIR /publish

    #设置Docker容器对外暴露60000端口
    EXPOSE 60000

    #使用`dotnet HelloWebApp.dll`来运行应用程序

    CMD ["dotnet", "HelloWebApp.dll", "--server.urls", "http://*:60000"]
  ```
4. `docker build -t HelloWebApp:1.0`构建一个镜像
5. `docker run --name HelloWebApp -d -p 60000:60000 HelloWebApp:1.0`
