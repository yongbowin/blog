## 将div保存为图片

```
<script type="text/javascript" src="../build/html2canvas.js"></script>
<script type="text/javascript">
    //将试卷内容转为图片格式
    $(".save_test_paper").click(function(){
        alert("进入");
        html2canvas($(".test_paper_tab"), {
            onrendered: function(canvas) {
                //document.body.appendChild(canvas);
                //$("canvas").height(5000);
                document.getElementById("test_paper_content_id").appendChild(canvas);
            }
        });
    });
</script>
```
