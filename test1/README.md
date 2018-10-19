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
   
我认为查询一的语句是最优，因为代码中应该避免使用having子句，having只会在检索出所有记录之后才会对结果进行过滤，因此查询二代码会更慢
