## mysql查询结果添加自动编号

第一种方法：
`select (@i:=@i+1) as i,table_name.* from table_name,(select @i:=0) as it;`

第二种方法：
```
set @rownum=0;
select @rownum:=@rownum+1 as rownum, t.username from auth_user t limit 1,5;
```
