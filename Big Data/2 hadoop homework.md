# 第二次Hadoop作业 
## 网络配置


1.分别修改三个节点的hostname
`hostnamectl set-hostname k8smaster`
`hostnamectl set-hostname k8snode1`
`hostnamectl set-hostname k8snode2`

2.修改网络配置文件(/etc/hosts)
`vim /etc/hosts`

```shell
192.168.88.100  k8smaster
192.168.88.101  k8snode1
192.168.88.102  k8snode2
```

3.检查网络连接是否正常：
![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/10/24/818h4JmQOGFhzUPH.png)



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI5Mzc0OTk3NV19
-->