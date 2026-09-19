# SQL 常用语句

## 1. 创建表

```sql
CREATE TABLE student (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    age INT,
    gender CHAR(1),
    score DECIMAL(5,2),
    create_time DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

## 2. 插入数据

### 单条插入

```sql
INSERT INTO student (name, age, gender, score)
VALUES ('张三', 20, '男', 88.5);
```

### 批量插入

```sql
INSERT INTO student (name, age, gender, score)
VALUES
('李四', 21, '女', 92.0),
('王五', 19, '男', 85.0),
('赵六', 22, '女', 90.5);
```

## 3. 查询数据

### 查询全部

```sql
SELECT * FROM student;
```

### 查询指定字段

```sql
SELECT id, name, score FROM student;
```

### 条件查询

```sql
SELECT * FROM student WHERE age >= 20 AND gender = '男';
```

### 模糊查询

```sql
SELECT * FROM student WHERE name LIKE '张%';
```

### 排序

```sql
SELECT * FROM student ORDER BY score DESC;
```

### 分页

```sql
SELECT * FROM student LIMIT 0, 10;
```

## 4. 修改数据

```sql
UPDATE student
SET score = 95
WHERE id = 1;
```

## 5. 删除数据

```sql
DELETE FROM student WHERE id = 1;
```

### 删除整表

```sql
DELETE FROM student;
```

## 6. 多表查询

假设有两张表：student 和 course

```sql
SELECT s.name, c.course_name
FROM student s
JOIN course c ON s.course_id = c.id;
```

### 左连接

```sql
SELECT s.name, c.course_name
FROM student s
LEFT JOIN course c ON s.course_id = c.id;
```

## 7. 聚合查询

### 统计人数

```sql
SELECT COUNT(*) AS total FROM student;
```

### 按性别分组

```sql
SELECT gender, COUNT(*) AS num
FROM student
GROUP BY gender;
```

### 平均分

```sql
SELECT AVG(score) AS avg_score FROM student;
```

### 最大值和最小值

```sql
SELECT MAX(score) AS max_score, MIN(score) AS min_score
FROM student;
```

## 8. 备注

- `SELECT *` 表示查询全部字段。
- `WHERE` 用于条件筛选。
- `ORDER BY` 用于排序，`ASC` 升序，`DESC` 降序。
- `LIMIT` 用于分页限制结果数量。
- `JOIN` 用于多表关联查询。
- `GROUP BY` 常用于分组统计。

## 9. 常见 SQL 语法速记

```sql
SELECT 列名
FROM 表名
WHERE 条件
GROUP BY 分组字段
HAVING 筛选条件
ORDER BY 排序字段
LIMIT 数量;
```

如果你愿意，我还可以继续把这份文档扩展成“数据库基础题 + SQL 实战题”的版本，适合直接用于作业或复习。