## jquery交替点击时间

```
$(document).ready(function(){
    $("button").toggle(function(){
        $("body").css("background-color","green");},
        function(){
        $("body").css("background-color","red");},
        function(){
        $("body").css("background-color","yellow");}
    );
});
```
