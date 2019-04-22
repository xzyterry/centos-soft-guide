# mysql不能插入中文问题

环境: centos7

版本: 5.6.35 MySQL Community Server (GPL)



1. 修改mysql中的配置my.cnf   

   - 查看mysql编码

   ```mysql
   show variables like '%character%';
   #会有如下显示：
   +--------------------------+----------------------------+
   | Variable_name            | Value                      |
   +--------------------------+----------------------------+
   | character_set_client     | utf8                       |
   | character_set_connection | utf8                       |
   |character_set_database   | latin1                     |
   | character_set_filesystem | binary                     |
   | character_set_results    | utf8                       |
   |character_set_server     | latin1                     |
   | character_set_system     | utf8                       |
   | character_sets_dir       | /usr/share/mysql/charsets/ |
   +--------------------------+----------------------------+
   8 rows in set (0.00 sec)
   ```

   - 修改编码

   ```shell
   vim/etc/my.cnf
   
   
    #在[client]下追加：
    [client]
   default-character-set=utf8mb4
   
   
   #在[mysqld]下追加：
   [mysqld]
   character-set-server=utf8mb4
   
   
   #在[mysql]下追加：
   [mysql]
   default-character-set=utf8mb4
   
   ```

   

   - 重启mysql

     ```shell
     service mysql restart 
     ```

     

2. 将已经建好的数据库和表修改编码

   ```mysql
   #更改数据库编码：
   ALTER DATABASE xxxx CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
   #（将TABLE_NAME替换成你的表名）
   alter table xxxx convert to character set utf8mb4 collate utf8mb4_bin; 
   ```

   