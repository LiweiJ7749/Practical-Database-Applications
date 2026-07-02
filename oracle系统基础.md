# 进入MySQL

## 启动
```bash
[root@ocpjlw ~]# su oracle
[oracle@ocpjlw root]$ systemctl stop mysql
[oracle@ocpjlw root]$ service mysql start
[oracle@ocpjlw root]$ mysql -u root -p123456
```

## 查看基础信息
```sql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| hr                 |
| mysql              |
| performance_schema |
| scott              |
| sys                |
+--------------------+
6 rows in set (0.01 sec)

mysql> use scott;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+-----------------+
| Tables_in_scott |
+-----------------+
| bonus           |
| buyers          |
| dept            |
| emp             |
| product         |
| sales           |
| salgrade        |
| st              |
+-----------------+
8 rows in set (0.00 sec)
```

-------

# oracle 基础

## 启动
```bash
[root@ocpjlw ~]# su oracle
[oracle@ocpjlw root]$ echo $ORACLE_SID
db19c
[oracle@ocpjlw root]$ export ORACLE_SID=sales
[oracle@ocpjlw root]$ cd $ORACEL_HOME
[oracle@ocpjlw ~]$ lsnrctl start
[oracle@ocpjlw ~]$ sqlplus / as sysdba
```

```sql
SYS@db19c>startup
SYS@db19c>conn hr/hr
```




## 数据字典视图

内容来自**数据文件**

- USER_：用户拥有的表

  ```sql
  --- 查看scott/tiger用户的表
  SCOTT@db19c>select table_name from user_tables;
  
  TABLE_NAME
  ------------------------------
  DEPT
  EMP
  BONUS
  SALGRADE
  ST1
  ```

  ```sql
  --- 查看hr的触发器
  HR@db19c>select TRIGGER_NAME from user_trigger
  
  TRIGGER_NAME
  ------------------------------
  UPDATE_JOB_HISTORY
  SECURE_EMPLOYEES
  ```

- ALL_：用户有权限查看的表

- DBA_：数据库管理员有权限查看的表

数据文件的统计数据需要及时更新

```sql
SCOTT@db19c>select num_rows from user_tables where table_name='EMP';
NUM_ROWS
----------
14

SCOTT@db19c>insert into emp(empno,ename) values(1111,'Tom');
1 row created.

SCOTT@db19c>commit;
commit completed;

SCOTT@db19c>select num_rows from user_tables where table_name='EMP';
NUM_ROWS
----------
14

SCOTT@db19c>analyze table emp compute statistics;
Table analyzed.

SCOTT@db19c>select num_rows_from user_tables where table_name='EMP';
NUM_ROWS
-----------
15
```

----------

## 动态性能视图

在oracle中，以 `v$`开头的一些表，内容来自**内存**或者**控制文件**。

### desc 查看
```sql
SYS@db19c>desc v$instance
 Name           Null?       Type
 ------------- ------ ----------------------
 INSTANCE_NUMBER            NUMBER
 INSTANCE_NAME              VARCHAR2(16)
 HOST_NAME                  VARCHAR2(64)
 VERSION                    VARCHAR2(17)
 VERSION_LEGACY             VARCHAR2(17)
 VERSION_FULL               VARCHAR2(17)
 STARTUP_TIME               DATE
 STATUS                     VARCHAR2(12)
 PARALLEL                   VARCHAR2(3)
 THREAD#                    NUMBER
 ARCHIVER                   VARCHAR2(7)
 LOG_SWITCH_WAIT            VARCHAR2(15)
 LOGINS                     VARCHAR2(10)
 SHUTDOWN_PENDING           VARCHAR2(3)
 DATABASE_STATUS            VARCHAR2(17)
 INSTANCE_ROLE              VARCHAR2(18)
 ACTIVE_STATE               VARCHAR2(9)
 BLOCKED                    VARCHAR2(3)
 CON_ID                     NUMBER
 INSTANCE_MODE              VARCHAR2(11)
 EDITION                    VARCHAR2(7)
 FAMILY                     VARCHAR2(80)
 DATABASE_TYPE              VARCHAR2(15)

```


### select 查看
```sql
--- 例子：查看SGA内各个部分的大小
SYS@db19c>select name,bytes/1024/1024 sizes from V$SGAINFO;

NAME                               SIZES
---------------------------------- ----------
Fixed SGA Size                     8.4854126
Redo Buffers                       7.51171875
Buffer Cache Size                  1280
In-Memory Area Size                0
Shared Pool Size                   384
Large Pool Size                    32
Java Pool Size                     0
Streams Pool Size                  0
Shared I/O Pool Size               96
Data Transfer Cache Size           0
Granule Size                       16
Maximum SGA Size                   1711.99713
Startup overhead in Shared Pool    177.44812
Free SGA Memory Available          0

14 rows selected.

SYS@db19c>select name from v$controlfile;
NAME
---------------------------------------------------
/u01/app/oracle/oradata/DB19C/control01.ctl
/u01/app/oracle/fast_recovery_area/DB19C/control02.ctl

SYS@db19c>select status from v$instance;
STATUS
------------
OPEN

SYS@db19c>select member from v$logfile;

MEMBER
----------------------------------------------------
/u01/app/oracle/oradata/DB19C/redo03.log
/u01/app/oracle/oradata/DB19C/redo02.log
/u01/app/oracle/oradata/DB19C/redo01.log

```



# 作业：依赖日志文件的数据恢复

```sql
SYS@db19c>!rm /u01/app/oracle/oradata/DB19C/demo01.dbf

SYS@db19c>ls /u01/app/oracle/oradata/DB19C/demo01.dbf
ls: cannot access /u01/app/oracle/oradata/DB19C/demo01.dbf: No such file or directory

SYS@db19c>conn / as sysdba
Connected.

SYS@db19c>shutdown abort;
ORACLE instance shut down.

SYS@db19c>startup
ORACLE instance started.

Total System Global Area 1795159104 bytes
Fixed Size             8897600 bytes
Variable Size          436207616 bytes
Database Buffers      1342177280 bytes
Redo Buffers            7876608 bytes
Database mounted.
ORA-01157: cannot identify/lock data file 5 - see DBWR trace file
ORA-01110: data file 5: '/u01/app/oracle/oradata/DB19C/demo01.dbf'

SYS@db19c>alter database create datafile 5;
Database altered.

SYS@db19c>ls /u01/app/oracle/oradata/DB19C/demo01.dbf
/u01/app/oracle/oradata/DB19C/demo01.dbf

SYS@db19c>recover datafile 5;
Media recovery complete.
SYS@db19c>alter database open;

Database altered.

SYS@db19c>select * from scott.st1;

 ID  NAME    
--- ---------------------------
 1   zhangsan 
 2   lisi    
```



# instance(例程)

## 1. 内存

- SGA( system global arrangement, 系统全局区)：多个会话共享的区域

  - 共享池
    - 库缓存 (Library Cache) ，分为两个子区。缓存代码结果
      - SQL区
      - PL/SQL区
    -  数据字典缓存（Dictionary Cache）
  - 数据库高速缓存
  - 重做日志缓存
- PGA

## 2. 进程
- 用户进程(User Process):

  `tnsping `是 Oracle 数据库提供的一个**网络诊断工具**，主要用来检查 **Oracle 客户端能否通过网络连接到数据库服务器的监听器（Listener）**

  ```sh
  [oracle@ocphwj ~]$ tnsping db19c
  
  TNS Ping Utility for Linux: Version 19.0.0.0.0 - Production on 14-MAY-2026 14:04:43
  
  Copyright (c) 1997, 2019, Oracle.  All rights reserved.
  
  Used parameter files:
  
  Used TNSNAMES adapter to resolve the alias
  Attempting to contact (DESCRIPTION = (ADDRESS = (PROTOCOL = TCP) (HOST = ocphwj.com) (PORT = 1521)) (CONNECT_DATA = (SERVER = DEDICATED) (SERVICE_NAME = db19c)))
  OK (0 msec)
  ```

  

- 服务器进程(Sever Process)

  - 模式：专用服务器进程、共享服务器进程

- 后台进程

  ```sh
  # 查看oracle 后台进程
  ps -ef | grep ora_
  ```

  | 进程名 (缩写) | 英文全称        | 核心职责       | 类比说明                                                     |
  | :------------ | :-------------- | :------------- | :----------------------------------------------------------- |
  | **PMON**      | Process Monitor | 进程监控与清理 | 清理异常退出的进程，释放资源并回滚未完成的事务。             |
  | **SMON**      | System Monitor  | 系统监控与恢复 | 在数据库启动时执行崩溃恢复，并负责清理临时段、合并空闲空间。 |
  | **DBWR**      | Database Writer | 数据写入       | 将数据缓存（Buffer Cache）中被修改的“脏”数据，异步写入磁盘的数据文件。 |
  | **LGWR**      | Log Writer      | 日志写入       | 将内存中的重做日志条目实时、顺序地写入联机日志文件，是确保数据不丢失的第一道防线。 |
  | **CKPT**      | Checkpoint      | 检查点         | 检查点的作用：<br/>1 同步所有的数据文件<br/>2 同步所有的控制文件<br/>3 发送信号通知dbwr写盘 |

```sql
--- 查看scn号和检查点号
SYS@seu>select current_scn,checkpoint_change# from v$database;

CURRENT_SCN CHECKPOINT_CHANGE#
---------- ------------------
   2457055          2454004
SYS@seu>select current_scn,checkpoint_change# from v$database;

CURRENT_SCN CHECKPOINT_CHANGE#
---------- ------------------
   2457062          2454004
   
SYS@seu>alter system checkpoint;
System altered.

SYS@seu>select current_scn, checkpoint_change# from v$database;

CURRENT_SCN CHECKPOINT_CHANGE#
---------- ------------------
   2457112          2457111
```





# database(数据库)

在 Oracle 数据库中，**数据库（Database）** 本质上是存储在磁盘上的一系列**操作系统文件的集合**。
## 1、外部文件

- 初始化参数文件(parameter file)

   ```sql
   SYS@db19c>alter system set db writer processes=2 scope=spfile;
   System altered.
   
   SYS@db19c>show parameter db_writer_processes
   NAME                   TYPE    VALUE
   ---------------------- ------- -----
   db_writer_processes    integer 1
   
   
   SYS@db19c>shutdown immediate
   Database closed.
   Database dismounted.
   ORACLE instance shut down.
   
   SYS@db19c>startup
   ORACLE instance started.
   Total System Global Area 1795159104 bytes
   Fixed Size		    8897600 bytes
   Variable Size		  436207616 bytes
   Database Buffers	 1342177280 bytes
   Redo Buffers		    7876608 bytes
   Database mounted.
   Database opened.
   
   SYS@db19c>show parameter db_writer_processes
   NAME			     TYPE	 VALUE
   -------------------------------- ---- ----------
   db_writer_processes		     integer	 2
   ```

  

- 口令验证文件(password file)

- 归档日志文件(archived log file)

## 2、内部文件

- 数据文件（Data File）

  ```sql
  SYS@db19c>select name from v$datafile;
  NAME
  -----------------
  /u01/app/oracle/oradata/DB19C/system01.dbf
  /u01/app/oracle/oradata/DB19C/sysaux01.dbf
  /u01/app/oracle/oradata/DB19C/undotbs01.dbf
  /u01/app/oracle/oradata/DB19C/demo01.dbf
  /u01/app/oracle/oradata/DB19C/users01.dbf
  ```

  

- 控制文件（Control File)

  ```sql
  SYS@db19c>select name from v$controlfile;
  NAME
  ---------
  /u01/app/oracle/oradata/DB19C/control01.ctl
  /u01/app/oracle/fast_recovery_area/DB19C/control02.ctl
  ```

  

- 联机重做日志文件（Online Redo Log）

  ```sql
  SYS@db19c>select member from v$logfile;
  MEMBER
  ---
  /u01/app/oracle/oradata/DB19C/redo03.log
  /u01/app/oracle/oradata/DB19C/redo02.log
  ```

  

