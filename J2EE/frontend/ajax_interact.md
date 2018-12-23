## Ajax前后台交互

### 后台代码

方法一：

* 获取List<Map<String, Object>>对象，需要将该对象转化为String格式才可以用outer.write();进行输出
* 用JSONArray.fromObject();进行必要的数据格式的转换

```
HttpServletResponse response = ServletActionContext.getResponse();
PrintWriter outer = response.getWriter();

List<Map<String, Object>> labelStatusArr  = LevelDao.getLevelObject(labelStatusID);

JSONArray label_status_arr = JSONArray.fromObject(labelStatusArr);
outer.write(label_status_arr.toString());
```

* 在前端ajax里边加上类型dataType:"json",才可以将接受到的String类型的数据转化为json对象，
* 获取该对象里边的属性的属性值，方法为：json[0].level_name

方法二：

* 获取JSONObject对象，这个时候不需要进行类型转换，等输出该对象的时候需要一样的转化为Stirng对象

```
JSONObject labelStatusArr = LevelDao.getLevelObject1(labelStatusID);
outer.write(labelStatusArr.toString());
```

* 在前端获取该对象的属性的属性值的时候，方法为：

`alert(json.level_name);`

```
HttpServletResponse response = ServletActionContext.getResponse();
PrintWriter outer = response.getWriter();

if(GoodsDao.saveRemark(price, rangeID, productID)){
    outer.write(SUCCESSCODE);
}
else{
    outer.write(ERRORCODE);
}
```

### 前端代码

```
$.ajax({
    url:"obtain_label_name",
    type:"post",
    data:"labelStatusID=" + labelStatusID,
    dataType:"json",
    success:function(json){
        //alert(typeof(json));

        //alert(json[0].level_name);
        var labelValue = json.level_name;

        $(".breadcrumb-1 .active").html("labelValue");
    }
});
```
