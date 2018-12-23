## 使用sql语句对单科成绩进行排名

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
