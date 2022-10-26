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


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NDQ3MzgxMTVdfQ==
-->