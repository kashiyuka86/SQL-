# SQL 常用聚合函数笔记

## 一、COUNT 函数

- **功能细节**：
  - COUNT 函数主要用于统计满足特定条件的行数。它存在两种常见的使用形式：COUNT (\*) 和 COUNT (column_name)。COUNT (*) 会对表中的所有行进行统计，即便其中包含 NULL 值的行也会被计入。而 COUNT (column_name) 则仅统计指定列中非 NULL 值的行数。
- **应用场景示例**：
  - 假设有一个名为 “orders” 的表，其中包含 “order_id”（订单编号）、“customer_id”（顾客编号）以及 “order_date”（订单日期）等列。
  - 若想要知晓该表中总的订单数量，可使用以下语句：

```sql
SELECT COUNT(*) FROM orders;
```

- 若仅希望统计具有顾客编号（即 “customer_id” 列非 NULL）的订单数量，则可使用：

```sql
SELECT COUNT(customer_id) FROM orders;
```

- **性能考虑**：
  - 在不同的数据库系统中，COUNT (\*) 的性能表现会因数据库的存储引擎以及数据结构的差异而有所不同。对于大型数据表而言，使用 COUNT (column_name) 可能会具有更快的执行速度，这是因为数据库在执行时能够直接跳过包含 NULL 值的行进行统计。然而，某些数据库对 COUNT (\*) 进行了优化处理，使得其执行性能与 COUNT (column_name) 相当，甚至在某些情况下表现更为出色。

## 二、SUM 函数

- **功能细节**：
  - SUM 函数专门用于计算指定列中所有数值的总和。需要注意的是，它只能应用于数值类型的列，如果在包含非数值数据的列上使用 SUM 函数，将会引发错误。
- **应用场景示例**：
  - 假设有一个名为 “sales” 的表，其中包含 “sales_amount”（销售金额）列。
  - 若要计算该表中的总销售额，可使用以下语句：

```sql
SELECT SUM(sales_amount) FROM sales;
```

- 倘若表中还存在 “quantity”（销售数量）和 “unit_price”（单价）等列，若要通过这两列计算总销售额，则可使用：

```sql
SELECT SUM(quantity * unit_price) FROM sales;
```

- **性能考虑**：
  - SUM 函数的性能受到数据量大小以及索引情况的影响。当所操作的列上存在合适的索引时，数据库在计算总和时可能会具有更快的速度。但在面对大型数据集进行求和运算时，该操作可能会消耗较多的系统资源。

## 三、AVG 函数

- **功能细节**：
  - AVG 函数用于计算指定列中数值的平均值。其计算过程是先对列中所有非 NULL 值进行求和运算，然后再将总和除以非 NULL 值的数量，从而得到平均值。
- **应用场景示例**：
  - 假设有一个名为 “student_scores” 的表，其中包含 “score”（成绩）列。
  - 若要计算学生的平均成绩，可使用以下语句：

```sql
SELECT AVG(score) FROM student_scores;
```

- 假设该表中还存在 “course_id”（课程编号）列，用于区分不同课程的成绩。若要计算每门课程的平均成绩，则可使用：

```sql
SELECT course_id, AVG(score) FROM student_scores GROUP BY course_id;
```

- **性能考虑**：
  - 与 SUM 函数类似，AVG 函数的性能同样会受到数据量大小以及索引情况的影响。并且由于 AVG 函数需要先计算总和再进行除法运算以得到平均值，对于大型数据集而言，其计算过程相对会更加复杂一些，可能会消耗更多的计算资源和时间。

## 四、MAX 和 MIN 函数

- **功能细节**：
  - MAX 函数的作用是返回指定列中的最大值，而 MIN 函数则用于返回指定列中的最小值。这两个函数能够应用于多种数据类型的列，包括数值类型、日期类型以及字符串类型等。对于字符串类型的列，MAX 和 MIN 函数是依据字典序（即字符编码顺序）来比较大小的。
- **应用场景示例**：
  - 假设有一个名为 “product_prices” 的表，其中包含 “price”（价格）列。
  - 若要查找该表中的最高价格和最低价格，可分别使用以下语句：

```sql
SELECT MAX(price) FROM product_prices;
```

```sql
SELECT MIN(price) FROM product_prices;
```

- 假设有一个名为 “employees” 的表，其中包含 “hire_date”（入职日期）列。
- 若要查找最早和最晚入职的员工日期，可分别使用：

```sql
SELECT MIN(hire_date) FROM employees;
```

```sql
SELECT MAX(hire_date) FROM employees;
```

- **性能考虑**：
  - 当所操作的列上存在索引时，MAX 和 MIN 函数的查询速度通常会比较快。这是因为数据库能够利用索引的有序性，从而快速地定位到列中的最大值和最小值。然而，如果列上没有索引，数据库可能就需要对整个列进行扫描操作，以此来找到极值，这样的话查询效率就会相对较低。
