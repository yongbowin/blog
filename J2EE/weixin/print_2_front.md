## 后台打印信息到页面问题

后台`out.print(__br___);`打印到页面：

为什么不能正常打印出来？而out.print("\n");是可以的

解决乱码问题：

```
response.setCharacterEncoding("GBK");    或是
String CONTENT_TYPE = "text/html; charset=GBK";
response.setContentType(CONTENT_TYPE);
```
