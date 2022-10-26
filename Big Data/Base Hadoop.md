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

![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/gwZF0dpKoVeMQVTu.png)

让操作生效：
>  source ~/.bashrc

查看java_home:
>  echo $JAVA_HOME

![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/sSDcpD8iXoQEzqz5.png)


### 安装hadoop
1.使用fpt上传hadoop包至  master 节点
![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/GzAB4nIpOFN3iE6p.png)2.解压hadoop包,改名为 hadoop, 在 /  下新建 soft 目录 将hadoop 移动 至 /soft/下 修改文件权限
```shell
tar -zxvf hadoop-3.2.0.tar.gz
mkdir /soft
mv hadoop-3.2.0.tar.gz  /soft/hadoop
chown -R hadoop ./hadoop # 修改文件权限
```
3.修改 .bashrc 文件 ，添加如下 内容：
 ```
export HADOOP_HOME=/usr/local/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```

## hadoop 配置的修改
1.进入/soft/hadoop/etc/hadoop 修改配置文件  workers、core-site.xml、hdfs·site.xml、mapred-site.xml、yarn-site.xml。

2.修改workers，该文件内容可以指定某几个节点作为数据节点，默认为localhost,将node1,node2,master 添加进去

```shll
k8smaster
k8snode1
k8snode2
```
3.修改 core-site.xml

```
		<property>
                <name>fs.defaultFS</name>
                <value>hdfs://k8smaster:9000</value>
        </property>
        <property>
                <name>hadoop.tmp.dir</name>
                <value>file:/soft/hadoop/tmp</value>
        </property>
```

4.修改hdfs-site.xml文件
> 
添加如下内容：

```shell
		<property>
                <name>dfs.namenode.secondary.http-address</name>
                <value>master:50090</value>
        </property>
        <property>
                <name>dfs.replication</name>
                <value>3</value>
        </property>
        <property>
                <name>dfs.namenode.name.dir</name>
                <value>file:/usr/local/hadoop/tmp/dfs/name</value>
        </property>
        <property>
                <name>dfs.datanode.data.dir</name>
                <value>file:/usr/local/hadoop/tmp/dfs/data</value>
        </property>
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkwMjA0MTY3NCwxOTY2NzU0NDAyLDIxND
czNDA2NjUsNDUyOTUxNDA5LDExNTAxMzA4MTUsMTYxMTM0NzAz
NywtMjA4ODc0NjYxMiwxNDUyMjk2MjkxXX0=
-->