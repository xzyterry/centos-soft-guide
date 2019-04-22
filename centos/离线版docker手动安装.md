# 离线版docker手动安装



环境是基于局域网,没有配置yum源的参考下面这个帖子

[配置yum源](https://github.com/xzyterry/centos-soft-guide/blob/master/centos/%E9%85%8D%E7%BD%AE%E6%9C%AC%E5%9C%B0%E5%B1%80%E5%9F%9F%E7%BD%91yum%E6%BA%90.md)

## 1. 修改yum文件


vi /etc/yum.repos.d/CentOS-Base.repo

### (1) 编辑配置文件：/etc/yum.repos.d/CentOS-Base.repo，
   然后注释mirrorlist，放开baseurl配置成yum源位置，还有gpgkey也配置成对应位置

```shell
[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
baseurl=http://192.168.153.128/CentOS7/
gpgcheck=1
gpgkey=http://192.168.153.128/CentOS7/RPM-GPG-KEY-CentOS-7
```

### (2) 配置完这些以后，然后在[updates]和[extras]都添加一个enabled=0配置项，表示不生效，一般只用[base]中的配置即可，配置好之后保存退出
```shell
#released updates 
[updates]
name=CentOS-$releasever - Updates
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
enabled=0

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
enabled=0
```

## 2. 测试是否配置成功
安装lrzsz 用于本地上传文件到服务器(<2G)

```shell
yum install -y lrzsz 

#查看是否安装成功
#上传 rz     下载:sz filename
```



### 3. 下载docker安装包及其依赖包

安装顺序
libcgroup
policycoreutils-python
container-selinux-2.74-1.el7.noarch.rpm
docker-ce-17.06.2.ce-1.el7.centos.x86_64.rpm

```shell
#libcgroup:
yum install -y libcgroup

#policycoreutils-python:
yum install -y policycoreutils-python

#container-selinux:
#[下载地址](http://mirror.centos.org/centos/7/extras/x86_64/Packages/)
rpm -ivh container-selinux-2.74-1.el7.noarch.rpm

#docker-ce-17.06.2.ce-1.el7.centos.x86_64.rpm:
#[下载地址](https://download.docker.com/linux/centos/7/x86_64/stable/Packages/ )
rpm -ivh docker-ce-17.06.2.ce-1.el7.centos.x86_64.rpm
```





4.查看是否安装成功
查看状态:

```shell
systemctl status docker 
```

启动:

```shell
systemctl start docker 
```

关闭:

```shell
systemctl stop docker 
```



