# oricle

教材中的查询语句
查询1:  
![image](https://github.com/lfd1109550635/oracle/blob/master/TEST1/1.png)<br>
SELECT d.department_name，count（e.job_id）as“部门总人数” <br>
avg（e.salary）as“平均工资<br>
from hr.departments d，hr.employees e<br>
where d.department_id = e.department_id<br>
and d.department_name in ('IT'，'Sales')<br>
GROUP BY department_name;<br>
![image](https://github.com/lfd1109550635/oracle/blob/master/TEST1/11.png)<br>
查询2：  
![image](https://github.com/lfd1109550635/oracle/blob/master/TEST1/2.png)<br>
SELECT d.department_name，count(e.job_id)as "部门总人数"，<br>
avg(e.salary)as "平均工资"<br><br><br>
FROM hr.departments d，hr.employees e<br><br>
WHERE d.department_id = e.department_id<br>
GROUP BY department_name<br>
HAVING d.department_name in ('IT'，'Sales');<br>
![image](https://github.com/lfd1109550635/oracle/blob/master/TEST1/22.png)<br>
   
分析：查询1语句优于查询2语句，因为代码中应该避免使用having子句，而查询2语句中有，所以查询2语句更慢
3.自己的代码
```java
SELECT d.department_name ,count(e.job_id)as "部门总人数" ,
avg(e.salary)as "平均工资"
FROM hr.departments d, hr.employees e
WHERE d.department_id = e.department_id
and d.department_name = 'IT' or d.department_name = 'Sales'
GROUP BY department_name 
```
##3.1 自己的代码
![image](https://github.com/lfd1109550635/oricle/blob/master/test1/3.png)

3.2指导优化
取消where语句处大量的笛卡尔积操作

4  总结
oracle的优化指导很好用
