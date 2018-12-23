## JSONObject添加元素

### JSONObject的 put、accumulate、element 方法的区别：

* public Object put (Object key, Object value) 将value映射到key下。如果此JSONObject对象之前存在一个value在这个key下，
当前的value会替换掉之前的value

* public JSONObject accumulate (String key, Object value) 累积value到这个key下。这个方法同element()方法类似，特殊的
是，如果当前已经存在一个value在这个key下那么一个JSONArray将会存储在这个key下来保存所有累积的value。如果已经存在一个
JSONArray，那么当前的value就会添加到这个JSONArray中。相比之下replace方法会替代先前的value

* public JSONObject element (String key, Object value) 将键/值对放到这个JSONObject对象里面。如果当前value为空(null)，
那么如果这个key存在的话，这个key就会移除掉。如果这个key之前有value值，那么此方法会调用accumulate()方法。

```
//方法一：将某个元素添加到JSONObject对象中（当不知道该用什么方法的时候，alt+/键 看提示）
//student_score_detail.accumulate("s_grade", s_grade);
//student_score_detail.element("s_grade", s_grade);
student_score_detail.put("s_grade", s_grade);
```
