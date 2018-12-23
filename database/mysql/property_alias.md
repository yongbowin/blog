## mysql中字段取别名问题

在mysql中进行联合查询，给字段取别名问题：

```
String sql = "select "
                + "te.teacher_id,"
                + "t.t_name,"
                + "t.t_number,"
                + "t.t_grade,"
                + "t.t_subject,"
                + "avg(te.professional_knowledge) as professional,"
                + "avg(te.prepare_ability) as prepare,"
                + "avg(te.teach_ability) as teach,"
                + "avg(te.practical_ability) as practical,"
                + "avg(te.task_ability) as task,"
                + "avg(te.course_satisfaction) as course,"
                + "avg(te.comprehensive_ability) as comprehensive,"
                + "te.total_score "
                + "from teach_evaluation te join teacher t on te.teacher_id=t.ID "
                + "group by te.teacher_id";
```

这样就解决了。

但是，前边的几个字段还是取法用as取别名?

```
//在mysql中不能用as取字段的别名
//String sql = "select teacher_id AS iddd from teach_evaluation";
```
