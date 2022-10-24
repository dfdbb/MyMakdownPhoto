# Docker

### Dockers安装

1. yum 包更新
	 先将yum源 切换为国内 ： 
	 `yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo`
		随后更新： `yum update`

2. 安装需要的软件包，yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的
`yum install -y yum-utils device-mapper-persistent-data lvm2
`
3.Docker 安装：
`yum install -y docker-ce`   
-y为自动提示，自动输入yes

	查看版本，验证是否安装成功
	`docker -v`


### Docker架构
![](https://www.runoob.com/wp-content/uploads/2016/04/576507-docker1.png)

### 配置Docker镜像加速器

阿里云加速地址： https://jqrqu06z.mirror.aliyuncs.com

配置：
sudo mkdir -p /etc/docker sudo tee /etc/docker/daemon.json <<-'EOF' { "registry-mirrors": ["https://jqrqu06z.mirror.aliyuncs.com"] } EOF sudo systemctl daemon-reload sudo systemctl restart docker


### Docker命令

#### Docker服务
启动Docker:  `systemctl start docker`
查看状态： `systemctl stauts docker`
停止Docker: `systemctl stop docker`
重启DOcker: `systemctl restart docker`
开机自启:   `systemctl enable docker`

#### Docker镜像
- 查看镜像					`docker images`
- 搜索镜像					`docker search xxxx`
- 拉取镜像					`docker pull xxx:yy`
- 删除镜像					`docker rmi id`			删除全部:  `docker rmi `docker images -q
``



#### Docker容器命令

- 运行Docker:		  `docker run -it --name=xx centos:7 /bin/bash `		
- 查看在运行的容器：		`docker ps`    
- exec 进入容器：					`docker exec -it c2 /bin/bash`
- 关闭Docker:		`docker stop xxx`
- 查看容器信息		`docker inspect xxx`

##### 参数说明
-i：保持容器运行。通常与-t同时使用。加入it这两个参数后，容器创建后自动进入容器中，退出容器后，容器自动关闭
-t：为容器重新分配一个伪输入终端，通常与-i同时使用
-d：以守护（后台）模式运行容器。创建一个容器在后台运行，需要使用docker exec进入容器docker exec -it c2 /bin/bash。退出后，容器不会关闭
-it创建的容器一般称为交互式容器；-id创建的容器一般称为守护式容器
--name：为创建的容器命名

# 数据卷

### 数据卷概念与作用

思考：
		docker容器删除后，容器内的数据还在吗？
		docker容器和外部机器可以直接交换文件吗？
		容器之间如何进行数据交互

### 数据卷
- 数据卷是宿主机中的一个文件或目录
- 当容器目录与数据卷目录绑定之后，修改会立即同步
- 一个数据卷可以被多个容器挂载
- 一个容器也可以挂载多个数据卷

### 数据卷的作用
- 容器数据持久化
- 外部机器和容器间通信
- 容器间数据交换


### 配置数据卷

创建容器时，使用-v参数，设置数据卷
`docker run .... -v 宿主机目录文件：容器内目录文件 .....`
`docker run -it --name c1 -v /root/data:/root/data_container centos:7 /bin/bash
`
`docker run -it --name c2 -v /root/data2:/root/data2 -v /root/data3:/root/data3 centos:7 /bin/bash`


- 目录必须是绝对路径
-
- 2.目录不存在，自动创建
- 3.可挂载多个数据卷

#### 通过两个容器挂载相同数据卷，实现数据交换
   
   - 新建两个容器，并且挂载到相同数据卷
   - docker run -it --name c1 -v /root/data:/root/data centos:7 
   - docker run -it --name c2 -v /root/data:/root/data centos:7 

### 数据卷容器
多容器进行数据卷交换 
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/18/PrqpqiWwoWaRfQ56.png)



配置数据卷容器： 
1.创建c3数据卷容器，使用-v参数，设置数据卷
`docker run -it --name c3 -v /volume centos:7 /bin/bash`
2.将c1,c2挂载到c3
`docker run -it --name c1 --volumes-from c3 centos:7 /bin/bash`
`docker run -it --name c2 --volumes-from c3 centos:7 /bin/bash`

## Docker应用

### Mysql
	需求：
		在Docker容器中部署Mysql,并通过外部客户端操作MysqlServer.
	
	步骤：
		1.搜索Mysql镜像
		2.摘取镜像
		3.创建容器
端口映射：
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/18/zIp9hmYVClUJPBgC.png)

实操：
		1.搜索镜像：
		`docker search mysql`
		2.拉取镜像：
		`docker pull mysql:5.6`
		3.创建容器(设置端口映射，目录映射)：
		&emsp;&emsp;&emsp;1.在/root目录下创建Mysql目录用于存储mysql信息
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;`mkdir mysql`	 `cd mysql`			
&emsp;&emsp;&emsp;2.：
`docker run -id -p 3307:3306 --name c_mysql -v $PWD/conf:/etc/mysql/conf.d -v $PWD/logs:/logs -v $PWD/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root mysql:5.6`	

参数说明
-p 3307:3306：将容器的3306端口映射到宿主机的3307端口
--v $PWD/conf:/etc/mysql/conf.d：将主机当前目录下的conf/my.cnf挂载到容器/etc/mysql/my.cnf配置目录
-v $PWD/logs:/logs：将主机当前目录下的logs目录挂载到容器的/logs目录日志
-v $PWD/data:/var/lib/mysql：将主机当前目录下的data目录挂载到容器的/var/lib/mysql数据目录
-e MYSQL_ROOT_PASSWORD=123456：初始化root 用户密码

### Docker部署Tomcat 
需求：
在Docker中部署Tomcat ,且通过外部机器访问Tomcat部署的项目

步骤：
1，搜索镜像
2，拉取镜像
3，运行容器
4，部署项目
5，访问测试


实现：
1.`docker search tomcat`
2.`docker pull tomcat`
3.`mkdir tomcat cd tomcat`
4.`docker run -id --name c_tomcat -p 8080:8080 -v $pwd:/usr/local/tomcat/webapps tomcat`
`docker run -id --name=c_tomcat  -p 8080:8080  -v $PWD:/usr/local/tomcat/webapps  tomcat`

5.外部机器访问：
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/18/AeFHwTm6pC1Nlgl2.png)
					 


### Docker部署Ngnix

需求：
在DOcker中部署Ngnix ,且通过外部机器访问

步骤：
1，搜索镜像
2，拉取镜像
3，运行容器
docker run -id --name c_nginx -p 80:80 -v $PWD/conf/nginx.conf:/etc/conf/nginx.conf -v $PWD/log:/var/log/nginx -v $PWD/html:/usr/share/nginx/html  nginx

4，访问测试

### Redis部署

步骤：
1，搜索镜像
2，拉取镜像
3，运行容器


### DockerFile

#### Docker镜像原理

- Docker镜像的本质是什么?
- Docker中Centos为什么只要两百MB，而一个Centos操作系统要几个G？
- Docker 中Tomcat为什么有500MB,而一个tomcat安装包只有70MB？






##### linux文件管理系统 
linux文件系统由bootfs和rootfs组成
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/19/yMOqa71jTeGVAqzQ.png)

- bootfs：包含bootloader（引导加载程序）和kernel（内核）
- rootfs：root文件系统，包含的就是典型的Linux 系统中的/dev、/proc、/bin等标准目录和文件
- 不同的Linux 发行版，bootfs 基本一样，而rootfs 不同，如ubuntu，CentOS等





#### Docker镜像是由特殊的文件系统叠加而成
#### 最底端是bootfs,并使用宿主机的bootfs
#### 第二层是root 文件系统rootfs ，称为base image
#### 然后再往上可以叠加其他的镜像文件


- 统一文件系统（Union File System）技术能够将不同的层整合成一个文件系统，为这些层提供了一个统一的视角，这样就隐藏了多层的存在，在用户的角度看来，只存在一个文件系统
- 一个镜像可以放在另一个镜像的上面。位于下面的镜像称为父镜像，最底部的镜像称为基础镜像
- 当从一个镜像启动容器时，Docker会在最顶层加载一个读写文件系统作为容器


复用

![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/19/uWPggmPk3yLpYB0j.png)



### Docker镜像制作
1.容器转为镜像

`docker commit 容器id 镜像名称：版本号`

- 镜像保存为压缩文件
`docker save -o 压缩文件名 镜像名称：版本号`

-压缩文件加载为镜像：
`docker load -i 压缩文件名`

commit 不是目录挂载的文件可以生效


### DockerFile 概念
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/19/FJAQwmGuqRbVOHvx.png)

- dockerfile 是一个文本文件
- 包含一条条的指令
- 每一条指令构建一层镜像，基于基础镜像，最终构建出一个新的镜像
- 对开发人员来説：可以为开发团队提供一个完全一致的开发环境
-  对于测试人员，可以直接拿开发时所构建的镜像或者通过Dockerfile 文件构建一个新的镜像开始工作了
-   对于运维人员，在部署时，可以实现应用的无缝移植


### DockerFile关键字

![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/21/3gWvoCVo4K867d9L.png)



### DockerFile自定义centos7镜像

1.默认登录路径为/usr
2.可以使用vim

实现步骤:
1.定义父镜像：
`FROM centos:7`
2.定义作者信息：	
`MAINTAINER young`
3.执行yum安装命令：
`RUN yum install -y vim`
4.定义默认的工作目录：
`WORKDIR/usr`
5.  `CMD/bin/bash`


### DockerFile发布Spirngboot项目

需求：
定义DockerFile，发布Springboot项目


1.定义父镜像：
`FROM java:8`
2.定义作者信息：
`MAINTAINER young`
3.将jar包添加到容器：
`ADD springboot_demo.jar app.jar`
4.定义容器启动执行的命令：
`CMD java jar app.jar`
5.通过build命令来构建对应的镜像：
`docker build -f dockerfile路径 -t 镜像名：版本 .`


### DockerCompose

#### 概述

#### 服务编排的概念
 微服务架构的应用系统中一般包含若干个微服务，每个微服务都会部署多个实例，如果每个微服务都要手动启动，维护工作量会很大

要从Dockerfile build image 或者去 dockerhub 拉取image
要创建多个container
要管理这些container（启动停止删除）
服务编排：

​ 按照一定的业务规则批量管理容器



## docker compose可跳过，服务编排后续使用k8s做


### Dockers Compose 概述
​ Docker Compose 是一个编排多容器分布式部署的工具，提供命令集管理器化应用的完整开发期，包括服务构建，启动和停止。使用步骤：

利用 Dockerfile 定义运行环境镜像
使用 docker-compose.yml 定义组成应用的各服务
运行 docker-compose up 启动应用


### DockerCompose

安装DockerCompose:
`> curl -L https://get.daocloud.io/docker/compose/releases/download/v2.4.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose`

案例：

1.创建dockercompose文件夹
`mkdir docker-compose`


2.编写docker-compose.yml文件
`version: '3'
services:
  nginx:
   image: nginx
   ports:
    - 80:80
   links:
    - app
   volumes:
   	- ./nginx/conf.d:/etc/nginx/conf.d
  app:
    image: app
    expose:
      - "8080" `
      
3.编写



## docker私有仓库
### 一.私有仓库搭建：
1.摘取镜像
`docker pull registry`
私有仓库也是一个镜像

2.创建私有仓库容器：
`docker run -id --name=registry -p 5000:5000 registry`

3.  打开浏览器，输入地址`https://私有仓库服务器ip:5000/v2/_catalog`看到`{"repositories":[]}`表示私有仓库搭建成功

4.修改deamon.json,信任私有仓库:
`vim /etc/docker/daemon.json`
`#在上述文件中添加一个key，保存退出。  #此步用于让docker信任私有仓库地址  #注意将私有仓库服务器ip修改为自己私有仓库服务器真实ip  {"insecure-registries":  ["私有仓库服务器ip:5000"]}`

5.重启docker服务：
`systemctl restart docker  `
`docker start registry`


二、上传镜像到私有仓库
**docker tag :** 标记本地镜像，将其归入某一仓库。

1.标记镜像为私有仓库的镜像：
`docker tag centos:7 私有仓库服务器ip:5000/centos:7`

2.上传标记的镜像：
`docker push 私有仓库服务器ip:5000/centos:7`

![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/26/GmFxxH7DItAnrXmM.png)
三，拉取私有仓库镜像：
1.拉取镜像：
#拉取镜像
`#拉取镜像
docker pull 私有仓库服务器ip:5000/centos:7
`




## docker和虚拟机的区别
**Docker容器虚拟化 与 传统虚拟机比较**

容器就是将软件打包成标准化单元，以用于开发、交付和部署

-   容器镜像是轻量级的、可执行的独立软件包，包含软件运行所需要的所有内容：代码、运行时环境、系统工具、系统库和设置

-   容器化软件在任何环境中都能够始终如一地运行
-   容器赋予了软件独立性，使其免受外在环境差异的影响，从而有助于减少团队间在相同基础设施上运行不同软件时的冲突

**相同**
-   容器和虚拟机具有相似的资源隔离和分配优势

**不同：**
-   容器虚拟化的是操作系统，虚拟机虚拟化的时硬件
-   传统的虚拟机可以运行不同的操作系统，容器只能运行同一类型的操作系统

![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/26/WtwkHcPNBSVidfLe.png)



![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/26/e2dMo5JSwotI9zJi.png)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU2NzQxMjEzOF19
-->