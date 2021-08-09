# 数据库基础
- 数据库管理系统（DBMS）就是数据库管理系统，数据库是DBMS创建和管理的数据容器。
- 通常具体来讲是以表的形式来存储数据，表名唯一。
> 一列一字段，字段分解要清晰，由字段需求确定数据类型，数据类型限制了输入，对于数据处理和磁盘优化有帮助。
>> 一行代表一个记录，行就是记录，严格来讲，行是正确说法
- 主键：一列或一组列来唯一标识某一行
> 应该总是定义主键，便于后期处理
- 主键应该满足以下几个条件
> 1. 任意两行都不具有相同的主键值,如果是一组主键，组合要是唯一的
> 2. 每一行都要有一个主键值，主键不能为空
> 3. 主键值不允许更新和更改
> 4. 主键值不能重复使用，即使改行被删除
# 检索数据
## 基本查询语句
```SQL
SELECT pro_name FROM Products;
```
- 上面语句就是从Products表中查询pro_name列
- 关键字大小写不敏感
- 语句结束用分号标识
- 忽略空格，只识别分号
```SQL
SELECT prod_id, prod_name, prod_price FROM Products;
```
这样实现查询多列。
```SQL
SELECT *FROM Products;
```
这样实现查询所有内容
- 通配符`*`，一般不使用，影响效率和性能。
```SQL
SELECT DISTINCT vend_id FROM Products;
```
去掉了重复的值，保留了不同的值
- DISTINCT关键字实现此功能
- 不能部分使用DISTINCT，除非SELECT DISTINCT vend_id, prod_price里两个属性的记录完全一样，否则将会展示所有记录
## 限制查询结果
如果只想要前五行，不同的数据库管理软件有不同的方法。
- SQL SERVER 或 Access
```SQL
SELECT TOP 5 prod_name
FROM Products;
```
- DB2
```SQL
SELECT prod_nameFROM ProductsFETCH FIRST 5 ROWS ONLY;
```
- Oracle
```SQl
SELECT prod_name
FROM Products
WHERE ROWNUM <=5;
```
- MySQL、MariaDB、PostgreSQL或者SQLite
```SQL
SELECT prod_name
FROM Products
LIMIT 5;
```
如果从第五行开始后面的五行
```SQL
SELECT prod_name
FROM Products
LIMIT 5 OFFSET 5;
```
- LIMIT代表返回的行数，OFFSET代表从第几行开始，注意默认从第零行开始
- 简化语法是LIMIT 5,5；
## 注释
### 行内注释
```SQL
SELECT prod_name -- 这是一条注释 
FROM Products;
```
### 整行注释
```SQL
# 这是一条注释
SELECT prod_name 
FROM Products;
```
### 多行注释
```SQL
/* SELECT prod_name, vend_id 
FROM Products; */ 
SELECT prod_name 
FROM Products;
```
# 排序输出
## 
```SQL
SELECT prod_name
FROM Products
ORDER BY prod_name;
```
- ORDER BY 字句实现通过大小排列，必须是最后一个字句
- 可以不通过select后面的字段进行排序
## 多个排序条件
```SQL
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price, prod_name;
```
- 当前一个排序条件相同时，比较后面一个条件
### 按清单位置排序
```SQL
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY 2, 3;
```
- 2,3 代表select后面的位置，所以此时，不能出现清单中没有出现的字段
## 倒序排列
```SQL
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC
```
- 加入DESC 关键字，实现倒叙排列
### 倒序排列的多种条件
``` SQL
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC, prod_name;
```
- 对第一个条件倒序排序，对第二个条件正序排列
## 字母大小写的区分问题
- A和a谁排在前面，取决于数据库软件的设置
# 过滤结果
```SQL
SELECT prod_name, prod_price
FROM Products
WHERE prod_price = 3.49;
```
- 筛选了price等于3.49的物品名称
```SQL
SELECT prod_name, prod_price
FROM Products
WHERE prod_price < 10;
```
- 筛选了价格小于10的物品
---
- WHERE语句应该排在order by的前面
- 适用于WHERE语句的运算符 
| 操作符 |说 明|
|---|---|
|= |等于|
|< > |不等于|
|!= |不等于|
|< |小于|
|<= |小于等于|
|!| 不小于|
|> |大于|
|>= |大于等于|
|!>| 不大于|
|BETWEEN |在指定的两个值之间|
|IS NULL |为NULL值|
- 表中有些符号冗余，如< >与!=相同，!<相当于>=
## SQL过滤与应用层过滤
> 数据也可以在应用层过滤。为此，SQL的SELECT语句为客户端应用检索出超过实际所需的数据，然后客户端代码对返回数据进行循环，提取出需要的行。通常，这种做法极其不妥。优化数据库后可以更快速有效地对数据进行过滤。而让客户端应用（或开发语言）处理数据库的工作将会极大地影响应用的性能，并且使所创建的应用完全不具备可伸缩性。此外，如果在客户端过滤数据，服务器不得不通过网络发送多余的数据，这将导致网络带宽的浪费。
## 更多过滤列子
```SQl
SELECT vend_id, prod_name
FROM Products
WHERE vend_id <> 'DLL01';
```
- 筛选出不是戴尔公司的记录
- 单引号`'`用来表示字符串
- 其中<> 视情况可以用!=代替
```SQL
SELECT prod_name, prod_price
FROM Products
WHERE prod_price BETWEEN 5 AND 10;
```
- 筛选出区间内的记录
- 包含两端数值
```SQL
SELECT cust_name
FROM CUSTOMERS
WHERE cust_email IS NULL;
```
- 筛选出有空值的记录
- 没有不返回结果
# 高级数据过滤

