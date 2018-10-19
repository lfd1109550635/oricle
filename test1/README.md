# oricle
查询1
![image](https://github.com/lfd1109550635/oricle/blob/master/test1/1.png)
![image]
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
