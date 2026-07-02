# oracle_linux软件

##启动函数

nautilus：文件资源管理器

mousepad：文本编辑器

sqldeveloper: 

先杀mysql：systemctl stop mysql

mysql启动：service mysql start

mysql -uroot -p

查看数据库：show databases; 

打开数据库：use <数据库名称>; 

查看表：show tables;

export DISPLAY=192.168.11.1:0.0

sqlplus要startup;

lsnrctl start起监听（oracle账户下）

col note for a60;（调整格式）

conn scott/tiger

conn hr/hr

dbca (创建数据库)

netmgr (配置监听)

/usr/local/mysql/bin/ (mysql路径)

##**字符串函数：**

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



| 函数                          | 作用                             | 常见用法                                           | 例子结果               | 说明                     |
| ----------------------------- | -------------------------------- | -------------------------------------------------- | ---------------------- | ------------------------ |
| `TRIM()`                      | 去掉字符串两边的空格，或指定字符 | `TRIM('   hello   ')`                              | `hello`                | 默认去掉两端空格         |
| `TRIM(BOTH ... FROM ...)`     | 去掉两边指定字符                 | `TRIM(BOTH 'x' FROM 'xxhelloxx')`                  | `hello`                | `BOTH` 表示两边都去      |
| `TRIM(LEADING ... FROM ...)`  | 去掉左边指定字符                 | `TRIM(LEADING 'x' FROM 'xxhello')`                 | `hello`                | 只去左边                 |
| `TRIM(TRAILING ... FROM ...)` | 去掉右边指定字符                 | `TRIM(TRAILING 'x' FROM 'helloxx')`                | `hello`                | 只去右边                 |
| `REPLACE()`                   | 把字符串中的某部分替换成新的内容 | `REPLACE('apple banana apple', 'apple', 'orange')` | `orange banana orange` | 会把所有匹配内容都替换掉 |
| `REPLACE()`                   | 替换分隔符或符号                 | `REPLACE('a-b-c', '-', '_')`                       | `a_b_c`                | 常用于清洗文本           |

##**数字函数：**

| 函数                     | 作用                 | 常见用法               | 例子结果 | 说明                             |
| ------------------------ | -------------------- | ---------------------- | -------- | -------------------------------- |
| `ROUND()`                | 四舍五入             | `ROUND(3.14159, 2)`    | `3.14`   | 按指定小数位四舍五入             |
| `ROUND()`                | 对整数四舍五入       | `ROUND(3.6)`           | `4`      | 不写小数位时，结果通常保留整数   |
| `TRUNCATE()` / `TRUNC()` | 截断小数，不四舍五入 | `TRUNCATE(3.14159, 2)` | `3.14`   | 直接截去多余位数                 |
| `TRUNCATE()`             | 截断到整数           | `TRUNCATE(3.9, 0)`     | `3`      | 只保留整数部分                   |
| `MOD()`                  | 取余数               | `MOD(10, 3)`           | `1`      | 表示 10 除以 3 的余数            |
| `CEIL()` / `CEILING()`   | 向上取整             | `CEIL(3.2)`            | `4`      | 只要有小数部分，就进到下一个整数 |
| `FLOOR()`                | 向下取整             | `FLOOR(3.9)`           | `3`      | 永远取不大于原数的最大整数       |

##**日期函数：**

| 函数                           | 作用                             | 常见用法                                                     | 例子结果                      | 说明                                       |
| ------------------------------ | -------------------------------- | ------------------------------------------------------------ | ----------------------------- | ------------------------------------------ |
| `CURRENT_DATE`                 | 返回当前会话时区的日期和时间     | `SELECT CURRENT_DATE FROM dual;`                             | `2026-05-21 14:30:00`（示意） | 受会话时区影响，不一定等于数据库服务器时间 |
| `SYSDATE`                      | 返回数据库服务器当前日期和时间   | `SELECT SYSDATE FROM dual;`                                  | `2026-05-21 14:30:00`（示意） | 受服务器系统时间影响，Oracle 中最常用      |
| `MONTHS_BETWEEN(date1, date2)` | 计算两个日期相差多少个月         | `SELECT MONTHS_BETWEEN(SYSDATE, DATE '2025-01-01') FROM dual;` | `4.65...`                     | 返回月数，可能带小数                       |
| `ADD_MONTHS(date, n)`          | 在指定日期上加/减 n 个月         | `SELECT ADD_MONTHS(SYSDATE, 3) FROM dual;`                   | `2026-08-21`（示意）          | `n` 可为负数，表示往前减月                 |
| `NEXT_DAY(date, '星期几')`     | 返回指定日期之后的下一个某星期几 | `SELECT NEXT_DAY(SYSDATE, 'MONDAY') FROM dual;`              | `2026-05-25`（示意）          | 返回“下一个”指定星期日，不包含当天         |
| `LAST_DAY(date)`               | 返回指定日期所在月份的最后一天   | `SELECT LAST_DAY(SYSDATE) FROM dual;`                        | `2026-05-31`（示意）          | 常用于求月末日期                           |

SELECT
    last_name,
    TRUNC(TRUNC(MONTHS_BETWEEN(CURRENT_DATE, hire_date)) / 12) AS years,
    MOD(TRUNC(MONTHS_BETWEEN(CURRENT_DATE, hire_date)), 12)    AS months
FROM employees
ORDER BY years DESC, months DESC;

| Oracle 函数                    | MySQL 对应函数                                               | 作用                     | 常见写法                                                | 说明                                                         |
| ------------------------------ | ------------------------------------------------------------ | ------------------------ | ------------------------------------------------------- | ------------------------------------------------------------ |
| `CURRENT_DATE`                 | `CURRENT_DATE` / `CURDATE()`                                 | 返回当前日期             | `SELECT CURRENT_DATE();`                                | 只返回日期，不含时间                                         |
| `SYSDATE`                      | `SYSDATE()` / `NOW()`                                        | 返回当前日期和时间       | `SELECT SYSDATE();`                                     | `NOW()` 更常用；`SYSDATE()` 也可用                           |
| `MONTHS_BETWEEN(date1, date2)` | `TIMESTAMPDIFF(MONTH, date2, date1)`                         | 计算两个日期相差的月份数 | `SELECT TIMESTAMPDIFF(MONTH, hire_date, CURRENT_DATE);` | MySQL 没有完全等价的 `MONTHS_BETWEEN`，`TIMESTAMPDIFF` 返回整数月差 |
| `ADD_MONTHS(date, n)`          | `DATE_ADD(date, INTERVAL n MONTH)`                           | 日期加减月份             | `SELECT DATE_ADD(hire_date, INTERVAL 3 MONTH);`         | `n` 可正可负                                                 |
| `NEXT_DAY(date, '星期几')`     | 无直接函数，可用 `DATE_ADD` + `WEEKDAY()`/`DAYOFWEEK()` 实现 | 求下一个指定星期几       | 见下方示例                                              | MySQL 没有原生 `NEXT_DAY`                                    |
| `LAST_DAY(date)`               | `LAST_DAY(date)`                                             | 返回当月最后一天         | `SELECT LAST_DAY(CURDATE());`                           | MySQL 和 Oracle 都有这个函数                                 |

##**类型转换：**

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

##**oracle日期格式模型：**

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

###常见组合格式

| 格式                      | 示例结果              | 用途                |
| ------------------------- | --------------------- | ------------------- |
| `'YYYY-MM-DD'`            | `2026-05-21`          | 最常用日期格式      |
| `'YYYY/MM/DD'`            | `2026/05/21`          | 斜杠分隔            |
| `'YYYY-MM-DD HH24:MI:SS'` | `2026-05-21 14:30:45` | 完整时间            |
| `'MM/DD/YYYY'`            | `05/21/2026`          | 美式日期            |
| `'DD-MON-YYYY'`           | `21-MAY-2026`         | Oracle 常见默认格式 |
| `'HH24:MI:SS'`            | `14:30:45`            | 仅时间              |
| `'YYYY年MM月DD日'`        | `2026年05月21日`      | 中文显示            |

##MySQL 日期格式符表（`DATE_FORMAT`）

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

###MySQL 常见组合格式

| 格式字符串            | 示例结果              | 对应 Oracle               |
| --------------------- | --------------------- | ------------------------- |
| `'%Y-%m-%d'`          | `2026-05-21`          | `'YYYY-MM-DD'`            |
| `'%Y/%m/%d'`          | `2026/05/21`          | `'YYYY/MM/DD'`            |
| `'%Y-%m-%d %H:%i:%s'` | `2026-05-21 14:30:45` | `'YYYY-MM-DD HH24:MI:SS'` |
| `'%m/%d/%Y'`          | `05/21/2026`          | `'MM/DD/YYYY'`            |
| `'%d-%b-%Y'`          | `21-May-2026`         | `'DD-MON-YYYY'`           |
| `'%H:%i:%s'`          | `14:30:45`            | `'HH24:MI:SS'`            |

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

**例：**select empno,ename,sal,comm,nvl2(comm,sal+comm,sal) income from emp;

##SQL 条件判断函数对照表

| 功能            | Oracle 写法          | MySQL 写法                 | 作用              | 示例                                  |
| --------------- | -------------------- | -------------------------- | ----------------- | ------------------------------------- |
| 标准条件判断    | `CASE WHEN`          | `CASE WHEN`                | 通用 if-then-else | `CASE WHEN salary>5000 THEN '高' END` |
| 简单值匹配      | `CASE expr WHEN ...` | `CASE expr WHEN ...`       | 类似 switch-case  | `CASE grade WHEN 'A' THEN '优秀' END` |
| Oracle 条件映射 | `DECODE()`           | `CASE WHEN`                | Oracle 特有简写   | `DECODE(sex,'M','男','女')`           |
| MySQL 简单条件  | 无                   | `IF(condition,a,b)`        | MySQL 特有        | `IF(score>=60,'及格','不及格')`       |
| 多条件判断      | `CASE WHEN`          | `CASE WHEN`                | 多分支逻辑        | 多个 WHEN                             |
| 空值判断        | `NVL2()`             | `IF(expr IS NOT NULL,...)` | NULL 条件处理     | `NVL2(comm,'有','无`                  |

##1）标准 `CASE WHEN`

Oracle / MySQL 通用。

###**语法**

```
CASE
    WHEN 条件1 THEN 结果1
    WHEN 条件2 THEN 结果2
    ELSE 默认结果
END
```

###**示例**

```
SELECT
    last_name,
    CASE
        WHEN salary >= 10000 THEN '高薪'
        WHEN salary >= 5000 THEN '中薪'
        ELSE '低薪'
    END AS level
FROM employees;
```

###**结果示例**

| last_name | level |
| --------- | ----- |
| King      | 高薪  |
| Scott     | 中薪  |
| Smith     | 低薪  |

##2）简单 CASE（类似 switch）

###适合：

```
一个字段匹配多个固定值
```

###语法

```
CASE expr
    WHEN value1 THEN result1
    WHEN value2 THEN result2
    ELSE result
END
```

###示例

```
SELECT
    CASE department_id
        WHEN 10 THEN '财务部'
        WHEN 20 THEN '研发部'
        ELSE '其他部门'
    END
FROM employees;
```

##3）Oracle 的 `DECODE()`

Oracle 特有。

本质类似：

```
简化版 CASE
```

###示例

```
SELECT
    DECODE(gender,
           'M', '男',
           'F', '女',
           '未知')
FROM users;
```

###MySQL 对应

```
SELECT
    CASE
        WHEN gender='M' THEN '男'
        WHEN gender='F' THEN '女'
        ELSE '未知'
    END
FROM users;
```

###4）MySQL 的 `IF()`

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

SELECT last_name FROM employees ORDER BY CASE WHEN salary = (SELECT MAX(salary) FROM employees) THEN 0 ELSE 1 END, CASE WHEN salary = (SELECT MAX(salary) FROM employees) THEN last_name END DESC, CASE WHEN salary <> (SELECT MAX(salary) FROM employees) THEN last_name END ASC;

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

sqlldr scott/tiger control=st.ctl



SELECT name, MAX(CASE WHEN subject = 'chinese' THEN score END) AS chinese, MAX(CASE WHEN subject = 'maths' THEN score END) AS maths, MAX(CASE WHEN subject = 'english' THEN score END) AS english FROM st GROUP BY name;

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

###一、标准内连接（推荐写法）

```
SELECT *
FROM emp e
INNER JOIN dept d
ON e.dept_id = d.dept_id;
```

👉 Oracle / MySQL 完全一致

------

###  二、左外连接（LEFT JOIN）

```
SELECT *
FROM emp e
LEFT JOIN dept d
ON e.dept_id = d.dept_id;
```

👉 保留 emp 所有行

------

### 三、右外连接（RIGHT JOIN）

```
SELECT *
FROM emp e
RIGHT JOIN dept d
ON e.dept_id = d.dept_id;
```

👉 保留 dept 所有行

------

###  四、全外连接（Oracle 特有）

#### Oracle：

```
SELECT *
FROM emp e
FULL OUTER JOIN dept d
ON e.dept_id = d.dept_id;
```

#### MySQL（模拟）：

```
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

👉 一张表当两张用

------

### 六、多表连接（三表及以上）

```
SELECT e.name, d.dname, s.salary
FROM emp e
JOIN dept d ON e.dept_id = d.dept_id
JOIN salary s ON e.emp_id = s.emp_id;
```

👉 链式 JOIN（推荐）

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

⚠️ 会产生 m × n 行

------

### 八、Oracle 特殊旧写法（重点了解）

#### 左外连接（旧 Oracle）

```
SELECT *
FROM emp e, dept d
WHERE e.dept_id = d.dept_id(+);
```

👉 `(+)` 表示右表可为空

⚠️ MySQL 不支持

## **特殊备注：**

char变量1||‘ ’||char变量2 可实现连接字符串

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

#### 📌 1. JOIN写法

```
SELECT e.name, d.avg_sal
FROM emp e
JOIN (
    SELECT dept_id, AVG(salary) avg_sal
    FROM emp
    GROUP BY dept_id
) d
ON e.dept_id = d.dept_id
WHERE e.salary > d.avg_sal;
```

------

#### 📌 2. 子查询写法

```
SELECT name, salary
FROM emp e
WHERE salary > (
    SELECT AVG(salary)
    FROM emp
    WHERE dept_id = e.dept_id
);
```

👉 这是**相关子查询**

select last_name from employees where employee_id not in manager_id;

```
SELECT last_name, salary, department_id FROM employees e WHERE salary > (SELECT AVG(salary) FROM employees WHERE department_id = e.department_id);
```

##例程管理：

### 📊 Oracle vs MySQL 关闭数据库四种方式对照表

| 关闭方式                             | Oracle 命令              | MySQL 命令                                     | 是否一致   | 特点                         | 使用场景                 |
| ------------------------------------ | ------------------------ | ---------------------------------------------- | ---------- | ---------------------------- | ------------------------ |
| 正常关闭（Normal Shutdown）          | `SHUTDOWN NORMAL`        | `mysqladmin shutdown` 或 `service mysqld stop` | ⚠️ 部分一致 | 等用户断开，不允许新连接     | 最安全，等待所有会话结束 |
| 事务性关闭（Transactional Shutdown） | `SHUTDOWN TRANSACTIONAL` | ❌ MySQL无完全对应                              | ❌          | 等当前事务完成后关闭         | Oracle特有，保证事务完整 |
| 立即关闭（Immediate Shutdown）       | `SHUTDOWN IMMEDIATE`     | `mysqladmin shutdown -p`（或 stop service）    | ⚠️ 类似     | 回滚未完成事务，强制断开连接 | 最常用、安全快速         |
| 终止/强制关闭（Abort Shutdown）      | `SHUTDOWN ABORT`         | kill -9 mysqld / 强杀进程                      | ⚠️ 类似     | 直接杀进程，不做清理         | 紧急情况（可能损坏数据） |

------

### 🧠 一、Oracle 四种关闭方式详解

------

#### 1️⃣ 正常关闭（NORMAL）

```
SHUTDOWN NORMAL;
```

### 特点：

- 不允许新连接
- 等所有用户退出
- 完成当前操作才关闭

------

#### 2️⃣ 事务性关闭（TRANSACTIONAL）

```
SHUTDOWN TRANSACTIONAL;
```

### 特点：

- 等当前事务提交/回滚后关闭
- 不等待空闲连接

👉 Oracle 特有（MySQL没有）

------

#### 3️⃣ 立即关闭（IMMEDIATE）⭐（最常用）

```
SHUTDOWN IMMEDIATE;
```

### 特点：

- 立即断开所有连接
- 未提交事务自动回滚
- 数据文件正常关闭

------

#### 4️⃣ 强制关闭（ABORT）⚠️

```
SHUTDOWN ABORT;
```

### 特点：

- 直接终止数据库进程
- 不做任何清理
- 下次启动需要实例恢复

------

### 🧠 二、MySQL 对应方式

MySQL 没有 Oracle 那么细的 shutdown 分级，主要是：

------

#### 1️⃣ 正常关闭（优雅关闭）

```
mysqladmin -u root -p shutdown
```

或：

```
systemctl stop mysqld
```

### 特点：

- 等当前操作完成
- 安全关闭

------

#### 2️⃣ 立即关闭（类似 IMMEDIATE）

```
systemctl stop mysqld
```

或：

```
mysqladmin shutdown
```

### 特点：

- 关闭服务并回滚事务

------

#### 3️⃣ 强制终止（Abort 类似）

```
kill -9 mysqld_pid
```

### 特点：

- 直接杀进程
- 可能导致 InnoDB crash recovery

------

### 🧠 三、核心对比总结（必背）

👉 Oracle 有 4 种细粒度关闭方式
 👉 MySQL 只有“优雅关闭 + 强制关闭”两类

------

### 📌 四、记忆口诀（考试很好用）

👉 Oracle 四种关闭：

> 正常等人走，事务做完走，立即断连接，abort直接杀

### 📊 Oracle 数据库启动三阶段总对照表

| 启动阶段            | 状态含义        | Oracle 指令             | 作用说明                   | 可访问内容           | MySQL对比                          |
| ------------------- | --------------- | ----------------------- | -------------------------- | -------------------- | ---------------------------------- |
| NOMOUNT（实例启动） | 只启动实例      | `STARTUP NOMOUNT;`      | 启动后台进程 + 分配SGA     | ❌ 无法访问数据库文件 | MySQL启动 mysqld（等价于整体启动） |
| MOUNT（挂载数据库） | 实例 + 控制文件 | `ALTER DATABASE MOUNT;` | 读取控制文件，识别数据文件 | ❌ 不能访问表数据     | MySQL无独立阶段                    |
| OPEN（打开数据库）  | 完整运行        | `ALTER DATABASE OPEN;`  | 打开数据文件，允许用户访问 | ✅ 正常查询/增删改    | MySQL启动后直接可用                |

------

### 🚀 一、Oracle 完整启动流程（标准写法）

#### 1️⃣ 一步启动（最常用）

```
STARTUP;
```

👉 等价于：

- NOMOUNT
- MOUNT
- OPEN（自动完成）

------

#### 2️⃣ 分阶段启动（考试重点）

------

##### 🔹 ① NOMOUNT（实例启动）

```
STARTUP NOMOUNT;
```

或：

```
ALTER DATABASE NOMOUNT;
```

##### 作用：

- ###### 启动实例（SGA + background process）

- 读取 init.ora / spfile

------

##### 🔹 ② MOUNT（挂载数据库）

```
ALTER DATABASE MOUNT;
```

###### 作用：

- 读取控制文件（control file）
- 确认数据文件位置

------

##### 🔹 ③ OPEN（打开数据库）

```
ALTER DATABASE OPEN;
```

###### 作用：

- 打开 datafile
- 用户可正常访问数据

------

### 🧠 二、三阶段关系图（理解用）

```
STARTUP NOMOUNT
      ↓
读取参数文件（pfile/spfile）
      ↓
MOUNT
      ↓
读取控制文件
      ↓
OPEN
      ↓
数据库可用
```

------

### 📌 三、常见补充命令（很重要）

------

#### 1️⃣ 查看数据库状态

```
SELECT status FROM v$instance;
```

------

#### 2️⃣ 关闭数据库（对照理解）

```
SHUTDOWN IMMEDIATE;
```

------

#### 3️⃣ 只启动到 MOUNT（维护模式）

```
STARTUP MOUNT;
```

👉 用于：

- 备份恢复
- 控制文件操作

------

#### 4️⃣ 强制启动（修复用）

```
STARTUP FORCE;
```

------

### 🧠 四、MySQL 对比理解（帮助记忆）

| Oracle阶段 | MySQL对应          |
| ---------- | ------------------ |
| NOMOUNT    | mysqld启动         |
| MOUNT      | 无（内部自动完成） |
| OPEN       | 服务可直接用       |

👉 MySQL 没有三阶段概念，Oracle是“分层启动模型”

------

### 📌 五、考试速记总结（必背）

👉 Oracle 启动三阶段：

- NOMOUNT：启动实例
- MOUNT：读取控制文件
- OPEN：打开数据库

### 📊 Oracle：只读数据库 & 受限数据库（RESTRICTED）总对照表

| 类型                             | 功能                    | 启动/开启方式                                                | 关闭方式                                   | ALTER语句                                         | 是否影响用户                      | 说明             |
| -------------------------------- | ----------------------- | ------------------------------------------------------------ | ------------------------------------------ | ------------------------------------------------- | --------------------------------- | ---------------- |
| 只读数据库（READ ONLY）          | 数据库只能查询不能修改  | `STARTUP MOUNT;` + `ALTER DATABASE OPEN READ ONLY;`          | `SHUTDOWN` 后重启 NORMAL                   | `ALTER DATABASE OPEN READ ONLY;`                  | ❌ 不能DML（INSERT/UPDATE/DELETE） | 常用于备份、报表 |
| 读写数据库（READ WRITE）         | 默认正常模式            | `STARTUP;`                                                   | `SHUTDOWN`                                 | `ALTER DATABASE OPEN READ WRITE;`                 | ✅ 完全可用                        | 默认模式         |
| 受限数据库（RESTRICTED SESSION） | 仅管理员/授权用户可访问 | `STARTUP RESTRICT;` 或 `ALTER SYSTEM ENABLE RESTRICTED SESSION;` | `ALTER SYSTEM DISABLE RESTRICTED SESSION;` | `ALTER SYSTEM ENABLE/DISABLE RESTRICTED SESSION;` | ⚠️ 普通用户不可连接                | 用于维护/升级    |
| MOUNT只挂载状态                  | 数据库未打开            | `STARTUP MOUNT;`                                             | `ALTER DATABASE OPEN;`                     | 无                                                | ❌ 无用户访问                      | 控制文件已读     |
| NOMOUNT实例状态                  | 仅启动实例              | `STARTUP NOMOUNT;`                                           | `SHUTDOWN`                                 | 无                                                | ❌                                 | 初始化阶段       |

------

#### 🚀 一、只读数据库（READ ONLY）详细流程

------

##### 1️⃣ 启动到只读模式（标准写法）

```
STARTUP MOUNT;
ALTER DATABASE OPEN READ ONLY;
```

------

##### 2️⃣ 修改为只读（运行中切换）

```
ALTER DATABASE OPEN READ ONLY;
```

⚠️ 注意：

- 必须先处于 MOUNT 或关闭状态
- 不能在已有DML事务时切换

------

##### 3️⃣ 恢复为读写模式

```
ALTER DATABASE OPEN READ WRITE;
```

------

##### 📌 只读模式特点

- ❌ 不能 INSERT / UPDATE / DELETE
- ❌ 不能创建对象
- ✅ 可以 SELECT
- 常用于：
  - 备份恢复
  - 报表系统
  - 数据快照分析

------

#### 🚀 二、受限数据库（RESTRICTED SESSION）

------

##### 1️⃣ 启动受限数据库

```
STARTUP RESTRICT;
```

👉 或：

```
STARTUP;
ALTER SYSTEM ENABLE RESTRICTED SESSION;
```

------

##### 2️⃣ 开启/关闭受限模式（运行中）

###### 开启：

```
ALTER SYSTEM ENABLE RESTRICTED SESSION;
```

###### 关闭：

```
ALTER SYSTEM DISABLE RESTRICTED SESSION;
```

------

###### 📌 受限模式特点

| 项目     | 说明                   |
| -------- | ---------------------- |
| 普通用户 | ❌ 不能连接             |
| DBA用户  | ✅ 可以连接             |
| 用途     | 维护、升级、数据修复   |
| 数据读写 | 正常（只限制连接权限） |

------

#### 🧠 三、只读 vs 受限（核心区别）

| 对比项         | 只读数据库     | 受限数据库         |
| -------------- | -------------- | ------------------ |
| 是否可连接     | ✅ 可以连接     | ❌ 普通用户不能     |
| 是否可查询     | ✅ 可以         | ✅ 可以             |
| 是否可修改数据 | ❌ 不可以       | ✅ 可以（DBA）      |
| 控制方式       | OPEN READ ONLY | RESTRICTED SESSION |
| 使用场景       | 报表/备份      | 维护/升级          |

------

#### 📌 四、组合考试重点（非常常考）

------

##### 1️⃣ 只读数据库启动流程

```
STARTUP MOUNT;
ALTER DATABASE OPEN READ ONLY;
```

------

##### 2️⃣ 受限模式开启流程

```
STARTUP;
ALTER SYSTEM ENABLE RESTRICTED SESSION;
```

------

##### 3️⃣ 恢复正常模式

```
ALTER SYSTEM DISABLE RESTRICTED SESSION;
```

------

#### 🧠 五、一句话记忆（考试必背）

👉

- 只读数据库：**“能查不能改”**
- 受限数据库：**“能进的人变少，但DBA全能”**

##错误初始化参数修改

| 目的                             | 方式             | 常用命令                    | 作用说明                                                     |
| -------------------------------- | ---------------- | --------------------------- | ------------------------------------------------------------ |
| 根据 `SPFILE` 修复并生成 `PFILE` | `SPFILE → PFILE` | `CREATE PFILE FROM SPFILE;` | 将服务器参数文件 `spfile` 导出为文本参数文件 `pfile`，便于查看、修改或备份。 |
| 根据 `PFILE` 生成 `SPFILE`       | `PFILE → SPFILE` | `CREATE SPFILE FROM PFILE;` | 将文本参数文件 `pfile` 转换为服务器参数文件 `spfile`，数据库启动时可直接读取。 |

| 查看日志                                                     | 定位异常参数                                                 | 处理步骤                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 使用 Oracle 的 `ADRCI` 工具查看告警日志（alert log）：`adrci` → `show alert -tail -f`。可实时查看数据库启动、关闭、参数修改及报错信息。 | 在 alert log 中重点查找 `ORA-01078`、`LRM-00109`、`ORA-32004` 等参数相关错误；同时查看最近执行的 `ALTER SYSTEM` 记录，定位是哪一个初始化参数修改后导致异常。 | 若参数错误导致数据库无法启动，可先使用 `startup pfile=...` 以文本参数文件启动；修改错误参数后，再执行 `create spfile from pfile;` 重建 `SPFILE` 并重启数据库。 |
| 使用 `show parameter 参数名` 查看当前实例参数，例如：`show parameter processes`。 | 使用 `show spparameter` 查看 `SPFILE` 中保存的参数，与 `show parameter` 对比，判断是否存在“内存参数正常但 SPFILE 错误”的情况。 | 若只是某个参数配置错误，可执行：`alter system set 参数=值 scope=spfile;` 修改后重启生效；若当前实例已无法正常启动，则需修改 `PFILE` 后恢复。 |
| 使用 `show parameter spfile` 查看当前数据库是否使用 `SPFILE` 启动，以及 `SPFILE` 文件路径。 | 若数据库启动时报参数错误，可根据 alert log 中显示的参数名定位问题初始化参数。 | 修复流程通常为：`SPFILE → PFILE → 修改参数 → PFILE → SPFILE → 重启数据库`。 |

常用恢复命令整理：

```
-- 根据 SPFILE 导出文本参数文件
create pfile from spfile;

-- 使用指定 PFILE 启动
startup pfile='/u01/app/oracle/init.ora';

-- 根据 PFILE 重新生成 SPFILE
create spfile from pfile;

-- 修改参数（写入 SPFILE）
alter system set processes=300 scope=spfile;
```

create database wxyc datafile '/u01/app/oracle/oradata/wxyc/system01.dbf' size 400m sysaux datafile '/u01/app/oracle/oradata/wxyc/sysaux01.dbf' size 400m default tablespace users datafile '/u01/app/oracle/oradata/wxyc/users01.dbf' size 100m default temporary tablespace temp tempfile '/u01/app/oracle/oradata/wxyc/temp01.dbf' size 20m undo tablespace undotbs1 datafile '/u01/app/oracle/oradata/wxyc/undotbs01.dbf' size 50m logfile group 1 '/u01/app/oracle/oradata/wxyc/redo01.log' size 10m, group 2 '/u01/app/oracle/oradata/wxyc/redo02.log' size 10m, group 3 '/u01/app/oracle/oradata/wxyc/redo03.log' size 10m;

#

select comp_name,status from dba_registry where comp_name like '%XML%';

@?/rdbms/admin/catproc.sql

## 控制文件的管理

CREATE CONTROLFILE REUSE DATABASE "wxyc" NORESETLOGS NOARCHIVELOG
  MAXLOGFILES 16
  MAXLOGMEMBERS 2
  MAXDATAFILES 30
  MAXINSTANCES 1
  MAXLOGHISTORY 292
LOGFILE
  GROUP 1 '/u01/app/oracle/oradata/wxyc/redo01.log' SIZE 10M BLOCKSIZE 512,
  GROUP 2 '/u01/app/oracle/oradata/wxyc/redo02.log' SIZE 10M BLOCKSIZE 512,
  GROUP 3 '/u01/app/oracle/oradata/wxyc/redo03.log' SIZE 10M BLOCKSIZE 512
DATAFILE
  '/u01/app/oracle/oradata/wxyc/system01.dbf',
  '/u01/app/oracle/oradata/wxyc/sysaux01.dbf',
  '/u01/app/oracle/oradata/wxyc/undotbs01.dbf',
  '/u01/app/oracle/oradata/wxyc/users01.dbf'
CHARACTER SET US7ASCII



CREATE CONTROLFILE REUSE DATABASE "WXYC" NORESETLOGS NOARCHIVELOG MAXLOGFILES 16 MAXLOGMEMBERS 2 MAXDATAFILES 30 MAXINSTANCES 1 MAXLOGHISTORY 292 LOGFILE GROUP 1 '/u01/app/oracle/oradata/wxyc/redo01.log' SIZE 10M BLOCKSIZE 512, GROUP 2 '/u01/app/oracle/oradata/wxyc/redo02.log' SIZE 10M BLOCKSIZE 512, GROUP 3 '/u01/app/oracle/oradata/wxyc/redo03.log' SIZE 10M BLOCKSIZE 512 DATAFILE '/u01/app/oracle/oradata/wxyc/system01.dbf', '/u01/app/oracle/oradata/wxyc/sysaux01.dbf', '/u01/app/oracle/oradata/wxyc/undotbs01.dbf', '/u01/app/oracle/oradata/wxyc/users01.dbf' CHARACTER SET US7ASCII



## 用户管理备份

### 冷备份 (具体由deepseek生成，需根据目录修改)

vim /etc/oratab在db19c中添加数据库便于删除
复制db19c行即可，db19c换成其他数据库名字

mkdir -p /u01/app/oracle/admin/qyc
mkdir -p /u01/app/oracle/admin/qyc/adump
mkdir -p /u01/app/oracle/oradata/qyc

```bash
# 演示：用户管理的冷备份恢复流程 (对应图2 1-17步)

# ==========================================
# 前置变量设置 (请根据实际环境修改以下路径)
export ORACLE_SID=ORCL
export DATA_DIR=/u01/app/oracle/oradata/ORCL
export BACKUP_DIR=/u01/backup/cold_backup
# ==========================================


# 第1步：产生文件列表 (查询数据库物理文件路径)
sqlplus / as sysdba <<EOF
select name from v\$datafile;
select name from v\$controlfile;
select member from v\$logfile;
EOF


# 第2步：创建测试表 scott.cold
sqlplus / as sysdba <<EOF
create table scott.cold as select * from scott.emp where 1=0;
EOF


# 第3步：shutdown (关闭数据库)
sqlplus / as sysdba <<EOF
shutdown immediate;
EOF


# 第4步：备份数据库 (物理层冷备份)
mkdir -p $BACKUP_DIR
cp -r $DATA_DIR/* $BACKUP_DIR/


# 第5步：插入新数据 (重启数据库并写入数据，用于验证恢复)
sqlplus / as sysdba <<EOF
startup;
insert into scott.cold select * from scott.emp;
commit;
EOF


# 第6步：复制控制文件和联机重做日志文件 (将当前日志与控制也纳入备份)
cp $DATA_DIR/control*.ctl $BACKUP_DIR/
cp $DATA_DIR/redo*.log $BACKUP_DIR/


# 第7步：破坏数据库 (使用 dbca 静默删除数据库模拟灾难)
dbca -silent -deleteDatabase -sourceDB $ORACLE_SID
# 注意：如果未安装 dbca，也可直接暴力删除数据文件目录模拟破坏
# rm -rf $DATA_DIR/*


# 第8步：还原 spfile(初始化参数文件)
# 从刚才的冷备份目录中，将参数文件拷贝回 $ORACLE_HOME/dbs 目录下
cp $BACKUP_DIR/spfile$ORACLE_SID.ora $ORACLE_HOME/dbs/


# 第9步：创建相应的目录
mkdir -p /u01/app/oracle/admin/$ORACLE_SID/adump
mkdir -p $DATA_DIR


# 第10步：启动例程 (仅启动实例，不加载数据库)
sqlplus / as sysdba <<EOF
startup nomount;
EOF


# 第11步：还原控制文件 (从备份目录拷贝回数据文件目录)
cp $BACKUP_DIR/control*.ctl $DATA_DIR/


# 第12步：加载数据库
sqlplus / as sysdba <<EOF
alter database mount;
EOF


# 第13步：还原数据文件 (从备份目录拷贝回所有数据文件)
cp $BACKUP_DIR/*.dbf $DATA_DIR/


# 第14步：复制回联机重做日志文件 (从备份目录拷贝回联机日志)
cp $BACKUP_DIR/redo*.log $DATA_DIR/


# 第15步：恢复数据库
# 注：冷备份文件完全一致，执行 recover 通常会提示 "没有需要恢复的介质恢复"
sqlplus / as sysdba <<EOF
recover database;
EOF


# 第16步：打开数据库
sqlplus / as sysdba <<EOF
alter database open;
EOF


# 第17步：检查数据 (验证之前插入的数据是否恢复)
sqlplus / as sysdba <<EOF
select * from scott.cold;
EOFsql```
```



## RMAN全量备份

五 演示RMAN全库备份和恢复
1 备份前的配置
spfile,controlfile,online redo logfile,datafile,archivelog
RMAN> CONFIGURE CONTROLFILE AUTOBACKUP ON;
RMAN> CONFIGURE CONTROLFILE AUTOBACKUP FORMAT FOR DEVICE TYPE DISK TO '/u01/hwj_backup/auto/%F';
RMAN> CONFIGURE channel device type disk format '/u01/hwj_backup/%d_%u_%T';
2 创建测试表
RMAN> create table scott.bak(a varchar(30));
RMAN> insert into scott.bak values('Before Backup');
3 日志归档
RMAN> alter system archive log current;
4 备份数据库和归档日志文件
backup database plus archivelog;
5 插入新数据
insert into scott.bak values('AFTER Backup');
commit;
6 复制联机日志文件
cp /u01/app/oracle/oradata/hwj/*.log /u01/hwj_backup/log/
7 破坏数据库
利用dbca删除数据库

8 还原spfile
RMAN> restore spfile from '/u01/hwj_backup/auto/c-470906185-20250530-08';
9 创建相应的目录
mkdir -p /u01/app/oracle/admin/hwj/adump
mkdir -p /u01/app/oracle/oradata/hwj
10 还原controlfile
RMAN> restore controlfile from '/u01/hwj_backup/auto/c-470906185-20250530-08';
11 加载数据库
alter database mount;
12 还原数据文件
restore database;
13 复制回联机日志文件
cp /u01/hwj_backup/log/*.log /u01/app/oracle/oradata/hwj/
14 恢复数据库
recover database;
15 打开数据库
RMAN> alter database open resetlogs;
16 检查数据
select * from scott.bak;





### 创建测试表
create table scott.suibian(id int);
insert into scott.suibian values(200);
commit;
alter system archive log current;
2 整库备份
rman>backup database;
3 再插入一条记录
rman>select * from scott.suibian;
4 查看时间 (!date) 16:10:42
5 删除表
drop table scott.suibian purge;
6 创建表
create table scott.suibian2 as select * from scott.emp;
select * from scott.suibian2;
7 启动数据库到mount阶段
8 设置环境变量
export NLS_DATE_FORMAT="yyyy-mm-dd hh24:mi:ss"
9 还原数据库
rman>restore database;
10 恢复数据库到指定时间点
rman>recover database until time '2026-06-11 15:55:58';
11 打开数据库
rman>alter database open resetlogs;
12 检查数据
select * from scott.suibian;(2条记录)
select * from scott.suibian2;(对象不存在)

星期四 15:55:58 CST

# 

##**mysql备份** 
逻辑备份数据库：
 ./mysqldump -uroot -pstarcraft2syhqyc --all-databases >/data/backupall.sql    ---备份所有的数据库
 ./mysqldump -uroot -pstarcraft2syhqyc --databases scott>/data/backupscott.sql ---备份scott数据库 
./mysqldump -uroot -pstarcraft2syhqyc scott emp>/data/backupemp.sql           ---备份单张表
 ./mysqldump -uroot -pstarcraft2syhqyc scott emp dept>/data/backup_emp_dept.sql ---备份多张表
 破坏表： 
mysql -uroot -pstarcraft2syhqyc
use scott; 
drop table emp; 
恢复表 
mysql -uroot -pstarcraft2syhqyc  scott</data/backupemp.sql 
检查数据 
mysql>select * from emp; 
破坏数据库 
mysql> drop database scott; 
恢复数据库 
mysql -uroot -pstarcraft2syhqyc  </data/backupscott.sql 
检查数据： 
mysql>show databases; 
mysql>use scott; 
mysql>select * from emp;



##**TPITR    ---表基于时间点的恢复**
Oracle 12c可以使用RMAN将一个或多个表（表分区）恢复到指定时间点，而不影响其余的数据库对象。
先决条件： 
1   compatible=12.0(或更大) 
2   ARCHIVELOG模式 
3   READ WRITE打开模式 演示： 
1    create table scott.emp1 tablespace users as select * from scott.emp; 
2    RMAN>backup database plus archivelog; 
3    select count(*) from scott.emp1;  14 
4    select current_scn from v$database;  2974474 
5    drop table scott.emp1 purge; 
6    create table scott.emp2 tablespace users as select * from scott.emp; 
7    mkdir /u01/aux 
8    RMAN>recover table scott.emp1 until scn 2974474 auxiliary destination '/u01/aux'; 
9    select count(*) from scott.emp1;     select * from scott.emp2;



##**辅助数据库**

原库：sales(归档模式)
新库：oradup

1  准备初始化参数文件   --添加下面3个初始化参数
DB_FILE_NAME_CONVERT=('/u01/app/oracle/oradata/sales/','/u01/app/oracle/oradata/oradup/')
LOG_FILE_NAME_CONVERT=('/u01/app/oracle/oradata/sales/','/u01/app/oracle/oradata/oradup/')
log_archive_dest_1='location=/u01/arch/oradup'

2  创建相应的目录结构
3  创建口令验证文件
4  配置好监听器和服务名    ---监听配置完需要重新启动监听生效
5  将oradup启动到nomount状态
6  启动rman，同时连接原库和新库
rman target sys/admin1#3@sales auxiliary sys/admin1#3@oradup
7  执行duplicate
rman>duplicate target database to oradup from active database;
8  检查新库状态

TSPITR   ---表空间基于时间点的恢复
1   创建测试表空间
create tablespace tbs1 datafile '/u01/app/oracle/oradata/sales/tbs101.dbf' size 10m;
create tablespace tbs2 datafile '/u01/app/oracle/oradata/sales/tbs201.dbf' size 10m;

2   创建测试表
create table scott.t1 tablespace tbs1 as select * from scott.emp;
create index scott.idx_ename on scott.t1(ename) tablespace tbs2;
alter system checkpoint;
3   备份数据库
RMAN>backup database plus archivelog;
RMAN>list backup;
4   误操作
select systimestamp from dual;   (04.22.02)
truncate table scott.t1;
5   之后再创建一张表
create table scott.t2 tablespace users as select * from scott.dept;
create table scott.t3 tablespace tbs1 as select * from scott.emp;
select * from scott.t1;
select * from scott.t2;
select * from scott.t3;
6   验证表空间的依赖性
conn / as sysdba
execute DBMS_TTS.TRANSPORT_SET_CHECK('TBS1', TRUE,TRUE);
SELECT * FROM TRANSPORT_SET_VIOLATIONS;
execute DBMS_TTS.TRANSPORT_SET_CHECK('TBS1,TBS2', TRUE,TRUE);
SELECT * FROM TRANSPORT_SET_VIOLATIONS;
可以看到，如果只恢复表空间TBS1，会有T1表的索引依赖表空间TBS2。
我们这里同时恢复表空间TBS1,TBS2，这样就解决了依赖关系。

7   确定执行TSPITR后会丢失的对象
-- 查询执行TSPITR后会丢失的对象
select owner,name,tablespace_name,to_char(creation_time,'yyyy-mm-dd hh24:mi:ss') creation_time
from TS_PITR_OBJECTS_TO_BE_DROPPED
where tablespace_name in ('TBS1','TBS2')
and creation_time>to_date('2026-06-11 16:22:02','yyyy-mm-dd hh24:mi:ss');

这里没有查出结果，如果有结果，最好先expdp导出这些对象的备份，待恢复表空间后，再导入这些对象。
当然如果确定这些对象是没有用的，可以直接忽略。

8    执行TSPITR
export NLS_DATE_FORMAT="yyyy-mm-dd hh24:mi:ss"
RMAN>recover tablespace TBS1,TBS2 until time '2026-06-11 16:22:02' auxiliary destination '/u01/aux';

9    检查
select tablespace_name,status from dba_tablespaces;
alter tablespace tbs1 online;
alter tablespace tbs2 online;
select * from scott.t1;
select * from scott.t2;
select * from scott.t3;

imp 
10   删除测试表空间和表
drop tablespace tbs1 including contents and datafiles;
drop tablespace tbs2 including contents and datafiles;
drop table scott.t2 purge;
drop table scott.t3 purge;

##闪回技术

一    **闪回数据库（闪回数据库日志）**
前提：
1    归档模式
2    启用闪回日志
select FLASHBACK_ON from v$database;
alter database FLASHBACK ON;

和闪回数据库相关的初始化参数：
db_recovery_file_dest
db_recovery_file_dest_size
db_flashback_retention_target

v$flashback_database_log
3158961


mount阶段：
flashback database to scn 3158961;

二    **闪回表（撤销表空间）**
前提：激活表的行移动特性

16:34:55
flashback table scott.emp to timestamp to_timestamp('2026-06-11 16:49:35','yyyy-mm-dd hh24:mi:ss');



三    **闪回删除（回收站）**
前提： 启用回收站
show parameter recyclebin
flashback table <table_name> to before drop;

查看用户回收站：
show recyclebin

四    **闪回查询（撤销表空间）**
1     闪回查询表  --select    as of 
11:31:25
select * from emp as of timestamp to_timestamp('2026-06-11 16:57:03','yyyy-mm-dd hh24:mi:ss') where deptno=30;
insert into emp select * from emp as of timestamp to_timestamp('2025-05-30 16:43:22','yyyy-mm-dd hh24:mi:ss') where deptno=30;

2     闪回版本查询
select sal from emp versions between scn minvalue and maxvalue where empno=1111;

3     闪回事务查询
前提：启用数据库补充日志
select SUPPLEMENTAL_LOG_DATA_MIN from v$database;
alter database add SUPPLEMENTAL LOG DATA;

delete from emp where empno=7934;
delete from emp where empno=7902;
commit;

select versions_xid,empno,ename,versions_operation from emp versions between scn minvalue and maxvalue;
090015002C020000
select undo_sql from FLASHBACK_TRANSACTION_QUERY where xid='090015002C020000';

insert into "SCOTT"."EMP"("EMPNO","ENAME","JOB","MGR","HIREDATE","SAL","COMM","DEPTNO") values ('7934','MILLER','CLERK','7782',TO_DATE('23-JAN-82', 'DD-MON-RR'),'1300',NULL,'10');



**五    闪回事务**
前提：启用数据库补充日志
dbms_flashback.TRANSACTION_BACKOUT
考虑事务的依赖性:
WAW
主键依赖
外键依赖

演示：
select salary from hr.employees where employee_id=201;
update hr.employees set salary=salary\*5;
commit;
update hr.employees set salary=salary\*1.1 where employee_id=201;
commit;
select salary from hr.employees where employee_id=201;

select distinct xid,commit_scn
    from flashback_transaction_query
    where table_owner='HR' and
    table_name='EMPLOYEES' and
    commit_timestamp > systimestamp - interval '2' minute
    order by commit_scn;

XID              COMMIT_SCN
---------------- ----------
0A001A0035020000    3170113
09001F002C020000    3170147

declare
     xids sys.xid_array;
    begin
     xids := sys.xid_array('0A000F00AE030000');
     dbms_flashback.transaction_backout(1,xids,options=>dbms_flashback.nocascade);
    end;
/

declare
     xids sys.xid_array;
    begin
     xids := sys.xid_array('0A001A0035020000');
     dbms_flashback.transaction_backout(1,xids,options=>dbms_flashback.cascade);
    end;
/



## 数据迁移

**建表：**

CREATE TABLE scott.animal_feeding (
animal_id NUMBER,
feeding_date DATE,
pounds_eaten NUMBER (5,2),
note VARCHAR2(80) );

100,1-jan-2000,23.5,"Flipper seemed unusually hungry today."
105,1-jan-2000,99.45,"Spread over three meals."
112,1-jan-2000,10,"No comment."
151,1-jan-2000,55
166,1-jan-2000,17.5,"Shorty ate Squacky."
145,1-jan-2000,0,"Squacky is no more."
175,1-jan-2000,35.5,"Paintuin skipped his first meal, but ate the other five."
199,1-jan-2000,0.5,"Nosey wasn't very hungry today."
202,1-jan-2000,22.0
240,1-jan-2000,28,"Snoops appeared lethargic, and was running a fever."
100,2-jan-2000,19.5,"Flipper's appetite has returned to normal."
105,2-jan-2000,89.0
112,2-jan-2000,12
151,2-jan-2000,50
166,2-jan-2000,16.0,"We are keeping Shorty isolated from the other animals."
175,2-jan-2000,30
199,2-jan-2000,9.5,"Nosey's appetite has returned."
202,2-jan-2000,19.3
240,2-jan-2000,22,"Snoops still lethargic, no fever."

##**普通sqlldr：**

LOAD
INFILE '/u01/demo/animal_feeding.dat'
insert
INTO TABLE scott.animal_feeding
TRAILING NULLCOLS
(
animal_id      INTEGER EXTERNAL TERMINATED BY ',',
feeding_date   DATE "dd-mon-yyyy" TERMINATED BY ',',
pounds_eaten   DECIMAL EXTERNAL TERMINATED BY ',',
note           CHAR TERMINATED BY ','
               OPTIONALLY ENCLOSED BY '"'
)

sqlldr scott/tiger control=animal_feeding.ctl

##**sqlloader快捷模式：**
create table scott.test
    ( region     char(3),
      region_name varchar2(12),
      bill_month number(6),
      fee        number(10,2)
    );

vim test.dat

530,HZ,200501,100.01
530,HZ,200502,800.23
531,JN,200501,5000.81
531,JN,200502,5360.00
532,QD,200501,20670.32
532,QD,200502,22000.08
533,ZB,200501,3050.56
533,ZB,200502,3108.14

sqlldr scott/tiger table=test



##**sqlldr2+sqlldr完成亿级数据迁移：**

1) 安装
将sqlldr2linux64.bin复制到$ORACLE_HOME/bin目录下，创建软链接 ln -s sqlldr2linux64.bin sqlldr2
2) 常规导出
sqlldr2 scott/tiger query=emp
sqlldr2 scott/tiger query='select * from scott.emp where sal>2500' head=yes file=emp.dat
3) 生成控制文件
sqlldr2 scott/tiger query="select * from emp" table=emp_his head=yes file=emp1.dat
4) 加载数据
sqlldr scott/tiger control=emp_his_sqlldr.ctl

CREATE TABLE emp_his (
EMPNO NUMBER(4),
ENAME VARCHAR2(10),
JOB VARCHAR2(9),
MGR NUMBER(4),
HIREDATE DATE,
SAL NUMBER(7,2),
COMM NUMBER(7,2),
DEPTNO NUMBER(2)
);



##Mysql导出和导入：

使用 select   into  outfile导出
select  *  from  scott.emp into  outfile  '/data/mysql/emp.txt'    fields terminated  by ','   optionally   enclosed  by '"'  lines   terminated  by '\n' ; 

show variables like '%secure%'\G

vim /etc/my.cnf
secure_file_priv=/data/mysql
service mysql restart

grant file on *.* to 'root'@'%';
flush privileges;

2）     LOAD DATA  INFILE导入
use hr;
create table hr.emp like scott.emp;
LOAD DATA  INFILE  '/data/mysql/emp.txt'  INTO TABLE hr.emp FIELDS TERMINATED BY ',' ENCLOSED BY '"' LINES TERMINATED BY '\n';

##使用logmnr解决用户错误

修改数据库日志模式（我没注释的全部是在oracle数据库状态下运行）：
1     启动数据库到mount阶段
确保开始时处于SYS用户状态
shutdown immediate
startup mount;
alter database open;
alter database archivelog;
archive log list;

2   启用数据库补充日志
select SUPPLEMENTAL_LOG_DATA_MIN from v$database;
alter database add SUPPLEMENTAL LOG DATA;

3    产生数据字典文件
mkdir -p /u01/demo/dict <oracle用户下>
create directory DICT as '/u01/demo/dict';
EXECUTE dbms_logmnr_d.build('v816dict.ora','DICT');


4    做事务（模拟scott用户的误删除）
conn scott/tiger
delete from scott.emp;
commit;
conn / as sysdba
5    添加需要分析的日志文件（这里不要加所有文件，只需要找处于current状态的log是哪个就行，第一句是创建，第二句是添加，如果只有一个就只执行上面一句就行，记得将redo{i}.log换成对应的）
select GROUP#, STATUS from v$log;
select member from v$logfile where group#=3; （这里不一定是3，看上一句的执行结果哪个或哪些是current）
EXECUTE dbms_logmnr.add_logfile(LogFileName=>'/u01/app/oracle/oradata/wxyc/redo03.log',Options=>dbms_logmnr.new);

EXECUTE dbms_logmnr.add_logfile(LogFileName=>'/u01/app/oracle/oradata/wxyc/redo01.log',Options=>dbms_logmnr.addfile);
5    启动分析
EXECUTE dbms_logmnr.start_logmnr(DictFileName=>'/u01/demo/dict/v816dict.ora');
6    查询(v$logmnr_contents)

alter session set nls_date_format='yyyy-mm-dd hh24:mi:ss';
select TIMESTAMP,USERNAME,SQL_REDO from v$logmnr_contents where SEG_NAME='EMP' and OPERATION='DELETE';
（这里你会发现所有都是一个时间戳，因为是定位的同一个scott.emp的误删除操作，然后记得把）

记录一下你的操作时间戳，如：2026-06-04 17:09:11（下面的闪回操作里的时间戳修改成你看到的）
conn scott/tiger
alter table emp enable ROW MOVEMENT;
flashback table emp to timestamp to_timestamp('2026-06-04 17:09:11','yyyy-mm-dd hh24:mi:ss');
alter table emp disable ROW MOVEMENT;

### 用expdp导出scott.emp中工资大于2500的记录，不导出约束，导入到hr.emp1,并更换表空间(users:users1) [deepseek]

```bash
#!/bin/bash
# ==========================================================
# 完整迁移脚本：expdp 导出 scott.emp (sal>2500)，impdp 导入至 hr.emp1 并更换表空间
# 注意：请确保在数据库服务器本地运行，且环境变量(ORACLE_SID)已配置
# ==========================================================

# --- [0. 环境变量设置] ---
# 请根据你的实际环境修改 ORACLE_SID 和 目录路径
export ORACLE_SID=orcl
# 确保下面的目录路径在操作系统上真实存在
export DPUMP_DIR_PATH="/u01/app/oracle/admin/orcl/dpdump/"

echo ">>> 步骤 1: 准备 SQL 环境 (创建用户、表空间、目录并授权) <<<"
sqlplus -s /nolog <<EOF
connect / as sysdba

-- 1.1 创建目标表空间 users1 (如果已存在可不执行)
CREATE TABLESPACE users1 DATAFILE '/u01/app/oracle/oradata/orcl/users01.dbf' SIZE 100M AUTOEXTEND ON;

-- 1.2 清理可能残留的 hr 用户和表 (防止导入时表已存在报错)
DROP USER hr CASCADE;

-- 1.3 创建 hr 用户并授予相关权限
CREATE USER hr IDENTIFIED BY hr_password;
GRANT CONNECT, RESOURCE, CREATE TABLE TO hr;
-- 授予目标表空间的无限配额
ALTER USER hr QUOTA UNLIMITED ON users1;

-- 1.4 创建/重建 Data Pump 目录对象
CREATE OR REPLACE DIRECTORY DATA_PUMP_DIR AS '${DPUMP_DIR_PATH}';
GRANT READ, WRITE ON DIRECTORY DATA_PUMP_DIR TO scott, hr;
exit;
EOF

echo ""
echo ">>> 步骤 2: 使用 expdp 导出 scott.emp 中 sal>2500 的记录 (排除约束) <<<"
expdp scott/tiger \
directory=DATA_PUMP_DIR \
dumpfile=emp_high_sal.dmp \
logfile=exp_emp_high_sal.log \
tables=scott.emp \
query=\"where sal > 2500\" \
exclude=constraint

if [ $? -ne 0 ]; then
    echo "错误: expdp 导出失败，请查看日志!"
    exit 1
fi

echo ""
echo ">>> 步骤 3: 使用 impdp 导入到 hr.emp1，并映射表空间 users -> users1 (排除约束) <<<"
impdp hr/hr_password \
directory=DATA_PUMP_DIR \
dumpfile=emp_high_sal.dmp \
logfile=imp_emp_high_sal.log \
remap_schema=scott:hr \
remap_table=emp:emp1 \
remap_tablespace=users:users1 \
exclude=constraint

if [ $? -ne 0 ]; then
    echo "错误: impdp 导入失败，请查看日志!"
    exit 1
fi

echo ""
echo ">>> 执行完成！请检查日志文件 $DPUMP_DIR_PATH/imp_emp_high_sal.log"
```

### 传输表空间

```bash
传输表空间
db19c
1 创建表空间XXX
2 在表空间XXX内创建一张表，插入几条记录
3 设置表空间为只读
4 从源数据库输出元数据
exp \'sys/admin as sysdba\' file=tts.dmp TRANSPORT_TABLESPACE=Y TABLESPACES=xxx
5 把数据文件和DMP文件复制到目标系统
sales
6 把元数据输入目标数据库
imp \'sys/admin1#3 as sysdba\' FILE=tts.dmp TRANSPORT_TABLESPACE=Y DATAFILES=/u01/app/oracle/oradata/sales/xxx01.dbf
7 如果需要，把表空间设置为读写
8 检查

查看Oracle数据库的字符编码
SELECT value$ FROM sys.props$ WHERE name = 'NLS_CHARACTERSET';
查询Oracle Server端的字符集:
select userenv('language') from dual;
修改字符集:
alter system enable restricted session;
alter database character set internal_use AL32UTF8; US7ASCII
alter system disable restricted session;

db19c(AL32UTF8)
sales(US7ASCII)
```

### 加载外部表

```data
7839,20,SMITH
7566,20,JONES
7782,10,CLARK
7839,10,KING
7902,20,FORD
7934,10,MILLER
7499,30,ALLEN
7521,30,WARD
7654,30,MARTIN
7698,30,BLAKE
7844,20,TURN
```

```sql
create table scott.ex1(empno int,deptno int,ename varchar(20))
ORGANIZATION EXTERNAL
(type oracle_loader default directory expdp_dest
access parameters
(records delimited by newline
fields terminated by ','
missing field values are null
(empno,deptno,ename))
location('employees.dat'));
```

### 导出/加载外部表

```sql
卸载数据：（db19c）
create table scott.ex2(empno,ename,sal)
ORGANIZATION EXTERNAL
(type oracle_datapump default directory exp_dest
location('emp1.dat'))
as select empno,ename,sal from scott.emp;

加载数据(sales)
create table scott.ex(empno int,ename varchar(20),sal number(7,2))
ORGANIZATION EXTERNAL
(type oracle_datapump default directory expdp_dest
location('emp1.dat'));
```

