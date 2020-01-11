**注：** 横线下面的答案来自牛客网的题解或者讨论区<br>
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
查找当前薪水详情以及部门编号dept_no
```
select s.* ,d.dept_no
from salaries as s, dept_manager as d 
where  s.emp_no=d.emp_no and s.to_date = '9999-01-01' and d.to_date='9999-01-01';
------------------

select s.* ,d.dept_no
from salaries as s 
join dept_manager as d 
on s.emp_no=d.emp_no
where s.to_date = '9999-01-01'
    and    d.to_date='9999-01-01';
```
查找所有已经分配部门的员工的last_name和first_name
```

select e.last_name, e.first_name, d.dept_no
from employees as e , dept_emp as d
where d.emp_no=e.emp_no
------------------------------------
下面写的其实和我写的一样，但是我写的据说不规范，对sql server可能没有用
SELECT
    e.last_name,
    e.first_name,
    d.dept_no
 
FROM
    dept_emp AS d
INNER JOIN
    employees AS e
ON
    e.emp_no = d.emp_no;


使用左连接查询时，employees中没有分配部门的员工（没有被记录在dept_emp表）dept_no字段被自动取NULL然后被输出，所以应当剔除（复合条件连接查询）。
select last_name,first_name,dept_no 
from employees left join dept_emp
on employees.emp_no = dept_emp.emp_no
where dept_emp.dept_no<>'';
```
查找所有员工的last_name和first_name以及对应部门编号dept_no
```
select last_name,first_name,dept_no 
from employees left join dept_emp
on employees.emp_no = dept_emp.emp_no;
```

查找所有员工入职时候的薪水情况
```
select e.emp_no, s.salary 
from employees as e, salaries as s
where e.emp_no = s.emp_no and e.hire_date = s.from_date
order by e.emp_no desc

select e.emp_no, s.salary 
from employees as e inner join salaries as s
on e.emp_no = s.emp_no and e.hire_date = s.from_date
order by e.emp_no desc
```
