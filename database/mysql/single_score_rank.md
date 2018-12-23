## 使用sql语句对单科成绩进行排名

对单科成绩进行排名：
```
String sql = "select "
                + "b.student_id,"
                + "b.name,"
                + "b.sum,"
                + "sc_grade,"
                + "(select count(1) + 1 from score a where a.sum > b.sum and a.sc_grade=?) sum_pm,"
                + "(select count(1) + 1 from score a where a.chinese_course > b.chinese_course and a.sc_grade=?) chinese_pm,"
                + "(select count(1) + 1 from score a where a.math_course > b.math_course and a.sc_grade=?) math_pm,"
                + "(select count(1) + 1 from score a where a.english_course > b.english_course and a.sc_grade=?) english_pm,"
                + "(select count(1) + 1 from score a where a.physics_course > b.physics_course and a.sc_grade=?) physics_pm,"
                + "(select count(1) + 1 from score a where a.chemistry_course > b.chemistry_course and a.sc_grade=?) chemistry_pm "
                + "from score b "
                + "where b.sc_grade=?";
```


rank() over,dense_rank(),row_number() 的区别：

* rank() over是的作用是查出指定条件后进行一个排名，但是有一个特点。假如是对学生排名，那么实用这个函数，成绩相同的两名是并列，例如下图1 2 2 4。
* dense_rank()的作用和rank()很像，唯一的一点区别就是，领命学生的成绩并列以后，下一位同学并不空出并列所占的名次，例如下图1 2 2 3。
* row_number()就不一样了，它和上面两种的区别就很明显了，这个函数不需要考虑是否并列，哪怕根据条件查询出来的数值相同也会进行连续排名，如下图
http://jingyan.baidu.com/article/597035521ff2ec8fc107404b.html

sql语句的写法：
`select 姓名,学号,总分,RANK() over(order by 总分 desc)排名 from 成绩表`
