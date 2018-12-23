## 获取项目地址的方式

* 获取项目绝对路径：

`String reqUrl2 =request.getRealPath("/");`

* 获取项目的域名：

```
StringBuffer url = request.getRequestURL();  
String tempContextUrl = url.delete(url.length() - request.getRequestURI().length(), url.length()).append("/").toString(); 
```

* 获取工程名（replace用来去掉斜杠）

`request.getContextPath().replace("/", "")`
