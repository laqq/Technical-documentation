master-slave mysql server create
=========================================

1 服务器环境

> 主(master_mysql):192.168.132.13  OS:CentOS 6.5 Mysql5.6
> 主(slave_mysql):192.168.132.14  OS:CentOS 6.5 Mysql5.6

2 master配置(master_mysql)

```bash
$ vim /etc/my.cnf

$ server-id=1  # 设置主服务器的ID
$ innodb_flush_log_at_trx_commit=2  # Innodb写入缓存提高效率
$ sync_binlog=1  # 开启binlog日志同步功能
$ log-bin=mysql-bin-13  # binlog日志文件名(后面的数字一般设为ip末位)
$ binlog-ignore-db=mysql # 忽略数据库
$ binlog-ignore-db=information_schema
$ binlog-ignore-db=performance_schema

$ service mysqld restart
```

3 master授权给从数据库服务器账号和主库状态

```mysql

$ mysql -uroot -p  # 登录mysql
$ mysql>grant replication slave on *.* to 'sync_user'@'192.168.132.14' identified by '111111';   # 授权给从数据库服务器192.168.132.14，用户名sync_user，密码111111
$ flush privileges;
$ mysql> flush tables with read lock; # 线上正在运行的数据库锁表丛库同步后解锁 `unlock tables`;
$ mysql>show master status ; # 查看主库的状态  file,position这两个值很有用。要放到slave配置中

```
4 slave配置(slave_mysql)

```bash
$ vim /etc/my.cnf

$ server-id=2
$ innodb_flush_log_at_trx_commit=2
$ sync_binlog=1
$ log-bin=mysql-bin-14

service mysqld restart
```

5 slave从库连接主库，主从同步，同步状态

```mysql
$ mysql -uroot -p
$ mysql> change master to  master_host='192.168.132.13', master_user='sync_user' ,master_password='111111', master_log_file='mysql-bin-200.000002' ,master_log_pos=1167, MASTER_CONNECT_RETRY=10; # MASTER_CONNECT_RETRY 同步数据频率 10秒同步一次
$ mysql> start slave;  ##开启从库   (stop slave：关闭从库）
$ mysql> show slave status; ###Slave_IO_Running,Slave_SQL_Running 都为Yes的时候表示配置成功
```
