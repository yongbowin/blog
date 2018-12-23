## session值的获取

设置:

`session.setAttribute("userInfo", user);`

在jsp里取：

`User aa=(User)session.getAttribute("userInfo");`

在java类里取：

```
HttpServletRequest request = ServletActionContext.getRequest();

String commonID = (String) request.getSession().getAttribute("commonID");
String username = (String) request.getSession().getAttribute("username");
```
