## jquery添加的class后,无法为其添加点击事件

* 方法一

```
$(document).on('click', '.J_classtree', function(e) {
 
  alert('');
 
});
```

* 方法二
使用onclick方法，注意将绑定的方法放在$(function(){});的外边
