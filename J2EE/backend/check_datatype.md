## 查看数据类型

可以通过Class的name属性来查看数据类型，每一个对象都有一个Class，在java中一切皆对象，故能够通过如下方式来获取对象的类型：
```
Date date = new Date();
System.out.println(date.getClass().getName()); //java.util.Date
```
