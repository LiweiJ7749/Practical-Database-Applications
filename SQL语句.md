# **排序函数**
1. `FETCH FIRST N ROWS ONLY`

取排序后的前 N 行（严格 N 行，不包含并列的多余行）。
```sql
-- 查询工资最高的前 3 名员工（若第3名有并列，只取其中一行，未定义具体哪一行）
SCOTT@db19c>SELECT empno, ename, sal FROM emp ORDER BY sal DESC FETCH FIRST 3 ROWS ONLY;

     EMPNO ENAME             SAL
---------- ---------- ----------
      7839 KING             5000
      7788 SCOTT            3000
      7902 FORD             3000
```

2. `FETCH FIRST N ROWS WITH TIES`

取排序后的前 N 行，**并且**包含所有与第 N 行排序键值相同的额外行（即并列也一并取出）。
```sql
SCOTT@db19c>SELECT empno, ename, sal FROM emp ORDER BY sal DESC FETCH FIRST 3 ROWS WITH TIES;

     EMPNO ENAME             SAL
---------- ---------- ----------
      7839 KING             5000
      7788 SCOTT            3000
      7902 FORD             3000
```

3. `OFFSET N ROWS FETCH FIRST M ROWS ONLY`

取排序后的**第 N+1 行到第 N+M 行**

```sql
--- 取工资最高的第4名和第5名
SCOTT@db19c>select empno,ename,sal from emp order by sal desc offset 3 rows fetch first 2 rows only;

     EMPNO ENAME             SAL
---------- ---------- ----------
      7566 JONES            2975
      7698 BLAKE            2850
```

课堂作业：mysql 中使用 `limit` 语句，查找emp表工资第4第5大的工人:  

```sql
mysql> use scott;
mysql> SELECT empno, ename, sal FROM emp ORDER BY sal DESC LIMIT 2 OFFSET 3;
+-------+-------+---------+
| empno | ename | sal     |
+-------+-------+---------+
|  7566 | JONES | 2975.00 |
|  7698 | BLAKE | 2850.00 |
+-------+-------+---------+
2 rows in set (0.01 sec)
```


## **字符串函数：**

| 函数                       | 作用                         | 语法示例                     | 例子结果                                                     | 备注                                    |
| -------------------------- | ---------------------------- | ---------------------------- | ------------------------------------------------------------ | --------------------------------------- |
| `CONCAT()`                 | 连接多个字符串               | `CONCAT(str1, str2, ...)`    | `CONCAT('Hello', ' ', 'World')` → `Hello World`              | 任一参数为 `NULL` 时，结果通常为 `NULL` |
| `SUBSTR()` / `SUBSTRING()` | 截取字符串的一部分           | `SUBSTR(str, start, length)` | `SUBSTR('Adam Barr', 6, 4)` → `Barr`                         | `start` 从 1 开始计数，`length` 可省略  |
| `INSTR()`                  | 查找子串第一次出现的位置     | `INSTR(str, substr)`         | `INSTR('Adam Barr', ' ')` → `5`                              | 找不到返回 `0`                          |
| `LPAD()`                   | 在字符串左边补齐字符         | `LPAD(str, len, padstr)`     | `LPAD('123', 6, '0')` → `000123`                             | 用于左侧补零、补空格等                  |
| `RPAD()`                   | 在字符串右边补齐字符         | `RPAD(str, len, padstr)`     | `RPAD('123', 6, '0')` → `123000`                             | 和 `LPAD()` 相反                        |
| `LENGTH()`                 | 返回字符串长度               | `LENGTH(str)`                | `LENGTH('abc')` → `3`                                        | 英文通常是字符数，中文常按字节算        |
| `LEFT()`                   | 取左边若干字符               | `LEFT(str, n)`               | `LEFT('Adam Barr', 4)` → `Adam`                              | 适合取前缀                              |
| `RIGHT()`                  | 取右边若干字符               | `RIGHT(str, n)`              | `RIGHT('Adam Barr', 4)` → `Barr`                             | 适合取后缀                              |
| `TRIM()`                   | 去掉字符串两边空格或指定字符 | `TRIM(str)`                  | `TRIM('   hello   ')` → `hello`                              | 常用于清理文本                          |
| `REPLACE()`                | 替换字符串中的内容           | `REPLACE(str, old, new)`     | `REPLACE('apple banana apple', 'apple', 'orange')` → `orange banana orange` | 全部替换匹配内容                        |

*课堂作业：*

*显示buyers表中顾客的last_name*

```sql
mysql> select buyer_id,substr(buyer_name,instr(buyer_name,' ')+1)last_name from buyers;
+----------+-----------+
| buyer_id | last_name |
+----------+-----------+
|        1 | Barr      |
|        2 | Chai      |
|        3 | Coret     |
|        4 | Melia     |
+----------+-----------+
4 rows in set (0.00 sec)
```



| 函数                          | 作用                             | 常见用法                                           | 例子结果               | 说明                     |
| ----------------------------- | -------------------------------- | -------------------------------------------------- | ---------------------- | ------------------------ |
| `TRIM()`                      | 去掉字符串两边的空格，或指定字符 | `TRIM('   hello   ')`                              | `hello`                | 默认去掉两端空格         |
| `TRIM(BOTH ... FROM ...)`     | 去掉两边指定字符                 | `TRIM(BOTH 'x' FROM 'xxhelloxx')`                  | `hello`                | `BOTH` 表示两边都去      |
| `TRIM(LEADING ... FROM ...)`  | 去掉左边指定字符                 | `TRIM(LEADING 'x' FROM 'xxhello')`                 | `hello`                | 只去左边                 |
| `TRIM(TRAILING ... FROM ...)` | 去掉右边指定字符                 | `TRIM(TRAILING 'x' FROM 'helloxx')`                | `hello`                | 只去右边                 |
| `REPLACE()`                   | 把字符串中的某部分替换成新的内容 | `REPLACE('apple banana apple', 'apple', 'orange')` | `orange banana orange` | 会把所有匹配内容都替换掉 |
| `REPLACE()`                   | 替换分隔符或符号                 | `REPLACE('a-b-c', '-', '_')`                       | `a_b_c`                | 常用于清洗文本           |



练习：

```sql
SCOTT@db19c>select trim('H' from 'HelloWorldH') from dual;

TRIM('H'F
---------
elloWorld
     
SCOTT@db19c>select trim(both 'H' from 'HelloWorldH') from dual;

TRIM(BOTH'H'FROM'H
---
helloWorld
```

```sql
SCOTT@db19c>select replace('abcdef','cd','xy') from dual;

REPLACE('ABC
---
abxyef
```


## **类型转换：**

| 函数          | 作用                     | 常见写法                            | 示例                                  | 说明                               |
| ------------- | ------------------------ | ----------------------------------- | ------------------------------------- | ---------------------------------- |
| `TO_CHAR()`   | 把日期或数字转换成字符串 | `TO_CHAR(value, format)`            | `TO_CHAR(SYSDATE, 'YYYY-MM-DD')`      | 最常用的格式化函数                 |
| `TO_DATE()`   | 把字符串转换成日期       | `TO_DATE(str, format)`              | `TO_DATE('2026-05-21', 'YYYY-MM-DD')` | 字符串转日期时必须配格式           |
| `TO_NUMBER()` | 把字符串转换成数字       | `TO_NUMBER(str)`                    | `TO_NUMBER('123')`                    | 常用于字符串数值计算               |
| `CAST()`      | 显式类型转换             | `CAST(expr AS datatype)`            | `CAST('123' AS NUMBER)`               | 比较通用，标准 SQL 也支持          |
| `CONVERT()`   | 按指定字符集或类型转换   | `CONVERT(expr, type)`               | `CONVERT('abc', CHAR)`                | Oracle 里常用于字符集/类型转换     |
| `TRUNC()`     | 截断日期或数字           | `TRUNC(date)` / `TRUNC(num, n)`     | `TRUNC(SYSDATE)`                      | 日期截断到当天；数字截断不四舍五入 |
| `ROUND()`     | 数字四舍五入             | `ROUND(num, n)`                     | `ROUND(3.14159, 2)`                   | 严格说不是转换函数，但常一起考     |
| `NVL()`       | 空值替换                 | `NVL(expr, value)`                  | `NVL(salary, 0)`                      | 也常和转换一起使用                 |
| `DECODE()`    | 条件值转换               | `DECODE(expr, v1, r1, v2, r2, ...)` | `DECODE(sex, 'M', '男', 'F', '女')`   | Oracle 特有，类似简单版 `CASE`     |

------


## **数字函数：**

| 函数                     | 作用                 | 常见用法               | 例子结果 | 说明                             |
| ------------------------ | -------------------- | ---------------------- | -------- | -------------------------------- |
| `ROUND()`                | 四舍五入             | `ROUND(3.14159, 2)`    | `3.14`   | 按指定小数位四舍五入             |
| `ROUND()`                | 对整数四舍五入       | `ROUND(3.6)`           | `4`      | 不写小数位时，结果通常保留整数   |
| `TRUNCATE()` / `TRUNC()` | 截断小数，不四舍五入 | `TRUNCATE(3.14159, 2)` | `3.14`   | 直接截去多余位数                 |
| `TRUNCATE()`             | 截断到整数           | `TRUNCATE(3.9, 0)`     | `3`      | 只保留整数部分                   |
| `MOD()`                  | 取余数               | `MOD(10, 3)`           | `1`      | 表示 10 除以 3 的余数            |
| `CEIL()` / `CEILING()`   | 向上取整             | `CEIL(3.2)`            | `4`      | 只要有小数部分，就进到下一个整数 |
| `FLOOR()`                | 向下取整             | `FLOOR(3.9)`           | `3`      | 永远取不大于原数的最大整数       |

-----

## **日期函数：**

| 函数                           | 作用                             | 常见用法                                                     | 例子结果                      | 说明                                       |
| ------------------------------ | -------------------------------- | ------------------------------------------------------------ | ----------------------------- | ------------------------------------------ |
| `CURRENT_DATE`                 | 返回当前会话时区的日期和时间     | `SELECT CURRENT_DATE FROM dual;`                             | `2026-05-21 14:30:00`（示意） | 受会话时区影响，不一定等于数据库服务器时间 |
| `SYSDATE`                      | 返回数据库服务器当前日期和时间   | `SELECT SYSDATE FROM dual;`                                  | `2026-05-21 14:30:00`（示意） | 受服务器系统时间影响，Oracle 中最常用      |
| `MONTHS_BETWEEN(date1, date2)` | 计算两个日期相差多少个月         | `SELECT MONTHS_BETWEEN(SYSDATE, DATE '2025-01-01') FROM dual;` | `4.65...`                     | 返回月数，可能带小数                       |
| `ADD_MONTHS(date, n)`          | 在指定日期上加/减 n 个月         | `SELECT ADD_MONTHS(SYSDATE, 3) FROM dual;`                   | `2026-08-21`（示意）          | `n` 可为负数，表示往前减月                 |
| `NEXT_DAY(date, '星期几')`     | 返回指定日期之后的下一个某星期几 | `SELECT NEXT_DAY(SYSDATE, 'MONDAY') FROM dual;`              | `2026-05-25`（示意）          | 返回“下一个”指定星期日，不包含当天         |
| `LAST_DAY(date)`               | 返回指定日期所在月份的最后一天   | `SELECT LAST_DAY(SYSDATE) FROM dual;`                        | `2026-05-31`（示意）          | 常用于求月末日期                           |

```sql
HR@db19c>select first_name,hire_date from employees order by 2;

FIRST_NAME           HIRE_DATE
-------------------- ---------
Lex                  13-JAN-01
William              07-JUN-02
Hermann              07-JUN-02
Susan                07-JUN-02
Shelley              07-JUN-02
```


```sql
--- 要求查询empolyees表last_name, 工作年数（多少年零多少月），按年份、月份降序排序
--- LAST_NAME,YEARS,MOUNTS
SELECT
    last_name,
    TRUNC(TRUNC(MONTHS_BETWEEN(CURRENT_DATE, hire_date)) / 12) AS years,
    MOD(TRUNC(MONTHS_BETWEEN(CURRENT_DATE, hire_date)), 12)    AS months
FROM employees
ORDER BY years DESC, months DESC;

LAST_NAME                      YEARS     MONTHS
------------------------- ---------- ----------
De Haan                           25          4
Gietz                             24          0
Baer                              24          0
Mavris                            24          0
Higgins                           24          0
Faviet                            23          9
Greenberg                         23          9
Raphaely                          23          6
Kaufling                          23          1
```

练习：
```sql
--- oracle
HR@db19c>select to_char(sysdate,'yyyy-mm-dd hh:mi:ss') from dual;
TO_CHAR(SYSDATE,'YY
-------------------
2026-06-11 01:36:05


--- mysql
mysql> select date_format(sysdate(),'%y-%m-%d %h:%i:%s');
+--------------------------------------------+
| date_format(sysdate(),'%y-%m-%d %h:%i:%s') |
+--------------------------------------------+
| 26-06-11 01:39:23                          |
+--------------------------------------------+

```


| Oracle 函数                    | MySQL 对应函数                                               | 作用                     | 常见写法                                                | 说明                                                         |
| ------------------------------ | ------------------------------------------------------------ | ------------------------ | ------------------------------------------------------- | ------------------------------------------------------------ |
| `CURRENT_DATE`                 | `CURRENT_DATE` / `CURDATE()`                                 | 返回当前日期             | `SELECT CURRENT_DATE();`                                | 只返回日期，不含时间                                         |
| `SYSDATE`                      | `SYSDATE()` / `NOW()`                                        | 返回当前日期和时间       | `SELECT SYSDATE();`                                     | `NOW()` 更常用；`SYSDATE()` 也可用                           |
| `MONTHS_BETWEEN(date1, date2)` | `TIMESTAMPDIFF(MONTH, date2, date1)`                         | 计算两个日期相差的月份数 | `SELECT TIMESTAMPDIFF(MONTH, hire_date, CURRENT_DATE);` | MySQL 没有完全等价的 `MONTHS_BETWEEN`，`TIMESTAMPDIFF` 返回整数月差 |
| `ADD_MONTHS(date, n)`          | `DATE_ADD(date, INTERVAL n MONTH)`                           | 日期加减月份             | `SELECT DATE_ADD(hire_date, INTERVAL 3 MONTH);`         | `n` 可正可负                                                 |
| `NEXT_DAY(date, '星期几')`     | 无直接函数，可用 `DATE_ADD` + `WEEKDAY()`/`DAYOFWEEK()` 实现 | 求下一个指定星期几       | 见下方示例                                              | MySQL 没有原生 `NEXT_DAY`                                    |
| `LAST_DAY(date)`               | `LAST_DAY(date)`                                             | 返回当月最后一天         | `SELECT LAST_DAY(CURDATE());`                           | MySQL 和 Oracle 都有这个函数                                 |



## **oracle日期格式模型：**

| 格式模型      | 含义           | 示例结果            | 说明               |
| ------------- | -------------- | ------------------- | ------------------ |
| `YYYY`        | 四位年份       | `2026`              | 最常用年份格式     |
| `YY`          | 两位年份       | `26`                | 仅后两位           |
| `MM`          | 两位月份       | `05`                | 01~12              |
| `MON`         | 月份英文缩写   | `MAY`               | 默认英文环境       |
| `MONTH`       | 月份英文全称   | `MAY` / `MAY      ` | 会补空格到固定长度 |
| `DD`          | 两位日期       | `21`                | 01~31              |
| `D`           | 一周中的第几天 | `5`                 | 与地区设置有关     |
| `DY`          | 星期英文缩写   | `THU`               | Thuursday 缩写     |
| `DAY`         | 星期英文全称   | `THURSDAY`          | 会补空格           |
| `HH24`        | 24小时制小时   | `14`                | 00~23              |
| `HH12` / `HH` | 12小时制小时   | `02`                | 01~12              |
| `MI`          | 分钟           | `30`                | 注意不是 month     |
| `SS`          | 秒             | `45`                | 00~59              |
| `AM` / `PM`   | 上午/下午      | `PM`                | 常配 `HH12` 使用   |
| `Q`           | 季度           | `2`                 | 1~4                |
| `WW`          | 一年中的第几周 | `21`                | 按年计周           |
| `W`           | 一月中的第几周 | `3`                 | 按月计周           |
| `DDD`         | 一年中的第几天 | `141`               | 1~366              |
| `J`           | 儒略日         | `2460817`           | 较少使用           |
| `RM`          | 罗马数字月份   | `V`                 | 5月 → V            |

### 常见组合格式

| 格式                      | 示例结果              | 用途                |
| ------------------------- | --------------------- | ------------------- |
| `'YYYY-MM-DD'`            | `2026-05-21`          | 最常用日期格式      |
| `'YYYY/MM/DD'`            | `2026/05/21`          | 斜杠分隔            |
| `'YYYY-MM-DD HH24:MI:SS'` | `2026-05-21 14:30:45` | 完整时间            |
| `'MM/DD/YYYY'`            | `05/21/2026`          | 美式日期            |
| `'DD-MON-YYYY'`           | `21-MAY-2026`         | Oracle 常见默认格式 |
| `'HH24:MI:SS'`            | `14:30:45`            | 仅时间              |
| `'YYYY年MM月DD日'`        | `2026年05月21日`      | 中文显示            |



## MySQL 日期格式符表（`DATE_FORMAT`）

| MySQL 格式符 | 含义                       | 示例结果   | 对应 Oracle |
| ------------ | -------------------------- | ---------- | ----------- |
| `%Y`         | 四位年份                   | `2026`     | `YYYY`      |
| `%y`         | 两位年份                   | `26`       | `YY`        |
| `%m`         | 两位月份                   | `05`       | `MM`        |
| `%c`         | 月份（不补0）              | `5`        | 无完全对应  |
| `%b`         | 月份英文缩写               | `May`      | `MON`       |
| `%M`         | 月份英文全称               | `May`      | `MONTH`     |
| `%d`         | 两位日期                   | `21`       | `DD`        |
| `%e`         | 日期（不补0）              | `21`       | 无完全对应  |
| `%a`         | 星期英文缩写               | `Thu`      | `DY`        |
| `%W`         | 星期英文全称               | `Thursday` | `DAY`       |
| `%H`         | 24小时制小时               | `14`       | `HH24`      |
| `%h` / `%I`  | 12小时制小时               | `02`       | `HH12`      |
| `%i`         | 分钟                       | `30`       | `MI`        |
| `%s` / `%S`  | 秒                         | `45`       | `SS`        |
| `%p`         | AM / PM                    | `PM`       | `AM/PM`     |
| `%j`         | 一年中的第几天             | `141`      | `DDD`       |
| `%U`         | 一年中的第几周（周日开始） | `21`       | `WW`        |
| `%u`         | 一年中的第几周（周一开始） | `21`       | `WW`        |
| `%w`         | 星期数字（0=周日）         | `4`        | `D`         |

### MySQL 常见组合格式

| 格式字符串            | 示例结果              | 对应 Oracle               |
| --------------------- | --------------------- | ------------------------- |
| `'%Y-%m-%d'`          | `2026-05-21`          | `'YYYY-MM-DD'`            |
| `'%Y/%m/%d'`          | `2026/05/21`          | `'YYYY/MM/DD'`            |
| `'%Y-%m-%d %H:%i:%s'` | `2026-05-21 14:30:45` | `'YYYY-MM-DD HH24:MI:SS'` |
| `'%m/%d/%Y'`          | `05/21/2026`          | `'MM/DD/YYYY'`            |
| `'%d-%b-%Y'`          | `21-May-2026`         | `'DD-MON-YYYY'`           |
| `'%H:%i:%s'`          | `14:30:45`            | `'HH24:MI:SS'`            |



-----


## 空值处理函数：

| 功能             | Oracle 函数          | MySQL 对应                   | 作用                          | 示例                                  |
| ---------------- | -------------------- | ---------------------------- | ----------------------------- | ------------------------------------- |
| 空值替换         | `NVL(expr, value)`   | `IFNULL(expr, value)`        | expr 为 NULL 时返回 value     | `NVL(salary,0)`                       |
| 非空/空分别处理  | `NVL2(expr,v1,v2)`   | `IF(expr IS NOT NULL,v1,v2)` | expr 非空返回 v1，否则返回 v2 | `NVL2(comm,'有奖金','无奖金')`        |
| 返回第一个非空值 | `COALESCE(a,b,c...)` | `COALESCE(a,b,c...)`         | 返回第一个非 NULL 值          | `COALESCE(phone,mobile,'无')`         |
| 相等则返回 NULL  | `NULLIF(a,b)`        | `NULLIF(a,b)`                | a=b 返回 NULL，否则返回 a     | `NULLIF(score,0)`                     |
| 条件判断         | `CASE WHEN`          | `CASE WHEN`                  | 通用条件表达式                | `CASE WHEN salary IS NULL THEN 0 END` |
| Oracle 条件映射  | `DECODE()`           | `CASE WHEN`                  | 条件值转换                    | `DECODE(sex,'M','男','女')`           |
| 判断空值         | `IS NULL`            | `IS NULL`                    | 判断是否为空                  | `WHERE comm IS NULL`                  |

**例：**

```sql
--- 计算收入（工资+奖金）,奖金为空则表示为工资。
select empno,ename,sal,comm,nvl2(comm,sal+comm,sal) income from emp;

SCOTT@db19c>select empno,ename,sal,comm,nvl2(comm,sal+comm,sal) income from emp;

     EMPNO ENAME             SAL       COMM     INCOME
---------- ---------- ---------- ---------- ----------
      7369 SMITH             800                   800
      7499 ALLEN            1600        300       1900
      7521 WARD             1250        500       1750
      7566 JONES            2975                  2975
      7654 MARTIN           1250       1400       2650
      7698 BLAKE            2850                  2850
      7782 CLARK            2450                  2450
      7788 SCOTT            3000                  3000
      7839 KING             5000                  5000
      7844 TURNER           1500          0       1500
      7876 ADAMS            1100                  1100
      7900 JAMES             950                   950
      7902 FORD             3000                  3000
      7934 MILLER           1300                  1300

14 rows selected.

```

-----

##  SQL 条件判断函数对照表

| 功能            | Oracle 写法          | MySQL 写法                 | 作用              | 示例                                  |
| --------------- | -------------------- | -------------------------- | ----------------- | ------------------------------------- |
| 标准条件判断    | `CASE WHEN`          | `CASE WHEN`                | 通用 if-then-else | `CASE WHEN salary>5000 THEN '高' END` |
| 简单值匹配      | `CASE expr WHEN ...` | `CASE expr WHEN ...`       | 类似 switch-case  | `CASE grade WHEN 'A' THEN '优秀' END` |
| Oracle 条件映射 | `DECODE()`           | `CASE WHEN`                | Oracle 特有简写   | `DECODE(sex,'M','男','女')`           |
| MySQL 简单条件  | 无                   | `IF(condition,a,b)`        | MySQL 特有        | `IF(score>=60,'及格','不及格')`       |
| 多条件判断      | `CASE WHEN`          | `CASE WHEN`                | 多分支逻辑        | 多个 WHEN                             |
| 空值判断        | `NVL2()`             | `IF(expr IS NOT NULL,...)` | NULL 条件处理     | `NVL2(comm,'有','无`                  |

## 1）标准 `CASE WHEN`

Oracle / MySQL 通用。

### **语法**

```
CASE
    WHEN 条件1 THEN 结果1
    WHEN 条件2 THEN 结果2
    ELSE 默认结果
END
```

```
一个字段匹配多个固定值
```

### 练习

```sql
---工资最高的员工（可能多人）按 last_name 降序排列并放在最前，其他员工按 last_name 升序排列在后

SELECT last_name, salary
FROM employees
ORDER BY
    CASE WHEN salary = (SELECT MAX(salary) FROM employees) THEN 0 ELSE 1 END,
    CASE WHEN salary = (SELECT MAX(salary) FROM employees) THEN last_name END DESC, 
    last_name ASC;
    
LAST_NAME                     SALARY
------------------------- ----------
Seo                            24000
King                           24000
Gates                          24000
Abel                           11000
Ande                            6400
Atkinson                        2800
Austin                          4800
```

## 3）Oracle 的 `DECODE()`

Oracle 特有。

本质类似：

```
简化版 CASE
```

### 示例

```
SELECT
    DECODE(gender,
           'M', '男',
           'F', '女',
           '未知')
FROM users;
```

### MySQL 对应

```sql
SELECT
    CASE
        WHEN gender='M' THEN '男'
        WHEN gender='F' THEN '女'
        ELSE '未知'
    END
FROM users;
```

### 4）MySQL 的 `IF()`

MySQL 特有。

**语法**

```
IF(condition, true_value, false_value)
```

**示例**

```
SELECT
    IF(score >= 60, '及格', '不及格')
FROM students;
```

```
SELECT last_name FROM employees ORDER BY CASE WHEN salary = (SELECT MAX(salary) FROM employees) THEN 0 ELSE 1 END, CASE WHEN salary = (SELECT MAX(salary) FROM employees) THEN last_name END DESC, CASE WHEN salary <> (SELECT MAX(salary) FROM employees) THEN last_name END ASC;
```

##Oracle vs MySQL：GROUP BY / HAVING / 组函数总对照表

| 类别       | 功能                | Oracle 写法                   | MySQL 写法                 | 是否一致 | 说明                    |
| ---------- | ------------------- | ----------------------------- | -------------------------- | -------- | ----------------------- |
| 基础分组   | 单字段分组          | `GROUP BY dept`               | `GROUP BY dept`            | ✅        | 完全一致                |
| 基础分组   | 多字段分组          | `GROUP BY dept, job`          | 同左                       | ✅        |                         |
| 分组过滤   | 条件过滤（分组后）  | `HAVING COUNT(*) > 5`         | 同左                       | ✅        | HAVING必须在GROUP BY后  |
| 行过滤     | 分组前过滤          | `WHERE salary > 1000`         | 同左                       | ✅        | WHERE不能用聚合函数     |
| 计数       | 统计行数            | `COUNT(*)`                    | 同左                       | ✅        | 包含NULL行              |
| 计数       | 非空计数            | `COUNT(col)`                  | 同左                       | ✅        | NULL不计入              |
| 去重计数   | 唯一值统计          | `COUNT(DISTINCT col)`         | 同左                       | ✅        |                         |
| 求和       | 数值求和            | `SUM(col)`                    | 同左                       | ✅        | NULL忽略                |
| 平均值     | 平均数              | `AVG(col)`                    | 同左                       | ✅        | NULL忽略                |
| 最大值     | 最大值              | `MAX(col)`                    | 同左                       | ✅        |                         |
| 最小值     | 最小值              | `MIN(col)`                    | 同左                       | ✅        |                         |
| 分组扩展   | 小计/汇总（ROLLUP） | `GROUP BY ROLLUP(a,b)`        | `GROUP BY a,b WITH ROLLUP` | ❌        | MySQL语法不同           |
| 分组扩展   | 多维汇总（CUBE）    | `GROUP BY CUBE(a,b)`          | ❌ 不支持                   | ❌        | MySQL需UNION模拟        |
| 分组扩展   | 多组合分组          | `GROUP BY GROUPING SETS(...)` | ❌ 不支持                   | ❌        | MySQL需拆SQL            |
| NULL处理   | NULL分组            | NULL作为一组                  | 同左                       | ✅        |                         |
| 非分组字段 | SELECT限制          | 必须在GROUP BY中              | 早期宽松，5.7+严格         | ⚠️        | MySQL历史差异           |
| 排序       | 分组结果排序        | `ORDER BY`                    | 同左                       | ✅        | GROUP BY不保证排序      |
| 分组后筛选 | 聚合条件过滤        | `HAVING SUM(salary)>1000`     | 同左                       | ✅        | 可用别名（MySQL更宽松） |

```sh
[oracle@ocpjlw demo]$ vim st.dat
[oracle@ocpjlw demo]$ export ORACLE_SID=jlw
[oracle@ocpjlw demo]$ echo $ORACLE_SID
jlw
[oracle@ocpjlw demo]$ vim st.ctl
[oracle@ocpjlw demo]$ sqlldr scott/tiger control=st.ctl
```



```sql
SCOTT@jlw>select * from st;

NAME                 SUBJECT                   SCORE
-------------------- -------------------- ----------
zhangsan             chinese                      90
zhangsan             maths                       100
zhangsan             english                      96
lisi                 chinese                      95
lisi                 maths                        98
lisi                 english                      94

6 rows selected.

----- 执行 行转列
---- oracle


SCOTT@jlw> SELECT name, MAX(CASE WHEN subject = 'chinese' THEN score END) AS chinese, MAX(CASE WHEN subject = 'maths' THEN score END) AS maths, MAX(CASE WHEN subject = 'english' THEN score END) AS english FROM st GROUP BY name;

NAME                    CHINESE      MATHS    ENGLISH
-------------------- ---------- ---------- ----------
lisi                         95         98         94
zhangsan                     90        100         96


---- mysql
mysql> select name,
    -> sum(case when subject='chinese' then score else 0 end) chinese,
    -> sum(case when subject='maths' then score else 0 end) maths,
    -> sum(case when subject='english' then score else 0 end) english
    -> from st
    -> group by name;
+----------+---------+-------+---------+
| name     | chinese | maths | english |
+----------+---------+-------+---------+
| lisi     |      95 |    98 |      94 |
| zhangsan |      90 |   100 |      96 |
+----------+---------+-------+---------+
2 rows in set (0.01 sec)

```



## Oracle vs MySQL 连接（JOIN）语法总表

| 连接类型             | 功能           | Oracle 写法                                 | MySQL 写法 | 是否一致 | 说明               |
| -------------------- | -------------- | ------------------------------------------- | ---------- | -------- | ------------------ |
| 内连接（INNER JOIN） | 只返回匹配行   | `SELECT * FROM A INNER JOIN B ON A.id=B.id` | 同左       | ✅        | 最标准写法         |
| 内连接（旧写法）     | 隐式内连接     | `SELECT * FROM A, B WHERE A.id=B.id`        | 同左       | ⚠️        | 不推荐，容易出错   |
| 左外连接             | 左表全保留     | `SELECT * FROM A LEFT JOIN B ON ...`        | 同左       | ✅        | 常用               |
| 右外连接             | 右表全保留     | `RIGHT JOIN`                                | 同左       | ✅        | 较少用             |
| 全外连接             | 左右全保留     | `FULL OUTER JOIN`                           | ❌ 不支持   | ❌        | MySQL需用UNION模拟 |
| 交叉连接（笛卡尔积） | 全组合         | `CROSS JOIN` 或 `FROM A,B`                  | 同左       | ⚠️        | 会产生 m×n 行      |
| 自连接               | 表与自身连接   | `FROM emp e1 JOIN emp e2 ON ...`            | 同左       | ✅        | 常用于层级/比较    |
| 多表连接             | 三表及以上连接 | `A JOIN B ON ... JOIN C ON ...`             | 同左       | ✅        | 链式JOIN           |
| 多表隐式连接         | 多表逗号连接   | `FROM A, B, C WHERE ...`                    | 同左       | ⚠️        | 不推荐             |
| 外连接旧语法         | Oracle专用写法 | `A.col = B.col(+)`                          | ❌ 不支持   | ❌        | Oracle特有         |
| 多条件连接           | 多字段关联     | `ON A.id=B.id AND A.type=B.type`            | 同左       | ✅        | 常用               |

### 一、标准内连接（推荐写法）

```sql
SELECT *
FROM emp e
INNER JOIN dept d
ON e.dept_id = d.dept_id;
```

Oracle / MySQL 完全一致

------

###  二、左外连接（LEFT JOIN）

```sql
SELECT *
FROM emp e
LEFT JOIN dept d
ON e.dept_id = d.dept_id;
```

保留 emp 所有行

------

### 三、右外连接（RIGHT JOIN）

```sql
SELECT *
FROM emp e
RIGHT JOIN dept d
ON e.dept_id = d.dept_id;
```

保留 dept 所有行

------

###  四、全外连接（Oracle 特有）

#### Oracle：

```sql
SELECT *
FROM emp e
FULL OUTER JOIN dept d
ON e.dept_id = d.dept_id;
```

#### MySQL（模拟）：

```sql
SELECT * FROM emp e
LEFT JOIN dept d ON e.dept_id = d.dept_id
UNION
SELECT * FROM emp e
RIGHT JOIN dept d ON e.dept_id = d.dept_id;
```

------

### 五、自连接（经典面试题）

#### 例：员工 + 经理

```
SELECT e.name AS employee, m.name AS manager
FROM emp e
JOIN emp m
ON e.manager_id = m.emp_id;
```

 一张表当两张用

------

### 六、多表连接（三表及以上）

```
SELECT e.name, d.dname, s.salary
FROM emp e
JOIN dept d ON e.dept_id = d.dept_id
JOIN salary s ON e.emp_id = s.emp_id;
```

链式 JOIN（推荐）

------

### 七、笛卡尔积（CROSS JOIN）

```
SELECT *
FROM emp
CROSS JOIN dept;
```

或：

```
SELECT *
FROM emp, dept;
```

会产生 m × n 行

------

### 八、Oracle 特殊旧写法（重点了解）

#### 左外连接（旧 Oracle）

```
SELECT *
FROM emp e, dept d
WHERE e.dept_id = d.dept_id(+);
```

 `(+)` 表示右表可为空

MySQL 不支持

## **特殊备注：**

char变量1||‘ ’||char变量2 可实现连接字符串

```sql
SELECT 
    e.last_name,
    e.salary,
    e.department_id,
    d.salavg
FROM employees e
JOIN (
    SELECT 
        department_id,
        AVG(salary) AS salavg
    FROM employees
    GROUP BY department_id
) d
ON e.department_id = d.department_id
WHERE e.salary > d.salavg;

SELECT e.last_name, e.salary, e.department_id, d.salavg FROM employees e JOIN (SELECT  department_id, AVG(salary) AS salavg FROM employees GROUP BY department_id) d ON e.department_id = d.department_id WHERE e.salary > d.salavg;
```



## Oracle vs MySQL：关联查询 + 子查询语法总表

### 一、关联查询（JOIN）

| 类型         | 功能       | Oracle 写法                | MySQL 写法 | 是否一致 | 说明         |
| ------------ | ---------- | -------------------------- | ---------- | -------- | ------------ |
| 内连接       | 取交集     | `A JOIN B ON A.id=B.id`    | 同左       | ✅        | 标准写法     |
| 内连接旧写法 | 隐式连接   | `FROM A,B WHERE A.id=B.id` | 同左       | ⚠️        | 不推荐       |
| 左外连接     | 左表全保留 | `LEFT JOIN`                | 同左       | ✅        | 常用         |
| 右外连接     | 右表全保留 | `RIGHT JOIN`               | 同左       | ✅        | 较少用       |
| 全外连接     | 全部保留   | `FULL OUTER JOIN`          | ❌不支持    | ❌        | MySQL需UNION |
| 自连接       | 表连自己   | `A e JOIN A m ON ...`      | 同左       | ✅        | 层级/比较    |
| 多表连接     | 多表JOIN   | `A JOIN B JOIN C`          | 同左       | ✅        | 链式写法     |
| 笛卡尔积     | 全组合     | `CROSS JOIN`               | 同左       | ⚠️        | 会爆炸行数   |

------

### 二、子查询（Subquery）

------

#### 1️⃣ 标量子查询（返回单值）

| 功能     | Oracle                                         | MySQL | 说明 |
| -------- | ---------------------------------------------- | ----- | ---- |
| 单值查询 | `WHERE salary > (SELECT AVG(salary) FROM emp)` | 同左  | 常见 |

------

#### 2️⃣ 多行子查询（IN / ANY / ALL）

| 类型       | Oracle 写法                                   | MySQL 写法 | 说明         |
| ---------- | --------------------------------------------- | ---------- | ------------ |
| IN 子查询  | `WHERE dept_id IN (SELECT dept_id FROM dept)` | 同左       | 常用         |
| ANY 子查询 | `> ANY (SELECT salary ...)`                   | 同左       | 至少满足一个 |
| ALL 子查询 | `> ALL (SELECT salary ...)`                   | 同左       | 必须全部满足 |

------

#### 3️⃣ EXISTS 子查询（相关子查询）

| 功能       | Oracle                                        | MySQL | 说明     |
| ---------- | --------------------------------------------- | ----- | -------- |
| 存在性判断 | `WHERE EXISTS (SELECT 1 FROM dept WHERE ...)` | 同左  | 性能常用 |

------

#### 4️⃣ NOT EXISTS（反向存在）

| 功能       | Oracle                   | MySQL | 说明       |
| ---------- | ------------------------ | ----- | ---------- |
| 不存在判断 | `WHERE NOT EXISTS (...)` | 同左  | 常用于差集 |

------

#### 5️⃣ FROM 子查询（派生表）

| 功能       | Oracle                | MySQL | 说明       |
| ---------- | --------------------- | ----- | ---------- |
| 子查询当表 | `FROM (SELECT ...) t` | 同左  | 必须起别名 |

------

#### 6️⃣ SELECT 子查询（标量列）

| 功能       | Oracle                                    | MySQL | 说明     |
| ---------- | ----------------------------------------- | ----- | -------- |
| 列中子查询 | `SELECT name, (SELECT MAX(...) FROM ...)` | 同左  | 每行执行 |

------

#### 7️⃣ UPDATE / DELETE 子查询

| 操作   | Oracle                                   | MySQL | 说明           |
| ------ | ---------------------------------------- | ----- | -------------- |
| UPDATE | `UPDATE emp SET salary=(SELECT...)`      | 同左  | 常用于批量更新 |
| DELETE | `DELETE FROM emp WHERE dept_id IN (...)` | 同左  | 常用           |

------

### 三、JOIN vs 子查询（核心对比）

| 对比点         | JOIN  | 子查询 |
| -------------- | ----- | ------ |
| 可读性         | ⭐⭐⭐⭐  | ⭐⭐⭐    |
| 性能（大数据） | ⭐⭐⭐⭐⭐ | ⭐⭐⭐    |
| 多表关联       | 强    | 一般   |
| 复杂条件拆解   | 一般  | 强     |
| 面试使用       | 常用  | 必考   |

------

### 四、经典等价写法对比（重点）

------

####  1. JOIN写法

```sql
SELECT e.employee_id, e.first_name, e.last_name, e.salary, e.department_id,
       d.avg_salary AS dept_avg_salary
FROM employees e
JOIN (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    WHERE department_id IS NOT NULL
    GROUP BY department_id
) d ON e.department_id = d.department_id
WHERE e.salary > d.avg_salary
ORDER BY e.department_id, e.salary DESC;
```

------

####  2. 子查询写法

```
SELECT name, salary
FROM emp e
WHERE salary > (
    SELECT AVG(salary)
    FROM emp
    WHERE dept_id = e.dept_id
);
```

这是**相关子查询**

select last_name from employees where employee_id not in manager_id;

```
SELECT last_name, salary, department_id FROM employees e WHERE salary > (SELECT AVG(salary) FROM employees WHERE department_id = e.department_id);
```
