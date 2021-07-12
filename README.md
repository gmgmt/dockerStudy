# dockerStudy

@[TOC](目录)
# 基本概念
本文中用到的在线docker网站：[link](https://labs.play-with-docker.com/)
## 镜像(image)
**镜像：创建容器的模板，根据不同配置的镜像来创建不同的容器使用。镜像和容器的关系可以理解为面向对象中类和实例对象的关系。**
## 容器(container)
**容器：具体的运行应用程序的一个进程，它里面包含应用程序的各种依赖。**
## 仓库(registry)
**仓库：一个镜像只可以创建一种类型的容器，镜像多了就需要放到镜像仓库中存起来，仓库有本地镜像仓库和公共镜像仓库，平时使用本地仓库的镜像，没有的话去Registry hub公共镜像仓库下载。**
# 基本命令
## 镜像相关操作
```
1、下载镜像：(默认最新版本)
	docker pull nginx	等价于 docker pull nginx:latest
2、查看本地镜像：
	docker images
3、运行镜像：（返回容器ID）
	docker run -d -p 80:80 nginx
	-d 后台运行
	-p 内外端口映射    外部端口：内部端口
4、查看正常运行的容器：（返回运行的容器的基本信息）
	docker ps
```
## 容器相关操作
```
1、进入容器：
	docker exec -it e5 bash
	e5：容器id前几位（标志位，会自动查找，能唯一识别）
2、修改展示页面：
	cd /usr/share/nginx/html/
	echo hello > index.html
3、退出容器：
	exit
4、删除容器：
	docker rm -f 1d
	1d：容器id前几位（标志位，会自动查找，能唯一识别）
5、commit容器：
	docker commit e5 test
	e5：容器id前几位（标志位，会自动查找，能唯一识别）
```
## Dockerfile构建镜像并运行
```
1、新建Dockerfile文件：
	FROM nginx
	ADD ./ /usr/share/nginx/html/
2、新建index.html：
	outSide words
3、构建镜像：
	docker build -t test2 .
	test2：容器名
	.：当前目录下的Dockerfile文件
4、运行镜像：
	docker run -d -p 100:80 test2 
5、查看容器：
	docker ps
```
## 通过tar文件压缩和解压镜像
```
1、将镜像保存带tar文件：
	docker save test2 > 1.tar
	test2：镜像名
2、删除镜像test2：（前提镜像不能运行为容器）
	docker rmi test2
3、删除容器：
	docker rm -f 78
	78：容器id前几位（标志位，会自动查找，能唯一识别）
4、删除镜像：
	docker rmi test2
	test2：镜像名
5、查看本地镜像：
	docker images
6、tar加载镜像：
	docker load < 1.tar
```
**使用笔记**
```
安装和常用CLI：
添加阿里云镜像：sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
安装命令：sudo yum install -y  docker-ce docker-ce-cli containerd.io
启动命令：sudo systemctl start docker
添加当前用户到docker用户组：sudo usermod -aG docker $USER （需注销），newgrp docker （立即生效）
Helloworld：docker run hello-world  （本地没有镜像的话会自动从远端仓库pull）
pull nginx 镜像：docker pull nginx（等效于nginx:latest）
运行：docker run -【d】（后台运行不阻塞shell） 【-p 80:80】（指定容器端口映射，内部：外部） nginx
查看正在运行：docker ps
删除容器：docker rm -f <container id(不用打全，前缀区分)>
进入bash：docker exec -it <container id(不用打全，前缀区分)> bash
commit镜像：docker commit <container id(不用打全，前缀区分)>  <name>
查看镜像列表：docker images （刚才commit的镜像）
使用运行刚才commit的镜像：docker run -d <name>
使用Dockerfile构建镜像：docker build -t <name> <存放Dockerfile的文件夹>
删除镜像：docker rmi <name>
保存为tar：docker save <name> > <tar name>
从tar加载：docker load < <tar name>

一些启动参数：
后台运行容器：-d
容器内外端口映射：-p 内部端口号:外部端口号
目录映射：-v 'dir name' : <dir>
指定映像版本：<name>:<ver>

Docker build 创建镜像
Docker run  利用镜像运行容器
Docker image 关于镜像的一系列操作
Docker pull  从镜像仓库下载镜像到本地仓库
Docker push  上传镜像到镜像仓库
Docker container 执行关于容器的一系列操作
Docker stats 实时监控该节点容器的资源使用情况
```
[大佬视频教程链接](https://www.bilibili.com/video/BV1R4411F7t9)