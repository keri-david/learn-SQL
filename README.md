- [数据库基础](#数据库基础)
- [检索数据](#检索数据)
  - [基本查询语句](#基本查询语句)
  - [限制查询结果](#限制查询结果)
  - [注释](#注释)
    - [行内注释](#行内注释)
    - [整行注释](#整行注释)
    - [多行注释](#多行注释)
- [排序输出](#排序输出)
  - [单个排序条件](#单个排序条件)
  - [多个排序条件](#多个排序条件)
    - [按清单位置排序](#按清单位置排序)
  - [倒序排列](#倒序排列)
    - [倒序排列的多种条件](#倒序排列的多种条件)
  - [字母大小写的区分问题](#字母大小写的区分问题)
- [过滤结果](#过滤结果)
  - [- 筛选了价格小于10的物品](#--筛选了价格小于10的物品)
  - [SQL过滤与应用层过滤](#sql过滤与应用层过滤)
  - [更多过滤列子](#更多过滤列子)
- [高级数据过滤](#高级数据过滤)
  - [操作符](#操作符)
  - [使用通配符](#使用通配符)
    - [like操作符](#like操作符)
    - [%通配符](#通配符)
    - [_通配符](#_通配符)
    - [[]通配符](#通配符-1)
    - [^ 通配符](#-通配符)
    - [技巧](#技巧)
- [计算字段](#计算字段)
  - [拼接字段](#拼接字段)
  - [使用别名](#使用别名)
  - [算数运算](#算数运算)
- [函数](#函数)
  - [认识函数](#认识函数)
  - [使用函数](#使用函数)
    - [文本处理函数](#文本处理函数)
    - [日期和时间处理函数](#日期和时间处理函数)
    - [数值处理函数](#数值处理函数)
- [汇总数据](#汇总数据)
  - [聚集函数](#聚集函数)
    - [AVG（）](#avg)
    - [COUNT（）](#count)
    - [MAX和MIN](#max和min)
  - [聚集不同类型的值](#聚集不同类型的值)
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

## 单个排序条件
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

- 适用于WHERE语句的运算符 
 
| 操作符  | 说 明              |
| ------- | ------------------ |
| =       | 等于               |
| < >     | 不等于             |
| !=      | 不等于             |
| <       | 小于               |
| <=      | 小于等于           |
| !       | 不小于             |
| >       | 大于               |
| >=      | 大于等于           |
| !>      | 不大于             |
| BETWEEN | 在指定的两个值之间 |
| IS NULL | 为NULL值           |
- **表中有些符号冗余，如< >与!=相同，!<相当于>=**

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

## 操作符
1. and就是且
2. or就是或
3. and和or可以进行组合
```SQL
SELECT prod_name, prod_price
FROM Products
WHERE (vend_id = 'DLL01' OR vend_id = ‘BRS01’)
AND prod_price >= 10;
```
- **如果不加括号，就会优先计算AND，这样会得到预期外的结果**
4. in匹配关键字
```SQL
SELECT prod_name, prod_price
FROM Products
WHERE vend_id IN ( 'DLL01', 'BRS01' )
ORDER BY prod_name;
```
- 等价与下面的语句
```
SELECT prod_name, prod_price
FROM Products
WHERE vend_id = 'DLL01' OR vend_id = 'BRS01'
ORDER BY prod_name;
```
- in运算符优于or表达式
> 1. 语法更加清楚
> 2. 避免与其他or、and混淆
> 3. 执行更快
> 4. 可以包含在其他select语句中
5. not操作符
- 用来否定其他运算符
```SQL
SELECT prod_name
FROM Products
WHERE NOT vend_id = 'DLL01'
ORDER BY prod_name;
```
- 筛选出来不是戴尔的产品
- 等价于下面的语句
```SQL
SELECT prod_name
FROM Products
WHERE vend_id <> 'DLL01'
ORDER BY prod_name;
```
- 对于复杂语句来说，not对比其他语句有优势

## 使用通配符
### like操作符
- 用来匹配值的一部分的特殊字符。
- 只能用于文本字段搜索

>- 操作符何时不是操作符？答案是，它作为谓词时。从技术上说，LIKE是谓词而不是操作符。虽然最终的结果是相同的，但应该对此术语有所了解。
### %通配符
```SQL
SELECT prod_id, prod_name 
FROM Products 
WHERE prod_name LIKE 'Fish%';
```
- 匹配以Fish开头的任意文本
- 匹配搜索是区分大小写的
> 如果使用的是Microsoft Access，需要使用*而不是%
```SQL
SELECT prod_id, prod_name 
FROM Products 
WHERE prod_name LIKE '%bean bag%';
```
- 搜索包含bean bag的文本字段记录
```SQL
SELECT prod_name
FROM Products
WHERE prod_name LIKE 'F%y%';
```
- 匹配以F开头，y结尾的文本记录
- 后面的`%`，目地是为了防止后面有空格，导致的不能匹配
### _通配符
- 用途和`%`一样，但是`_`之匹配一个字符，而`%`则可以匹配多任意字符，0个到无限个
- DB2不支持`_`
- 如果使用的是Microsoft Access，需要使用?而不是_。
```SQL
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE '__ inch teddy bear';
```
```SQL
SELECT prod_id, prod_nameFROM ProductsWHERE prod_name LIKE '% inch teddy bear';
```
> 下面使用`%`会多一个结果`BR01 8 inch teddy bear`，因为%可以匹配任意多字符
### []通配符
```SQL
SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[JM]%'
ORDER BY cust_contact;
```
- 匹配以J、M开头的联系人
- 使用[]必须给定关键字位置
### ^ 通配符
```SQL
SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[^JM]%'
ORDER BY cust_contact;
```
- 匹配开头不是J、M的联系人
- 如果使用的是Microsoft Access，需要用!而不是^来否定一个集合，因此，使用的是[!JM]而不是[^JM]。
- 也可以用not语句来实现
### 技巧
> 1. 不要过度使用通配符。如果其他操作符能达到相同的目的，应该使用其他操作符。
>2. 在确实需要使用通配符时，也尽量不要把它们用在搜索模式的开始处。把通配符置于开始处，搜索起来是最慢的。
>3. 仔细注意通配符的位置。如果放错地方，可能不会返回想要的数据。
# 计算字段
- 有时数据并不是应用层需要的，这时就要进行转换
- 虽然有些数据转换在应用层也可以完成，但一般来讲数据库的转换更加高效
- 字段就是列，就如行就是记录一样
- 从应用层来讲，计算字段和普通的数据字段开起来没有区别
## 拼接字段
```SQL
SELECT vend_name + ' (' + vend_country + ')'
FROM Vendors
ORDER BY vend_name;
```
```SQL
SELECT vend_name || ' (' || vend_country || ')'
FROM Vendors
ORDER BY vend_name;
```
- `+`和`||`等价
- Access和SQL Server使用+号。DB2、Oracle、PostgreSQL、SQLite和Open Office Base使用||
- 在MySQL和MariaDB中，必须使用特殊的函数
>1. 存储在vend_name列中的名字；
>2. 包含一个空格和一个左圆括号的字符串；
>3. 存储在vend_country列中的国家；
>4. 包含一个右圆括号的字符串。
- 之间的结果输出，有较多的空格输出，需要用函数RTRIM()来去除。
```SQL
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')'
FROM Vendors
ORDER BY vend_name;
```
- 大多数DBMS都支持RTRIM()（正如刚才所见，它去掉字符串右边的空格）、LTRIM()（去掉字符串左边的空格）以及TRIM()（去掉字符串左右两边的空格）。
## 使用别名
```SQL
SELECT RTRIM(vend_name) + ' (' + RTRIM(vend_country) + ')' 
 AS vend_title
FROM Vendors
ORDER BY vend_name;
```
- MySQL和Mariadb
```SQL
SELECT Concat(vend_name, ' (', vend_country, ')')
 AS vend_title
FROM Vendors
ORDER BY vend_name;
```
- 别名既可以是一个单词也可以是一个字符串。如果是后者，字符串应该括在引号中。虽然这种做法是合法的，但不建议这么去做。多单词的名字可读性高，不过会给客户端应用带来各种问题。因此，别名最常见的使用是将多个单词的列名重命名为一个单词的名字。
- 也叫导出列
## 算数运算
```SQL
SELECT prod_id,
 quantity,
 item_price,
 quantity*item_price AS expanded_price
FROM OrderItems
WHERE order_num = 20008;
```
- 有加减乘除四个运算符
- 可以用括号调整或区分优先级
- 虽然SELECT通常用于从表中检索数据，但是省略了FROM子句后就是简单地访问和处理表达式，例如SELECT 3 * 2;将返回6，SELECT Trim(' abc ');将返回abc，SELECT Now();使用Now()函数返回当前日期和时间。。
# 函数
## 认识函数
- DBMS函数的差异

| 函数                 | 语法                                                                                                                                        |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| 提取字符串的组成部分 | Access使用MID()；DB2、Oracle、PostgreSQL和SQLite使用SUBSTR()；MySQL和SQL Server使用SUBSTRING()                                              |
| 数据类型转换         | Access和Oracle使用多个函数，每种类型的转换有一个函数；DB2和PostgreSQL使用CAST()；MariaDB、MySQL和SQL Server使用CONVERT()                    |
| 取当前日期           | Access使用NOW()；DB2和PostgreSQL使用CURRENT_DATE；MariaDB和MySQL使用CURDATE()；Oracle使用SYSDATE；SQL Server使用GETDATE()；SQLite使用DATE() |
> 为了代码的可移植，许多SQL程序员不赞成使用特定于实现的功能。虽然这样做很有好处，但有的时候并不利于应用程序的性能。如果不使用这些函数，编写某些应用程序代码会很艰难。必须利用其他方法来实现DBMS可以非常有效完成的工作。
## 使用函数
- 分为以下几个函数类型
1. 用于处理文本字符串（如删除或填充值，转换值为大写或小写）的文本函数。 
2. 用于在数值数据上进行算术操作（如返回绝对值，进行代数运算）的数值函数。
3. 用于处理日期和时间值并从这些值中提取特定成分（如返回两个日期之差，检查日期有效性）的日期和时间函数。
4. 返回DBMS正使用的特殊信息（如返回用户登录信息）的系统函数
### 文本处理函数
- 常用的文本处理函数

| 函数                                  | 说明                  |
| ------------------------------------- | --------------------- |
| LEFT()（或使用子字符串函数）          | 返回字符串左边的字符  |
| LENGTH()（也使用DATALENGTH()或LEN()） | 返回字符串的长度      |
| LOWER()（Access使用LCASE()）          | 将字符串转换为小写    |
| LTRIM()                               | 去掉字符串左边的空格  |
| RIGHT()（或使用子字符串函数）         | 返回字符串右边的字符  |
| RTRIM()                               | 去掉字符串右边的空格  |
| SOUNDEX()                             | 返回字符串的SOUNDEX值 |
| UPPER()（Access使用UCASE()）          | 将字符串转换为大写    |
- 关于Soundex（）的说明
> SOUNDEX是一个将任何文本串转换为描述其语音表示的字母数字模式的算法。SOUNDEX考虑了类似的发音字符和音节，使得能对字符串进行发音比较而不是字母比较。虽然SOUNDEX不是SQL概念，但多数DBMS都提供对SOUNDEX的支持。
```SQL
SELECT cust_name, cust_contact
FROM Customers
WHERE SOUNDEX(cust_contact) = SOUNDEX('Michael Green');
```
输出结果：
```SQL
cust_name     cust_contact
-------------------------- 
Kids Place     Michelle Green
```
- 很明显就是靠发音检索
### 日期和时间处理函数
- 一个列子，返回同样的结果，不同的DBMS
```SQL
# SQL SERVER、Sybase
SELECT order_num
FROM Orders
WHERE DATEPART(yy, order_date) = 2012;
# Access
SELECT order_num
FROM Orders
WHERE DATEPART('yyyy', order_date) = 2012;
# postgreSQL
SELECT order_num
FROM Orders
WHERE DATE_PART('year', order_date) = 2012;
# oracle(1)
SELECT order_num
FROM Orders
WHERE to_number(to_char(order_date, 'YYYY')) = 2012;
# oracle(2)
SELECT order_num
FROM Orders
WHERE order_date BETWEEN to_date('01-01-2012')
AND to_date('12-31-2012');
# MySQL和MariaDB
SELECT order_num
FROM Orders
WHERE YEAR(order_date) = 2012;
# SQLite
SELECT order_num
FROM Orders
WHERE strftime('%Y', order_date) = 2012;
```
### 数值处理函数
- 较为统一

| 函数   | 解释               |
| ------ | ------------------ |
| ABS()  | 返回一个数的绝对值 |
| COS()  | 返回一个角度的余弦 |
| EXP()  | 返回一个数的指数值 |
| PI()   | 返回圆周率         |
| SIN()  | 返回一个角度的正弦 |
| SQRT() | 返回一个数的平方根 |
| TAN()  | 返回一个角度的正切 |
# 汇总数据
## 聚集函数

| 函数    | 说明             |
| ------- | ---------------- |
| AVG()   | 返回某列的平均值 |
| COUNT() | 返回某列的行数   |
| MAX()   | 返回某列的最大值 |
| MIN()   | 返回某列的最小值 |
| SUM()   | 返回某列值之和   |
### AVG（）
```SQL
SELECT AVG(prod_price) AS avg_price
FROM Products
WHERE vend_id = 'DLL01';
```
- 列出dell产品的平均值
- AVG（）作用于单列，忽略值为None的行
### COUNT（）
```SQL
SELECT COUNT(*) AS num_cust
FROM Customers;
```
### MAX和MIN
- 如果用于文本类数据，则返回最后一行和第一行
## 聚集不同类型的值