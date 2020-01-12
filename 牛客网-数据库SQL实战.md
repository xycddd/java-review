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

-----------------------
丢了一些题的答案，烦


------------------------

对所有员工的薪水按照salary进行按照1-N的排名
```
----------------

本题的主要思想是复用salaries表进行比较排名，具体思路如下：
1、从两张相同的salaries表（分别为s1与s2）进行对比分析，先将两表限定条件设为to_date = '9999-01-01'，挑选出当前所有员工的薪水情况。
2、本题的精髓在于 s1.salary <= s2.salary，意思是在输出s1.salary的情况下，有多少个s2.salary大于等于s1.salary，比如当s1.salary=94409时，有3个s2.salary（分别为94692,94409,94409）大于等于它，但由于94409重复，利用COUNT(DISTINCT s2.salary)去重可得工资为94409的rank等于2。其余排名以此类推。
3、千万不要忘了GROUP BY s1.emp_no，否则输出的记录只有一条（可能是第一条或者最后一条，根据不同的数据库而定），因为用了合计函数COUNT()
4、最后先以 s1.salary 逆序排列，再以 s1.emp_no 顺序排列输出结果
SELECT s1.emp_no, s1.salary, COUNT(DISTINCT s2.salary) AS rank
FROM salaries AS s1, salaries AS s2
WHERE s1.to_date = '9999-01-01'  AND s2.to_date = '9999-01-01' AND s1.salary <= s2.salary
GROUP BY s1.emp_no
ORDER BY s1.salary DESC, s1.emp_no ASC
```
获取所有非manager员工当前的薪水情况
```
select de.dept_no, s.emp_no, s.salary 
from (employees as e inner join salaries as s on s.emp_no = e.emp_no and s.to_date = '9999-01-01')
inner join dept_emp as de ON e.emp_no = de.emp_no
where de.emp_no not in (select emp_no from dept_manager where to_date = '9999-01-01')
```

```
-------------------
本题主要思想是创建两张表（一张记录当前所有员工的工资，另一张只记录部门经理的工资）进行比较，具体思路如下：
1、先用INNER JOIN连接salaries和demp_emp，建立当前所有员工的工资记录sem
2、再用INNER JOIN连接salaries和demp_manager，建立当前所有员工的工资记录sdm
3、最后用限制条件sem.dept_no = sdm.dept_no AND sem.salary > sdm.salary找出同一部门中工资比经理高的员工，并根据题意依次输出emp_no、manager_no、emp_salary、manager_salary
SELECT sem.emp_no AS emp_no, sdm.emp_no AS manager_no, sem.salary AS emp_salary, sdm.salary AS manager_salary
FROM (SELECT s.salary, s.emp_no, de.dept_no FROM salaries s INNER JOIN dept_emp de
ON s.emp_no = de.emp_no AND s.to_date = '9999-01-01' ) AS sem, 
(SELECT s.salary, s.emp_no, dm.dept_no FROM salaries s INNER JOIN dept_manager dm
ON s.emp_no = dm.emp_no AND s.to_date = '9999-01-01' ) AS sdm
WHERE sem.dept_no = sdm.dept_no AND sem.salary > sdm.salary
```
