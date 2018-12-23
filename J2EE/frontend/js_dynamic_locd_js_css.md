## jquery动态加载css和js文件

jquery自带的getSrcript方法加载js文件
```
$.getScript(url,callback)

$('#loadButton').click(function(){ 
    $.getScript('new.js',function(){ 
```
js可以直接用$.getScript()方法

css可以
`$('head').append('<link href="xxx.css" rel="stylesheet" type="text/css" />')`
