[[mysql安装]]

---

### 数据库三层结构

[[数据库三层结构.canvas]] = >普通表本质上仍然是文件

---

### 数据库简介

sql：用于操作数据库。

关系型数据库：由行(row)和列(column)组成。其中行是可变的，列（属性）是不变的。

一行称为一条记录。

---

### SQL语句分类

[[SQL语句分类.canvas]]

---

数据库服务启动与登录
```dos
net mysql start
mysql -u root -p
```

### 数据库

==数据库语句结尾需要添加分号;== 而DOS命令行下和结尾不能有分号。
1. 创建数据库
```sql
CREATE DATABASE [IF NOT EXISTS] name CHARACTER SET utf8 COLLATE utf8_bin（区分大小写）;
```
在创建数据库或者表的时候可以使用反引号==规避关键字==，删除的时候也需要加上反引号。
```
CREATE DATABASE `INT`;
```

不写后面的，默认：
- 字符集默认为utf8
- 数据库校对规则默认为utf_general_ci（不区分大小写）

**注意**：若在创建表的时候没有指定字符集和校对规则，则表默认跟随数据库的规则。

2. 删除数据库（慎重）
```sql
DROP DATABASE name;
```

3. 显示数据库
```sql
SHOW DATABASES;
#注意一定要加S
```

5. 显示某数据库创建的语句信息
```sql
SHOW CREATE DATEBASE name;
```

6. 数据库备份(==DOS执行==)命令行
```dos
mysqldump -u 用户名 -p -B 数据库1 数据库2 数据库n > 文件名.sql（路径）
```

7. 数据库恢复（需要在==mysql命令行执行==）
```sql
source 文件名.sql（路径）
```
或者：直接将文件.sql的内容放入查询编辑器（SQLyog中的）中执行。

8. 数据库的表恢复
```dos
mysqldump -u 用户名 -p 数据库 表1 表2 表n > 文件名.sql（路径）
```

---
### 表

1. 创建表：
```sql
CREATE TABLE name
(
	field1 datatype,
	field2 datatype,
	field3 datatype
)character set 字符集 collate 校验规则 engine 储存引擎
```
注：field：指定列名。datatype指定列数据类型。字符集，校验，引擎->（不指定默认跟随数据库）

定义无符号整数：
```sql
CREATE TABLE t1(id tinyint);//默认是有符号的: 0 - 255
CREATE TABLE t1(id tinyint unsigned);//默认是无符号的 :-128 - 127
```

2. 显示表：

```sql
SHOW TABLES;
```

3. 删除表
```sql
DROP TABLE name;
```

4. 修改表
- 添加列
```sql
ALTER TABLE name ADD(column datatype NOT NULL DEFULT '',othercolum datatype);

//添加一个列的时候,可以指定添加的位置，到colname后
ALTER TABLE name ADD column datatype NOT NULL DEFAULT '' AFTER colname;
```
- 修改列
```sql
ALTER TABLE tablename MODIFY column datatype;//注意只能一个一个修改
```
- 删除列
```sql
ALTER TABLE tablename DROP colname字段;//只能一个一个删
```
- 修改列名
```sql
ALTER TABLE tablename CHANGE colname othercolname 数据类型 NOT NULL DEFAULT '';
//注意colname到othercolname中间没有to
```
- 修改表的字符集
```sql
ALTER TABLE name charset 字符集;
```
- 修改表名
```sql
RENAME TABLE name to newname;
```
- 查看表的结构
```sql
desc name;
```

- ==复制表的结构==
```sql
CREATE TABLE my_table2 LIKE emp; //将emp表的结构复制给my_table表
```

5. 表的复制和去重
- 表的复制
```sql
//将emp表的记录复制到my_table表
CREATE TABLE my_table(
	id INT,
	`name` VARCHAR(32),
	sal DOUBLE,
	deptno INT
	);
	
SELECT *FROM my_table;

INSERT INTO my_table (id,`name`,sal,deptno) 
	SELECT empno,ename,sal,deptno FROM emp;
```


```sql
//表的自我复制（记录成倍增长）
INSERT INTO my_table
SELECT *FROM my_table
```

- 表的去重
```sql
//1.创建一个临时表,并复制emp表结构
CREATE TABLE temp LIKE emp;
//2.将emp表的记录去重复制到临时表
INSERT into temp SELECT DISTINCT * FROM emp;
//3.删除emp表的记录
DELETE FROM emp;
//4.将临时表的记录复制到emp
INSERT into emp SELECT * FROM temp;
DROP TABLE temp;
```

---

### Mysql数据类型

[[Mysql数据类型（列类型）.canvas]]

1. 小数
```
FLOAT/DOUBLE [UNSIGNED]
DECIMAL[M,D][UNSIGNED]  //M表示小数位数总数默认为10，最大为64。D表示小数点后面的位数默认为0，最大为30。
```
2. 字符串
```sql
char(size) - >固定长度字符串， 0-255个字符。
varchar(size) ->可变长度字符串，0-xxx个字符。总 0-65535个字节， 1-3个字节用于记录大小。和表的编码有关，utf8编码最大 65535-3/3 = 21844 个字符。gbk编码最大 65535-3/2 = 32766 个字符。
```

注：utf8用3个字节表示一个字符。gbk用两个字节表示一个字符。不管是中文还是英文都是按字符来存，一个字符就是存放一个文字或字母。

==注明==：char和varchar的size都是表示存储字符数大小，但是varchar会根据编码改变存储字符数大小。按照实际占用空间分配。
如varchar(3) -> 'aa' -> 实际长度 2+（1~3） = xxx ; ->（1~3）是用于记录大小的。  
然而char不一样，如char(3) -> 'aa' -> 实际长度 3;

定长使用char，变长使用varchar。在查询的速度方面，char比varchar更快。

3. 日期类型的基本使用
```sql
CREATE TABLE birthday1(t1 DATE , t2 DATETIME , t3 TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP);

INSERT into birthday(t1,t2) value('2022-3-1','2022-3-1 10:10:10')
```

日期应该用单引号括起来，TIMESTAMP默认为当前时间，如果更新表的内容，时间也会更新。

---

### 数据库CRUD语句

#### insert语句（行）
```sql
insert into tablename(形式参数1,形式...) values(实际参数1,实际...),(实际参数1,实际...);//可以添加一条或多条记录。

//给表中所有字段添加数据，可以省略形式参数(字段名称)
insert into tablename values(实际参数1,实际...);

//给字段添加数据时，未指定数据默认为NULL。若强调不能为NULL，又未指定数据时，报错。
```

#### update语句（列）
```sql
update tablename SET 字段(列) = xxx,字段2 = xxx WHERE 条件;//当为带条件where时，默认修改所有此字段的值;
```

#### delete语句（行）
```sql
DELETE FROM tablename WHERE 条件；//不带条件时默认删除表中所有记录
```

==注==：delete不能删除一列的数据，最多将一列数据置为NULL；
使用delete语句只是删除记录，表还存在。删除表需要使用 DROP TABLE tablename;

#### Select
---

##### 合并查询
将两个查询结果合并
- union（会去重）
```sql
SELECT ename ,sal FROM table1;
union
SELECT ename ,sal FROM table2;
```
- union all（不会去重）
```sql
SELECT ename ,sal FROM table;
union all
SELECT ename ,sal FROM table1;
```

##### 子查询
- 单行子查询
```sql
SELECT* FROM emp
    WHERE deptno = (SELECT deptno FROM emp WHERE ename = 'SMITH');
```

- 多行子查询（用IN）
```sql
 SELECT ename,job,sal,deptno FROM emp
     WHERE job IN (
     SELECT DISTINCT job FROM emp
     WHERE deptno = 10) AND deptno !=10;
```
注意：需要注意特点场景筛选唯一结果（DISTINCT）

- 子查询临时表（将子查询的的表作为临时表）
```sql
SELECT goods_id,ecs_goods.cat_id,goods_name,shop_price 
	FROM (
		SELECT cat_id,MAX(shop_price)AS max_price 
		FROM ecs_goods
		GROUP BY cat_id
	) temp,ecs_goods
	WHERE temp.cat_id = ecs_goods.cat_id
	AND shop_price = temp.max_price;
```

- ALL和ANY
```sql
SELECT ename,sal,deptno FROM emp
	WHERE sal > ALL(  //sal大于子查询中所有的sal
		SELECT sal AS max_sal FROM emp
		WHERE deptno = 30
	);
```

```sql
SELECT ename,sal,deptno FROM emp
	WHERE sal > ANY( //sal大于子查询中其中一个的sal
		SELECT sal AS max_sal FROM emp
		WHERE deptno = 30
	);
```

- 多列子查询（查询返回多个列的子查询语句）
```sql
SELECT * FROM emp
WHERE(deptno,job) = (
	SELECT deptno,job FROM emp
	WHERE ename = 'SMITH'
	);
```

==注意==：
```sql
表.*  =>  表示显示表的所有字段。
```


--- 

##### 单表
语句：
1. SELECT查询：用于查询数据和转化计算。
```sql
查询属性列
SELEST 列名1,...FROM 表名;

查询所有列
SELEST * FROM 表名;
```

注意: 查询的列名（属性）不区分大小写。具体的每一项区分大小写。关键字也兼容大小写，但是为了区分，最好写成大写。==结尾分号，只在语句最后加上分号==。

- select查询可以使用表达式加别名的形式
```sql
SELECT (chinese+english+math+10) AS total_score FROM student;//as可以省略
```

2. 条件查询：筛选行
```sql
SELECT colum,another_colum,...FROM mytable
WHERE condition
	AND/OR _another_condition_ AND/OR …;
```

- ==其中condition是对筛选列的描述可以筛选整数和浮点数==

| Operator（关键字）  | Condition（意思）                                            | SQL Example(例子）            |
| ------------------- | ------------------------------------------------------------ | ----------------------------- |
| =, !=, < <=, >, >=  | Standard numerical operators 基础的 大于，等于等比较         | col_name != 4                 |
| BETWEEN … AND …     | Number is within range of two values (inclusive) 在两个数之间 | col_name BETWEEN 1.5 AND 10.5 |
| NOT BETWEEN … AND … | Number is not within range of two values (inclusive) 不在两个数之间 | col_name NOT BETWEEN 1 AND 10 |
| IN (…)              | Number exists in a list 在一个列表                           | col_name IN (2, 4, 6)         |
| NOT IN (…)          | Number does not exist in a list 不在一个列表                 | col_name NOT IN (1, 3, 5)     |

- ==筛选字符串==

| Operator（操作符） | Condition（解释）                                            | Example（例子）                                              |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| =                  | Case sensitive exact string comparison (*notice the single equals*)完全等于 | col_name = "abc"                                             |
| != or <>           | Case sensitive exact string inequality comparison 不等于     | col_name != "abcd"                                           |
| LIKE               | Case insensitive exact string comparison 没有用通配符等价于 = | col_name LIKE "ABC"                                          |
| NOT LIKE           | Case insensitive exact string inequality comparison 没有用通配符等价于 != | col_name NOT LIKE "ABCD"                                     |
| %                  | Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE) 通配符，代表匹配0个以上的字符 | col_name LIKE "%AT%" (matches "AT", "ATTIC", "CAT" or even "BATS") "%AT%" 代表AT 前后可以有任意字符 |
| _                  | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE) 和% 相似，代表1个字符 | col_name LIKE "AN_" (matches "AND", but not "AN")            |
| IN (…)             | String exists in a list 在列表                               | col_name IN ("A", "B", "C")                                  |
| NOT IN (…)         | String does not exist in a list 不在列表                     | col_name NOT IN ("D", "E", "F")                              

3. DISTINCT筛选唯一结果（会==删除掉重复==的），每个字段相同才会去重
```sql
选取出唯一的结果的语法

SELECT DISTINCT column, another_column, … FROM mytable WHERE condition(s);`
```

4. ORDER BY结果排序
```sql
结果排序（ordered results）

SELECT column, another_column, … FROM mytable 
WHERE condition(s)
ORDER BY column ASC/DESC;//放在SELECT语句结尾，默认就是升序。
```
ASC是升序，DESC是降序

5. LIMIT选取部分结果
 ```sql
 limited查询

SELECT column, another_column, … FROM mytable 
WHERE condition(s) ORDER BY column ASC/DESC 
LIMIT num_limit OFFSET num_offset;  或者使用 LIMIT 0,3; //从1行截取到第三行
```
LIMIT限制截取长度，OFFSET设置从哪一行开始截取，不包含开始行。

---

##### 查询加强

1. where字句
```sql
SELECT *FROM emp
	WHERE hiredate > '1992-01-01'; //日期类型可以直接比较，注意格式
```
2. 判断为空
```sql
SELECT *FROM emp
	WHERE mgr IS NULL;//条件选择mgr是NULL的
```

3. 查询总结（多个语句结合的顺序）
```sql
SELECT col FROM TABLE
	GROUP BY col 
	HAVING condition
	ORDER BY col
	LIMIT start , end;
```

---
##### 多表 


1. 多表联合
```sql
SELECT *FROM table1,table2;
```

注意：当==查询的条件小于表个数-1==，会出现==笛卡尔集==（笛卡尔集行数：表1行数*表2行数..*表n行数）。指定某个表列的需要使用=>表.列
此查询方式是基于多个表的关联条件来联合查询的，==不存在关联的，不会在查询结果中显示==

- 自连接（把同一张表当作两张表使用，需要给表取别名）
```sql
SELECT worker.ename AS '职员',boss.ename AS '上司' FROM emp worker,emp boss
 WHERE worker.mgr = boss.empno;
```


2. 用JOINs多表联合查询

数据库范式：表中的重复的数据最低，表与表之间耦合性低，独立维护和增长。

主键：一个表中有一个属性列作为主键，用于标识一条数据

- 内连接
```SQL
用INNER JOIN 连接表的语法

SELECT column, another_table_column, … FROM mytable （主表） 
INNER JOIN another_table （要连接的表） ON mytable.id = another_table.id 
WHERE _condition(s)_ 
ORDER BY column, … ASC/DESC LIMIT num_limit 
OFFSET num_offset;
```
将两个表以id对应合并，相当于交集，==其他没对应id的数据丢失==。

- 外连接
```sql
用LEFT/RIGHT/FULL JOINs 做多表查询 
等价于：LEFT OUTER JOIN,RIGHT OUTER JOIN,FULL OUTER JOIN

SELECT column, another_column, … FROM mytable LEFT/RIGHT/FULL JOIN another_table ON mytable.id = another_table.matching_id 
WHERE _condition(s)_ 
ORDER BY column, … ASC/DESC LIMIT num_limit OFFSET num_offset;
```

  ==保留规则==：LEFT 是保留左边没匹配的
  RIGHT 是保留右边没匹配的
  FULL 是保留所有没匹配的

---

### 函数

#### 统计函数
1. 合计/统计函数
```sql
SELECT COUNT(*) FROM tablename;//统计所有行共有多少
SELECT COUNT(列) FROM tablename;//统计满足条件的行，共有多少。排除NULL;
```
2. 统计数值合（只能用于数值统计）
```sql
//单列
SELECT SUM(列) FROM tablename;//数值和
//多列
SELECT SUM(列),SUM(列)... FROM tablename;//注意要加逗号。
```
3. ==平均==分（数值）
```sql
SELECT AVG(列) FROM tablename;
```
5. 最大值和最小值（数值）
```sql
SELECT MIN(列),MAX(列) FROM tablename;
```

6. ==分组==统计（对查询结果进行分组统计，列属性相同的时分到一起，将会留下不同属性的列。）
```sql
SELECT 列 FROM tablename group BY 列 HAVING 条件;//按照某列进行分组，相同的分在一起，对其进行过滤
```

#### 字符串函数

测试使用的：DUAL亚元表，系统表
|:------------|:---------------|
|:---------------|:------------|
|CHARSET(str)|返回字符串集|
|==concat==(str1,str2)|连接字符串，==拼接==|
|instr(str,substring)|返回字符串在Str中出现的位置，第一个下标为1，没有则返回0|
|UCASE(str)|转换大写|
|LACSE(str)|转换小写|
|LEFT(str,lengtjh)|返回字符串左边起的第length个字符|
|LENGTH(str)|返回字符串长度，按照字节|
|REPLACE(str,searchstr,replacestr)|替换子串|
|STRCMP(str1,str2)|==逐==字符比较两子串大小，后比前大返回1，否则返回-1|
|SUBSTRING(str,position,length)|取字符串的第position开始的以length为长度的字符串,第一个下标为1。如果不写length默认取完|
|LTRIM(str)和RTRIM(str)和TRIM(str)|去左前空格，右前空格，前后空格|

#### 数学函数
|       |            |
|:--------|:----------|
|ABS(num)|绝对值|
|BIN(num)|十进制转二进制|
|ceiling(num)|向上取整|
|floor(num)|向下取整|
|CONV(num,form,toform)|进制转换|
|FORMAT(num,decimal)|保留decimal位小数位数，四舍五入|
|LEAST(num1,num2,num3)|求最小值|
|MOD(10,3)|求余-->10%3|
|RAND()|返回随机数 0<= v <= 1.0，当传入常数参数时，产生随机数且不会再该变|


- COVN使用方式：
```
SELECT CONV(8,10,2) FROM DUAL;//将十进制的8转换为二进制
```

#### 日期函数

|       |            |
|:--------|:----------|
|CURRENT_TIME|当前时间|
|NOW()|当前时间|
|CURRENT_DATE|当前日期|
|DATE(date)|选出当前年月日时分秒的年月日（保留日期）|
|CURRENT_TIMESTAMP()|当前时间戳（年月日时分秒）|
|DATE_ADD(date,INTERVAL value type)|其中type可以是year day hour minute second|
|LEAST(num1,num2,num3)|求最小值|
|DATEDIFF(date1,date2)|日期的差（结果是天）|
|TIMEDIFF(date1,date2)|时间的差（结果是时间）|
|YEAR或MONTH或DAY或(datetime)|获取年或月或日|
|UNIX_TIMESTAMP()|返回1970-1-1到现在的秒数|
|FROM_UNIXTIME()|把UNIX_TIMESTAMP秒数转换为指定格式|
|LAST_DAY(date)|当前日期的月的最后一天|

==注意==:日期一定要加引号。

- DATE_ADD使用:
```sql
//十分钟内发布的信息
SELECT * FROM tablename WHERE DATE_ADD(date_time, INTERVAL 10 MINUTE) >=now();
```
- FROM_UNINTIME使用：
```sql
SELECT FROM_UNixTIME(秒数,'%y-%m-%d %h:%i:%s') FROM DUAL;
```

#### 加密函数
| | |
|:-----------|:--------------|
|USER()|查询用户及登录ip|
|DATABASE()|数据库名称|
|MD5(str)|对字符串进行加密 得到32位字符串|
|PASSWORD(str)|对字符串进行加密得到解密后的密码字符串|

#### 流程控制函数

|              |                  |
|:---------|:---------|
|IF(expr1,expr2,expr3)|如果expr1为真返回expr2，否则返回expr3|
|IFNULL(expr1,pxper2)|如果expr1为NULL返回exper1，否则返回expr2|
|CASE WHEN expr1 THEN expr2 WHEN ... ESLE expr3 END|如果expr1为真返回expr2，否则如果... 多分支|

- IF(expr1,expr2,expr3)的使用：
```sql
SELECT IF(name IS NULL, 0, name) AS name FROM tablename;
```
- 多分支
```DOS
SELECT ename,(CASE WHEN job = "CLERK" THEN "职员"
    -> WHEN job = "MANAGER" THEN "经理"
    -> ELSE job END) AS 'job'
    -> FROM emp;
```

注意:==判断是否为nul使用is null==

---

### MySQL约束

1. primary key (主键)
用于标示唯一行的数据
```sql
CREATE TABLE tablename(id PRIMARY KEY,name);
或者CREATE TABLE tablename(id ,name,PRIMARY KEY(id));
```

复合使用：
```sql
CREATE TABLE tablename(id,name,PRIMARY KEY(id,name));
```

==注意事项==：
- 主键不能重复，且不能为空
- 一张表就一个主键，但是可以是符合主键
- 指定主键方式有两种
- 使用（desc 表名）可以查看谁是主键

2. NOT NULL非空
```
CREATE TABLE tablename(id NOT NULL,name);
```

3. unique唯一（表示某列值不可以重复）
```sql
CREATE TABLE tablename(id UNIQUE,name);
```
==注意==：
- 只有unique约束，当没有非空约束时，NULL可以重复。
- ==一张表可以有多个unique==


4. 外键约束

**只有表的引擎是innoDB时才支持。**

![[Pasted image 20230305135901.png]]

- FOREIGN KEY（外键）
注意：
        - ==主表与从表关联的列必须是unique或者主键==，从表指定某列为外键并关联主表的某列
        - 两个关联的列类型必须一致，长度可以不同。
        - 外键字段按可以为空。
```sql
CREATE TABLE stu(id INT unique NOT NULL,class varchar(32));//主表
CREATE TABLE class(class_id INT ,class varchar(32), FOREIGN KEY(class_id) REFERENCES stu(id));//从表
```
- 添加
 主表可以添加记录，但是从表只能添加外键与主表有对应的记录。
- 删除
（从表）外键表和主表形成外键约束的时候。从表记录未删时，主表对应的记录删不掉。

5. check
在mysql5.7中还未支持，只能做个语法检验
```sql
CREATE TABLE sut(
		id INT,sex varchar(32) CHECK (sex IN('man','woman'))//只能是man或woman
		);
```


6. 自增长

```sql
字段名 整型 primary key auto_increment

CREATE TABLE table1(id INT primary key auto_increment,name varchar(32) NUT NULL,home varchar(32)NOT NULL);
//使用方式一
INSERT INTO tablen1 VALUES(NULL,'jack','home');//自增长id=1
//使用方式二
INSERT INTO table1 (name,home)VALUES('jack','home');//自增长id=1

```


注意：
- 自增长需要配合primary key使用，也可以配合unique使用。
- 自增长修饰的字段可以为整型或者小数。
- 前面没有，默认从1开始自增。前面有跟随前面的。也可以指定。ALTER TABLE tablename auto_increment = xxx;

---

### 索引

```sql
CREATE INDEX indexname ON tablename (列);//在某表的某字段上创建索引--->普通索引
```

- 优点：普通查询会全表扫描，而索引会形成数据结构，如二叉树，查询速度很快。
- 缺点：索引的代价是会占用磁盘，形成的数据结构树会降低update   delete  insert 的效率。

1. 主键索引
主键本身就是索引。
```sql
CREATE TABLE tablename(id INT,primary ker(id));
```

2. 唯一索引
```sql
CREATE TABLE tablename(id INT unique);
```

3. 普通索引
```sql
CREATE INDEX indexname ON tablename (列);
```

4. 全文索引
开发中不常用，推荐使用框架 全文搜索 Solr和ElasticSearch

**索引使用**

- 查询是否有索引
```sql
SELECT INDEX FROM tablename;
```
- 添加索引
```sql
添加唯一索引
CREATE UNIQUE INDEX id_index ON tablename(id);

添加普通索引
CREATE INDEX id_index ON tablename(id);//方式一
ALTER TABLE tablename ADD INDEX id_index (id);//方式二

添加主键索引
ALTER TABLE tablename ADD primary key(id); 
```

区别：唯一和主键索引的值是不会重复的，而普通索引允许重复的值。

- 删除索引
```sql
DROP INDEX id_index ON tablename;

删除主键索引
ALTER TABLE tablename DROP primary key;
```

- 查询索引
```sql
SHOW INDEX FROM tablename;
或者SHOW INDEXS FROM tablename;

SHOW KEYS FROM tablename;
```

**索引使用场景**
- 使用频繁的字段
- 唯一性差的不适合使用索引
- 更新频繁的不适合使用索引

---

### 事务

用于保证数据的一致性·，由一组dml语句组成。该语句要么全部成功，要么全部失败。

事务和锁
当执行事务操作时，MySQL会在表上加锁，防止其他用户更改数据。

1. 基本操作
- start trasaction --> 开始一个事务（或者 set autocommit = off）
- savepoint 保存点名 --> 设置保存点
- rollback to 保存点名 --> 回退事务
- rollback --> 回退全部事务
- commit  --> 提交事务，所有操作生效，所有保存点被删除，不能再回退，锁被释放数据生效。

注意：
- 未开启事务，dml操作默认自动提交
- mysql事务机制只能在innoDB存储引擎下，才被支持。

2. 隔离级别
介绍：事务与事务之间的隔离程度
注意：==在开启事务之前设置隔离级别才会生效，如果才开启事务后设置隔离级别，则需要提交当前事务后才生效。==
多个连接开启事务操作数据库中的数据时，数据库系统要负责隔离操作，以保证各个连接的在获取数据时的准确性。
|     mysql隔离级别   |  脏读        | 不可重复读       |  幻读         |     加锁读     |
|:----------------------|:-----------|:------------|:----------------|:---------------|
|读未提交 Read uncommitted|V|V|V|不加锁|
|读已提交 Read committed|X|V|V|不加锁|
|可重复读 Repeatable read|X|X|X|不加锁|
|可串行化 Serializable|X|X|X|加锁|

说明：V可能出现，X不会出现。加锁后，一个事务未结束（提交）另一个事务的查询会停留等待。

- 脏读：一个事务读取另一个事务==未提交==的dml时。
- 不可重复读：在一个事务中进行多次查询，由于其他==提交==事务的==修改和删除==，导致每次返回的结果集不同。
- 幻读：在一个事务中进行多次查询，由于其他==提交==事务的==插入==操作，导致每次返回的结果集不同。

4. 
查看隔离级别
```sql
查看会话隔离级别
SELECT @@tx_isolation;
查看系统隔离级别
SELECT @@global.tx_isolation;
```
设置隔离级别
```sql
设置会话隔离级别
SET SESSION TRANSACTION ISOLATION LEVEL 级别;
设置系统隔离级别
SET GLOBAL TRANSACTION ISOLATION LEVEL 级别;
```

5. mysql默认隔离级别是可重复读

全局设置隔离级别：在my.ini文件中加-->transaction-isolation = 级别;

6. 事务特性
 - 原子性    事务是一个不可分割的单位，事务的操作要么都发生，要么都不发生。
 - 一致性    事务必须使数据库从一个状态到另一个状态。
 - 隔离性    多个用户并发访问数据库时，多个事务之间不能相互干扰。 
 - 持久性     事务一旦被提交，对数据库的数据的改变就是永久性的。

---

### 存储引擎

表的类型由存储引擎决定，主要支持，CSV，Memory ，ARCHIVE，MRG_MYISAM，MYISAM，InnoBDB
两大类，事务安全型与非事务安全性
1. MyIASM不支持事务，也不支持外键，但其访问速度块，对事务完整性没有要求。使用场景，多见于只用处理基本的CRUD
2. InnoDB储存引擎提供了具有提交，回滚和崩溃恢复能力的事务安全，处理效率差一些，且会占用更多的磁盘空间以保留数据和索引
3. MEMORY存储引擎使用存在内存中的内容来创建表。每个MEMORY表只实际对应一个磁盘文件。其访问速度非常块，使用HASH索引。但是MySQL服务关闭，表中数据会丢失，表的结构还在。（经典使用场景：用户在线状态）

```sql
查看所有存储引擎
SHOW ENGINES
```

```sql
修改存储引擎
ALTER TABLE tablename ENGINE = 引擎;
```

---
### 视图

基本概念：
1. 视图是一个虚拟表，在数据库中只是一个结构文件。其内容由查询定义，数据来自真实表。
2. 构成的是一个映射关系，通过视图可以修改基表的数据，基表的改变也会影响视图的数据。

基本使用
1. 视图的创建（视图也可以映射出另一个视图）
```sql
CREATE view 视图名 as select FROM tablename;
或
ALTER view 视图名 as select FROM tablename;
```
2. 显示如何创建的视图
```sql
SHOW CREATE view 视图名
```

3. 删除视图
```sql
DROP view 视图1,视图2;
```

视图的优点
1. 安全   只保留可以给用户看的数据
2. 性能  多个表联合视图，可以不用JOIN
3. 灵活   刘表映射到新建立的表

==多表联合视图==的使用

```sql
CREATE view select from 
	SELECT * FROM table1,table2,table3
	    WHERE table1.id = table2.id AND ...; 
```

### mysql用户管理

所有MySQL的用户都存储在系统数据库MySQL中的user表中。

1. user表的重要字段
- host    允许登录的位置
- user     用户名
- authentication_string        密码，是通过MySQL的password函数加密后的密码

2. 相关操作：
- 创建用户
```sql
CREATE user '用户名'@'允许登录位置' identified by '密码';
```
- 删除操作
```sql
DROP user '用户名'@'允许登录位置' ;
```
- 用户修改密码
```sql
修改自己的密码
set password = password('密码');
修改他人的密码
set password for '用户名'@'登录位置' = password('密码');//此方式需要有权限，root用户权限最高
```

3. 授权（root用户才有授权权限）
基本语法：
```sql
grant 权限1,权限2 on 库.对象名 to '用户名'@'登录位置' identified by '密码';
```

```sql
说明:  *.*表示本系统中的所有数据库和所有对象
       库.*表示某个数据库的所有对象
       identified by可以省略，若不省略表示修改已经存在的用户，或者创建改用户。
```

4. 回收权限
```sql
revoke 权限 on 库.对象名  FROM '用户名'@'登录位置';
```
5. 权限生效指令fulsh privileges
```sql
FLUSH PRIVILEGES;//若设置的权限未生效，可以使用改指令
```


细节：在创建用户时，可以不指定Host，默认为%,表示所有IP都由连接权限。create user xxx;
          也可以使用  create user 'xxx'@'192.168.1.%'; 表示以它为开头的所有ip;
