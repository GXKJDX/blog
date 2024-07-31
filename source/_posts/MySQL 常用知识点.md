---
title: 一、常用的 MySQL 命令示例
date: 2024-07-31 22:32:50
tags: mysql
---
### 一、常用的 MySQL 命令示例

1. **SELECT** 用于查询数据

   ```sql
   SELECT id, name FROM users;
   ```

2. **WHERE** 查询数据时给定条件筛选

   ```sql
   SELECT * FROM users WHERE id >= 1;
   ```

3. **DISTINCT** 去掉重复值

   ```sql
   SELECT DISTINCT name FROM users;
   ```

4. **AND, OR, NOT** 逻辑操作符

   ```sql
   SELECT * FROM users WHERE id >= 1 AND name = 'Alice';
   ```

5. **IN** 查询多个数据

   ```sql
   SELECT * FROM users WHERE id IN (1, 2, 3);
   ```

6. **BETWEEN** 查询某个范围的数据

   ```sql
   SELECT * FROM users WHERE id BETWEEN 1 AND 5;
   ```

7. **LIKE** 模糊查询

   ```sql
   SELECT * FROM users WHERE name LIKE 'A%';
   ```

8. **REGEXP** 通过正则表达式查找记录

   ```sql
   SELECT * FROM users WHERE name REGEXP 'A.*';
   ```

9. **GROUP BY** 结果集分组

   ```sql
   SELECT item_name, COUNT(*) AS total FROM orders GROUP BY item_name;
   ```

10. **ORDER BY** 排序结果集

    ```sql
    SELECT * FROM users ORDER BY id;
    ```

11. **LIMIT** 限定返回记录条数

    ```sql
    SELECT * FROM users LIMIT 10;
    ```

12. **JOIN** 连接查询

    ```sql
    SELECT users.name, orders.item_name 
    FROM users 
    INNER JOIN orders ON users.id = orders.user_id;
    ```

### 二、连表操作示例

1. **INNER JOIN** 连接用户表和订单表，显示所有用户及其订单。

```sql
SELECT users.name, orders.item_name, orders.price 
FROM users 
INNER JOIN orders ON users.id = orders.user_id;
```

2. **LEFT JOIN** 连接用户表和订单表，显示所有用户及其订单（包括没有订单的用户）。

```sql
SELECT users.name, orders.item_name, orders.price 
FROM users 
LEFT JOIN orders ON users.id = orders.user_id;
```

3. **RIGHT JOIN** 连接用户表和订单表，显示所有订单及其用户（包括没有匹配用户的订单）。

```sql
SELECT users.name, orders.item_name, orders.price 
FROM users 
RIGHT JOIN orders ON users.id = orders.user_id;
```

4. **FULL JOIN**（在 MySQL 中可以使用 UNION ALL 模拟 FULL JOIN）显示所有用户及其订单（包括没有匹配的用户或订单）。

```sql
SELECT users.name, orders.item_name, orders.price 
FROM users 
LEFT JOIN orders ON users.id = orders.user_id
UNION ALL
SELECT users.name, orders.item_name, orders.price 
FROM users 
RIGHT JOIN orders ON users.id = orders.user_id
WHERE users.id IS NULL;
```

5. **GROUP BY 和 HAVING** 查询每个用户的订单总金额，并筛选出总金额超过 200 的用户。

```sql
SELECT users.name, SUM(orders.price) AS total_spent 
FROM users 
INNER JOIN orders ON users.id = orders.user_id 
GROUP BY users.name 
HAVING total_spent > 200;
```

6. **ORDER BY** 按用户姓名和订单价格排序。

```sql
SELECT users.name, orders.item_name, orders.price 
FROM users 
INNER JOIN orders ON users.id = orders.user_id 
ORDER BY users.name, orders.price DESC;
```

7. **LIMIT** 只显示前 5 个订单。

```sql
SELECT users.name, orders.item_name, orders.price 
FROM users 
INNER JOIN orders ON users.id = orders.user_id 
ORDER BY orders.price DESC 
LIMIT 5;
```

### 三、锁的概念和示例

#### 1.共享锁（SHARED LOCK）

允许多个事务同时读取数据，但不能进行修改。

```sql
SELECT * FROM orders WITH (HOLDLOCK);
```

**共享锁示例**

假设有两个事务T1和T2，都想读取同一行数据：

- T1获取共享锁，读取数据。
- T2获取共享锁，读取数据。

在这种情况下，两个事务可以同时读取数据，但在它们持有共享锁期间，其他事务不能对数据进行修改。

#### 2.排他锁（EXCLUSIVE LOCK）

允许事务读取和修改数据，当持有独占锁时，其他事务无法读取锁住的数据。

```sql
UPDATE orders SET price = 99.0 WHERE id = 1;
```

**排他锁示例**

* T1获取排他锁，读取并修改数据。

* T2尝试获取排他锁，但必须等待T1释放锁后才能继续。

在这种情况下，只有一个事务可以同时对数据进行修改，防止并发修改导致的数据不一致问题。

#### 3.更新锁（UPDATE LOCK）

用于处理事务先读取数据后进行更新操作，防止死锁。

```sql
SELECT * FROM orders WHERE id = 1 FOR UPDATE;
```

**更新锁示例**

假设有事务T1和T2都要读取并修改同一行数据，流程如下：

1. **T1获取更新锁**：
   - T1: 获取更新锁，读取数据。
   - T2: 尝试获取更新锁，被阻塞；可以获取共享锁，读取数据。
2. **T1准备修改数据**：
   - T1: 将更新锁升级为独占锁，进行数据修改。
   - T2: 仍然阻塞，等待T1释放锁。
3. **T1完成修改并释放锁**：
   - T1: 完成修改，释放独占锁。
   - T2: 获取更新锁，读取数据，并准备修改。

#### 4.行级锁（Row-Level Lock）

行级锁用于锁定特定行，在执行更新操作时常用。这样可以避免其他事务同时修改同一行。

```sql
-- 锁定特定行
UPDATE orders SET price = 99.99 WHERE id = 1 AND user_id = 1 LOCK IN SHARE MODE;
```

#### 5.页级锁（Page-Level Lock）

页级锁用于锁定包含多行数据的页，这种锁在某些情况下可以提高性能，但可能会导致锁争用。

```sql
-- MySQL 不支持直接的页级锁，下面的例子是逻辑上的演示
SELECT * FROM orders WHERE id BETWEEN 1 AND 10 LOCK IN SHARE MODE;
```

#### 6.表级锁（Table-Level Lock）

表级锁用于锁定整个表，当需要对整个表进行操作时使用。例如，进行批量更新时。

```sql
-- 锁定整个表以防止其他事务访问
LOCK TABLES orders WRITE;

-- 执行批量更新
UPDATE orders SET price = price * 1.1;

-- 释放表锁
UNLOCK TABLES;
```

#### 7.意向锁（Intention Lock）

意向锁用于在多级锁定中标记意图，避免锁冲突。常见的意向锁有意向共享锁（IS）和意向排他锁（IX）。

```sql
-- MySQL 自动处理意向锁，无需手动设置
-- 以下为逻辑示例，展示意向锁的工作原理

-- 事务 1 获取意向排他锁（IX）
START TRANSACTION;
SELECT * FROM orders WHERE user_id = 1 FOR UPDATE;

-- 事务 2 获取意向共享锁（IS）
START TRANSACTION;
SELECT * FROM orders WHERE user_id = 2 LOCK IN SHARE MODE;

-- 事务 1 和事务 2 可以同时执行，因为意向锁不冲突
```

#### 8.自定义锁（User-Defined Lock）

MySQL 提供 `GET_LOCK` 和 `RELEASE_LOCK` 函数，用于实现自定义锁。这对于需要在应用层实现复杂锁定逻辑时很有用。

```sql
-- 获取自定义锁
SELECT GET_LOCK('my_lock', 10);

-- 执行操作
UPDATE orders SET price = 99.99 WHERE id = 1;

-- 释放自定义锁
SELECT RELEASE_LOCK('my_lock');
```

MySQL的事务机制提供了一种确保数据库操作的可靠性和一致性的方法。事务是一组原子操作，要么全部成功，要么全部失败。MySQL支持事务的存储引擎主要是InnoDB。以下是关于MySQL事务的详细介绍。

### 四、事务的四大特性（ACID）

1. **原子性（Atomicity）**：事务中的所有操作要么全部完成，要么全部不完成。任何一个操作失败都会导致整个事务的失败，已执行的操作会被回滚到初始状态。
2. **一致性（Consistency）**：事务执行前后，数据库的状态必须保持一致。数据从一种一致状态变为另一种一致状态。
3. **隔离性（Isolation）**：一个事务的执行不能被其他事务干扰。不同事务之间的操作是相互隔离的。
4. **持久性（Durability）**：一旦事务提交，其结果就永久保存，即使数据库发生故障。

#### 事务的基本操作

在MySQL中，可以通过以下SQL命令来管理事务：

- `START TRANSACTION`：开始一个新的事务。
- `COMMIT`：提交事务，保存所有更改。
- `ROLLBACK`：回滚事务，撤销所有更改。

#### 示例

以下是一个简单的事务操作示例：

```sql
-- 开始事务
START TRANSACTION;

-- 执行一些数据库操作
INSERT INTO accounts (account_id, balance) VALUES (1, 1000);
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;

-- 提交事务
COMMIT;
```

如果在执行过程中发生错误，可以使用 `ROLLBACK` 撤销更改：

```sql
-- 开始事务
START TRANSACTION;

-- 执行一些数据库操作
INSERT INTO accounts (account_id, balance) VALUES (1, 1000);
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;

-- 如果发生错误，回滚事务
ROLLBACK;
```

### 五、索引

MySQL中的索引是用于提高查询速度的数据结构。索引类似于书籍的目录，通过索引可以快速定位数据，而不需要扫描整个表。下面是关于MySQL中索引的详细介绍：

#### 索引的类型

1. **主键索引（Primary Key Index）**：

   - 每个表只能有一个主键索引。
   - 主键索引是一种唯一索引，不允许重复和NULL值。
   - 创建主键时，数据库会自动创建主键索引。

   ```sql
   CREATE TABLE users (
       id INT PRIMARY KEY,
       name VARCHAR(100)
   );
   ```

2. **唯一索引（Unique Index）**：

   - 唯一索引不允许两行具有相同的索引值。
   - 一个表可以有多个唯一索引。

   ```sql
   CREATE UNIQUE INDEX idx_email ON users (email);
   ```

3. **普通索引（Index）**：

   - 普通索引允许重复和NULL值。
   - 用于加速数据查询，但不会强制唯一性。

   ```sql
   CREATE INDEX idx_name ON users (name);
   ```

4. **全文索引（Fulltext Index）**：

   - 用于全文搜索，适用于较大的文本字段。
   - 主要用于`CHAR`、`VARCHAR`和`TEXT`类型的列。

   ```sql
   CREATE FULLTEXT INDEX idx_content ON articles (content);
   ```

5. **组合索引（Composite Index）**：

   - 由多个列组合而成的索引，用于多列的查询优化。
   - 可以部分匹配列，提高查询效率。

   ```sql
   CREATE INDEX idx_name_email ON users (name, email);
   ```

#### 索引的创建与删除

**创建索引**：

```sql
-- 创建普通索引
CREATE INDEX idx_name ON users (name);

-- 创建唯一索引
CREATE UNIQUE INDEX idx_email ON users (email);

-- 创建全文索引
CREATE FULLTEXT INDEX idx_content ON articles (content);

-- 创建组合索引
CREATE INDEX idx_name_email ON users (name, email);
```

**删除索引**：

```sql
-- 删除索引
DROP INDEX idx_name ON users;
```

**在创建表时定义索引**：

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    content TEXT,
    FULLTEXT (content),
    UNIQUE (email),
    INDEX idx_name_email (name, email)
)
```