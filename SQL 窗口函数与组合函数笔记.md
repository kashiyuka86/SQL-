# SQL 窗口函数与组合函数笔记

## 一、窗口函数

1. **ROW_NUMBER()函数**
   - 功能：为查询结果集中的每一行分配一个唯一的连续数字，从 1 开始，按照指定的排序规则。
   - 示例：
     
     ```sql
     SELECT ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num, employee_name, salary
     FROM employees;
     -- 按照 salary 降序为 employees 表中的每行数据分配一个序号，可用于排名操作，例如对员工工资进行排名。
     ```
2. **RANK()函数和 DENSE_RANK()函数**
   - 功能：`RANK`函数在排序时，如果有相同的值，它们会得到相同的排名，并且下一个排名会跳过相应的数量；`DENSE_RANK`函数在排序时，相同的值会得到相同的排名，下一个排名不会跳过。
   - 示例：
     
     ```sql
     -- RANK 函数示例
     SELECT RANK() OVER (ORDER BY score DESC) AS ranking, student_name, score
     FROM scores;
     -- DENSE_RANK 函数示例
     SELECT DENSE_RANK() OVER (ORDER BY score DESC) AS ranking, student_name, score
     FROM scores;
     -- 可用于处理学生成绩排名等场景，对比不同排名规则下的结果差异，如竞赛排名中确定相同分数的选手排名情况。
     ```
3. **LEAD()函数和 LAG()函数**
   - 功能：`LEAD`函数用于访问当前行之后的行数据；`LAG`函数用于访问当前行之前的行数据。可以指定偏移量和默认值。
   - 示例：
     
     ```sql
     SELECT order_date, amount,
     LEAD(amount, 1, 0) OVER (ORDER BY order_date) AS next_amount,
     LAG(amount, 1, 0) OVER (ORDER BY order_date) AS prev_amount
     FROM orders;
     -- 在 orders 表中，获取每行订单金额的下一个和上一个订单金额，常用于分析数据的前后关系，如查看销售额的环比变化。
     ```
4. **NTILE()函数**
   - 功能：将结果集按照指定的数量进行分组（分桶），并为每行分配一个所属分组的编号。
   - 示例：
     
     ```sql
     SELECT student_name, score,
     NTILE(5) OVER (ORDER BY score DESC) AS grade_group
     FROM students;
     -- 将学生按照成绩降序排列后，平均分成 5 个组，为每个学生分配所在的组号，可用于成绩等级划分等场景，比如将成绩划分为不同的等级区间。
     ```
5. **CUME_DIST()函数**
   - 功能：计算当前行在分区内的累积分布值，即小于等于当前行值的行数占分区总行数的比例。
   - 示例：
     
     ```sql
     SELECT product_name, price,
     CUME_DIST() OVER (ORDER BY price) AS cum_dist
     FROM products;
     -- 计算每个产品价格在所有产品价格中的累积分布比例，可用于分析价格的分布情况，比如确定某个产品价格在整体价格体系中的位置。
     ```

## 二、组合函数

- **GROUP_CONCAT()函数**
  - 功能：将分组中的字符串列连接成一个字符串，可指定分隔符。
  - 示例：
    
    ```sql
    SELECT department, GROUP_CONCAT(employee_name SEPARATOR ', ')
    FROM employees
    GROUP BY department;
    -- 按照部门分组，将每个部门的员工姓名连接成一个字符串，以逗号和空格分隔，常用于将相关数据合并展示，如展示每个部门的员工名单。
    ```
    
    窗口函数能够对数据进行灵活的分析和处理，在数据排名、数据对比、分组内分析等方面有广泛应用；组合函数如 GROUP_CONCAT() 则可以方便地将分组内的多个值组合成一个字符串，便于数据的整合与展示。不同数据库系统对这些函数的具体实现和语法可能略有差异，但核心功能大致相同。

# SQL组合函数类似功能函数笔记

## 一、GROUP_CONCAT函数回顾

- GROUP_CONCAT函数用于将分组中的字符串列连接成一个字符串，可指定分隔符。
  
  ```sql
  SELECT department, GROUP_CONCAT(employee_name SEPARATOR ', ')
  FROM employees
  GROUP BY department;
  ```
- 上述示例按照部门分组，将每个部门的员工姓名连接成一个以逗号和空格分隔的字符串，用于整合和展示分组内的数据。
  
  ## 二、类似性质的函数
  
  ### 1. LISTAGG函数（在Oracle中）
- **功能**：和GROUP_CONCAT类似，用于将分组中的行数据按照指定的分隔符连接成一个字符串。
- **示例**：
  
  ```sql
  SELECT department, LISTAGG(employee_name, ', ') WITHIN GROUP (ORDER BY employee_name)
  FROM employees
  GROUP BY department;
  ```
- 这个示例是在Oracle数据库中使用LISTAGG函数。它按照部门分组，将每个部门内的员工姓名连接成一个字符串，用逗号和空格分隔，并且可以通过`ORDER BY`指定连接顺序。
  
  ### 2. STRING_AGG函数（在SQL Server和PostgreSQL中）
- **功能**：将一组值连接成一个字符串，支持指定分隔符，与GROUP_CONCAT的功能相似。
- **示例（SQL Server）**：
  
  ```sql
  SELECT department, STRING_AGG(employee_name, ', ')
  FROM employees
  GROUP BY department;
  ```
- **示例（PostgreSQL）**：
  
  ```sql
  SELECT department, STRING_AGG(employee_name, ', ')
  FROM employees
  GROUP BY department;
  ```
- 在SQL Server和PostgreSQL中，STRING_AGG函数的用法基本相同，都是按照部门分组后将员工姓名连接成一个以逗号分隔的字符串。
  
  ### 3. ARRAY_AGG函数（在PostgreSQL中）
- **功能**：将分组后的列值收集到一个数组中，而不是连接成一个字符串。这在需要以数组形式处理分组数据时非常有用。
- **示例**：
  
  ```sql
  SELECT department, ARRAY_AGG(employee_name)
  FROM employees
  GROUP BY department;
  ```
- 此示例在PostgreSQL中使用，按照部门分组后将员工姓名收集到一个数组中，用于后续可能的数组操作，比如对数组元素进行索引访问、统计数组长度等。
  这些函数在处理分组数据的组合展示方面都很有用，不过它们在不同的数据库系统中有不同的名称和语法细节，在实际使用时需要根据所使用的数据库进行调整。它们可以帮助我们更方便地将分组后的相关数据整合在一起，以满足特定的数据分析和展示需求。
