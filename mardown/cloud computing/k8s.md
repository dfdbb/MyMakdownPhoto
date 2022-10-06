kubeadm init \
 --apiserver-advertise-address=192.168.88.100  \
 --image-repository  registry.aliyuncs.com/google_containers  \
 --kubernetes-version=v1.17.4  \
 --service-cidr=10.96.0.0/12  \
 --pod-network-cidr=10.244.0.0/16
 
`kubectl apply -f `
重新init 先执行 kubeadm reset




![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/10/06/Y9J1aFf8bDX2OKmf.png)

## 三

### 3.1 资源管理介绍 
**在k8s中，所有的内容都被抽象为资源，用户须要通过操作资源来管理k8s**

kubernetes的本质上就是一个集群系统，用户可以在集群中部署各种服务，所谓的部署服务，其实就是在kubernetes集群中运行一个个的容器，并将指定的程序跑在容器中。

kubernetes的最小管理单元是pod而不是容器，所以只能将容器放在`Pod`中，而kubernetes一般也不会直接管理Pod，而是通过`Pod控制器`来管理Pod的。

Pod可以提供服务之后，就要考虑如何访问Pod中服务，kubernetes提供了`Service`资源实现这个功能。

当然，如果Pod中程序的数据需要持久化，kubernetes还提供了各种`存储`系统。

**学习kubernetes的核心，就是学习如何对集群上的`Pod、Pod控制器、Service、存储`等各种资源进行操作**

### 3.2YAML介绍
YAML是一个类似 XML、JSON 的标记性语言。它强调以**数据**为中心，并不是以标识语言为重点。因而YAML本身的定义比较简单，号称"一种人性化的数据格式语言"。

```xml
<young>
	<age>18</age>
	<address>guiyang</address>
</young>
```
```yml
young:
	age:18
	address:qinzheng
```
YAML语法简单，主要有：
-   大小写敏感
-   使用缩进表示层级关系
-    缩进不允许使用tab，只允许空格( 低版本限制 )
-   缩进的空格数不重要，只要相同层级的元素左对齐即可
-   '#'表示注释

YAML支持的数据类型:
-   纯量：单个的、不可再分的值
-   对象：键值对的集合，又称为映射（mapping）/ 哈希（hash） / 字典（dictionary）
-   数组：一组按次序排列的值，又称为序列（sequence） / 列表（list）

```yml
# 纯量, 就是指的一个简单的值，字符串、布尔值、整数、浮点数、Null、时间、日期
# 1 布尔类型
c1: true (或者True)
# 2 整型
c2: 234
# 3 浮点型
c3: 3.14
# 4 null类型 
c4: ~  # 使用~表示null
# 5 日期类型
c5: 2018-02-17    # 日期必须使用ISO 8601格式，即yyyy-MM-dd
# 6 时间类型
c6: 2018-02-17T15:02:31+08:00  # 时间使用ISO 8601格式，时间和日期之间使用T连接，最后使用+代表时区
# 7 字符串类型
c7: heima     # 简单写法，直接写值 , 如果字符串中间有特殊字符，必须使用双引号或者单引号包裹 
c8: line1
 line2     # 字符串过多的情况可以拆成多行，每一行会被转化成一个空格 


# 对象
# 形式一(推荐):
heima:
 age: 15
 address: Beijing
# 形式二(了解):
heima: {age: 15,address: Beijing}


# 数组
# 形式一(推荐):
address:
 - 顺义
 - 昌平 
 
young: 
	- 18
	- 10
	- 20
# 形式二(了解):
address: [顺义,昌平]
 ```

### 3.3资源管理方式
-命令式管理方式：  直接使用命令去操作kubernetes资源
`kubectl run nginx-pod --image=ngixn:1.17.1 --port=80`

- 命令式对象配置： 通过命令配置和配置文件去操作kubernetes资源
`kubectl create/patch -f nginx-pod.yml`

-声明式对象配置： 通过apply命令和配置文件去操作kubernetes资源 
`kubectl apply -f nginx-pod.yml`		apply只能完成创建和更新

![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/28/LBt91SY9IAstQfjg.png)

### 3.3.1命令式管理方式
**kubectl命令**

kubectl是kubernetes集群的命令行工具，通过它能够对集群本身进行管理，并能够在集群上进行容器化应用的安装部署。kubectl命令的语法如下：

`kubectl [command] [type] [name] [flags]`

**comand**：指定要对资源执行的操作，例如create、get、delete

**type**：指定资源类型，比如deployment、pod、service

**name**：指定资源的名称，名称大小写敏感

**flags**：指定额外的可选参数

ps:

```
# 查看所有pod
kubectl get pod

# 查看某个pd
# kubectl get pod podname
kubectl get pod nginx-6867cdf567-c9psb 

#查看详细信息
kubectl get pod nginx-6867cdf567-c9psb -o wide

#以yaml格式查看详细信息
kubectl get pod nginx-6867cdf567-c9psb -o yaml
kubectl get pod nginx-6867cdf567-c9psb -o json
```


**资源类型**

kubernetes中所有的内容都抽象为资源，可以通过下面的命令进行查看:
`kubectl api-resources`

经常使用的资源有下面:

|资源分类|资源名称|缩写|资源作用|
|:--|:---|:---|:---|
|集群级别资源|nodes|no|集群组成部分|
|namespaces|ns|隔离Pod||
|pod资源|pods|po|装载容器|
|pod资源控制器|replicationcontrollers|rc|控制pod资源|
||replicasets|rs|控制pod资源|
||deployments|deploy|控制pod资源|
||daemonset|ds|控制pod资源|
||jobs||控制pod资源|
||cronjobs|cj|控制pod资源|
||horizontalpodautoscalers|hpa|控制pod资源|
||sattefulsets|sts|控制pod资源|
|服务发现资源|services|svc|统一pod对外接口|
||ingrss|ing|统一pod对外接口|
|存储资源|volumeattachments||存储|
||persistentvolumes|pv|存储|
||persistentvolumeclaims|pvc|存储|
|配置资源|configmaps|cm|配置|
||secrets||配置|


**操作**

kubernetes允许对资源进行多种操作，可以通过--help查看详细的操作命令

常用操作方式：
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/28/YGCkSpFeWwSX6EUj.png)




例一：
下面以一个namespace / pod的创建和删除简单演示下命令的使用：

1.创建一个namespace:
`kubectl create namespace dev`
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/28/93vXiKLEmVmRiALb.png)
2.查看namespace:
`kubectl get ns`
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/28/uKRJXYDi7PTK4wp7.png)
3.在ns下创建并运行一个nginx 的 pod
`kubectl run pod --image=nginx -n dev`
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/28/PzqR2maa4LuNDKzx.png)

4.查看新创建的pod
`kubectl get pod -n dev`
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/28/6KdtWMAcsV6RnDBk.png)

5.删除pod
`kubectl delete pods pod-864f9875b9-ns9md -n dev`
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/28/3oDpucmauoGZ0w29.png)

6.删除namespace
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/28/KnojAnCR0tl2cP00.png)


### 3.3.2命令式对象配置
 
 1.编写yaml文件
 ```yaml
 apiVersion: v1
kind: Namespace
metadata:
 name: dev
---
apiVersion: v1
kind: Pod
metadata:
 name: nginxpod
 namespace: dev
spec:
 containers:
 - name: nginx-containers
 image: nginx:latest
 ```
2.执行命令，创建资源：
`kubectl create -f nginxpod.yaml `
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/28/xQ74bjaKc6I3nbVa.png)

3.查询namespace是否创建成功
`kubectl get ns `
4.查询pod是否创建成功
`kubectl get pod -n dev`

5.删除ns和pod
![`!\[\](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/28/vgIRV2UydQ7ru4Nt.png)`](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/28/LOYlayULso6ipPGj.png)


### 3.3.3声明式对象配置

声明式对象配置跟命令式对象配置很相似，但是它只有一个命令apply。

1.执行apply
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/28/vJVVvrRwQsdasf6A.png)

2.再次执行apply 
![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/28/J3HnYmjqFepk7Fg9.png)

总结:
 其实声明式对象配置就是使用apply描述一个资源最终的状态（在yaml中定义状态）
 使用apply操作资源：
 如果资源不存在，就创建，相当于  kubectl  create
 如果资源已存在，就更新，相当于  kubectl  patch


>扩展：kubectl可以在node节点上运行吗 ?

kubectl的运行是需要进行配置的，它的配置文件是$HOME/.kube，如果想要在node节点运行此命令，需要将master上的.kube文件复制到node节点上，即在master节点上执行下面操作：
`scp  -r  HOME/.kube   node1: HOME/`


>  使用推荐: 三种方式应该怎么用 ?


创建/更新资源 使用声明式对象配置 kubectl apply -f XXX.yaml

删除资源 使用命令式对象配置 kubectl delete -f XXX.yaml

查询资源 使用命令式对象管理 kubectl get(describe) 资源名称

## 4.实战入门
本章节将介绍如何在kubernetes集群中部署一个nginx服务，并且能够对其进行访问。

Namespace是kubernetes系统中的一种非常重要资源，它的主要作用是用来实现**多套环境的资源隔离**或者**多租户的资源隔离**。

默认情况下，kubernetes集群中的所有的Pod都是可以相互访问的。但是在实际中，可能不想让两个Pod之间进行互相的访问，那此时就可以将两个Pod划分到不同的namespace下。kubernetes通过将集群内部的资源分配到不同的Namespace中，可以形成逻辑上的"组"，以方便不同的组的资源进行隔离使用和管理。

可以通过kubernetes的授权机制，将不同的namespace交给不同租户进行管理，这样就实现了多租户的资源隔离。此时还能结合kubernetes的资源配额机制，限定不同租户能占用的资源，例如CPU使用量、内存使用量等等，来实现租户可用资源的管理。

![](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/09/30/7tHrtju5Hwoo96zF.png)


kubernetes在集群启动之后，会默认创建几个namespace

`[root@master ~]# kubectl  get namespace
NAME              STATUS   AGE
default           Active   45h     #  所有未指定Namespace的对象都会被分配在default命名空间
kube-node-lease   Active   45h     #  集群节点之间的心跳维护，v1.13开始引入
kube-public       Active   45h     #  此命名空间下的资源可以被所有人访问（包括未认证用户）
kube-system       Active   45h     #  所有由Kubernetes系统创建的资源都处于这个命名空间`


下面来看namespace资源的具体操作：	
# 1 查看所有的ns  命令：kubectl get ns
[root@master ~]# kubectl get ns
NAME              STATUS   AGE
default           Active   45h
kube-node-lease   Active   45h
kube-public       Active   45h 
kube-system       Active   45h 
# 2 查看指定的ns   命令：kubectl get ns ns名称
[root@master ~]# kubectl get ns default
NAME      STATUS   AGE
default   Active   45h
# 3 指定输出格式  命令：kubectl get ns ns名称  -o 格式参数
# kubernetes支持的格式有很多，比较常见的是wide、json、yaml
[root@master ~]# kubectl get ns default -o yaml
apiVersion: v1
kind: Namespace
metadata:
 creationTimestamp: "2021-05-08T04:44:16Z"
 name: default
 resourceVersion: "151"
 selfLink: /api/v1/namespaces/default
 uid: 7405f73a-e486-43d4-9db6-145f1409f090
spec:
 finalizers:
 - kubernetes
status:
 phase: Active
  
# 4 查看ns详情  命令：kubectl describe ns ns名称
[root@master ~]# kubectl describe ns default
Name:         default
Labels:       <none>
Annotations:  <none>
Status:       Active  # Active 命名空间正在使用中  Terminating 正在删除命名空间
# ResourceQuota 针对namespace做的资源限制
# LimitRange针对namespace中的每个组件做的资源限制
No resource quota.
No LimitRange resource.


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNzk3NDExNjEsMTA3ODM0Nzg1NSw4Nj
U5MDY5MzEsLTUxNzc1NzU1OSwtMzY5MzkwNzkxLC0xOTI1Mjgx
MDA4LDE1OTA3ODUwOTQsNDM1ODI5MTE4LC05OTI4NDk0MywtMj
E5Mjg2MjcyLDExOTkxNzYxNDMsLTgzODA5NTU1MywxNDk4Mjgz
Mzg1XX0=
-->