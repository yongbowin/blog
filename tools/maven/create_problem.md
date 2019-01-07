## idea创建maven项目

参考网址：http://blog.csdn.net/myarrow/article/details/50824793

* 遇到的坑：
   * 新建maven webapp项目
   * 设置java文件夹为source root，设置resource文件夹为resource root
   * 配置web.xml文件
   * 配置pom.xml文件
   * 进行compile
   * 在project structire配置Artifacts选项，==> exploded ，点击绿色加号，新建Directory content，选择tomcat的webapps文件夹（选择生成的war包位置）
* 配置tomcat服务器: https://www.cnblogs.com/Miracle-Maker/articles/6476687.html
