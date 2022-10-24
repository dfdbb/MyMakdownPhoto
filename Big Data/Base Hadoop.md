# hadoop 分布式集群安装

略过网络配置
### 安装jdk
在3台虚拟机上安装jdk

```
yum install -y java-1.8.0-openjdk
```
![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/objMw5ayGINvnmwM.png)
安装成功：
![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/fjoH6RuvFfseUo9c.png)

### 配置JAVA_HOME
修改 .bashrc 配置文件,添加 JAVA_HOME 
在root用户 ~目录下
```shell
vim  .bashrc
```
添加如下 内容：
> export JAVA_HOME=/usr/lib/jvm/default-java

让操作生效：
>  source ~/.bashrc

查看java_home:
>  echo $JAVA_HOME

![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/sSDcpD8iXoQEzqz5.png)



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM5MDg2MDI3MCwxNjExMzQ3MDM3LC0yMD
g4NzQ2NjEyLDE0NTIyOTYyOTFdfQ==
-->