Centos6.5 Install Mongodb
===================================

1 需要检测系统是否自带安装mongodb  

```bash
$ yum list installed | grep mongodb
```

2 安装mongodb两个版本源`mongodb-org-3.2` and mongodb-org-2.6

```bash
create a /etc/yum.repos.d/mongodb-org-3.2.repo or /etc/yum.repos.d/mongodb-org-2.6.repo

[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc

[mongodb-org-2.6]
name=MongoDB 2.6 Repository
baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64/
gpgcheck=0
enabled=1
```

3 Install the MongoDB packages and associated tools

```bash
$ yum install -y mongodb-org
```

4 创建数据库路径和日志路径

```bash
$ mkdir -p /opt/mongodb/db
$ mkdir -p /opt/mongodb/log
```

5 修改配置

```bash
$ vi /etc/mongod.conf
#端口
port = 27017
#数据目录
dbpath = /opt/mongodb/db
#日志目录
logpath = /opt/mongodb/log/mongodb.log
#设置后台运行
fork = true
#日志输出方式
logappend = true
#开启认证
#auth = true
```

6 关闭SELinux and iptables启动mongodb：

```bash
$ getenforce
$ iptables -L
$ chown -R mongod.mongod /opt/mongodb/log /opt/mongodb/db /var/run/mongodb
$ service mongod start
$ chkconfig mongod on
$ kill mongod #启动出错
```

7 操作mongodb数据库

```mongo
$ mongo
$ show dbs
$ use db
$ db.test.insert({"name":"测试"})
```

8 uninstall mongodb

```bash
$ service mongod stop  
$ yum erase $(rpm -qa | grep mongodb-org)
# Remove MongoDB databases and log files
$ rm -r /opt/mongodb/db
$ rm -r /opt/mongodb/log/mongodb.log
```

9 安装php插件

```bash
$ yum install php-pecl-mongo

```

10 good night 

```bash
$ exit
```
