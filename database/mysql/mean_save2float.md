## mysql中sql语句，保留两位小数，求平均数

```
String sql = "select "
                + "te.teacher_id,"
                + "t.t_name,"
                + "t.t_number,"
                + "t.t_grade,"
                + "t.t_subject,"
                + "cast(avg(te.professional_knowledge) as decimal(5,2)) as professional,"
                + "cast(avg(te.prepare_ability) as decimal(5,2)) as prepare,"
                + "cast(avg(te.teach_ability) as decimal(5,2)) as teach,"
                + "cast(avg(te.practical_ability) as decimal(5,2)) as practical,"
                + "cast(avg(te.task_ability) as decimal(5,2)) as task,"
                + "cast(avg(te.course_satisfaction) as decimal(5,2)) as course,"
                + "cast(avg(te.comprehensive_ability) as decimal(5,2)) as comprehensive,"
                + "cast(avg(te.total_score/7.0) as decimal(5,2)) as total_avg "
                + "from teach_evaluation te join teacher t on te.teacher_id=t.ID "
                + "group by te.teacher_id";
```

> 注意中文空格和英文空格的区别
