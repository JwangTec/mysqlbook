1. Combine Two Tables 175

	建表语句
	
	```
	Create table Person (PersonId int, FirstName varchar(255), LastName varchar(255))
	Create table Address (AddressId int, PersonId int, City varchar(255), State varchar(255))
	Truncate table Person
	insert into Person (PersonId, LastName, FirstName) values ('1', 'Wang', 'Allen')
	Truncate table Address
	insert into Address (AddressId, PersonId, City, State) values ('1', '2', 'New York City', 'New York')
	```

	Table: Person
	
	```
	+-------------+---------+
	| Column Name | Type    |
	+-------------+---------+
	| PersonId    | int     |
	| FirstName   | varchar |
	| LastName    | varchar |
	+-------------+---------+
	PersonId is the primary key column for this table.
	```
	
	Table: Address
	
	```
	+-------------+---------+
	| Column Name | Type    |
	+-------------+---------+
	| AddressId   | int     |
	| PersonId    | int     |
	| City        | varchar |
	| State       | varchar |
	+-------------+---------+
	AddressId is the primary key column for this table.
	
	```
	
	
	> 找出关于person的firstname和lastname city state，如果没有city或者state设为null
	
	>测试结果
	
	```
	
	{"headers":["FirstName","LastName","City","State"],"values":[["Allen","Wang",null,null]]}

	```
	
	答案:
	
	```
	select a.FirstName,a.LastName,b.City,b.State from Person as a left join Address as b on a.PersonId=b.PersonId
	```
	
2. 176 Second Highest Salary

	建表语句
	
	```
	Create table If Not Exists Employee (Id int, Salary int)
	Truncate table Employee
	insert into Employee (Id, Salary) values ('1', '100')
	insert into Employee (Id, Salary) values ('2', '200')
	insert into Employee (Id, Salary) values ('3', '300')
	```

	Employee
	
	```
	+----+--------+
	| Id | Salary |
	+----+--------+
	| 1  | 100    |
	| 2  | 200    |
	| 3  | 300    |
	+----+--------+
	```
	
	```
	返回第二高的薪水,如果上面有第二高的薪水 则返回第二高的薪水200,如果没有 就返回null
	```
	
	```
	+---------------------+
	| SecondHighestSalary |
	+---------------------+
	| 200                 |
	+---------------------+
	```
	
	答案:
	
	```
	
	select ifnull((select DISTINCT Salary from Employee ORDER BY Salary desc limit 1,1),null) as SecondHighestSalary
	```
	
	2.1 接上题,书写一个存储函数 书写一个关于算出排名第n的薪水的值,比如传递2就返回200 比如传递1 返回 300
	
	>讲函数写在里面
	
	```
	
	CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
	BEGIN
	  RETURN (
		......
	      
	  );
	END
	
	```
	
	答案:
	
	```
	
	CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
SET N = N - 1;
RETURN (
select ifnull((select DISTINCT salary
from Employee
order by salary Desc
Limit N,1), NULL) as 'getNthHighestSalary'
);
END
	```
	
3. https://leetcode.com/problems/rank-scores/description/

	@占位符,创建新字段
	
	https://leetcode.com/problems/consecutive-numbers/description/
	
4. 查找字段是否重复出现三次以上

	```
	
	Create table If Not Exists Logs (Id int, Num int)
	Truncate table Logs
	insert into Logs (Id, Num) values ('1', '1')
	insert into Logs (Id, Num) values ('2', '1')
	insert into Logs (Id, Num) values ('3', '1')
	insert into Logs (Id, Num) values ('4', '2')
	insert into Logs (Id, Num) values ('5', '1')
	insert into Logs (Id, Num) values ('6', '2')
	insert into Logs (Id, Num) values ('7', '2')
	```
	
	```
	+----+-----+
	| Id | Num |
	+----+-----+
	| 1  |  1  |
	| 2  |  1  |
	| 3  |  1  |
	| 4  |  2  |
	| 5  |  1  |
	| 6  |  2  |
	| 7  |  2  |
	+----+-----+
	```
	期望:
	
	```
	+-----------------+
	| ConsecutiveNums |
	+-----------------+
	| 1               |
	+-----------------+
	
	```
	答案:
	
	```
	select num  from `Logs` group by `Num` having count(num) >= 3
	```


5. 181 Employees Earning More Than Their Managers

	1. 语句

		```
		
		Create table If Not Exists Employee (Id int, Name varchar(255), Salary int, ManagerId int)
Truncate table Employee
insert into Employee (Id, Name, Salary, ManagerId) values ('1', 'Joe', '70000', '3')
insert into Employee (Id, Name, Salary, ManagerId) values ('2', 'Henry', '80000', '4')
insert into Employee (Id, Name, Salary, ManagerId) values ('3', 'Sam', '60000', 'None')
insert into Employee (Id, Name, Salary, ManagerId) values ('4', 'Max', '90000', 'None')
		
		```
		
	2. 表

		```
		
		+----+-------+--------+-----------+
		| Id | Name  | Salary | ManagerId |
		+----+-------+--------+-----------+
		| 1  | Joe   | 70000  | 3         |
		| 2  | Henry | 80000  | 4         |
		| 3  | Sam   | 60000  | NULL      |
		| 4  | Max   | 90000  | NULL      |
		+----+-------+--------+-----------+
		
		```
		
	3. 期望:

		找出雇员比经理工资高的雇员
		
		```
		
		+----------+
		| Employee |
		+----------+
		| Joe      |
		+----------+
		
		```
		
	4. 答案:

		```
		
		select E1.Name as Employee
from Employee as E1, Employee as E2 
where E1.ManagerId = E2.Id and E1.Salary > E2.Salary;

		```