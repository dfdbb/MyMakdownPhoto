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
[图片上传失败...(image-Mbol8dEzyR7NfOo3)]


<!--stackedit_data:
eyJoaXN0b3J5IjpbNDY4NzAzMDEzXX0=
-->