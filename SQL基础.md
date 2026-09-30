# 一、DDL
--Data Definition Language（数据定义语言）

## 数据库操作

```sql
#查看所有数据库
SHOW DATABASES;

#创建数据库
CREATE DATABASE mydb1;
CREATE DATABASE IF NOT EXISTS mydb1;

#切换要操作的数据库
USE mydb1;

#删除数据库
DROP DATABASE mydb1;
DROP DATABASE IF EXISTS mydb1;

```

## 表操作

```sql
#创造表
CREATE TABLE IF NOT EXISTS student(
sid INT,
name VARCHAR(20),
gender VARCHAR(20),
age INT,
adress VARCHAR(20)
);

#查看所有表
SHOW TABLES;

#查看表结构
SHOW CREATE TABLES student;

#查看表数据
DESCRIBE student;
DESC student;

#DROP TABLE student;
```

# 二、DML
--Data Manipulation Language（数据操作语言）
update更 delete删 insert插

## insert插入

```sql
#向表中插入某些/全部列
INSERT INTO student(sid, name, gender, age, adress)
            VALUES(01, '张三'， 'M'，18， '广州');
INSERT INTO student VALUES(01, '张三', 'M', 18, '广州');
```

## update更新

```sql
#UPDATE 表名 SET 字段=值，字段=值...;
#UPDATE 表名 SET 字段=值，字段=值...WHERE 条件
UPDATE student SET address = '广州', age = 19 WHERE sid = 01
--例子
UPDATE student SET age = age + 1;
```

## delete删除

```sql
#删除表内容
DELETE FROM student WHERE sid = 01;

#删除表结构
DROP student;
TRUNCATE student;--删掉整个表再新建一个
```

>在对表格操作之前，先切换出要操作的数据库

# 二、DQL
--Data Query Language(数据查询语言)

## 简单查询

```sql
CREATE TABLE product (
    pid INT AUTO_INCREMENT PRIMARY KEY
    COMMENT '商品ID，主键自增',
    pname VARCHAR(100) NOT NULL COMMENT '商品名称',
    price DECIMAL(10, 2) COMMENT '商品价格',
    category_id VARCHAR(20) COMMENT '分类ID'
) COMMENT '商品表';
--COMMENT是描述文字，注释

#数据例子-批量插入
insert into product values 
(null, '啄木鸟衬衣', 300, 'c002'),
(null, '恒源祥西裤', 800, 'c002'),
(null, '花花公子夹克', 440, 'c002'),
(null, '劲霸休闲裤', 266, 'c002'),
(null, '海澜之家卫衣', 180, 'c002'),
(null, '杰克琼斯运动裤', 430, 'c002'),
(null, '兰蔻面霜', 300, 'c003'),
(null, '雅诗兰黛精华水', 200, 'c003'),
(null, '香奈儿香水', 350, 'c003'),
(null, 'SK-II神仙水', 350, 'c003'),
(null, '资生堂粉底液', 180, 'c003'),
(null, '老北京方便面', 56, 'c004'),
(null, '良品铺子海带丝', 17, 'c004'),
(null, '三只松鼠坚果', 88, null);
--因为pid是auto_increment，因此会忽略插入时的null
```

```sql
#1.查询所有
SELECT * FROM product; 

#2.查询某些属性
SELECT pname, price FROM product;

#3.别名查询-关键字as
SELECT * FROM product as p; --表别名
SELECT pname as pn FROM product; --列别名

#4.去掉重复值
SELECT DISTINCT price FROM product;

#5.运算查询-查询表达式结果
SELECT price+10 FROM product;
```

## 运算符

>1.算数运算符： + - * / %  ->SELECT 2+1;

>2.比较运算符：
<=>安全的等于，两个操作值均为NULL时为1
<> != 不等于
IS NULL | IS NOT NULL
LEAST | GREATEST --当有两个及以上的参数时，返回最小/大值
BETWEEN AND --判断是否在两数之间
NOT IN | IN --判断是否在列表中
LIKE --字符匹配

>3.逻辑运算符：
NOT 或者 | --逻辑非
AND 或者 &&
OR 或者 ||
XOR --逻辑异或

>4.位运算：
| 按位或
& 按位与
^ 按位异或
<< 按位左移
'>>'按位右移
~  按位取反


## 条件查询

```sql
#查询名为“海尔洗衣机”的商品
SELECT * FROM product WHERE pname = '海尔洗衣机';

#查询价格不是800的商品
SELECT * FROM product WHERE price <> 800;
SELECT * FROM product WHERE PRICE != 800;
SELECT * FROM product WHERE NOT(price = 800);

#查询在某一价格范围的商品
SELECT * FROM product WHERE price >= 200 AND price <= 800;
SELECT * FROM product WHERE price BETWEEN 200 AND 800;

#查询最小值
SELECT LEAST(1,4,10) as small_num;
SELECT LEAST(1,2,null); 
--有个null则不会进行比较，结果直接为null

#查询最大值
SELECT GREATEST(1,2,3) as big_num;

#❗字符查询
SELECT * FROM product WHERE pname LIKE '%鞋%'; 
--查询含'鞋'的商品

SELECT * FROM product WHERE pname LIKE '海%'; 
--查询'海'开头的商品

SELECT * FROM product WHERE pname LIKE '_蔻%'; 
--查询第二个字为'蔻'的商品
--🔺%代表0个或多个任意字符  _代表恰好一个字符

#查询某属性为空
SELECT * FROM product WHERE category_id IS NULL;

```

## 排序查询
>asc升序 desc降序 默认升序
order by子句放在查询语句最后面，LIMI子句除外

```sql
#价格降序的基础上，分类升序
SELECT * FROM product ORDER BY price desc, category_id asc;

#降序查询不重复的商品价格
SELECT DISTINCT price FROM product ORDER BY price desc;
```

## 聚合查询
>count() --列 不为null的行数
sum() --列 数值和，若指定列类型不是数值类型，计算结果为0
max() | min() --计算最大|小值
avg() --列平均值 若指定列类型不是数值类型，计算结果为0

```sql
#查询商品条数
SELECT count(pid) FROM product; --不统计null
SELECT count(*) FROM product; --统计所有个数

SELECT max(price) FROM product WHERE category_id ='c001';
```

## 分组查询

```sql
SELECT category_id, count(*) cnt FROM product 
GROUP BY category_id HAVING cnt > 4 ORDER BY cnt;

#分组条件：
--HAVING count(*)>5 （条数大于5的）--分组后进行
--WHERE category_id = 'c001' --分组前进行

#WHERE 里面不能写聚合函数，HAVING 能写聚合函数
#WHERE 效率比 HAVING 高 

#❗SQL执行顺序：from-group by-count()-select-having-order by
```

>❗若要进行分组，则select子句后只能出现分组和统计，其他不能出现


## 分页查询

>分页显示数据，例如30条数据，每页显示5条
❗一般放在语句的最后

```sql
#显示前n条
SELECT * FROM product LIMIT 5;

#从第m条开始显示，显示n条
SELECT * FROM product LIMIT 0, 50;--第一页从第1条开始显示，显示50条
SELECT * FROM product LIMIT 50, 50;--第二页（2-1）*50
SELECT * FROM product LIMIT 100, 50;--第n页 （n-1）*50
```


## insert into select语句

>将table1的数据导入table2 ->要求table2存在

```sql
INSERT INTO table2(n1, n2) SELECT v1, v2 FROM table1; --插入部分数据
INSERT INTO table2 SELECT * FROM table1; --插入全部数据

#例：把表1分类信息导入到表2
INSERT INTO table2 
SELECT category_id, count(*) cnt FROM product ORDER BY cnt;
```
