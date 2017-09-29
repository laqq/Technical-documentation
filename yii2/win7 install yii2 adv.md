win7 install yii2 adv.md
===================================

1 从  http://www.yiiframework.com/download/下载

```bash
#使用迅雷下载比较快
```

2 配置php.exe 环境变量，cmd执行初始化

```bash
$ init
```

3 创建数据库，配置数据库链接 common/config/main.php  `components['db'] `

```php
	'db' => [
		'class' => 'yii\db\Connection',
		'dsn' => 'mysql:host=localhost;dbname=dbdata',
		'username' => 'root',
		'password' => 'root',
		'charset' => 'utf8',
	],
```

4 打开cmd ,项目/根目录，执行 

```bash
yii migrate
生成user数据表和migration数据表
```

5 设置nginx服务器：

```bash
#前端：/frontend/web 域名
#后端：/backend/web 域名
#开发环境：host:域名
	#autoindex  on;
	if (!-e $request_filename){
		rewrite ^/(.*) /index.php last;
	}
```

6 设置path模式common/config/main.php：`components['urlManager'] `

```bash
        'urlManager' => [
            'enablePrettyUrl' => true,  //美化url==ture
            //'enableStrictParsing' => false,  //不启用严格解析
            'showScriptName' => false,   //隐藏index.php
            'rules' => [
                //'<module:\w+>/<controller:\w+>/<id:\d+>' => '<module>/<controller>/view',
                //'<controller:\w+>/<id:\d+>' => '<controller>/view',
            ],
        ],
```

7 喜欢Vagrant安装可以不先安装的软件（如Web服务器，PHP，MySQL等）20分钟左右

```bash
#具体参考：
#https://github.com/yiisoft/yii2-app-advanced/blob/master/docs/guide-zh-CN/start-installation.md
```

8 good night

```bash
$ exit
```