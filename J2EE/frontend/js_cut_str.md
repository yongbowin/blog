## js截取字符串

```
var from = 'abcdefg';
var to = from.substr(1,3); //表示从1位开始截取后面的3个字符，"bcd"
to = from.substring(1,3); //表示从1位开始截取到3位，"bc"
```

或者直接用from[0]，获取第0个字符

> 注意：均为小写；

使用原生JavaScript即可实现字符串截取功能，有几个函数可以使用：
* stringObject.substr(start,length)

> start：必需。要抽取的子串的起始下标。必须是数值。如果是负数，那么该参数声明从字符串的尾部开始算起的位置。也就是说，-1 指字符串中最后一个字符，-2 指倒数第二个字符，以此类推。
> length：可选。子串中的字符数。必须是数值。如果省略了该参数，那么返回从 stringObject 的开始位置到结尾的字串。

返回值: 一个新的字符串，包含从 stringObject 的 start（包括 start 所指的字符） 处开始的 length 个字符。如果没有指定 length，那么返回的字符串包含从 start 到 stringObject 的结尾的字符。

* stringObject.substring(start,stop)

> start：必需。一个非负的整数，规定要提取的子串的第一个字符在 stringObject 中的位置。
> stop：可选。一个非负的整数，比要提取的子串的最后一个字符在 stringObject 中的位置多1。

如果省略该参数，那么返回的子串会一直到字符串的结尾。

返回值: 一个新的字符串，该字符串值包含 stringObject 的一个子字符串，其内容是从 start 处到 stop-1 处的所有字符，其长度为 stop 减start。 ...
