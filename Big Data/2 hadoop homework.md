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

![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/10/24/i8EPQHwgpd0FUndZ.png)

4.ssh 免密登录
获得密钥
```
 ssh-keygen
```
![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/10/24/xWifOhO4rToi1Ujf.png)
分发密钥
```
ssh-copy-id  root@k8snode1

ssh-copy-id  root@k8snode2
```

5.ssh登录测试：
![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/10/24/Xl5PFDrXC03nLlMk.png)
![输入图片说明](https://raw.githubusercontent.com/dfdbb/MyMakdownPhoto/master/2022/10/24/xP9Ze96au4CwrrrI.png)



### 集群网络
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzc3Nzk3MjgyXX0=
-->