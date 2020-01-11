**注：** 横线下面的答案来自牛客网的题解或者讨论区
查找最晚入职员工的所有信息
```
select * from employees where hire_date = (select max(hire_date) from employees);
---------------------------
select * from employees
    order by hire_date desc
    limit 1;
*/
 
/* 使用limit 与 offset关键字  */
/*
select * from employees
    order by hire_date desc
    limit 1 offset 0;
*/
 
/* 使用limit关键字 从第0条记录 向后读取一个，也就是第一条记录 */
/*
select * from employees
    order by hire_date desc
    limit 0,1;
*/
```

查找入职员工时间排名倒数第三的员工所有信息
```
select * from employees order by hire_date desc limit 1 offset 2;
-----------------
由于可能有重复，所以最好这样写
select * from employees 
where hire_date = (
    select distinct hire_date from employees order by hire_date desc limit 2,1
)

```
