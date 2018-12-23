## js中小数计算的精度问题

在javascript中不分单精度float和双精度double，凡事有小数的变量都认为是float，因此要取小数后的n位，要用方法toFixed(n)来得到

js数字转换为float,取N个小数：因为js为弱类型的语言

javascript中的变量都是弱类型，所有的变量都声明为var，在类型转换过程中就没有java那么方便，它是通过 parseInt(变量)、parseFloat(变量)等方法来进行类型转换的。注意：没有parseDouble(变量)这种类型转换，因为在javascript中不分单精度float和双精度double，凡事有小数的变量都认为是float，因此要取小数后的n位，要用方法toFixed(n)来得到。

例如：

```
var f=2.56556;
f.toFixed(2);
```

可取到变量f小数后的2位,如果不是float型，还需要进行类型转换(parseFloat(变量名))，才能通过此方法获得。

Number()函数将指定对象转化为数字
