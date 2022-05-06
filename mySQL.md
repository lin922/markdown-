#### 基本概念

---

###### 1、数据库好处：

- 持久化数据到本地

- 可以实现结构化查询，方便管理

###### 2、相关概念：

DB：数据库，保存一组有组织的数据的容器

DBMS：数据库管理系统，管理DB中数据

基于共享文件系统的DBMS（Access）

基于C/S的DBMS（MySQL、Oracle、SqlServer）

SQL：结构化查询语言，用于和DBMS通信的数据

DDL:数据定义语言 创建和管理表create alter drop rename truncate清空

DML:数据操作语言 insert delete update select增删改查

DCL:数据控制语言commit rollback savepoint grant revoke

###### 3、特点：

1、将数据放入表中，表再放入库中。

2、一个数据库有多个表，表名具有唯一性

3、表有一些特性，由列组成，也称为字段。

###### 4、常见命令：

1、查看当前所有数据库 show databases;

2、打开指定的库 use 库名

3、查看当前库的所有表：show tables;

4、查看其它库的所有表 show tables from 库名

5、创建表 create table 表名（

​		列名 列类型，

​		列名 列类型，

​		...）

6、查看表结构 desc 表名；

7、查看服务器版本 select version();

###### 5、语法规范：

1、不区分大小写，建议关键字大写，表名 列名小写

2、每条命令用；结尾

3、一条命令根据需要可以缩进或换行

```mysql
mysql -u root -p3306 -p
select version();
show databases;
create database dbtest1;
use dbtest1;
create table employees(id int,name varchar(15));
insert into employees values(1001, 'llin');
insert into employees values(1002, 'jjie');
select * from employees;
insert into employees values(1002, '三三');  error
drop database dbtest1; 删除 
net stop mysql;
net start mysql;
```

#### 基础查询

---

###### 1、导入数据表

方式一：命令行 source sql文件路径

方式二：图形化界面 右键运行sql文件并刷新

###### 2、select查询语句

-  select 查询列表 from 表名；

查询列表：字段，常量值，表达式，函数

单/多个字段 以逗号隔开 双击点名字

- 所有字段 select  * from 表名；

查询之前要打开相应的库 use 库名；

 用着重号`  name`区分关键字和字段

- 查询常量值 表达式

 select 100; select 'join'; select 100%98;

查询版本 select version();

当需要加入某一列时，可以用 将公司加入每一列

select '**公司'，employee_id from employees;

- 为字段起别名

 select 100%98 as 结果;

select  100%98 结果； as也可以省略

select salary as "out put" from 表；**别名加上双引号**

- 去除重复

select distinct 字段 from 表；

- null型参与运算，结果都为NULL

  采用 ifnull 可以将null赋值为0

- 着重号``

  表名或字段和关键字重复时使用着重号

- 将两个字段连起来用concat

select concat (last_name,first_name) as 姓名 from 表

###### 3、显示表结构

describe employees;

###### 4、过滤数据

select last_name,first_name from 表

where last_name='King';

#### 基本运算

---

###### 1、算数运算符

- +号 仅表示运算符 两个数值型相加

字符型和数值型相加，将字符型做转换，转换失败则字符型为0

- null型参与运算，结果都为NULL null专用<=>
- 不等于 <>

###### 2、逻辑运算符

表示不确定的用_    表示任意字符用%

第三个字母是a的员工姓名 WHERE last_name LIKE '__a%'

最后一个字母是e    where last_name like '%e'  或者 REGEXP e$

以e开头 REGEXP '^e'  包含e REGEXP '[e]'

名字含有a和k   where last_name like '%a%k%' or '%k%a%';

#### 排序和分页

---

###### 1、排序

使用 order by对查询到的数据进行排序

升序 ASC 降序 DESC

列的别名只能在order_by中使用，不能在where中使用

where需要声明在from之后，order by之前

二级排序 

###### 2、分页

mysql使用LIMIT实现数据的分页显示,有两个参数（偏移量，条目数）

每页显示20条记录   第一页：LIMIT 0，20   表示偏移量

第二页： LIMIT 20,20   第三页 ：LIMIT 40，20

 每页显示pagesize条记录

LIMIT (pageNo-1)*pagesize，pagesize；

顺序： where    order by      LIMIT 

8.0版本新特性：LIMIT 31，2 等价于 LIMIT 2 OFFSET 31

###### 3、拓展

不同的DBMS中使用的关键字可能不同 比如 SQL server Oracle

#### 多表查询

---

###### 1、笛卡尔积（交叉连接）错误

每个员工和每个部门都匹配了一遍

缺少了多表连接的条件

两个集合 X 和Y ，X和Y的笛卡尔积就是X和Y的所有可能组合，即为两个集合中元素个数的乘积。

```sql
#笛卡尔积错误
SELECT employee_id,department_name
FROM employees,departments;
#正确的多表查询方式
SELECT employee_id,department_name
FROM employees,departments
WHERE employees.department_id = departments.department_id;
```

如果查询语句中出现了多个表中都存在的字段，则必须指定所在的表。建议多表查询时，每个字段都指定对应的表

可以给表起别名增强可读性，避免太长；一旦起了别名，就必须使用

要实现n个表的多表查询，至少需要n-1个查询条件

###### 2、多表查询的分类

- 角度一：等值连接 vs 非等值连接

  等值连接：多表之间存在相同字段，可以等值查询

  ```mysql
  #查询员工id和员工所属部门名称
  SELECT employee_id,department_name
  FROM employees,departments
  WHERE employees.department_id=departments.department_id;
  ```

  非等值连接：表之间不能建立起等值连接

  ```mysql
  #查询员工的姓名及薪资 所在的等级
  SELECT last_name,salary,grade_level
  FROM employees e,job_grades j
  WHERE e.salary BETWEEN j.lowest_sal AND j.highest_sal;
  ```

- 角度二：自连接 vs 非自连接

  自连接：从同一个表中查询关联的字段

  ```mysql
  #查询员工id,员工姓名及其管理者的id和姓名
  SELECT e.employee_id,e.last_name,ee.employee_id,ee.last_name
  FROM employees e,employees ee
  WHERE e.manager_id=ee.employee_id;
  ```

- 角度三：内连接 vs 外连接

  以上几种都属于内连接

  **内连接**： 合并具有同一列的两个以上的表的行，结果集中不包含一个表与另一个表不匹配的行

  **外连接**：合并具有同一列的两个以上的表的行，结果集中除了包含一个表与另一个表匹配的行，还查询到左表或右表中不匹配的行。

  分类：左外连接 右外连接 满外连接（都有）

![image-20220319202508341](C:\Typora\mylearning\\img\image-20220319202508341.png)

左外连接：两个表在连接过程中除了返回满足连接条件的行之外，还返回左表中不满足条件的行

右外连接：两个表在连接过程中除了返回满足连接条件的行之外，还返回右表中不满足条件的行

###### 附录：常用的sql标准

SQL92和SQL99 Y也叫做SQL-2 SQL-3

#SQL92实现外连接：使用+   ----MYSQL不支持SQL92标准中的外连接，

以上均为SQL92内连接

#SQL99使用JOIN.. ON实现多表查询，这种方式也能解决外连接。

![image-20220319210043020](C:\Typora\mylearning\\img\image-20220319210043020.png)

内连接查询到：106条

左外连接应查询到结果：107条

右外连接应查询到结果：122条

满外连接应查询到：106+1+16条 123条

```mysql
##查询所有员工的last_name department_name
#SQL99语法实现内连接：106条
#SQL99使用JOIN.. ON
SELECT last_name,department_name
FROM employees e 
INNER JOIN departments d
ON e.department_id = d.department_id; 

#三个表查询
SELECT last_name,department_name,city
FROM employees e 
INNER JOIN departments d
ON e.department_id = d.department_id
INNER JOIN locations l
ON d.location_id = l.location_id;

#SQL99语法实现外连接：107条全部数据
#左外连接（左表数据多）
SELECT last_name,department_name
FROM employees e 
LEFT OUTER JOIN departments d
ON e.department_id = d.department_id; 

#右外连接（右表数据多，还有没查出来的）
SELECT last_name,department_name
FROM employees e 
RIGHT OUTER JOIN departments d
ON e.department_id = d.department_id; 

#满外连接（右表数据多，还有没查出来的）
#mysql不支持full outer join
SELECT last_name,department_name
FROM employees e 
FULL OUTER JOIN departments d
ON e.department_id = d.department_id; 
```

###### 3、UNION的使用

合并查询结果：利用UNION关键词可以合并多条select语句，并组合成单个结果集。

合并条件：两个表的列数和数据类型下必须相同，并且相互对应。

各个SELECT语句之间使用UNION或UNION ALL关键字分隔

```mysql
SELECT column, ... FROM tables
UNION
SELECT column, ... FROM tables
```

- UNION操作符

  ![image-20220321092908503](C:\Typora\mylearning\\img\image-20220321092908503.png)

  UNION操作符返回两个查询的结果集的并集，去除重复记录。

- UNION ALL操作符

![image-20220321092949488](C:\Typora\mylearning\\img\image-20220321092949488.png)

​		UNION ALL操作符返回两个查询结果的并集，对于两个结果集的重复部分，不去重。

​	尽量使用UNION ALL语句，以提高查询的效率

###### 4、7种JOIN的数据

![image-20220321094230137](C:\Typora\mylearning\\img\image-20220321094230137.png)

```mysql
#7种JOIN的实现
#1.中图：内连接
SELECT employee_id,department_name
FROM employees e 
JOIN departments d
ON e.department_id=d.department_id;
#2.左上：左外连接
SELECT employee_id,department_name
FROM employees e 
LEFT JOIN departments d
ON e.department_id=d.department_id;
#3.右上：右外连接
SELECT employee_id,department_name
FROM employees e 
RIGHT JOIN departments d
ON e.department_id=d.department_id;
#4.左中：
SELECT employee_id,department_name
FROM employees e 
LEFT JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
#不加最后一行是左外连接，结果只想要A独有的，那么加上过滤条件使b.为null,而共同部分为不是NULL的，那么就得到了A独有的。
#5.右中：
SELECT employee_id,department_name
FROM employees e 
RIGHT JOIN departments d
ON e.department_id = d.department_id
WHERE e.department_id IS NULL;
#6.左下图：满外连接
#方式一：左上图 UNION ALL 右下图
#方式二：左中图 UNION ALL 右下图
#mysql不支持SQL99的 full outer join
#7.右下图： 左中图 UNION ALL 右中图
```

###### 5、SQL99语法新特性

- 新特性1：自然连接

  自动查询两张连接表中所有相同的字段，然后进行等值连接。

- 新特性2：USING

  USING要指定具体的相同字段的名称

```mysql
SELECT employee_id,last_name,department_name
FROM employees e
JOIN departments d
ON e.department_id=d.department_id
AND e.manager_id=d.manager_id;
#自然连接 NATURAL JOIN
SELECT employee_id,last_name,department_name
FROM employees e NATURAL JOIN departments d;

SELECT employee_id,last_name,department_name
FROM employees e JOIN departments d
ON e.department_id=d.department_id;
#新特性2：USING
SELECT employee_id,last_name,department_name
FROM employees e JOIN departments d
USING (department_id);
```

多表查询要控制表的数量。超过3个表不建议用join

#### 单行函数

---

MYSQL提供的内置函数分为两类：单行函数和多行函数（聚合函数或分组韩素）

单行函数：

- 操作数据对象
- 接受参数返回一个结果
- 只对一行进行变换
- 每行返回一个结果
- 可以嵌套
- 参数可以是一列或一个值

###### 1、数值函数

- 基本函数

```mysql
ABS(x)    绝对值
SIGN(x)   返回(1,-1,0) 
PI()      返回圆周率
CEIL(x) CEILING(x) 返回大于或等于某个值的最小整数
FLOOR(x)  返回小于或等于某个值的最大整数
LAEST(e1,e2,e3...)   返回列表中的最小值
GREAEST(e1,e2,e3...) 返回列表中的最大值
MOD(x,y)  取余
RAND()    返回0-1的随机值
RAND(x)   返回0-1随机值，x为种子值，相同的x产生相同的随机数
ROUND(x)  返回一个对x的值进行四舍五入后最接近x的值
ROUND(x,y)并保留小数点后Y位
TRUNCATE(x,y) 返回数字x截断为y位小数的结果
SQRT(x)   返回平方根，x为负数时返回NULL
```

- 三角函数

参数为弧度值

```
RADIANS(x) 将角度转化为弧度值
DEGREES(x) 将弧度转化为角度值
```

- 指数与对数

```
POW((x,y) POWER(x,y)  返回x的y次方
EXP(x)        返回e的x次方，其中e是一个常数, 2.718281828459045
LN(x) LOG(X)  返回以e为底的X的对数，当x<=0时,返回的结果为NULL
LOG10(X)      返回以10为底的X的对数，当X<=0时， 返回的结果为NULL
LOG2(X)       返回以2为底的x的对数，当X <=0时,返回NULL
```

- 进制间的转换

```
BIN(x) 返回x的二进制编码
HEX(x) 返回x的十六进制编码
0CT(x) 返回x的八进制编码
CONV(x,f1,f2)返回f1进制数变成f2进制数
```

###### 2、字符串函数

```
ASCII(S)       返回字符串中第一个字符的ASCII码
CHAR_LENGTH(S) 返回字符串的字符数
LENGTH(s)      返回字符串s的字节数
CONTACT(s1,s2,sn) 连接字符串
CONTACT_WS(x,s1,s2,sn) 字符串之间用x连接
INSERT(str,idx,len,replacestr)
字符串的索引从1开始，将字符串str的第idx位置开始的len个字符换成replacestr
REPLACE(str,a,b) 将str中的a换成b
UPPER(s) 全部变成大写
LOWER(s) 全部变成小写
LEFT(str,n) 取左边n个字符
RIGHT(str,n)取右边n个字符
LPAD(str,len,pad)在字符串左边用pad补成len长度，实现左对齐
RPAD(str,len,pad)右对齐
```

###### 3、日期和时间函数

- 获取日期时间

```
CURDATE()，CURRENT_DATE() 返回当前日期，只包含年、月、日
CURTIME()，CURRENT_TIME() 返回当前时间，只包含时、分、秒
NOW()/SYSDATE()/CURRENT_TIMESTAMP()/LOCALTIME() 返回当前系统日期和时间
LOCALTIMESTAMP() UTC_DATE()
返回UTC (世界标准时间)日期
UTC_TIME()
返回UTC (世界标准时间)时间
```

- 日期与时间戳的转换

```
UNIX_TIMESTAMP() 以UNIX时间戳的形式返回当前时间
SELECT UNIX_TIMESTAMP() ->1634348884
UNIX_TIMESTAMP(date)将时间date以UNIX时间戳的形式返回
FROM_UNITIME(timestamp)将UNIX时间戳的时间转换为普通格式的时间

```

- 获取月份、星期、星期数、天数

```
YEAR(date)/ MONTH(date)/ DAY(date)
返回具体的日期值
HOUR(time) / MINUTE(time) / SECOND(time)
返回具体的时间值
MONTHNAME(date)
返回月份: January, ..
DAYNAME(date)
返回星期几: MONDAY, TUEODA....SUNDAY
WEEKDAY(date)
返回周几，注意，周1是0，周2是1,...周日是6
QUARTER(date)
返回日期对应的季度，范围为1 ~4
WEEK(date)，WEEKOFYEAR(date)
返回一年中的第几周
DAYOFYEAR(date)
返回日期是一年中的第几天
DAYOFMONTH(date)
返回日期位于所在月份的第几天
DAYOFWEEK(date)
返回周几，注意:周日是1，周一是2，...周六是7
```

- 日期的操作函数

```
SELECT EXTRACT(SECOND FROM NOW());
```

- 时间和秒钟转换的函数

```
TIME_TO_SEC()
SEC_TO_TIME()
```

- 计算日期和时间的函数

```
SELECT DATE_ADD(NOW(),INTERVAL,1 YEAR),
DATE_ADD(NOW(),INTERVAL,-1 YEAR),
DATE_SUB(NOW(),INTERVAL, 1 YEAR)
FROM DUAL;
在原基础上加一年，减一年
```

- 日期的格式或与解析

###### 4、流程控制函数

```
IF(value,value1,value2) 如果value的值为TRUE,返回value1, 否则返回value2
IFNULL(value1, value2)  如果value1不为NULL,返回value1, 否则返
回value2
CASE WHEN条件1 THEN结果1 WHEN条件2 THEN结果2... [ELSE resultn] END
相当于Java的if..else
CASE expr WHEN常量值1 THEN值1 WHEN常量值1 THEN值1....[ELSE值n] END
相当于Java的switch..case...
```

###### 5、加密与解密函数

###### 6、MySQL信息函数

#### 子查询

---

###### 1、子查询基本使用

一个查询语句嵌套在另一个查询语句内部的查询 外查询（主查询） 内查询（子查询）

子查询要包含在括号内

将子查询放在比较条件的右侧

单行操作符对应单行子查询，多行操作符对应多行子查询

分类：

角度1：单行子查询 vs 多行子查询      从内查询返回的条目数

角度2：相关子查询 vs 不相关子查询   内查询是否被执行多次

子查询需要执行多次，采用循环的方式，先从外部查询开始，每次都传入子查询进行查询，然后再将结果反馈给外部，这种嵌套的执行方式就称为相关子查询。

###### 2、单行子查询

内查询只返回一条记录

```mysql
#查询工资大于149号员工工资的员工信息
SELECT employee_id,last_name,salary
FROM employees
WHERE salary > (
				SELECT salary FROM employees
				WHERE employee_id=149
				);
#返回公司工资最少的员工信息
SELECT last_name,job_id,salary
FROM employees
WHERE salary = (
				SELECT MIN(salary)
				FROM employees
				);
#查询最低工资大于50号部门最低工资的部门id和最低工资
SELECT department_id,MIN(salary)
FROM employees
WHERE department_id IS NOT NULL
GROUP BY department_id  #查询的结果有函数和非函数，用GROUP BY
HAVING MIN(salary) > (  #这里条件是个函数，用HAVING
					  SELECT MIN(salary)
					  FROM employees
					  WHERE department_id=50
					  );
#CASE中的子查询
#题目：显示员工的employee_id,last_name和location
#其中，若员工的department_id与location_id为1800的department_id相同
#则location为'Canada',其余为'USA'
```

子查询中的空值问题

###### 3、多行子查询

- 多行比较操作符

```mysql
IN   等于列表中的“任意-个”
ANY  需要和单行比较操作符一起使用，和子查询返回的某一个值比较
ALL  需要和单行比较操作符一起使用，和子查询返回的所有值比较
SOME 实际上是ANY的别名，作用相同，一般常使用ANY
```

###### 4、相关子查询

![image-20220322150916259](C:\Typora\mylearning\\img\image-20220322150916259.png)

```mysql
#查询员工中工资大于本部门平均工资的员工的last_name,salary,department_id
SELECT last_name,salary,department_id
FROM employees e1
WHERE salary > (
				SELECT AVG(salary)
				FROM employees e2
				WHERE department_id = e1.department_id
				);
```

#### 创建和管理表

---

###### 1、基础知识

- 一条数据存储的过程

![image-20220322153212094](C:\Typora\mylearning\\img\image-20220322153212094.png)

​       MySQL数据库系统从大到小依次是：数据库系统 数据库 数据表 数据表的行与列

- 标识符命名规则

​       命名不要包含空格

​       字段和关键字重复，使用着重号``

- MySQL中的数据类型

###### 2、创建和管理数据库

- 创建数据库

```mysql
CREATE DATABASE 数据库名；
#创建并指定字符集
CREATE DATABASE 数据库名 CHARACTER SET 'utf8'；
#判断数据库是否已经存在，不存在则创建数据库
CREATE DATABASE IF NOT EXISTS 数据库名；
```

- 使用数据库

```mysql
#查看当前所有的数据库
SHOW DATABASES;
#查看当前正在使用的数据库
SELECT DATABASE();
#查看指定库下所有的表
SHOW TABLES FROM 数据库名；
#查看数据库的创建信息
SHOW CREATE DATABASE 数据库名；
#使用、切换数据库
USE 数据库名；
```

- 修改数据库

```mysql
#更改数据库字符集
ALTER DATABASE 数据库名 CHARACTER SET 'utf8';
```

- 删除数据库

```mysql
DROP DATABASE 数据库名；
#删除指定的数据库
DROP DATABASE IF EXISTS 数据库名；
```

###### 3、创建表

- 方式一

必须具备：CREATE TABLE权限 存储空间

必须指定：表名 列名 数据类型 长度

可选指定：约束条件 默认值

使用VARCHAR来定义字符串，必须指定长度

```mysql
CREATE TABLE[IF NOT EXISTS] 表名(
	字段1，数据类型[约束条件] [默认值],
    字段2，数据类型[约束条件] [默认值]，
    字段3，数据类型[约束条件] [默认值]，
    ...
)
-- 创建表
CREATE TABLE emp (
        -- int类型
        emp_id INT,
        -- 最多保存20个中英文字符
        emp_name VARCHAR(20),
        -- 总位数不超过15位
        salary DOUBLE,
        -- 日期类型
        birthday DATE
);
```

- 方式二

```mysql
#基于现有的表
CREATE TABLE myemp2
AS
SELECT employee_id,last_name,salary
FROM employees;
#用JOIN ON 可以通过现有的多表创建新表
```

- 查看数据表结构

```mysql
DESCRIBE dept80;
DESC dept80;
SHOW CREATE TABLE 表名\G
```

###### 4、修改表

```mysql
#添加一个字段
ALTER TABLE 表名 ADD [COLUMN] 字段名 字段类型 [FIRST|AFTER 字段名];
#举例
ALTER TABLE dept80
ADD job_id varchar(15)
#修改一个字段
ALTER TABLE 表名 MODIFY [COLUMN] 字段名1 字段类型 [DEFAULT 默认值][FIRST|AFTER 字段名
2];
ALTER TABLE dept80
MODIFY last_name VARCHAR(30);
ALTER TABLE dept80
MODIFY salary double(9,2) default 1000;
#重命名一个字段
ALTER TABLE 表名 CHANGE [column] 列名 新列名 新数据类型;
#删除一个字段
ALTER TABLE 表名 DROP [COLUMN]字段名
```

###### 5、重命名表

```mysql
#方式一：
RENAME TABLE emp
TO myemp;
#方式二：
ALTER table dept
RENAME [TO] detail_dept; -- [TO]可以省略
```

###### 6、删除表

```mysql
DROP TABLE [IF EXISTS]  [数据表1, 数据表2, …, 数据表n];
DROP TABLE dept80;
```

###### 7、清空表

```mysql
TRUNCATE TABLE detail_dept;
```

#### 数据管理之增删改

---

###### 1、插入数据

```mysql
#方式一：values的方式添加
INSERT INTO 表名
VALUES (value1,value2,....);
INSERT INTO departments
VALUES (70, 'Pub', 100, 1700);
#插入多条数据
INSERT INTO table_name
VALUES
(value1 [,value2, …, valuen]),
(value1 [,value2, …, valuen]),
……
(value1 [,value2, …, valuen]);
#方式二：使用SELECT语句查询的结果插入表中
INSERT INTO 目标表名
(tar_column1 [, tar_column2, …, tar_columnn])
SELECT (src_column1 [, src_column2, …, src_columnn])
FROM 源表名
[WHERE condition]
#举例
INSERT INTO emp2
SELECT *
FROM employees
WHERE department_id = 90;
```

###### 2、更新数据

```mysql
UPDATE table_name
SET column1=value1, column2=value2, … , column=valuen
[WHERE condition]
#举例
UPDATE employees
SET department_id = 70
WHERE employee_id = 113;
```

###### 3、删除数据

```mysql
DELETE FROM table_name [WHERE <condition>];
#举例
DELETE FROM departments
WHERE department_name = 'Finance';
```

###### 4、MySQL8新特性：计算列

