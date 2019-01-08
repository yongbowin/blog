## linux下配置apache

> 注意：Apache一般部署静态页面（性能优于Tomcat），而动态处理和action之列的需要交给Tomcat处理，也就是说使用Tomcat来部署Java Web项目

以下项目全部部署在公司本地服务器 `http://192.168.100.251`

#### Apache安装配置（没用上，在部署Django项目的时候要用到）
* 1.先安装apache和maven

```
sudo apt install apache2
sudo apt install maven
```

* 2.启动apache服务

| cmd | action |
| ------ | ------ |
| /etc/init.d/apache2 start | 启动 |
| /etc/init.d/apache2 restart | 重启 |
| /etc/init.d/apache2 stop | 停止 |

查看apache的启动状态：

```
systemctl status apache2.service
```

* 3.将apache设置为开启自启动

> ubuntu中apache2的安装目录为 `/etc/apache2`
