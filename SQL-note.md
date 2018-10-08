# 基本概念

SQL: Structured Query Language，是访问和操作数据库的标准语言。

RDBMS：Relational Database Management System 关系型数据库管理系统。

RDBMS中的数据存储在表中。table由行和列组成。Column也叫做filed，row也叫做record。



# SQL语法

SQL的关键字是不区分大小写的。

有些数据库要求SQL语句要以分号结尾。一般来说，都要这么做。



### **SELECT**

extracts data from a database

返回的数据存储在result table中，称为result-set。

```sql
SELECT column1, column2 FROM table_name;
SELECT * FROM table_name;
```



### **SELECT DISTINCT**

只返回distinct的数据。

```sql
SELECT DISTINCT column1, column2 FROM table_name;
```

获取某一列中独特值的数量：

```sql
SELECT COUNT(DISTINCT column1) FROM table_name;
```



### **WHERE**

用于筛选record，WHERE后面跟某种条件。

```sql
SELECT column1, column2 FROM table_name WHERE condition;
```

注意：WHERE除了可以和SELECT，还可以和UPDATE，DELETE等搭配使用。

条件语句中，text value必须带上单引号（大部分数据库也允许是双引号）；而numeric value就不需要引号。

条件语句中常用的操作符有：`=`,`<>`,`>`,`<`,`>=`,`<=`,`BETWEEN`,`LIKE`,`IN`



### **AND, OR, NOT**

条件语句可以和**与或非**一起使用，使得条件复杂化。

```sql
SELECT column1, column2 FROM table_name WHERE condition1 AND condition2;
SELECT column1, column2 FROM table_name WHERE condition1 OR condition2;
SELECT column1, column2 FROM table_name WHERE NOT condition;
```

三者可以结合在一起使用。



**ORDER BY**

用于将result-set排序，升序或是降序。

```sql
SELECT column1, column2, column3 FROM table_name ORDER BY column1 ASC, column2 DESC;
```

注意：

- 默认的是升序ASC
- 可以按照多个列来排序
- 并且每列的排序规则（升序还是降序）都是独立的。



### **INSERT INTO**

inserts new data into a database

在表中插入新的纪录。

```sql
INSERT INTO table_name (column1, column2, column3) VALUES (value1, value2, value3);
```

如果是插入完整的包含所有列的一行，那么无需指明列名，语句简化如下：

```sql
INSERT INTO table_name VALUES (value1, value2, value3, ...);
```



### **NULL Values**

结合`WHERE`使用的方法是：

```sql
SELECT column_names FROM table_name WHERE column_name IS NULL;
SELECT column_names FROM table_name WHERE column_name IS NOT NULL;
```



### **UPDATE**

updates data in a database

语法：

```sql
UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
```

如果没有`WHERE`，那么表中所有行都会被更新。



### **DELETE**

deletes data from a database

从表中删除已存在的记录。

语法：

```sql
DELETE FROM table_name WHERE condition;
```

注意：如果没有`WHERE`，那么表中所有行都会被删除！



### **TOP, LIMIT, ROWNUM**

这些语句都是用来在查询时返回指定数量的记录。

（1）TOP：适用于SQL Server，MS Access

语法：

```sql
SELECT TOP number|percent column_names FROM table_name WHERE condition;
```

（2）LIMIT：适用于MySQL

语法：

```sql
SELECT column_names FROM table_name WHERE condition LIMIT number;
```

（3）ROWNUM：适用于Oracle

语法：

```sql
SELECT column_names FROM table_name WHERE ROWNUM <= number;
```



### **MIN(), MAX() 函数**

返回的是选定列的最小和最大值。（不是所在的那一行，是单个值）

语法分别是：

```sql
SELECT MIN(column_name) FROM table_name WHERE condition;
SELECT MAX(column_name) FROM table_name WHERE condition;
```

可以结合`AS`一起使用，如：

```sql
SELECT MIN(Price) AS SmallestPrice FROM Products;
```



### **COUNT(), AVG(), SUM() 函数**

- COUNT() 函数返回符合指定条件的行数
- AVG() 函数返回一个数值型列的平均值
- SUM() 函数返回一个数值型列的总和

```sql
SELECT COUNT(column_name) FROM table_name WHERE condition;
SELECT AVG(column_name) FROM table_name WHERE condition;
SELECT SUM(column_name) FROM table_name WHERE condition;
```

> 注意：这些函数的参数里只能包含一个列名，不能有多个。



### **LIKE 操作符**

LIKE用在WHERE语句中，表示符合某种自定义的pattern。

语法：

```sql
SELECT column1, column2 FROM table_name WHERE columnN LIKE pattern;
```

`pattern`的写法中常用到两个**通配符**：

- %，表示>=0个任意的字符
- _，表示一个字符。（在MS Access中不用 _ 而用 ?）



### **Wildcards**

通配符除了上面提到的%和?，在MS Access和SQL Server中还是有：

- [*charlist*] - Defines sets and ranges of characters to match
- [^*charlist*] or [!*charlist*] - Defines sets and ranges of characters NOT to match



### **IN 操作符**

IN 用在WHERE语句中，表示符合某几种条件的一种。

语法：

```sql
SELECT column_names FROM table_name WHERE column1 IN (value1, value2, ...);
SELECT column_names FROM table_name WHERE column1 IN (SELECT statement);
// 和NOT一起
SELECT column_names FROM table_name WHERE column1 NOT IN (value1, value2, ...);
```



### **BETWEEN 操作符**

用在WHERE语句中，界定了条件的值的范围。适用于number， text， date。

语法：

```sql
SELECT column_names FROM table_name WHERE column1 BETWEEN value1 AND value2;
SELECT column_names FROM table_name WHERE column1 NOT BETWEEN value1 AND value2;
```

> 注意：表示date的格式是：#01/01/1970#代表1970年1月1日，即#DD/MM/YYYY#。
>
> 或者'YYYY-MM-DD'



### **Aliases**

用于给表格或者列一个临时名字。这样做通常的出于可读性考虑。

这个临时名字只在query持续期间有效。

语法：

```sql
// column
SELECT column_name AS alias_name FROM table_name;
// table
SELECT column_names FROM table_name AS alias_name;
```



### JOIN

> A **JOIN** clause is used to combine rows from two or more tables, based on a related column between them. 



JOIN的类型：

- **(INNER) JOIN**: Returns records that have matching values in both tables
- **LEFT (OUTER) JOIN**: Return all records from the left table, and the matched records from the right table. The result is NULL from the right side, if there is no match. 
- **RIGHT (OUTER) JOIN**: Return all records from the right table, and the matched records from the left table. The result is NULL from the left side, when there is no match. 
- **FULL (OUTER) JOIN**: Return all records when there is a match in either left or right table



#### INNER JOIN

语法：

```sql
SELECT column_names
FROM table1
INNER JOIN table2 ON table1.column_name = table2.column_name;
```

![SQL INNER JOIN](https://www.w3schools.com/sql/img_innerjoin.gif) 



注意：可以JOIN多于两张表。



#### LEFT JOIN

语法：

```sql
SELECT column_names
FROM table1
LEFT JOIN table2 ON table1.column_name = table2.column_name;
```

![SQL LEFT JOIN](https://www.w3schools.com/sql/img_leftjoin.gif) 



#### RIGHT JOIN

语法：

```sql
SELECT column_names
FROM table1
RIGHT JOIN table2 ON table1.column_name = table2.column_name;
```

![SQL RIGHT JOIN](https://www.w3schools.com/sql/img_rightjoin.gif) 



#### FULL JOIN

语法：

```sql
SELECT column_names
FROM table1
FULL OUTER JOIN table2 ON table1.column_name = table2.column_name;
```

![SQL FULL OUTER JOIN](https://www.w3schools.com/sql/img_fulljoin.gif) 

注意：FULL JOIN可能会返回很大的结果集。



#### SELF JOIN

> A self JOIN is a regular join, but the table is joined with itself. 

语法：

```sql
SELECT column_name(s)
FROM table1 T1, table1 T2
WHERE condition;
```



### UNION 操作符

UNION用于将两个或更多SELECT语句的结果合并在一起。使用UNION有这些限制条件：

- 每个SELECT语句的列数要一致；
- 列的数据类型类似；
- 列的顺序要一样。

语法：

```
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```

注意：

1. UNION默认只选取distinct的值。若要允许重复值，那么将上面的UNION换成UNION ALL即可。
2. 结果中的列名一般是和第一个SELECT中的列名一样。



### GROUP BY

`GROUP BY`通常是和聚类函数（COUNT, MAX, MIN, SUM, AVG）一起使用。

语法：

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s);
```



### HAVING

> The HAVING clause was added to SQL because the WHERE keyword could not be used with **aggregate** functions. 

语法：

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s);
// example
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5
ORDER BY COUNT(CustomerID) DESC;
```

注意：GROUP BY， HAVING, ORDER BY的顺序不能乱，否则就是语法错误。GROUP BY是必须的。



### EXISTS 操作符

> The EXISTS operator is used to test for the existence of any record in a subquery.
>
> The EXISTS operator returns true if the subquery returns one or more records.

语法：

EXISTS用在WHERE后，表示一种条件。

```sql
SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);
```



### ANY, ALL 操作符

用在WHERE或HAVING后。

> The ANY operator returns true if any of the subquery values meet the condition.
>
> The ALL operator returns true if all of the subquery values meet the condition.

语法：

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name operator ANY/ALL
(SELECT column_name FROM table_name WHERE condition);
```

> **Note:** The *operator* must be a standard comparison operator (=, <>, !=, >, >=, <, or <=). 



### SELECT INTO

用于将数据从一张表复制到另一张**新表**里。

语法：

```sql
SELECT * INTO newtable [IN externaldb]
FROM oldtable
WHERE condition;
// 或者
SELECT column1, column2, ...
INTO newtable [IN externaldb]
FROM oldtable
WHERE condition;
```

- `SELECT INTO`可以和`JOIN`一起使用，从多张表中复制数据。

- 复制表的schema而不复制数据：

  ```sql
  SELECT * INTO newtable
  FROM oldtable
  WHERE 1 = 0;
  ```



### INSERT INTO SELECT

从一张表里复制数据，再插入到另一张表里。

- 要求两张表的数据类型是匹配的。
- target table的已存在数据不会受影响。

语法：

```sql
INSERT INTO table2
SELECT * FROM table1
WHERE condition;
// 或者
INERT INTO table2 (column1, column2, ...)
SELECT column1, column2, ... FROM table1
WHERE condition;
```



### Stored Procedures for SQL Server

将一段SQL代码存起来，以便后面复用。也支持传入参数。（就像脚本一样把）。

语法：

```sql
CREATE PROCEDURE procedure_name
AS
sql_statement
GO;
```

执行：

```sql
EXEC procedure_name;
```

如何传参：

- 单个参数

```sql
CREATE PROCEDURE SelectAllCustomers @City nvarchar(30)
AS
SELECT * FROM Customers WHERE City = @City
GO;
// 执行
EXEC SelectAllCustomers City = "London"
```

- 多个参数：只需列出多个参数和数据类型，以逗号分隔

```sql
CREATE PROCEDURE SelectAllCustomers @City nvarchar(30), @Country nvarchar(30)
AS
SELECT * FROM Customers WHERE City = @City AND Country = @Country
GO;
// 执行
EXEC SelectAllCustomers City = "London", Country = "England"
```



### 注释

- 单行注释：注释符号是`--`
- 多行注释：`/*...*/`，类似Java，也呢个用于注释一行中的一段。



### **CREATE DATABASE**

creates a new database

创建数据库：

```sql
CREATE DATABASE testDB;
```

检查数据库是否创建成功：

```sql
SHOW DATABASES;
```

> 注意：sqlite不是这样创建数据库的，而是`sqlite3 test.db`这种形式。



### **DROP DATABASE**

删除数据库，慎用！语法：

```sql
DROP DATABASE testDB;
```



### **CREATE TABLE**

creates a new table

创建新表，要设定表名、列名和每一列的数据类型：

```sql
CREATE TABLE table_name (
	column1 datatype;
    column2 datatype;
    ...
);
```

> 也适用于sqlite

关于数据类型的知识，要另开一节记录。

一种特别的case是根据已有的表创建新表：

```sql
CREATE TABLE new_table_name AS
    SELECT column1, column2,...
    FROM existing_table_name
    WHERE ....;
```



### **DROP TABLE**

deletes a table

删除表格，语法：

```sql
DROP TABLE table_name;
// 也适用于sqlite
```

另外，如果只想删除表格中内容并保留表格，可以用：

```sql
TRUNCATE TABLE table_name;
```



### **ALTER TABLE**

modifies a table

用于增、删、改表格中的**列**。

增加列：

```sql
ALTER TABLE table_name ADD column_name datatype;
```

删除列：

```sql
ALTER TABLE table_name DROP COLUMN column_name;
```

修改一列的数据类型：不同的数据库有不同的语法形式

```sql
// SQL Server/MS Access
ALTER TABLE table_name ALTER COLUMN column_name datatype;
// My SQL/Oracle(<10G)
ALTER TABLE table_name MODIFY COLUMN column_name datatype;
// Oracle(>10G)
ALTER TABLE table_name MODIFY column_name datatype;
```



### **Constraints**

数据必须遵循一档的规则，常在`CREATE TABLE`或者`ALTER TABLE`时使用。例如：

```sql
CREATE TABLE table_name (
	column1 datatype constraint,
	column2 datatype constraint,
	column3 datatype constraint,
	...
)
```

常用的限制规则有：

#### **NOT NULL** 

Ensures that a column can **NOT** have a NULL value

#### **UNIQUE**

Ensures that all values in a column are different

设置表格中某列或者某几列为UNIQUE：

```sql
ALTER TABLE table_name ADD UNIQUE (column_name);

ALTER TABLE table_name ADD CONSTRAINT constraint_name UNIQUE (column1, column2);
```

删除之前设置的UNIQUE限制：

```sql
// MySQL
ALTER TABLE table_name DROP INDEX constraint_name;
// SQK Server/Oracle/MS Access
ALTER TABLE table_name DROP CONSTRAINT constraint_name;
```



#### **PRIMARY KEY**

A combination of a NOT NULL and UNIQUE. Uniquely identifies each row in a table

创建表格时设置一列为主键：

```sql
// MySQL
CREATE TABLE Persons (
	ID int NOT NULL,
    PRIMARY KEY (ID)
);

// SQL Server / Oracle / MS Access
CREATE TABLE Persons (
	ID int NOT NULL PRIMARY KEY
);
```

几列为主键：

```sql
CREATE TABLE table_name (
	CONSTRAINT constraint_name PRIMARY KEY (column1, column2)
);
```

修改表格时设置一列或几列为主键：

```sql
ALTER TABLE table_name ADD PRIMARY KEY (column_name);

ALTER TABLE table_name ADD CONSTRAINT constraint_name PRIMARY KEY (column1, column2);
```

删除主键限制：

```sql
// MySQL
ALTER TABLE table_name DROP PRIMARY KEY;
// SQK Server/Oracle/MS Access
ALTER TABLE table_name DROP CONSTRAINT constraint_name;
```

> 一个表中可以有很多列是**UNIQUE**的，但只能有一列是**PRIMARY KEY**



#### **FOREIGN KEY**

Uniquely identifies a row/record in another table

外键用于联系两个表。表A中的外键是表B中的主键。因此在设置外键时要写明reference。

创建表格时设定外键：

```sql
// MySQL
CREATE TABLE Orders (
	PersonID int,
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);

// SQL Server / Oracle / MS Access
CREATE TABLE Orders (
	PersonID int FOREIGN KEY REFERENCES Persons(PersonID)
);
```

其他语法类似于主键。



#### **CHECK**

Ensures that all values in a column satisfies a specific condition

用于设定某一列的数据的范围，例如：

```
// MySQL
CREATE TABLE Persons (
	Age int,
	CHECK (Age>=18)
);

// SQL Server / Oracle / MS Access
CREATE TABLE Persons (
	Age int CHECK (Age>=18)
);
```

修改、删除的语法类似于上述的UNIQUE、主键等。



#### **DEFAULT**

Sets a default value for a column when no value is specified

```sql
CREATE TABLE Persons (
	City varchar(255) DEFAULT 'NewYork'
)；
// 或者使用函数
CREATE TABLE Orders (
	OrderDate date DEFAULT GETDATE()
);
```



#### **INDEX**

Used to create and retrieve data from the database very quickly

用户看不到Index。

更新带索引的表比更新不带索引的表要更加耗时。所以应该只对那些经常搜索的列创建索引。

创建索引：（允许重复的值）

```sql
CREATE INDEX index_name ON table_name (column1, column2, ...);
```

创建独特的索引，不允许重复的值：

```sql
CREATE UNIQUE INDEX index_name ON table_name (column1, column2, ...);
```

删除索引：

```sql
DROP INDEX index_name ON table_name;
```

注意：对于不同数据库，语法有些微差别，使用时要搜索确认下。



### AUTO INCREMENT 域

> Auto-increment allows a unique number to be generated automatically when a new record is inserted into a table. 

**这常用于主键**。

语法，这里仅列举MySQL

```sql
CREATE TABLE Persons (
	ID int NOT NULL AUTO_INCREMENT,
    PRIMARY KEY (ID)
);
```

默认地，起始值是1，增大步长也是1. 可以自定义起始值。

对于其他类型的数据库，自增的关键词，起始值和步长的设定方法有所不同，要进一步查阅。



### Dates

和日期时间打交道，关键在于确保格式无误，也就是要插入的记录中的日期时间格式和数据库中规定的格式要一致。

MySQL中的格式有：

- DATE - format YYYY-MM-DD
- DATETIME - format: YYYY-MM-DD HH:MI:SS
- TIMESTAMP - format: YYYY-MM-DD HH:MI:SS
- YEAR - format YYYY or YY



### Views

> In SQL, a view is a **virtual table** based on the result-set of an SQL statement.
>
> A view contains rows and columns, just like a real table. The fields in a view are fields from one or more real tables in the database.

可以像操作实际的表一样操作view，包括使用SELECT, WHERE, JOIN, DROP 等。



### SQL 注入（Injection）

SQL注入是一种常见的网络攻击技术，通过在网页输入中填入SQL语句来欺骗代码中的SQL操作或者破坏数据库。

详见：[sql injection](https://www.w3schools.com/sql/sql_injection.asp)



## 进阶内容

### 索引

索引的作用和工作原理？

主键和索引的关系？主键是默认创建了索引的。



使用索引会提升读的性能（搜索），而降低写的性能（插入、更新、删除）。



ref：

[The Basics of Database Indexes For Relational Databases](https://medium.com/jimmy-farrell/the-basics-of-database-indexes-for-relational-databases-bfc634d6bb37)





self join有啥用？