升级 phpStudy 中 MySQL 版本至 5.7.17 
===================================

1 停止mysql服务，备份原来的phpStudy/MySQL目录  

2 下载新版https://dev.mysql.com/downloads/mysql/ 下载mysql-5.7.19-winx64
```bash
#复制到phpStudy/MySQL复制my.ini
[client] 
port=3306 
default-character-set=utf8 

[mysqld]
#skip-grant-tables
port=3306
basedir="D:/Program Files/phpStudy/MySQL/"
datadir="D:/Program Files/phpStudy/MySQL/data/"
character-set-server=utf8
default-storage-engine=INNODB
#支持 INNODB 引擎模式。修改为　default-storage-engine=INNODB　即可。
#如果 INNODB 模式如果不能启动，删除data目录下ib开头的日志文件重新启动。
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES 
#sql-mode="NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
[WinMySQLAdmin] 
Server = "D:/Program Files/phpStudy/MySQL/bin/mysqld.exe"
```

3 添加环境变量（追加）

```bash
$ ;D:\Program Files\phpStudy\MySQL\bin
```

4 在cmd进入MySQL 的 bin 目录执行：

```bash
$ mysqld -remove
#初始化数据库
$ mysqld --initialize
#安装服务
mysqld -install
#启动服务
net start MySQL
```

5 mysql登陆报错，打开my.ini找到[mysqld]，在下面添加：skip-grant-tables(此参数用于忘记mysql密码)

```bash
$ mysql -uroot
#修改密码
$ mysql>update mysql.user set authentication_string=password('新密码') where user='root' and Host ='localhost';
$ mysql> ALTER USER USER() IDENTIFIED BY '新密码';
#刷新权限：
FLUSH PRIVILEGES;
```

6 注释掉 my.ini 中刚才添加的skip-grant-tables登陆

```bash
$ mysql -uroot -p
$ select version();
```

7 goodnight

```bash
$ exit;
```