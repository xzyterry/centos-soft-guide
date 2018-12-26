# Centos 7 安装mysql8.0教程

*标签:非yum方式 	tar.gz	mysql安装	8.0	 *

### 一.安装包下载

~~~shell
#软件下载
wget https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.13-el7-x86_64.tar.gz

#解压
tar -xzvf mysql-8.0.11-el7-x86_64.tar.gz -C /usr/local/mysql


~~~

### 二.卸载自带mariadb(mysql的一个分支)

~~~shell
#检查是否存在
yum list installed | grep mariadb
#卸载
yum -y remove mariadb-libs.x86_64
~~~



### 三.添加mysql用户和组



~~~shell
groupadd mysql
useradd -g mysql mysql
~~~



### 四.安装

~~~shell
cd /usr/local/mysql/bin
./mysqld --initialize --user=mysql --datadir=/usr/local/mysql/data --basedir=/usr/local/mysql

#注意查看初始密码,复制密码

~~~



![截图](https://img-blog.csdn.net/20180705005054466?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1ZpdGFtaW5aSA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)





### 五.修改密码及开通远程权限

~~~shell
cd /usr/local/mysql/bin
#初始化数据目录
./mysql_ssl_rsa_setup  --datadir=/usr/local/mysql/data
chown -R mysql: mysql /usr/local/mysql
./mysqld_safe & ./mysql -uroot -p
#输入刚才密码即可以登录，默认不能远程

#修改root密码
alter user 'root'@'localhost' identified by '123456';

#修改远程访问
use mysql;
update user set host='%' where user='root';
flush privileges;

#如果连接时 出现 Authentication plugin 'caching_sha2_password' cannot be loadedAuthentication plugin 'caching_sha2_password' cannot be loaded 
#修改验证方式
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
~~~



### 六.建立快捷访问



~~~shell
#注册服务 可以使用 service mysqld start 
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld

#添加到环境变量 可以直接	mysql -u root -p 进入mysql
echo export PATH=$PATH:/usr/local/mysql/bin
~~~

