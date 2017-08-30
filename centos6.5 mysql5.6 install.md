Centos6.5 Mysql5.6 Yum install
===================================

1 需要检测系统是否自带安装mysql  

```bash
$ yum list installed | grep mysql
```

2 系统自带安装mysql，先卸载

```base
$ yum -y remove mysql-libs.x86_64
```

3 由于mysql的yum源服务器在国外，所以下载速度比较慢还好mysql5.6只有79M大，而mysql5.7就有182M了，所以这是我不想安装mysql5.7的原因

```base
$ wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
```

4 接着执行这句,解释一下，这个rpm还不是mysql的安装文件，只是两个yum源文件，执行后，在`/etc/yum.repos.d/` 这个目录下多出`mysql-community-source.repo`和`mysql-community.repo`

```base
$ rpm -ivh mysql-community-release-el6-5.noarch.rpm
```

5 yum repolist mysql这个命令查看一下是否已经有mysql可安装文件

```base
$ yum repolist all | grep mysql
```

6 安装mysql 服务器命令（一路yes）：

```base
$ yum install mysql-community-server
```

7 安装成功后,启动数据库

```base
$ service mysqld start
```

8 由于mysql刚刚安装完的时候，mysql的root用户的密码默认是空的，所以我们需要及时用mysql的root用户登录（第一次回车键，不用输入密码），并修改密码

```base
$ mysql -u root  
$ use mysql;
$ update user set password=PASSWORD("这里输入root用户密码") where user='root';
```

9 授权（自动创建）一个mysql的的aaa用户，能访问localhost上的testdb数据库，密码是xxxx，最后刷新权限

```base
$ grant all privileges on testdb.* to aaa@localhost identified by 'xxxx';
$ flush privileges;
```

10 查看mysql是否自启动,并且设置开启自启动命令

```base
$ chkconfig --list | grep mysqld
$ chkconfig mysqld on
```

11 mysql安全设置(系统会一路问你几个问题，看不懂复制之后翻译，基本上一路yes)：

```base
#1 为root用户设置密码
#2 删除匿名账号
#3 取消root用户远程登录
#3 删除test库和对test库的访问权限
#4 刷新授权表使修改生效

$ mysql_secure_installation
```

12 good night 

```base
$ exit
```
