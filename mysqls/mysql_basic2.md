## 基本查询（重要）

#### mysql 查询数据格式

1. 查询数据

	
	1. 查看所有数据,使用* 代表查询所有字段
	
		```
		select * from user;
		```
		
	2. 查询部分数据,字段名加逗号隔开,代表查询部分字段
	
		```
		select col1,col2 from user;
		```
		
	3. where 限制条件
	
		>使用where 表示查询必须符合某些条件的数据,你平时查询的时候不想去查询所有的数据
	
		```
		
		select col1,col2 from user where col1=xxx
		```
	
	4. limit 表示查询多少条数据或者从多少条到多少条数据
	
		```
		SELECT * FROM table LIMIT 5;     //检索前 5 个记录行
		
		SELECT * FROM table LIMIT 95,-1; // 检索记录行 96-last
		
		SELECT * FROM table LIMIT 5,10; //检索记录行6-15
		```
		
	5. order by 排列数据顺序,可以让数据遵从某个字段来排序,desc 倒序,asc正序
	
		```
		
		select * from test order by case when num = -1 then 1 else 0 end desc;
		
		```
	
	6. group by 分组查询,把某个列相同的数据分为一组来查询
	
		>按照grade一样的数据分组查询
		```
		select max(user_id),grade from user_info group by grade ;
		```
		
	7. in,between in表示在其中的数据,between表示在两个数之间的数据
	
		```
		
		select * from atable where column beteen min and max  //表示在min 和max之间的数据
		elect * from atable where column in(n1,n2,n3) //表示数据是 n1或者n2或者n3
		```
		
	8. or,and 表示在数据满足一个或者全部都满足
	
		```
		
		select * from atable where col1=n1 or col2=n2//表示数据必须满足启动一项
		
		select * from atable where col1=n1 and col2=n2//表示数据必须满足两个才行
		
		```
		
	9. 运算符,可以使用运算符表示数据大于 小于等于等情况,
	
		
		```
		
		select * from atable where col1>=n1//col 大于等于n1
		
		select * from atable where col1!=n1//col不等于n1
		
		```
		
	10. like 模糊查询,使用like语句表示查询的时候匹配查询, %表示0，1或者多个字符的占位符, _ 表示一个字符的占位符
	
		```
		select * from atable where col1 like '%test1%'//col
		```

	

2. 增加数据
	1. 在insert 语句中列出表中的所有字段名，其值与其字段名、类型要一一对应
	    ```
		 INSERT INTO 表名 （字段名1，字段名2，...）
        VALUES(值1，值2，...); 
        ```
        >“字段名1，字段名2，…”表示数据表中的字段名称，此处应为表中所有字段名称。 
“值1，值2…”表示每一个字段的值，其值顺序、类型须与对应的字段相匹配！
        
   2. insert语句中不指定字段名

   		在不指定字段名的情况下，直接用values为其默认赋值
   		``` INSERT INTO 表名 VALUES(值1，值2，....);```
   		
3. 更新数据

	更新数据，即对表中存在的数据进行修改。
	
	
	```
	UPDATE 表名
   SET  字段名1=值1[,字段名2=值2，...]
   [WHERE 条件表达式] 
   ```
   
   >字段名1，字段名2，用于指定更新的字段名称 值1，值2，用于表示字段更新的新数据。 where条件表达式，可选参数，用于指定更新数据需要满足的条件。
   
4. 删除数据

	``` DELETE FROM 表名 [ WHERE 条件表达式 ]  ; ```
	
	> 表名指的是要执行删除操作的表。 
where 条件表达式，可选参数，只要满足条件的记录会被删除！

## 聚合函数 (重要)

#### 简介 

聚合函数aggregation function又称为组函数。 认情况下 聚合函数会对当前所在表当做一个组进行统计。

#### 聚合函数的特点

1.每个组函数接收一个参数（字段名或者表达式） 统计结果中默认忽略字段为NULL的记录

2.要想列值为NULL的行也参与组函数的计算，必须使用IFNULL函数对NULL值做转换。

3.不允许出现嵌套 比如sum(max(xx))

#### 详细的聚合函数 

1. count 求数据表的行数

    	```select count(*|字段名) from 数据表```

2. max| min  求某列的最大|小数值
	
		``` select max(字段名）from 数据表 ```
	
3. sum 对数据表的某列进行求和操作

		``` select sum(字段名) from 数据表 ```
	
4. avg，对数据表的某列进行求平均值操作

		``` select avg(字段名） from 数据表```

5. GROUP BY 分组

		```SELECT 列名 COUNT（*）表名 GROUP BU 列名 ```
	
6. 去重 distinct 用来去除查询结果中的重复记录,具体的语法如下

		``` select distinct expression[,expression...] from tables [where conditions]; ```
		
	列如:
	
	建表语句
		
	```
		CREATE TABLE `person` (
		    `id`  int(11) NOT NULL AUTO_INCREMENT ,
		    `name`  varchar(30) NULL DEFAULT NULL ,
		    `country`  varchar(50) NULL DEFAULT NULL ,
		    `province`  varchar(30) NULL DEFAULT NULL ,
		    `city`  varchar(30) NULL DEFAULT NULL ,
		    PRIMARY KEY (`id`)
		)ENGINE=InnoDB
		;
	```
	插入语句	
		
	```
		insert into `test111`.`person1` ( `country`, `province`, `name`, `city`) values ( 'China', 'sichuang', 'abc1', 'chengdu');
		insert into `test111`.`person1` ( `country`, `province`, `name`, `city`) values ( 'China', 'sichuang', 'abc2', 'mianyang');
		insert into `test111`.`person1` ( `country`, `province`, `name`, `city`) values ( 'China', 'chongqing', 'abc3', 'chongqing');
		insert into `test111`.`person1` ( `country`, `province`, `name`, `city`) values ( 'japan', 'bingku', 'abc4', 'shenhu');
		insert into `test111`.`person1` ( `country`, `province`, `name`, `city`) values ( 'korea', 'jingji', 'abc5', 'shuiyuan');
	```
	
	1. 只对一列操作

		```
		select distinct country from person1
		
		```
		
	2. 多列操作

		```
		
		select distinct country, province from person1
		```
		
		>从上例中可以发现，当distinct应用到多个字段的时候，其应用的范围是其后面的所有字段，而不只是紧挨着它的一个字段，而且distinct只能放到所有字段的前面
		
7. having表示筛选 和where不同点在于having后面可以跟上聚合函数

	```
	
	SELECT region, SUM(population), SUM(area)
	FROM bbc
	GROUP BY region
	HAVING SUM(area)>1000000
	```

##  mysql 多表查询

1. 说明:

	之前的讨论都是针对于单表的查询,接下来将讨论多表的查询,多表之间的关系。

2. 准备数据

	>创建数据表t1
	
	```
	CREATE TABLE `t1` (`i1` int(11) NOT NULL,`c1` varchar(255) DEFAULT NULL,PRIMARY KEY (`i1`)) 
	```
	
	>创建数据表t2
	
	```
	CREATE TABLE `t2` (`i2` int(11) NOT NULL,`c2` varchar(255) DEFAULT NULL,PRIMARY KEY (`i1`)) 
	```
	
	>插入数据
	
	```
	
	insert into t1 (i1,c1) values(1,'a'),(2,'b'),(3,'c');
	
	insert into t2 (i2,c2) values(2,'c'),(3,'b'),(4,'a');
	```
	
	显示如下 t1
	
	```
	+----+------+
	| i1 | c1   |
	+----+------+
	|  1 | a    |
	|  2 | b    |
	|  3 | c    |
	+----+------+
	
	```
	t2
	
	```
	+----+------+
	| i2 | c2   |
	+----+------+
	|  2 | c    |
	|  3 | b    |
	|  4 | a    |
	+----+------+
	
	```

3. 内联结

	1. 说明:
	
	    如果你在SELECT语句的FROM子句里列出了多个数据表，并用INNER JOIN将它们的名字隔开，MySQL将执行内联结（inner join）操作，这将通过把一个数据表里的数据行与另一个数据表里的数据行进行匹配来产生结果。
	    
	 2. 语句
	
		>执行查询语句
		
		```	
		mysql> select * from t1 inner join t2;	
		```
		
		>结果
		
		```
		+----+------+----+------+
		| i1 | c1   | i2 | c2   |
		+----+------+----+------+
		|  1 | a    |  2 | c    |
		|  2 | b    |  2 | c    |
		|  3 | c    |  2 | c    |
		|  1 | a    |  3 | b    |
		|  2 | b    |  3 | b    |
		|  3 | c    |  3 | b    |
		|  1 | a    |  4 | a    |
		|  2 | b    |  4 | a    |
		|  3 | c    |  4 | a    |
		+----+------+----+------+	
		```
		
		>在这条语句里，SELECT \*的含义是，从FROM子句所列出的每一个数据表里选取每一个数据列。你也可以把这写成SELECT t1.\*，t2.\*：
		
		```
		select t1.*,t2.* from t1 inner join t2;
		```
		
       <b>根据某个数据表里的每一个数据行与另一个数据表里的每一个数据行得到全部可能的组合的联结操作叫做生成笛卡儿积（cartesian product）。像这样来联结数据表有可能产生非常多的结果数据行，因为结果数据行的总数将是对每个数据表的数据行个数进行连续乘法而得到的积。假设你有3个数据表分别包含100、200和300个数据行，它们的交叉联结将返回6百万个（100×200×300）数据行。这可是个相当大的数字，而那3个数据表都很小。在这种情况下，通常需要增加一条WHERE子句以便把结果集压缩到一个更便于管理的尺寸。</b>
       
       >执行语句
       
       ```
       elect t1.*,t2.* from t1 inner join t2 where t1.i1=t2.i2;
       ```
       
       >结果
       
       ```
       +----+----+----+----+
		| i1 | c1 | i2 | c2 |
		+----+----+----+----+
		| 2  | b  | 2  | c  |
		| 3  | c  | 3  | b  |
		+----+----+----+----+
       ```
       
   3. 等价的查询

   	    1. CROSS JOIN和JOIN联结类型类似于INNER JOIN。

	   	    ```
	   	    select t1.*,t2.* from t1 cross join t2 where t1.i1=t2.i2;
	   	    
	   	    select t1.*,t2.* from t1 join t2 where t1.i1=t2.i2;
	   	    
	   	    ```
   	    
   	    2. “,”（逗号）关联操作符的效果与INNER JOIN相似

	   	    ```
	   	    select t1.*,t2.* from t1,t2 where t1.i1=t2.i2;
	   	    ```
	   	    
	   	>请注意，逗号操作符的优先级和其他联结类型不一样，有时还会导致语法错误，而其他联结类型没有这些问题。我个人认为应该尽量避免使用逗号操作符。
	   	 
	   	
	   	3.  on语句

	   		用一条ON子句代替WHERE子句。下面是一个使用了ON子句的INNER JOIN操作的例子
	   		
	   		```
	   		select t1.*,t2.* from t1 join t2 on t1.i1=t2.i2;
	   		```
	   		
	  
	   4. 别名的使用

	       在SELECT语句里给出的数据列的名字不允许产生歧义，而且必须来自FROM子句里的某个数据表。如果只涉及一个数据表，就不会产生歧义，被列出的所有数据列都来自那个数据表。如果涉及多个数据表，只在一个表中出现的数据列名称不会产生歧义。不过，如果某个数据列的名字出现在多个数据表里，在引用这个数据列时必须使用tbl_namel.col_name语法给它加上一个数据表的名字来表明它来自哪一个数据表。
	       
	     直接指定表和字段名称
	       
		  ```
		  select t1.i1,t2.i2 from t1 join t2 on t1.i1=t2.i2;
		  ```
	    给表起别名
	   		
		   	```
		   	select a.i1,b.i2 from t1 as a join t2 as b on a.i1=b.i2;
		   	```
		   	
4. 外连接

    1. 说明:

    	内联结只显示在两个数据表里都能找到匹配的数据行。外联结除了显示同样的匹配结果，还可以把其中一个数据表在另一个数据表里没有匹配的数据行也显示出来。外联结分左联结和右联结两种。本小节里的绝大多数例子使用的是LEFT JOIN，意思是把左数据表在右数据表里没有匹配的数据行也显示出来。RIGHT JOIN刚好与此相反，它将把右数据表在左数据表里没有匹配的数据行也显示出来，数据表的角色调换了一下。
    	
    	LEFT JOIN的工作情况是这样的：你给出用来匹配两个数据表里的数据行的数据列，当来自左数据表的某个数据行与来自右数据表的某个数据行匹配时，那两个数据行的内容就会被选取为一个输出数据行；如果来自左数据表的某个数据行在右数据表里找不到匹配，它也会被选取为一个输出数据行，此时与它联结的是一个来自右数据表的“假”数据行，这个“假”数据行的所有数据列都包含NULL值。
    	
    
    2. LEFT JOIN

    	```
    	select a.i1,b.i2 from t1 as a left join t2 as b on a.i1=b.i2;
    	```
    	
    	```
    	+----+--------+
		| i1 | i2     |
		+----+--------+
		| 1  | <null> |
		| 2  | 2      |
		| 3  | 3      |
		+----+--------+
    	```
    	
    3. right join

    	```
    	
    	elect a.i1,b.i2 from t1 as a right join t2 as b on a.i1=b.i2;
    	```
    	
    	```
    	+--------+----+
		| i1     | i2 |
		+--------+----+
		| 2      | 2  |
		| 3      | 3  |
		| <null> | 4  |
		+--------+----+
    	```
    	
    4. 用处

    	LEFT JOIN很有用，尤其是在你只想找出在右数据表里没有匹配的左数据表的行时。具体地说，增加一条WHERE子句，让它把右数据表的数据列全部是NULL值的（也就是那些在一个数据表里有匹配，但在另一个数据表里没有匹配）数据行筛选出来
    	
    	```
    	 select a.i1,b.i2 from t1 as a right join t2 as b on a.i1=b.i2 where a.i1 is null;
    	```
    	
    	```
		+--------+----+
		| i1     | i2 |
		+--------+----+
		| <null> | 4  |
		+--------+----+
    	```
    	
    	>LEFT JOIN还有几种同义词和变体。LEFT OUTER JOIN是LEFT JOIN的一个同义词


## mysql 子查询

#### 数据准备

```
CREATE TABLE `goods` (
  `id` int(11) NOT NULL,
  `name` varchar(20) DEFAULT NULL,
  `kind` varchar(20) DEFAULT NULL,
  `price` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1
```


```
insert into `test111`.`goods` ( `id`, `kind`, `name`, `price`) values ( '1', 'phone', 'phone1', '1000');
insert into `test111`.`goods` ( `id`, `kind`, `name`, `price`) values ( '2', 'phone', 'phone2', '999');
insert into `test111`.`goods` ( `id`, `kind`, `name`, `price`) values ( '3', 'phone', 'phone3', '1001');
insert into `test111`.`goods` ( `id`, `kind`, `name`, `price`) values ( '4', 'phone', 'phone4', '3000');
insert into `test111`.`goods` ( `id`, `kind`, `name`, `price`) values ( '5', 'car', 'car1', '400000');
insert into `test111`.`goods` ( `id`, `kind`, `name`, `price`) values ( '6', 'car', 'car2', '800000');
insert into `test111`.`goods` ( `id`, `kind`, `name`, `price`) values ( '7', 'car', 'car3', '150000');
insert into `test111`.`goods` ( `id`, `kind`, `name`, `price`) values ( '8', 'com', 'computer1', '10000');
insert into `test111`.`goods` ( `id`, `kind`, `name`, `price`) values ( '9', 'com', 'computer2', '8000');
insert into `test111`.`goods` ( `id`, `kind`, `name`, `price`) values ( '10', 'com', 'computer3', '9000');
```

#### 概述 mysql可以把查询的结果做为另外一个查询的条件来使用

#### 1、where型子查询（把内层查询结果当作外层查询的比较条件）

1. 查询id最大的一件商品(使用排序+分页实现)

	```select * from goods ORDER BY id desc LIMIT 0,1 ```
	
2. 查询id最大的一件商品(使用where子查询实现)

	``` select * from goods where id=(select max(id) from goods); ```
	
3. 查询每个类别下id最大的商品(使用where子查询实现)

	``` select * from goods where id in (select max(id) from goods group by kind)```
	
#### 2.from型子查询(把内层的查询结果当成临时表，供外层sql再次查询。查询结果集可以当成表看待。临时表要使用一个别名。)

1. 查询car的值,重点 使用了 as tmp给内存表取名字

	``` select * from (select * from goods where kind='car') as tmp order by id desc```
	

## some any all exists

#### 数据准备

建表

```
CREATE TABLE salary_table(id SMALLINT UNSIGNED NOT NULL PRIMARY KEY AUTO_INCREMENT,position VARCHAR(40) NOT NULL,salary INT);
```

插入数据

```
INSERT salary_table(position,salary) VALUES('JAVA',8000),('Java',8400),('Java',9000),('Python',6500),('Python',10000),('Python',8900);
```


#### 1. any 和 some


```select * from salary_table where salary > some (select salary from salary_table where position = 'Python'); ```
> some和any会帮助我们筛选出最小的一个数来作为条件



#### 2. all

```select * from salary_table where salary > all (select salary from salary_table where position = 'java'); ```

> all 会筛选出满足所有条件的选项


#### some 和 any 等于 in

```
SELECT * FROM salary_table WHERE salary IN (SELECT salary FROM salary_table WHERE position = 'Python');
```

```
SELECT * FROM salary_table WHERE salary =some (SELECT salary FROM salary_table WHERE position = 'Python');
```

> = some 或者 any可以帮助我们筛选出来像in一样的条件语句

#### 3. exists

exists会判断数据是否存在 如果不存在则不会筛选数据

``` select * from salary_table where exists(SELECT * from salary_table where id = 1)```
