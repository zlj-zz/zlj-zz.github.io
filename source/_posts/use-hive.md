---
title: Detailed explanation of using hive
date: 2020-03-12 
category: Database
tags: Hive

---

最近用到hive，这里做个笔记. hive 的 hql 的使用总结也算的上七七八八了. 以后在补充吧.

<!--more-->

# hive导入数据
hive不支持 `insert` 语句，数据只能通过 `load` 导入
**1.本地导入**
` hive> load data local inpath 'file-path' into table table-name;`
**2.HDFS导入**
`hive> load data inpath 'hdfs-path' into table table-name;`
>本地导入的过程就是先加载到HDFS中，再导入hive表中

**3.查询导入**
`hive> insert into table table-name 查询语句`
**4.创建表时查询导入** (建表的方式之一)
`hive> create table table-name as 查询语句`

# hive建表
首先看官网介绍 (进行一定标注)
`[ ]`表示可选，`|` 表示二选一

```sql
CREATE [TEMPORARY] [EXTERNAL] TABLE [IF NOT EXISTS] [db_name.]table_name    
	-- (Note: TEMPORARY 0.14.0版本及以后存在)
  [(col_name data_type [COMMENT col_comment], ... [constraint_specification])]
  --(列名 data_type comment 列注释内容)
  [COMMENT table_comment]
  --(comment 表注释内容)
  [PARTITIONED BY (col_name data_type [COMMENT col_comment], ...)]
  [CLUSTERED BY (col_name, col_name, ...) [SORTED BY (col_name [ASC|DESC], ...)] INTO num_buckets BUCKETS]
  [SKEWED BY (col_name, col_name, ...)                  -- (Note: Available in Hive 0.10.0 and later)]
     ON ((col_value, col_value, ...), (col_value, col_value, ...), ...)
     [STORED AS DIRECTORIES]
  [
   [ROW FORMAT row_format] --(行格式)
   [STORED AS file_format]
     | STORED BY 'storage.handler.class.name' [WITH SERDEPROPERTIES (...)]  -- (Note: Available in Hive 0.6.0 and later)
  ]
  [LOCATION hdfs_path]
  [TBLPROPERTIES (property_name=property_value, ...)]   -- (Note: Available in Hive 0.6.0 and later)
 

[AS select_statement];   -- (Note: Available in Hive 0.5.0 and later; not supported for external tables)
 

CREATE [TEMPORARY] [EXTERNAL] TABLE [IF NOT EXISTS] [db_name.]table_name
  LIKE existing_table_or_view_name
  [LOCATION hdfs_path];
 
data_type --(支持数据类型)
  : primitive_type --(原始类型)
  | array_type --(存储同类型的list，如:array<data_type>)
  | map_type --(key-value形式，如：map<primitive_type, data_type>)
  | struct_type --(存储不同类型)
  		：STRUCT < col_name : data_type [COMMENT col_comment], ...>
  | union_type  -- (有限取值范围内取值，Note:0.7.0版本及之后存在)
  		: UNIONTYPE < data_type, data_type, ... >  
  		
primitive_type --(支持原始类型)
  : TINYINT --(1bit整数，如：45Y)
  | SMALLINT --(2bit整数，如：45S)
  | INT --(4bit整数，如：45)
  | BIGINT --(8bit整数，如：45L)
  | BOOLEAN --(布尔类型，true / false)
  | FLOAT --(4字节单精度浮点数)
  | DOUBLE --(8字节单精度浮点数)
  | DOUBLE PRECISION -- (Note: 2.2.0版本及之后存在)
  | STRING --(字符串，不定长)
  | BINARY -- (变长二进制，Note:0.8.0版本及之后存在)
  | TIMESTAMP -- (时间戳，纳秒精度，Note:0.8.0版本及之后存在)
  | DECIMAL -- (任意精度带符号小数，如：DECIMAL(4, 2)范围：-99.99到99.99，Note: 0.11.0版本及之后存在)
  | DECIMAL(precision, scale)  -- (Note: 0.13.0版本及之后存在)
  | DATE        -- (日期，如：'2020-3-10'，Note: 0.12.0版本及之后存在)
  | VARCHAR     -- (变长字符串，长度有上限，Note: 0.12.0版本及之后存在)
  | CHAR        -- (定长字符串，Note: 0.13.0版本及之后存在)
 
row_format --(行样式格式)
  : DELIMITED [FIELDS TERMINATED BY char [ESCAPED BY char]] [COLLECTION ITEMS TERMINATED BY char]
        [MAP KEYS TERMINATED BY char] [LINES TERMINATED BY char]
        [NULL DEFINED AS char]   -- (Note: 0.13.0版本及之后存在)
  | SERDE serde_name [WITH SERDEPROPERTIES (property_name=property_value, property_name=property_value, ...)]
 
file_format: --(文件存储格式)
  : SEQUENCEFILE
  | TEXTFILE    -- (Default, 文本格式, 常用)
  | RCFILE      -- (Note: 0.6.0版本及之后存在)
  	--(基于HDFS架构，表格占用多个HDFS块。每个HDFS块中，RCFile以行组为基本单位来组织记录。)
  | ORC         -- (Note: 0.11.0版本及之后存在)
  | PARQUET     -- (Note: 0.13.0版本及之后存在)
  | AVRO        -- (Note: 0.14.0版本及之后存在)
  | INPUTFORMAT input_format_classname OUTPUTFORMAT output_format_classname
 
constraint_specification: --(约束规范)
  : [, PRIMARY KEY (col_name, ...) DISABLE NOVALIDATE ] --(主键)
  	--(外键约束)
    [, CONSTRAINT constraint_name FOREIGN KEY (col_name, ...) REFERENCES table_name(col_name, ...) DISABLE NOVALIDATE
```
样例
```sql
hive> create table cpo_1 (url string, title string, reply_number int, last_time string, content array<string>)
    > row format delimited fields terminated by '\t' collection items terminated by '-';
```
### like建表
会创建结构完全相同的表，但是没有数据。 
常用语中间表 `CREATE TABLE empty_key_value_store LIKE key_value_store;`

### 外部表
未被 `external` 修饰的是内部表(managed table)，被 `external` 修饰的为外部表(external table)；  
>内部表数据由 Hive 自身管理，外部表数据由 HDFS 管理； 
内部表数据存储的位置是hive.metastore.warehouse.dir（默认：/user/hive/warehouse），外部表数据的存储位置由自己制定； 
删除内部表会直接删除元数据（metadata）及存储数据；删除外部表仅仅会删除元数据，HDFS上的文件并不会被删除； 
对内部表的修改会将修改直接同步给元数据，而对外部表的表结构和分区进行修改，则需要修复（MSCK REPAIR TABLE table_name;）

样例
```sql
hive> create external table cpo_1 (url string, title string, reply_number int, last_time string, content array<string>, add map<String,string>)
    > row format delimited fields terminated by '\t' 
    > collection items terminated by '-'  
	> map keys terminated by ':'
	> location '/user/t2';
```

# hive查询
1. 显示表结构：`desc tablename` or `desc formatted tablename`
1.  全表查询：`select * from emp;`
2.  选择特定列查询：`select empno, ename from emp;`
3.  列别名查询：`select ename AS name, deptno dn from emp;`
4.  算术运算符：`select sal + 1 from emp;`
5.  常用函数
    ① 求总行数（**count**）：`hive> select count(*) cnt from emp;`
    ② 求工资的最大值（**max**）：`hive> select max(sal) max_sal from emp;`
    ③ 求工资的最小值（**min**）：`hive> select min(sal) min_sal from emp;`
    ④ 求工资的总和（**sum**）：`hive> select sum(sal) sum_sal from emp;`
    ⑤ **Limit** 语句：`hive> select * from emp limit 5;`

6. where语句
    `hive> select * from emp where sal >1000;`

### 运算符
>下面表中描述了谓词操作符，这些操作符同样可以用于JOIN…ON和HAVING语句中。
	
**比较运算符**

```
| 操作符                  | 支持的数据类型 | 描述                                                                                                                                                                                                                                                      |
|-------------------------|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| A=B                     | 基本数据类型   | 如果A等于B则返回TRUE，反之返回FALSE                                                                                                                                                                                                                       |
| A<=>B                   | 基本数据类型   | 如果A和B都为NULL，则返回TRUE，其他的和等号（=）操作符的结果一致，如果任一为NULL则结果为NULL                                                                                                                                                               |
| A<>B, A!=B              | 基本数据类型   | A或者B为NULL则返回NULL；如果A不等于B，则返回TRUE，反之返回FALSE                                                                                                                                                                                           |
| A<B                     | 基本数据类型   | A或者B为NULL，则返回NULL；如果A小于B，则返回TRUE，反之返回FALSE                                                                                                                                                                                           |
| A<=B                    | 基本数据类型   | A或者B为NULL，则返回NULL；如果A小于等于B，则返回TRUE，反之返回FALSE                                                                                                                                                                                       |
| A>B                     | 基本数据类型   | A或者B为NULL，则返回NULL；如果A大于B，则返回TRUE，反之返回FALSE                                                                                                                                                                                           |
| A>=B                    | 基本数据类型   | A或者B为NULL，则返回NULL；如果A大于等于B，则返回TRUE，反之返回FALSE                                                                                                                                                                                       |
| A [NOT] BETWEEN B AND C | 基本数据类型   | 如果A，B或者C任一为NULL，则结果为NULL。如果A的值大于等于B而且小于或等于C，则结果为TRUE，反之为FALSE。如果使用NOT关键字则可达到相反的效果。                                                                                                                |
| A IS NULL               | 所有数据类型   | 如果A等于NULL，则返回TRUE，反之返回FALSE                                                                                                                                                                                                                  |
| A IS NOT NULL           | 所有数据类型   | 如果A不等于NULL，则返回TRUE，反之返回FALSE                                                                                                                                                                                                                |
| IN(数值1, 数值2)        | 所有数据类型   | 使用 IN运算显示列表中的值                                                                                                                                                                                                                                 |
| A [NOT] LIKE B          | STRING 类型    | B是一个SQL下的简单正则表达式，如果A与其匹配的话，则返回TRUE；反之返回FALSE。B的表达式说明如下：‘x%’表示A必须以字母‘x’开头，‘%x’表示A必须以字母’x’结尾，而‘%x%’表示A包含有字母’x’,可以位于开头，结尾或者字符串中间。如果使用NOT关键字则可达到相反的效果。 |
| A RLIKE B, A REGEXP B   | STRING 类型    | B是一个正则表达式，如果A与其匹配，则返回TRUE；反之返回FALSE。匹配使用的是JDK中的正则表达式接口实现的，因为正则也依据其中的规则。例如，正则表达式必须和整个字符串A相匹配，而不是只需与其字符串匹配。                                                       |
```

**逻辑运算符**

操作符|含义
|--|--|
AND|逻辑并
OR|逻辑或
NOT|逻辑否

### Like和RLike
1. 使用 `like` 运算选择类似的值
2.  选择条件可以包含字符或数字:`%` 代表零个或多个字符(任意个字符)。`_`  代表一个字符。
3.  `RLIKE` 子句是Hive中这个功能的一个扩展，其可以通过Java的正则表达式这个更强大的语言来指定匹配条件。

 `查找薪水中含有2的员工信息：hive (default)> select * from emp where sal RLIKE  '[2]’;`

### 分组
 **Group By语句**
>GROUP BY语句通常会和聚合函数一起使用，按照一个或者多个列队结果进行分组，然后对每个组执行聚合操作。

计算emp表每个部门的平均工资:`hive> select t.deptno, avg(t.sal) avg_sal from emp t group by t.deptno;`

**Having语句**
having与where不同点 :

1. where针对表中的列发挥作用,查询数据；having针对查询结果中的列发挥作用，筛选数据。
2.  where后面不能写分组函数，而having后面可以使用分组函数。
3. having只用于group by分组统计语句。

求每个部门的平均薪水大于2000的部门:`hive> select deptno, avg(sal) avg_sal from emp group by deptno having avg_sal > 2000;`

### join
 **等值Join**
>Hive支持通常的SQL JOIN语句，但是只支持等值连接，不支持非等值连接。

案例实操:根据员工表和部门表中的部门编号相等，查询员工编号、员工名称和部门编号:
```sql
select e.empno, e.ename, d.deptno, d.dname from emp e join dept d on e.deptno = d.deptno;
```
 **内连接**
 >只有进行连接的两个表中都存在与连接条件相匹配的数据才会被保留下来。
```sql
select e.empno, e.ename, d.deptno from emp e join dept d on e.deptno=d.deptno;
```
**左外连接**
>JOIN操作符左边表中符合WHERE子句的所有记录将会被返回。
```sql
select e.empno, e.ename, d.deptno from emp e left join dept d on e.deptno=d.deptno;
```
 **右外连接**
>JOIN操作符右边表中符合WHERE子句的所有记录将会被返回。
```sql
select e.empno, e.ename, d.deptno from emp e right join dept d on e.deptno= d.deptno;
```
**满外连接**
>将会返回所有表中符合WHERE语句条件的所有记录。如果任一表的指定字段没有符合条件的值的话，那么就使用NULL值替代。
```sql
select e.empno, e.ename, d.deptno from emp e full join dept d on e.deptno= d.deptno;
```
**多表连接**
>连接 n个表，至少需要n-1个连接条件。例如：连接三个表，至少需要两个连接条件。
```sql
SELECT e.ename, d.deptno, l. loc_name FROM  emp e JOIN  dept d ON d.deptno = e.deptno JOIN   location l ON  d.loc = l.loc;
```
>**说明**：大多数情况下，Hive会对每对JOIN连接对象启动一个MapReduce任务。本例中会首先启动一个MapReduce job对表e和表d进行连接操作，然后会再启动一个MapReduce job将第一个MapReduce job的输出和表l进行连接操作。
>**注意**：为什么不是表d和表l先进行连接操作呢？这是因为Hive总是按照从左到右的顺序执行的。


### 排序
**全局排序**（`Order By`）
>    `ASC`（ascend）: 升序（默认）
    `DESC`（descend）: 降序

ORDER BY 子句在SELECT语句的结尾:
`hive (default)> select * from emp order by col_name;`
>也可以用别名，和使用多个字段

**每个MapReduce内部排序**（`Sort By`）
 >Sort By：每个MapReduce内部进行排序，对全局结果集来说不是排序。

（1）设置reduce个数:`hive> set mapreduce.job.reduces=3;`
（2）查看设置reduce个数:`hive> set mapreduce.job.reduces;`
（3）根据部门编号降序查看员工信息:`hive> select * from emp sort by empno desc;`
（4）将查询结果导入到文件中（按照部门编号降序排序）:`hive> insert overwrite local directory '/opt/module/datas/sortby-result' select * from emp sort by deptno desc;`

**分区排序**（`Distribute By`）
>Distribute By：类似MR中partition，进行分区，结合sort by使用。
注意，Hive要求DISTRIBUTE BY语句要写在SORT BY语句之前。对于distribute by进行测试，一定要分配多reduce进行处理，否则无法看到distribute by的效果。

案例实操：
- 先按照部门编号分区，再按照员工编号降序排序。
```sql
hive> set mapreduce.job.reduces=3;
hive> insert overwrite local directory '/opt/module/datas/distribute-result' select * from emp distribute by deptno sort by empno desc;
```

**Cluster By**
>当distribute by和sorts by字段相同时，可以使用cluster by方式。
cluster by除了具有distribute by的功能外还兼具sort by的功能。但是排序只能是倒序排序，不能指定排序规则为ASC或者DESC。

以下两种写法等价
```sql
hive (default)> select * from emp cluster by deptno;
hive (default)> select * from emp distribute by deptno sort by deptno;
```
>注意：按照部门编号分区，不一定就是固定死的数值，可以是20号和30号部门分到一个分区里面去。

### 分桶及抽样查询

