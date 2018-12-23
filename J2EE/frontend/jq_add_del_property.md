## jquery添加和删除属性

代码如下:

```
$("#2args").attr("disabled",'disabled');
$("#2args").removeAttr("disabled");
```

或者：

```
function  remAttr(){
    //去除disabled属性
    $("#sheet").attr("disabled",false);           
}

function  addAttr(){
    //添加disabled属性
    $("#sheet").attr("disabled",true);           
}
```
