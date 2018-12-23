## java和前端页面-获取url中传来的参数-中文乱码问题

### java中解决url中参数中文乱码问题：

```
//获取url中的参数
HttpServletRequest request = ServletActionContext.getRequest();
String build_time = request.getParameter("questionBuildTime");
```

> 注意：
> 获取的时候进行转码 `String str = new String(request.getParameter("ptname").getBytes("ISO-8859-1"), "UTF-8");`


在前端页面：
```
//在学生作业上传页面搜索作业
$("#hw_btn").click(function(){

    var hw_grade_val = $(".hw-grade-select option:selected").val();
    var hw_subject_val = $(".hw-subject-select option:selected").val();

    $.ajax({
        url: "search_hw",
        type: "post",
        data: "hw_grade_val=" + hw_grade_val + "&hw_subject_val=" + hw_subject_val,
        contentType: "application/x-www-form-urlencoded; charset=utf-8", 
        success: function(){
            // 针对url参数中有中文乱码的问题，可以用contentType，但是对于下边的重定向操作，
            // 需要进行两次编码操作，后台再进行解码
            window.location.href = "teach_res?search_btn_click_param=1&hw_grade_val=" 
                + hw_grade_val + "&hw_subject_val=" + encodeURI(encodeURI(hw_subject_val)); 
        }
    });
});
``` 
