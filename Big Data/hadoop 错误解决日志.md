# but there is no HDFS_NAMENODE_USER defined. Aborting 

### 解决方案：
```
vim /etc/profile.d/my_env.sh
```

添加如下内容：
```
export HDFS_NAMENODE_USER=root
export HDFS_DATANODE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root
export YARN_RESOURCEMANAGER_USER=root
export YARN_NODEMANAGER_USER=root
```

让新配置生效：
```
source /etc/profile
sudo /usr/local/sbin/xsync /etc/profile.d/
```


# 第二次错误
![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/vTGVpBF7X07Tvti4.png)
```
Starting namenodes on [k8smaster]
Last login: Wed Oct 26 01:54:30 EDT 2022 from 192.168.88.1 on pts/1
k8smaster: Warning: Permanently added 'k8smaster,192.168.88.100' (ECDSA) to the list of known hosts.
k8smaster: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).
Starting datanodes
Last login: Wed Oct 26 03:41:09 EDT 2022 on pts/1
k8smaster: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).
k8snode1: ERROR: JAVA_HOME /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre does not exist.
k8snode2: WARNING: /soft/hadoop/logs does not exist. Creating.
Starting secondary namenodes [hadoopWyc2]
Last login: Wed Oct 26 03:41:09 EDT 2022 on pts/1
hadoopWyc2: ssh: Could not resolve hostname hadoopwyc2: Name or service not known
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAyOTc3MzQ0NywtMTc0NDczODExNV19
-->