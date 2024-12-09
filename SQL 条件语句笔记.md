# SQL 条件语句笔记

## 一、基本的 `WHERE` 子句

- `WHERE` 子句用于在 `SELECT`、`UPDATE`、`DELETE` 等语句中筛选数据。它可以包含各种比较运算符，如 `=`, `<`, `>`, `<=`, `>=`, `<>`（或 `!=`，用于不等于）。
  
  ```sql
  -- 从名为 customers 的表中选择 age 大于 20 的所有记录
  SELECT * FROM customers WHERE age > 20;
  ```

## 二、`AND` 和 `OR` 运算符

- `AND` 用于连接多个条件，要求所有条件都为真时才返回结果。常用于需要同时满足多个筛选条件的场景，比如查询特定城市且年龄在某个范围的客户信息。
  
  ```sql
  -- 选择 age 大于 20 且 city 为 'New York' 的记录
  SELECT * FROM customers WHERE age > 20 AND city = 'New York';
  ```
- `OR` 用于连接多个条件，只要其中一个条件为真就返回结果。例如查询位于某几个城市或者年龄符合特定条件的记录。
  
  ```sql
  -- 选择 age 大于 20 或者 city 为 'Los Angeles' 的记录
  SELECT * FROM customers WHERE age > 20 OR city = 'Los Angeles';
  ```

## 三、`IN` 运算符

- `IN` 用于指定一个值列表，只要字段值在这个列表中就返回结果。相比使用多个 `OR` 条件，代码更加简洁清晰，且在某些数据库中执行效率更高。
  
  ```sql
  -- 选择 city 为 'New York'、'Los Angeles' 或 'Chicago' 的记录
  SELECT * FROM customers WHERE city IN ('New York', 'Los Angeles', 'Chicago');
  ```

## 四、`BETWEEN` 运算符

- `BETWEEN` 用于筛选在某个范围内的值（包括边界值）。对于数值型和日期型数据的范围查询非常方便。

```sql
-- 选择 age 在 20 到 30 之间（包括 20 和 30）的记录
SELECT * FROM customers WHERE age BETWEEN 20 AND 30;
-- 选择 order_date 在 '2024-01-01' 到 '2024-02-01' 之间的订单记录
SELECT * FROM orders WHERE order_date BETWEEN '2024-01-01' AND '2024-02-01';
```

## 五、`LIKE` 运算符

- `LIKE` 用于模糊匹配字符串。它支持通配符 `%`（代表任意字符序列）和 `_`（代表单个任意字符）。常用于搜索具有特定模式的字符串数据，如搜索以某个字符开头或包含某个子串的记录。
  
  ```sql
  -- 选择 name 以 'J' 开头的记录
  SELECT * FROM customers WHERE name LIKE 'J%';
  -- 选择 name 第二个字符为 'o' 的记录
  SELECT * FROM customers WHERE name LIKE '_o%';
  -- 选择 address 中包含 'Street' 的记录
  SELECT * FROM customers WHERE address LIKE '%Street%';
  ```

## 六、`CASE` 语句

- 简单 `CASE` 表达式：
  
  ```sql
  SELECT column_name,
  CASE column_expression
  WHEN value1 THEN result1
  WHEN value2 THEN result2
  ELSE default_result
  END
  FROM table_name;
  ```
  
  例如：
  
  ```sql
  SELECT order_id,
  CASE status
  WHEN 'Pending' THEN 'Awaiting Processing'
  WHEN 'Shipped' THEN 'On the Way'
  ELSE 'Unknown Status'
  END AS new_status
  FROM orders;
  ```
- 搜索 `CASE` 表达式：更加灵活，可以使用复杂的条件判断。
  
  ```sql
  SELECT column_name,
  CASE 
  WHEN condition1 THEN result1
  WHEN condition2 THEN result2
  ELSE default_result
  END
  FROM table_name;
  ```
  
  例如：
  
  ```sql
  SELECT product_name,
  CASE 
  WHEN price > 100 THEN 'Expensive'
  WHEN price > 50 THEN 'Moderate'
  ELSE 'Cheap'
  END AS price_category
  FROM products;
  ```

## 七、`HAVING` 子句

- `HAVING` 子句用于在分组后对分组结果进行筛选，通常与 `GROUP BY` 子句一起使用。它可以使用聚合函数作为筛选条件，而 `WHERE` 子句不能直接使用聚合函数进行筛选分组后的结果。
  
  ```sql
  -- 先按照 department 分组，然后选择员工数量大于 5 的部门
  SELECT department, COUNT(*)
  FROM employees
  GROUP BY department
  HAVING COUNT(*) > 5;
  -- 按照 city 分组，选择平均年龄大于 30 且订单总额大于 10000 的城市
  SELECT city, AVG(age), SUM(order_amount)
  FROM customers
  GROUP BY city
  HAVING AVG(age) > 30 AND SUM(order_amount) > 10000;
  ```

## 八、条件判断函数

- **COALESCE函数**
  - 功能：返回参数列表中的第一个非空表达式的值。
  - 示例：
    
    ```sql
    SELECT COALESCE(column1, column2, '默认值')
    FROM table_name;
    ```
    
    如果 `column1` 的值不为空，则返回 `column1` 的值；如果 `column1` 为空且 `column2` 不为空，则返回 `column2` 的值；如果 `column1` 和 `column2` 都为空，则返回 `'默认值'`。在处理可能存在空值的数据合并或替换空值时很有用，比如在查询客户的备用联系电话时，如果主电话为空，则显示备用电话。
    
    ```sql
    SELECT customer_name, COALESCE(primary_phone, secondary_phone, '无联系电话')
    FROM customers;
    ```
- **NULLIF函数**
  - 功能：比较两个表达式，如果相等则返回 `NULL`，否则返回第一个表达式的值。
  - 示例：
    
    ```sql
    SELECT NULLIF(column1, column2)
    FROM table_name;
    ```
    
    假设在一个产品库存表中，有 `in_stock`（实际库存）和 `min_stock`（最小库存）两个字段，要找出库存数量与最小库存不相等的产品：
    
    ```sql
    SELECT product_name, NULLIF(in_stock, min_stock)
    FROM inventory;
    ```
    
    如果 `in_stock` 和 `min_stock` 相等，则返回 `NULL`，否则返回 `in_stock` 的值。
- **IF函数（在 MySQL 中）**
  - 功能：实现简单的条件判断，类似于其他编程语言中的三元表达式。语法为 `IF(condition, value_if_true, value_if_false)`。
  - 示例：
    
    ```sql
    SELECT product_name,
    IF(price > 100, 'Expensive', 'Not Expensive') AS price_category
    FROM products;
    ```
    
    这里如果 `price` 大于 100，则 `price_category` 为 `'Expensive'`，否则为 `'Not Expensive'`。
- **IIF函数（在 SQL Server 中）**
  - 功能：与 MySQL 的 `IF` 函数类似，语法为 `IIF(condition, true_value, false_value)`。
  - 示例：
    
    ```sql
    SELECT customer_name,
    IIF(age > 25, 'Adult', 'Youth') AS age_group
    FROM customers;
    ```
    
    根据 `age` 是否大于 25 来划分客户的年龄组。
    这些条件语句和函数在 SQL 数据查询、更新和删除操作中非常重要，可以根据不同的业务需求精确地筛选和处理数据。不同的数据库系统可能在语法细节上略有差异，但基本功能和用法是相似的。


