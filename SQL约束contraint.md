# 主键约束 primary key
>1.每个表只能有一个主键，主键可以由一个或多个列组成
 2.主键不能为空

```sql
#增加主键方法
CREATE TABLE student(
sid INT PRIMARY KEY,
name VARCHAR(20)
--另一种方法：PRIMARY KEY(sid)
);

#添加多列主键（联合主键）
CREATE TABLE student(
sid INT,
sname VARCHAR(20),
PRIMARY KEY(sid, sname)
);

#修改表结构增加主键
ALTER TABLE student ADD PRIMARY KEY (sname);

#删除主键约束
ALTER TABLE student DROP PRIMARY KEY;
```

# 自增约束 auto_increment

>1.属性自增长
 2.默认初始值1，自动增加+1
 3.一个字段使用auto_increment约束，且NOT NULL
 4.字段只能是整数类型

```sql
#创表时指定自增
CREATE TABLE student(
    sid INT PRIMARY KEY AUTO_INCREMENT,
    age INT,
    sname VARCHAR(20),
    gender VARCHAR(20),
)AUTO_INCREMENT = 10;--指定自增开始的数字
--ALTER TABLE student AUTO_INCREMENT = 10;

```

# 非空约束 not null

>字段值不能为空

```sql
#添加非空约束方法
CREATE TABLE student(
    sid INT NOT NULL,
    sname VARCHAR(20)
);
--另一种：ALTER TABLE student MODIFY sname VARCHAR(20) NOT NULL
--MODIFY修改时，要把属性全写上，因为是覆盖性的

#删除非空约束
ALTER TABLE student MODIFY sname VARCHAR(20);
--用覆盖删除非空约束
```

>❗注意：ALTER-MODIFY是覆盖性，修改一列，ALTER-ADD是加一列
# 唯一约束 unique

>1.字段的值不能重复出现
 2.可以为NULL值，因为NULL与任何值不相等，与自身也不相等

```sql
CREATE TABLE student(
    sid INT UNIQUE,
    sname VARCHAR(20),
    PRIMARY KEY(sid, sname)
);
--另一种方法：
ALTER TABLE student ADD gender VARCHAR(20);--加一列
ALTER TABLE student ADD UNIQUE(sname);

--修改列的约束
ALTER TABLE student MODIFY sid INT;
```

# 默认约束 default

>用来设定默认值

```sql
#创建时添加默认约束
CREATE TABLE student(
    sid INT,
    sname VARCHAR(20),
    adress VARCHAR(20) DEFAULT '广州'
);
--另一种：
ALTER TABLE student MODIFY adress VARCHAR(20) DEFAULT '佛山'
```

# 零填充约束 zerofill

>1.当字段的值长度小于定义长度时，在前面自动填充0
>2.默认int(10)->即长度为10

```sql
CREATE TABLE student(
    sid INT ZEROFILL,
    sname VARCHAR(20),
    PRIMARY KEY(sid, sname)
);
--ZEROFILL 目前的状态是 “已弃用”
```