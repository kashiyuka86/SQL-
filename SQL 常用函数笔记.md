### SQL 常用函数笔记

#### 一、聚合函数

- **COUNT()**
  - **功能**：计算查询结果中的行数。
  - **示例**：
    - 统计所有记录数：`SELECT COUNT(*) FROM employees;`
    - 统计特定条件记录数：`SELECT COUNT(*) FROM employees WHERE department = 'Sales';`
- **SUM()**
  - **功能**：计算数值列的总和。
  - **示例**：`SELECT SUM(order_amount) FROM sales_orders;`
- **AVG()**
  - **功能**：计算数值列的平均值。
  - **示例**：`SELECT AVG(score) FROM student_scores;`
- **MAX () 和 MIN ()**
  - **功能**：MAX () 返回指定列中的最大值，MIN () 返回指定列中的最小值。
  - **示例**：
    - `SELECT MAX(price) FROM product_prices;`
    - `SELECT MIN(price) FROM product_prices;`

#### 二、字符串函数

- **CONCAT()**
  - **功能**：将两个或多个字符串连接在一起。
  - **示例**：`SELECT CONCAT(first_name,' ',last_name) AS full_name FROM customers;`
- **SUBSTRING () 或 MID ()（不同数据库有不同用法）**
  - **功能**：从一个字符串中提取出一部分。
  - **示例**：
    - MySQL：`SELECT SUBSTRING(product_code, LOCATE(' - ',product_code)+ 2) FROM product_codes;`
    - SQL Server：`SELECT RIGHT(product_code, LEN(product_code)- CHARINDEX(' - ',product_code)- 1) FROM product_codes;`
- **UPPER () 和 LOWER ()**
  - **功能**：UPPER () 将字符串中的所有字符转换为大写，LOWER () 将所有字符转换为小写。
  - **示例**：
    - `SELECT UPPER(username) FROM user_names;`
    - `SELECT LOWER(username) FROM user_names;`

#### 三、日期和时间函数

- **GETDATE ()（在 SQL Server 中）或 CURRENT_DATE（在 MySQL 等中）**
  - **功能**：获取当前日期。
  - **示例**：
    - SQL Server：`INSERT INTO orders (order_date) VALUES (GETDATE());`
    - MySQL：`INSERT INTO orders (order_date) VALUES (CURRENT_DATE);`
- **DATEADD ()（在 SQL Server 中）或 DATE_ADD（在 MySQL 等中）**
  - **功能**：在日期上添加或减去指定的时间间隔。
  - **示例**：
    - SQL Server：`SELECT DATEADD(day, 3, GETDATE());`
    - MySQL：`SELECT DATE_ADD(CURRENT_DATE, INTERVAL 3 DAY);`
- **DATEDIFF ()（在 SQL Server 中）或 DATEDIFF（在 MySQL 等中）**
  - **功能**：计算两个日期之间的差值。
  - **示例**：
    - SQL Server：`SELECT DATEDIFF(day, hire_date, termination_date) FROM employees;`
    - MySQL：`SELECT DATEDIFF(termination_date, hire_date) FROM employees;`

#### 四、数学函数

- **ROUND()**
  - **功能**：将数字进行四舍五入。
  - **示例**：
    - 四舍五入到整数：`SELECT ROUND(price) FROM product_prices;`
    - 保留一位小数：`SELECT ROUND(price, 1) FROM product_prices;`
- **CEIL () 和 FLOOR ()**
  - **功能**：CEIL () 返回大于或等于给定数字的最小整数，FLOOR () 返回小于或等于给定数字的最大整数。
  - **示例**：
    - 对于数字 3.2，`SELECT CEIL(3.2);` 返回 4，`SELECT FLOOR(3.2);` 返回 3。
- **ABS()**
  - **功能**：返回数字的绝对值。
  - **示例**：对于数字 -5，`SELECT ABS(-5);` 返回 5。

#### 五、转换函数

- **CAST()**
  - **功能**：将一种数据类型的表达式转换为另一种数据类型。
  - **示例**：`SELECT CAST('20241209' AS DATE);`
- **CONVERT()**
  - **功能**：与 CAST 类似，但在某些数据库中提供了更多的转换选项和功能。
  - **示例**：SQL Server 中 `SELECT CONVERT(VARCHAR(10), GETDATE(), 120);`

#### 六、条件函数

- **IF () 或 CASE WHEN ()**
  - **功能**：根据条件返回不同的值。
  - **示例**：
    - MySQL：`SELECT IF(score >= 60, '及格', '不及格') AS result FROM students;`
    - CASE WHEN：`SELECT CASE WHEN age < 18 THEN '未成年' WHEN age >= 18 AND age < 60 THEN '成年' ELSE '老年' END AS age_group FROM persons;`
- **COALESCE()**
  - **功能**：返回参数列表中的第一个非 NULL 值。
  - **示例**：`SELECT COALESCE(NULL, 'default_value', NULL);`
- **NULLIF()**
  - **功能**：如果两个表达式相等，则返回 NULL；否则返回第一个表达式的值。
  - **示例**：`SELECT NULLIF(5, 5);` 会返回 NULL，`SELECT NULLIF(5, 3);` 会返回 5。
- **ISNULL()**
  - **功能**：检查表达式是否为 NULL。
  - **示例**：SQL Server 中 `SELECT ISNULL(NULL, 'not null');`

#### 七、分组函数

- **GROUP_CONCAT()**
  - **功能**：在 MySQL 等数据库中，用于将某个列的多个值连接成一个字符串。
  - **示例**：`SELECT GROUP_CONCAT(product_name) FROM products GROUP BY category;`

#### 八、窗口函数

- **ROW_NUMBER()**
  - **功能**：为查询结果中的每一行分配一个连续的序号。
  - **示例**：`SELECT ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num, employee_name, salary FROM employees;`
- **RANK()**
  - **功能**：与 ROW_NUMBER () 类似，但在遇到相同值时会分配相同的序号，且后续序号会出现间断。
- **DENSE_RANK()**
  - **功能**：在遇到相同值时分配相同序号，且后续序号不会间断。
