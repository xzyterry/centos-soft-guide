# 配置本地/局域网yum源
转载:https://www.cnblogs.com/freeweb/p/6513400.html

## 1. 配置httpd

### (1) 软件下载

[软件包下载](http://www.rpmfind.net/linux/rpm2html/search.php)

mailcap-2.1.41-2.el7.noarch.rpm
apr-1.4.8-3.el7.x86_64.rpm
apr-util-1.5.2-6.el7.x86_64.rpm
httpd-tools-2.4.6-45.el7.centos.x86_64.rpm
httpd-2.4.6-45.el7.centos.x86_64.rpm

```shell
rpm -ivh apr-1.4.8-3.el7_4.1.x86_64.rpm
rpm -ivh apr-util-1.5.2-6.el7.x86_64.rpm
rpm -ivh httpd-tools-2.4.6-88.el7.centos.x86_64.rpm
rpm -ivh httpd-2.4.6-88.el7.centos.x86_64.rpm
rpm -ivh mailcap-2.1.41-2.el7.noarch.rpm
```

### (2) 启动 httpd

```shell
systemctl start httpd.service
systemctl status httpd.service
```



### (3) 关闭防火墙

```shell
systemctl stop firewalld
systemctl disable firewalld
```

1、firewalld的基本使用
启动： systemctl start firewalld
关闭： systemctl stop firewalld
查看状态： systemctl status firewalld 
开机禁用  ： systemctl disable firewalld
开机启用  ： systemctl enable firewalld

### (4) 浏览器访问
当前ip

## 2. 下载Centos镜像

### (1) 下载

[下载镜像](https://mirrors.tuna.tsinghua.edu.cn/centos/7.6.1810/isos/x86_64/)

### (2) 挂载镜像

```shell
mkdir /mnt/cdrom
mount -t iso9660 -o loop CentOS-7-x86_64-DVD-1511.iso /mnt/cdrom/
ln -s /mnt/cdrom/ /var/www/html/CentOS7
```



### (3) 测试访问yum源
http://ip/CentOS7



### (4) 修改yum配置文件

1. 编辑配置文件：/etc/yum.repos.d/CentOS-Base.repo，
   然后注释mirrorlist，放开baseurl配置成yum源位置，还有gpgkey也配置成对应位置

```shell

[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
baseurl=http://192.168.153.128/CentOS7/
gpgcheck=1
gpgkey=http://192.168.153.128/CentOS7/RPM-GPG-KEY-CentOS-7
```

2. 配置完这些以后，然后在[updates]和[extras]都添加一个enabled=0配置项，表示不生效，一般只用[base]中的配置即可，配置好之后保存退出

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







