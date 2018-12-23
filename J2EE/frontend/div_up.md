## DIV相对于另一个元素定位，并且在最上层

```
<div class="userpic">
    <img src="用户头像" />
    <div class="userinfo hide">
        用户详细信息
    </div>
</div>
```

```
.userpic{position:relative;width:100px;height:100px;}
.userinfo{position:absolute;z-index:99;left:100px;top:0;}
.hide{display:none;}
```

> 注意：元素一定要加上position:relative;
