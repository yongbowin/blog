## Apache部署Django项目

> 服务器同步文件

在服务器部署项目步骤如下

* 1.创建项目

```
django-admin.py startproject aitechWeixin
```

* 2.进入项目目录

```
cd aitechWeixin
```

* 3.创建app

```
django-admin.py startapp weixinAPI
django-admin.py startapp userCenter
```

* 4.进行数据库同步
   * 先进入指定目录
	
	```
	C:\Django_project\aitechWeixin
	```

   * 先执行

	```
	manage.py makemigrations
	```

   * 后执行

	```
	manage.py migrate
	```

* 5.开启服务器

```
python manage.py runserver 0.0.0.0:8000
python manage.py runserver 0.0.0.0:8090
```

* 6.修改weixinAPI中的utils和views文件

	> 注意：还有views中几个参数的修改，和微信配置网页上的

   * ①连接地址
   * ②网页授权回调地址
   * ③修改 `httpd.conf` 文件的最底部
   * ④修改 `settings.py` 文件，将DEBUG改为False，添加一行

	```
	ALLOWED_HOSTS = ['127.0.0.1', 'localhost']
	```

* 7.在apache的 `httpd.conf` 文件里添加：

```
<Directory />
#    AllowOverride none
#    Require all denied
    Options FollowSymLinks
    AllowOverride None
    Order deny,allow
    Allow from all
    Satisfy all
</Directory>
```

* 8.在域名管理系统里配置域名解析，将域名和ip地址绑定起来
* 9.将wamp设置为在线模式
* 10.还需要在 `httpd-vhosts.conf` 中设置

```
	<VirtualHost *:80>
		ServerName localhost
		DocumentRoot c:/wamp/www
		<Directory  "c:/wamp/www/">
			Options +Indexes +FollowSymLinks +MultiViews
			AllowOverride All
			Require all granted
		</Directory>
	</VirtualHost>
```

* 11.对于static的配置
在 `httpd-vhosts.conf` 中增加一句：

```
Alias /static/ C:/Django_project/aitechWeixin/static/指定静态文件的路径
```

完整的配置如下所示：

```
<VirtualHost *:80>
	ServerName mobile.demo.com
	ServerAlias www.demo.com

	Alias /static/ C:/Django_project/aitechWeixin/static/
	
	DocumentRoot C:/Django_project/aitechWeixin
	<Directory  "C:/Django_project/aitechWeixin">
		Options +Indexes +FollowSymLinks +MultiViews
		AllowOverride All
		Require all granted
	</Directory>
</VirtualHost>
```
