schema

```
Create table If Not Exists Employee (Id int, Salary int)
Truncate table Employee
insert into Employee (Id, Salary) values ('1', '100')
insert into Employee (Id, Salary) values ('2', '200')
insert into Employee (Id, Salary) values ('3', '300')

```

Write a SQL query to get the second highest salary from the Employee table.

取出第二高的工资

```

+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+

```

比如 

```
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+

```

answer

```
select (
  select distinct Salary from Employee order by Salary Desc limit 1 offset 1
)as SecondHighestSalary

```