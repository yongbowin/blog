## 输入框限制只能输入数字或者字母

* 只能输入数字

```
$.fn.onlyNum = function () {
    $(this).keypress(function (event) {
        var eventObj = event || e;
        var keyCode = eventObj.keyCode || eventObj.which;
        if ((keyCode >= 48 && keyCode <= 57))
            return true;
        else
            return false;
    }).focus(function () {
    //禁用输入法
        this.style.imeMode = 'disabled';
    }).bind("paste", function () {
    //获取剪切板的内容
        var clipboard = window.clipboardData.getData("Text");
        if (/^\d+$/.test(clipboard))
            return true;
        else
            return false;
    });
};
```

* 只能输入字母

```
$.fn.onlyAlpha = function () {
    $(this).keypress(function (event) {
        var eventObj = event || e;
        var keyCode = eventObj.keyCode || eventObj.which;
        if ((keyCode >= 65 && keyCode <= 90) || (keyCode >= 97 && keyCode <= 122))
            return true;
        else
            return false;
    }).focus(function () {
        this.style.imeMode = 'disabled';
    }).bind("paste", function () {
        var clipboard = window.clipboardData.getData("Text");
        if (/^[a-zA-Z]+$/.test(clipboard))
            return true;
        else
            return false;
    });
};
```

* 只能输入数字和字母

```
$.fn.onlyNumAlpha = function () {
    $(this).keypress(function (event) {
        var eventObj = event || e;
        var keyCode = eventObj.keyCode || eventObj.which;
        if ((keyCode >= 48 && keyCode <= 57) || (keyCode >= 65 && keyCode <= 90) || (keyCode >= 97 && keyCode <= 122))
            return true;
        else
            return false;
    }).focus(function () {
        this.style.imeMode = 'disabled';
    }).bind("paste", function () {
        var clipboard = window.clipboardData.getData("Text");
        if (/^(\d|[a-zA-Z])+$/.test(clipboard))
            return true;
        else
            return false;
    });
};

$(function () {
    // 限制使用了onlyNum类样式的控件只能输入数字
    $(".onlyNum").onlyNum();
    //限制使用了onlyAlpha类样式的控件只能输入字母
    $(".onlyAlpha").onlyAlpha();
    // 限制使用了onlyNumAlpha类样式的控件只能输入数字和字母
    $(".onlyNumAlpha").onlyNumAlpha();
});
```

* 只能输入大于0的正整数
> jquery限制文本框只能输入数字,兼容IE、chrome、FF（表现效果不一样）,代码如下：

```
$("input").keyup(function(){ 
    //keyup事件处理 
    $(this).val($(this).val().replace(/\D|^0/g,''));
}).bind("paste",function(){ 
    //CTR+V事件处理 
    $(this).val($(this).val().replace(/\D|^0/g,''));
}).css("ime-mode", "disabled"); //CSS设置输入法不可用 
```

* 只能输入0-9的数字和小数点

```
$("#rnumber").keyup(function(){
    $(this).val($(this).val().replace(/[^0-9.]/g,''));  
}).bind("paste",function(){
    //CTR+V事件处理  
    $(this).val($(this).val().replace(/[^0-9.]/g,''));
}).css("ime-mode", "disabled"); //CSS设置输入法不可用
```
