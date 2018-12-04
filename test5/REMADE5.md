
验五
## 用户提示
```flow js
  用户：linfudong233
  密码：123
```

#### 创建包
```sql
create or replace PACKAGE MyPack IS
  FUNCTION Get_SaleAmount(V_DEPARTMENT_ID NUMBER) RETURN NUMBER;
  PROCEDURE Get_Employees(V_EMPLOYEE_ID NUMBER);
END MyPack;
```
#### 创建函数及过程
```sql
create or replace PACKAGE BODY MyPack IS
  FUNCTION Get_SaleAmount(V_DEPARTMENT_ID NUMBER) RETURN NUMBER
  AS
    N NUMBER(20,2);
    BEGIN
      SELECT SUM(O.TRADE_RECEIVABLE) into N  FROM ORDERS O,EMPLOYEES E
      WHERE O.EMPLOYEE_ID=E.EMPLOYEE_ID AND E.DEPARTMENT_ID =V_DEPARTMENT_ID;
      RETURN N;
    END;
  PROCEDURE GET_EMPLOYEES(V_EMPLOYEE_ID NUMBER)
  AS
    LEFTSPACE VARCHAR(2000);
    begin
      LEFTSPACE:=' ';
      for v in
      (SELECT LEVEL,EMPLOYEE_ID,NAME,MANAGER_ID FROM employees
      START WITH EMPLOYEE_ID = V_EMPLOYEE_ID
      CONNECT BY PRIOR EMPLOYEE_ID = MANAGER_ID)
      LOOP
        DBMS_OUTPUT.PUT_LINE(LPAD(LEFTSPACE,(V.LEVEL-1)*4,' ')||
                             V.EMPLOYEE_ID||' '||v.NAME);
      END LOOP;
    END;
END MyPack;
```
![images](https://github.com/lfd1109550635/oricle/blob/master/test5/包创建及设置函数,过程11.png)
#### 调用函数及过程
```sql
select count(*) from orders;
select MyPack.Get_SaleAmount(11) AS 部门11应收金额,MyPack.Get_SaleAmount(12) AS 部门12应收金额 from dual;
```
![images](https://github.com/lfd1109550635/oricle/blob/master/test5/函数调用11.png)

```sql
set serveroutput on
DECLARE
  V_EMPLOYEE_ID NUMBER;    
BEGIN
  V_EMPLOYEE_ID := 1;
  MYPACK.Get_Employees (  V_EMPLOYEE_ID => V_EMPLOYEE_ID) ;  
  V_EMPLOYEE_ID := 11;
  MYPACK.Get_Employees (  V_EMPLOYEE_ID => V_EMPLOYEE_ID) ;    
END;
```
![images](https://github.com/lfd1109550635/oricle/blob/master/test5/过程调用11.png)


#### 提高订单统计速度

1.对表日期字段建立索引可减少统计时的数据查询时间，可减少统计时间。
insert into linfudong233.products (product_name,product_type) values ('computer2','电脑');

insert into linfudong233.products (product_name,product_type) values ('computer3','电脑');




insert into linfudong233.products (product_name,product_type) values ('phone1','手机');

insert into linfudong233.products (product_name,product_type) values ('phone2','手机');

insert into linfudong233.products (product_name,product_type) values ('phone3','手机');




insert into linfudong233.products (product_name,product_type) values ('paper1','耗材');

insert into linfudong233.products (product_name,product_type) values ('paper2','耗材');

insert into linfudong233.products (product_name,product_type) values ('paper3','耗材');







--插入订单数据

declare

  dt date;

  m number(8,2);

  V_EMPLOYEE_ID NUMBER(6);

  v_order_id number(10);

  v_name varchar2(100);

  v_tel varchar2(100);

  v number(10,2);




begin

  for i in 1..10000

  loop

    if i mod 2 =0 then

      dt:=to_date('2015-3-2','yyyy-mm-dd')+(i mod 60);

    else

      dt:=to_date('2016-3-2','yyyy-mm-dd')+(i mod 60);

    end if;

    V_EMPLOYEE_ID:=CASE I MOD 6 WHEN 0 THEN 11 WHEN 1 THEN 111 WHEN 2 THEN 112

                                WHEN 3 THEN 12 WHEN 4 THEN 121 ELSE 122 END;

    --插入订单

    v_order_id:=SEQ_ORDER_ID.nextval; --应该将SEQ_ORDER_ID.nextval保存到变量中。

    v_name := 'aa'|| 'aa';

    v_name := 'zhang' || i;

    v_tel := '139888883' || i;

    insert  into linfudong233.ORDERS (ORDER_ID,CUSTOMER_NAME,CUSTOMER_TEL,ORDER_DATE,EMPLOYEE_ID,DISCOUNT)

      values (v_order_id,v_name,v_tel,dt,V_EMPLOYEE_ID,dbms_random.value(100,0));

    --插入订单y一个订单包括3个产品

    v:=dbms_random.value(10000,4000);

    v_name:='computer'|| (i mod 3 + 1);

    insert  into linfudong233.ORDER_DETAILS(ID,ORDER_ID,PRODUCT_NAME,PRODUCT_NUM,PRODUCT_PRICE)

      values (SEQ_ORDER_DETAILS_ID.NEXTVAL,v_order_id,v_name,2,v);

    v:=dbms_random.value(1000,50);

    v_name:='paper'|| (i mod 3 + 1);

    insert  into linfudong233.ORDER_DETAILS(ID,ORDER_ID,PRODUCT_NAME,PRODUCT_NUM,PRODUCT_PRICE)

      values (SEQ_ORDER_DETAILS_ID.NEXTVAL,v_order_id,v_name,3,v);

    v:=dbms_random.value(9000,2000);

    v_name:='phone'|| (i mod 3 + 1);

    insert  into linfudong233.ORDER_DETAILS(ID,ORDER_ID,PRODUCT_NAME,PRODUCT_NUM,PRODUCT_PRICE)

      values (SEQ_ORDER_DETAILS_ID.NEXTVAL,v_order_id,v_name,1,v);

    --在触发器关闭的情况下，需要手工计算每个订单的应收金额：

    select sum(PRODUCT_NUM*PRODUCT_PRICE) into m from linfudong233.ORDER_DETAILS where ORDER_ID=v_order_id;

    if m is null then

     m:=0;

    end if;

    UPDATE ORDERS SET TRADE_RECEIVABLE = m - discount WHERE ORDER_ID=v_order_id;

    IF I MOD 1000 =0 THEN

      commit; --每次提交会加快插入数据的速度

    END IF;

  end loop;

end;







--动态增加一个PARTITION_BEFORE_2018分区

ALTER TABLE linfudong233.ORDERS

ADD PARTITION PARTITION_BEFORE_2018 VALUES LESS THAN (TO_DATE(' 2018-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'));




ALTER INDEX linfudong233.ORDERS_INDEX_DATE

MODIFY PARTITION PARTITION_BEFORE_2018

NOCOMPRESS;




--查询数据 id值从10012到20021

select * from linfudong233.ORDERS where  order_id=10012;

select * from linfudong233.ORDER_DETAILS where  order_id=10012;

select * from linfudong233.VIEW_ORDER_DETAILS where order_id=10012;







--递归查询某个员工及其所有下属，子下属员工

WITH A (EMPLOYEE_ID,NAME,EMAIL,PHONE_NUMBER,HIRE_DATE,SALARY,MANAGER_ID,DEPARTMENT_ID) AS

  (SELECT EMPLOYEE_ID,NAME,EMAIL,PHONE_NUMBER,HIRE_DATE,SALARY,MANAGER_ID,DEPARTMENT_ID

    FROM linfudong233.employees WHERE employee_ID = 11

    UNION ALL

  SELECT B.EMPLOYEE_ID,B.NAME,B.EMAIL,B.PHONE_NUMBER,B.HIRE_DATE,B.SALARY,B.MANAGER_ID,B.DEPARTMENT_ID

    FROM A, linfudong233.employees B WHERE A.EMPLOYEE_ID = B.MANAGER_ID)

SELECT * FROM A;

--或

SELECT * FROM employees START WITH EMPLOYEE_ID = 11 CONNECT BY PRIOR EMPLOYEE_ID = MANAGER_ID;

>更多详细sql语句请查看实验4.sql
