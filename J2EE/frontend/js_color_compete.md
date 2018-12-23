## js文件中颜色进行比较的用法

进行颜色比较不能直接用#00ff00之类的颜色代码，需要用rgb的值进行比较

例如：
`$(this).css("background-color") == 'rgb(241, 157, 155)'`
> 注意：此时背景色应为 background-color

获取颜色代码相应的rgb的值，用方法：
`alert($(this).css("background-color"));`
