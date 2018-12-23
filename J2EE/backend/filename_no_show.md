## struts2下载文件时，中文文件名不显示

```
//先将jsp通过get或者post获取到的字段转为本页面使用的字符集。
String  fileName1 = ServletActionContext.getRequest().getParameter("fileName");
fileName1 = new String(fileName1.getBytes("ISO8859-1"), "utf-8");

//再将字段内容转为struts2配置文件设置的ISO8859-1的字符集。
downFileName = new String(downFileName.getBytes(), "ISO8859-1");
```

这样就行，一定要弄明白字符集的设置是对那个流的设置！例如在实际的项目中：
```
filePath = new String(filePath.getBytes("ISO-8859-1"), "utf-8");

//确定各个成员变量的值
contentType = "text/plain";
contentDisposition = "attachment;filename=" + new String(filePath.getBytes(), "ISO-8859-1");
```

> 编码格式要设置两次！
