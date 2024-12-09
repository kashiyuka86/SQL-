# SQL 日期与时间函数笔记

## 一、获取当前日期时间函数

- **CURRENT_DATE**：返回当前日期，格式为'YYYY-MM-DD'。
  
  ```sql
  SELECT CURRENT_DATE;
  -- 例如返回当前日期，如 '2024-12-09'（实际运行时的当前日期）
  ```

- **CURRENT_TIME**：返回当前时间，格式为'HH:MM:SS'。
  
  ```sql
  SELECT CURRENT_TIME;
  -- 例如返回当前时间，如 '15:30:00'（实际运行时的当前时间）
  ```

- **CURRENT_TIMESTAMP**：返回当前日期时间，格式为'YYYY-MM-DD HH:MM:SS'。
  
  ```sql
  SELECT CURRENT_TIMESTAMP;
  -- 例如返回 '2024-12-09 15:30:00'（实际运行时的当前日期时间）
  ```

- **GETDATE()（在 SQL Server 中）**：返回当前数据库系统日期和时间，包含年、月、日、时、分、秒和毫秒。
  
  ```sql
  SELECT GETDATE();
  -- 例如返回 '2024-12-09 15:30:00.123'（实际日期时间和毫秒部分）
  ```

- **SYSDATE（在 Oracle 中）**：返回当前日期和时间，包括服务器的日期和时间。
  
  ```sql
  SELECT SYSDATE FROM dual;
  -- 返回服务器当前日期时间，格式类似 '09-DEC-24 15.30.00'（依 Oracle 配置可能不同）
  ```

- **NOW()（在 MySQL 中）**：返回当前日期和时间，精确到秒。
  
  ```sql
  SELECT NOW();
  -- 例如返回 '2024-12-09 15:30:00'
  ```

## 二、日期时间运算函数

- **DATE_ADD(date, INTERVAL value unit)**：在日期`date`上添加指定的时间间隔。
  
  ```sql
  SELECT DATE_ADD('2024-12-01', INTERVAL 5 DAY);
  -- 结果为 '2024-12-06'，在给定日期上加 5 天
  SELECT DATE_ADD('2024-12-01 12:00:00', INTERVAL 2 HOUR);
  -- 结果为 '2024-12-01 14:00:00'，在给定时间上加 2 小时
  ```

- **DATE_SUB(date, INTERVAL value unit)**：在日期`date`上减去指定的时间间隔。
  
  ```sql
  SELECT DATE_SUB('2024-12-09', INTERVAL 3 DAY);
  -- 结果为 '2024-12-06'，从给定日期减去 3 天
  ```

- **ADD_MONTHS(date, n)（在 Oracle 中）**：在给定日期`date`基础上增加`n`个月。
  
  ```sql
  SELECT ADD_MONTHS('2024-01-31', 1);
  -- 结果为 '2024-02-29'（考虑闰年等月份天数变化）
  ```

- **DATE_TRUNC(unit, timestamp)（在 PostgreSQL 中）**：按照指定的时间单位`unit`（如 year、month、day、hour 等）截断时间戳`timestamp`。
  
  ```sql
  SELECT DATE_TRUNC('month', '2024-12-15 12:30:00');
  -- 结果为 '2024-12-01 00:00:00'，将日期时间截断到月份开始
  ```

## 三、日期时间差值函数

- **DATEDIFF(date1, date2)**：计算两个日期`date1`与`date2`之间的差值（天数）。
  
  ```sql
  SELECT DATEDIFF('2024-12-10', '2024-12-05');
  -- 结果为 5，计算两个日期相差的天数
  ```

- **TIMEDIFF(time1, time2)**：计算两个时间`time1`与`time2`之间的差值（时间间隔）。
  
  ```sql
  SELECT TIMEDIFF('15:30:00', '13:00:00');
  -- 结果为 '02:30:00'，计算两个时间相差的时间间隔
  ```

- **TIMESTAMPDIFF(unit, datetime1, datetime2)（在 MySQL 中）**：计算两个日期时间`datetime1`和`datetime2`之间的差值，单位由`unit`指定（如 SECOND、MINUTE、HOUR、DAY 等）。
  
  ```sql
  SELECT TIMESTAMPDIFF(DAY, '2024-12-01', '2024-12-10');
  -- 结果为 9，计算两个日期相差的天数
  ```

## 四、日期时间提取函数

- **EXTRACT(unit FROM date_time)**：从日期时间`date_time`中提取指定的部分（如年、月、日、时、分、秒等）。
  
  ```sql
  SELECT EXTRACT(YEAR FROM '2024-12-09 15:30:00');
  -- 结果为 2024，提取年份
  SELECT EXTRACT(MONTH FROM '2024-12-09 15:30:00');
  -- 结果为 12，提取月份
  SELECT EXTRACT(DAY FROM '2024-12-09 15:30:00');
  -- 结果为 9，提取日
  SELECT EXTRACT(HOUR FROM '2024-12-09 15:30:00');
  -- 结果为 15，提取小时
  ```

- **YEAR(date)、MONTH(date)、DAY(date)（在 SQL Server 等中）**：分别从日期`date`中提取年、月、日部分。
  
  ```sql
  SELECT YEAR('2024-12-09'), MONTH('2024-12-09'), DAY('2024-12-09');
  -- 结果分别为 2024、12、9
  ```

- **QUARTER(date)（在 SQL Server 等中）**：返回日期`date`所属的季度（1 - 4）。
  
  ```sql
  SELECT QUARTER('2024-12-09');
  -- 结果为 4，因为 12 月属于第四季度
  ```

## 五、日期时间格式化函数

- **DATE_FORMAT(date, format)**：按照指定的格式`format`对日期`date`进行格式化。
  
  ```sql
  SELECT DATE_FORMAT('2024-12-09', '%Y-%m-%d');
  -- 结果仍为 '2024-12-09'，这里格式未改变
  SELECT DATE_FORMAT('2024-12-09', '%W, %d %M %Y');
  -- 结果可能为 'Monday, 09 December 2024'，根据格式转换
  ```

- **TIME_FORMAT(time, format)**：按照指定的格式`format`对时间`time`进行格式化。
  
  ```sql
  SELECT TIME_FORMAT('15:30:00', '%H:%i:%s');
  -- 结果仍为 '15:30:00'，格式未改变
  SELECT TIME_FORMAT('15:30:00', '%r');
  -- 结果可能为 '03:30:00 PM'，根据格式转换
  ```

- **TO_CHAR(date, format)（在 Oracle 中）**：将日期`date`按照指定的格式`format`转换为字符型。
  
  ```sql
  SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS');
  -- 根据系统时间返回格式化后的字符串，如 '2024-12-09 15:30:00'
  ```

- **STR_TO_DATE(str, format)（在 MySQL 中）**：将字符串`str`按照指定的格式`format`转换为日期型。
  
  ```sql
  SELECT STR_TO_DATE('09 - Dec - 2024', '%d - %b - %Y');
  -- 结果为 '2024-12-09'，将字符串转换为日期格式
  ```

## 六、时区相关函数（部分数据库支持）

- **CONVERT_TZ(datetime, from_tz, to_tz)（在 MySQL 中）**：将日期时间`datetime`从`from_tz`时区转换为`to_tz`时区。
  
  ```sql
  -- 假设数据库配置了相关时区信息
  SELECT CONVERT_TZ('2024-12-09 15:30:00', 'UTC', 'America/New_York');
  -- 将 UTC 时间转换为美国纽约时间
  ```
  
  这些函数在处理与日期时间相关的数据（如订单时间、事件时间、统计周期等）时非常有用，可以帮助进行数据筛选、计算时间间隔、格式化输出等多种操作。不同的数据库系统可能在函数名称、参数格式和功能细节上略有差异，在实际使用时需要参考相应数据库的文档。
