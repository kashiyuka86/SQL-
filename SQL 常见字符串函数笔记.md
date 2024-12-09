# SQL 常见字符串函数笔记

## 一、字符串连接函数

1. **CONCAT**：用于连接两个或多个字符串。
   
   ```sql
   SELECT CONCAT('Hello', 'World');
   -- 结果为 'HelloWorld'
   ```
2. **CONCAT_WS**：可指定连接字符串时的分隔符。
   
   ```sql
   SELECT CONCAT_WS('-', 'John', 'Doe');
   -- 结果为 'John-Doe'
   ```

## 二、字符串截取函数

1. **SUBSTRING**：从指定字符串中截取一部分。
   
   ```sql
   SELECT SUBSTRING('HelloWorld', 1, 5);
   -- 结果为 'Hello'，从第 1 个字符开始，截取 5 个字符
   ```
2. **LEFT**：从字符串左边开始截取指定长度的子字符串。
   
   ```sql
   SELECT LEFT('HelloWorld', 5);
   -- 结果为 'Hello'
   ```
3. **RIGHT**：从字符串右边开始截取指定长度的子字符串。
   
   ```sql
   SELECT RIGHT('HelloWorld', 5);
   -- 结果为 'World'
   ```

## 三、字符串替换函数

1. **REPLACE**：将字符串中的指定子字符串替换为另一个字符串。
   
   ```sql
   SELECT REPLACE('HelloWorld', 'World', 'SQL');
   -- 结果为 'HelloSQL'
   ```

## 四、字符串长度函数

1. **LENGTH**：返回字符串的长度。
   
   ```sql
   SELECT LENGTH('HelloWorld');
   -- 结果为 10
   ```

## 五、字符串查找函数

1. **LOCATE**：查找子字符串在字符串中的位置。
   
   ```sql
   SELECT LOCATE('World', 'HelloWorld');
   -- 结果为 6，从 1 开始计数
   ```
2. **POSITION**：功能与 LOCATE 类似。
   
   ```sql
   SELECT POSITION('World' IN 'HelloWorld');
   -- 结果为 6
   ```

## 六、字符串转换函数

1. **UPPER**：将字符串转换为大写。
   
   ```sql
   SELECT UPPER('HelloWorld');
   -- 结果为 'HELLOWORLD'
   ```
2. **LOWER**：将字符串转换为小写。
   
   ```sql
   SELECT LOWER('HelloWorld');
   -- 结果为 'helloworld'
   ```

## 七、字符串填充函数

1. **LPAD**：在字符串左边填充指定字符达到指定长度。
   
   ```sql
   SELECT LPAD('Hello', 10, '*');
   -- 结果为 '*****Hello'
   ```
2. **RPAD**：在字符串右边填充指定字符达到指定长度。
   
   ```sql
   SELECT RPAD('Hello', 10, '*');
   -- 结果为 'Hello*****'
   ```

## 八、字符串去除空格函数

1. **TRIM**：去除字符串两端的空格。
   
   ```sql
   SELECT TRIM(' Hello ');
   -- 结果为 'Hello'
   ```
2. **LTRIM**：去除字符串左边的空格。
   
   ```sql
   SELECT LTRIM(' Hello');
   -- 结果为 'Hello'
   ```
3. **RTRIM**：去除字符串右边的空格。
   
   ```sql
   SELECT RTRIM('Hello ');
   -- 结果为 'Hello'
   ```
