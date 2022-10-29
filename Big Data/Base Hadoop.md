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
export HADOOP_HOME=/soft/hadoop
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
> dfs.secondary.http.address：secondarynamenode运行节点的信息，应该和namenode存放在不同节点
dfs.repliction：hdfs的副本数设置，默认为3
dfs.namenode.name.dir：namenode数据的存放位置，元数据存放位置
dfs.datanode.data.dir：datanode数据的存放位置，block块存放的位置


```shell
		<property>
                <name>dfs.namenode.secondary.http-address</name>
                <value>k8smaster:50090</value>
        </property>
        <property>
                <name>dfs.replication</name>
                <value>3</value>
        </property>
        <property>
                <name>dfs.namenode.name.dir</name>
                <value>file:/soft/hadoop/tmp/dfs/name</value>
        </property>
        <property>
                <name>dfs.datanode.data.dir</name>
                <value>file:/soft/hadoop/tmp/dfs/data</value>
        </property>
```

5. 修改mapred-site.xml文件
>mapreduce.framework.name：指定mapreduce框架为yarn方式
mapreduce.jobhistory.address：指定历史服务器的地址和端口
mapreduce.jobhistory.webapp.address：查看历史服务器已经运行完的Mapreduce作业记录的web地址，需要启动该服务才行

```
        <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
        </property>
        <property>
                <name>mapreduce.jobhistory.address</name>
                <value>k8smaster:10020</value>
        </property>
        <property>
                <name>mapreduce.jobhistory.webapp.address</name>
                <value>k8smaster:19888</value>
        </property>
        <property>
                <name>yarn.app.mapreduce.am.env</name>
                <value>HADOOP_MAPRED_HOME=/soft/hadoop</value>
        </property>
        <property>
                <name>mapreduce.map.env</name>
                <value>HADOOP_MAPRED_HOME=/soft/hadoop</value>
        </property>
        <property>
                <name>mapreduce.reduce.env</name>
                <value>HADOOP_MAPRED_HOME=/soft/hadoop</value>
        </property> 
```

6. 修改yarn-site.xml文件
```
<configuration>
        <property>
                <name>yarn.resourcemanager.hostname</name>
                <value>k8smaster</value>
        </property>
        <property>
                <name>yarn.nodemanager.aux-services</name>
                <value>mapreduce_shuffle</value>
        </property>
</configuration>
```

7.对上述文件进行修改后，将整个hadoop文件复制到各节点上
建议压缩后分发，速度较快

```
cd /soft
sudo rm -r ./hadoop/tmp     # 删除 Hadoop 临时文件
sudo rm -r ./hadoop/logs/*   # 删除日志文件
tar -zcf ./hadoop.master.tar.gz hadoop  # 先压缩再复制
cd ~
scp hadoop.master.tar.gz k8snode1:/home/
scp hadoop.master.tar.gz k8snode2:/home/
```

在其他节点解压，授予权限
```
tar -zxf /home/hadoop.master.tar.gz -C /soft
chown -R root /soft/hadoop
```

### hadoop初始化
> HDFS初始化只能在主节点上进行

```
cd /usr/local/hadoop 
./bin/hdfs namenode -format
```
当看到标白的这行有有个successfully formatted说明初始化成功。
![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/YdMXfvfLLQFH8dU6.png)


## hadoop集群启动
在master节点上执行以下命令：

```
./start-all.sh
```

运行结果如下:
![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/mk6FyyDlOy5WwIMK.png)



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ3MzgwNzQyNywxNTM3Njc4NDc0LC0xND
kzMTAyODk2LC04NTgwMDYwODQsLTE1NzU3MjU2ODUsMjQ4MTU0
NjIzLDEyNjUyMzQ4NSwxNzY5MTAxMTI1LDE5NjY3NTQ0MDIsMj
E0NzM0MDY2NSw0NTI5NTE0MDksMTE1MDEzMDgxNSwxNjExMzQ3
MDM3LC0yMDg4NzQ2NjEyLDE0NTIyOTYyOTFdfQ==
-->