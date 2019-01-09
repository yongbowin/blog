## nginx和elastic HQ安装

* 1.安装nginx
参见[官网](http://nginx.org/en/linux_packages.html)

建立文件 `/etc/yum.repos.d/nginx.repo` ，内容如下：

```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
```

然后执行

```
yum install nginx
```

查看版本

```
nginx -v
```

启动

```
nginx
```

* 2.安装HQ插件
官网[下载HQ](https://github.com/royrusso/elasticsearch-HQ)  
直接解压进入文件夹，打开index.html，对es进行连接（输入 `http://192.168.100.241:9200` ）

* 3.将HQ部署在nginx
将项目文件夹cp到 `/usr/share/nginx` 目录下，修改 `/etc/nginx/conf.d/default.conf` 文件,将

关闭nginx

```
nginx -s stop
```

开启，直接运行nginx
