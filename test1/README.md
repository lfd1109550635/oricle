# oricle
## 1.查询1
<br>
![image](https://github.com/lfd1109550635/oricle/blob/master/test1/1.png)
![image](https://github.com/lfd1109550635/oricle/blob/master/test1/11.png)
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
<br>
## 2.查询2
![image](https://github.com/lfd1109550635/oricle/blob/master/test1/2.png)
![image](https://github.com/lfd1109550635/oricle/blob/master/test1/22.png)
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
## 3.自定义语句
```java
SELECT d.department_name ,count(e.job_id)as "部门总人数" ,
avg(e.salary)as "平均工资"
FROM hr.departments d, hr.employees e
WHERE d.department_id = e.department_id
and d.department_name = 'IT' or d.department_name = 'Sales'
GROUP BY department_name 
```
### 3.1查询结果
![blockchain](https://github.com/DevinChenPeng/oracle/blob/master/UD4K3WWOLD_O%5B8_S0CO60%5DI.png "区块链")

### 3.2优化指导
取消where语句处大量的笛卡尔积操作

## 4.总结
oracle的优化指导很好用
