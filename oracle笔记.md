# Oracle学习笔记大纲

1. Oracle数据库基础
   - 数据库体系结构
   - 常用连接方式
   - 用户与权限管理

2. 数据对象与数据类型
   - 常用数据类型
   - 表、视图、索引、约束

3. SQL基础语法
   - 查询（SELECT）
   - 条件与排序
   - 分组与聚合
   - 连接查询（JOIN）
   - 子查询
   - 其他常用语法（别名、行数限制等）

4. PL/SQL编程
   - 块结构与变量
   - 流程控制
   - 游标与异常处理
   - 存储过程、函数、触发器

5. 性能优化与管理
   - 索引与执行计划
   - 常用视图与工具
   - 备份与恢复
   - 故障排查

---

## Oracle基础

## Oracle数据库基础

### 1.1 数据库体系结构
- Oracle数据库由实例（Instance）和数据库（Database）组成。
- 实例包括内存结构（SGA、PGA）和后台进程（如SMON、PMON、DBWR、LGWR等）。
- 数据库由表空间、数据文件、控制文件、重做日志文件等组成。
- 常用视图：
  ```sql
  SELECT INSTANCE_NAME, STATUS FROM V$INSTANCE; -- 查看实例信息
  SELECT NAME FROM V$DATABASE; -- 查看数据库名
  SELECT TABLESPACE_NAME FROM DBA_TABLESPACES; -- 查看表空间
  ```

### 1.2 常用连接方式
- 以系统管理员身份连接数据库：
  ```sql
  conn / as sysdba           -- 本地操作系统认证
  conn system/admin          -- 仅本地，无需listener
  conn system/admin@orcl     -- 远程或本地，需listener
  ```
- 远程连接需配置监听（listener.ora）和服务名（tnsnames.ora）。

### 1.3 用户与权限管理
- 创建用户：
  ```sql
  CREATE USER 用户名 IDENTIFIED BY 密码;
  ```
- 授权与回收权限：
  ```sql
  GRANT CONNECT, RESOURCE TO 用户名;
  REVOKE RESOURCE FROM 用户名;
  ```
- 修改密码与锁定/解锁：
  ```sql
  ALTER USER 用户名 IDENTIFIED BY 新密码;
  ALTER USER 用户名 ACCOUNT LOCK;
  ALTER USER 用户名 ACCOUNT UNLOCK;
  ```
- 删除用户：
  ```sql
  DROP USER 用户名 CASCADE;
  ```

## 数据对象与数据类型

### 2.1 常用数据类型
- NUMBER：数值型，可指定精度和小数位。
- VARCHAR2(n)：变长字符串，最大长度n。
- CHAR(n)：定长字符串，最大长度n。
- DATE：日期和时间。
- TIMESTAMP：带有更高精度的日期时间。
- CLOB/BLOB：大文本/二进制对象。

### 2.2 表（Table）
- 用于存储结构化数据。
- 创建表：
  ```sql
  CREATE TABLE 表名 (
    id NUMBER PRIMARY KEY,
    name VARCHAR2(50) NOT NULL,
    created_at DATE
  );
  ```
- 删除表：
  ```sql
  DROP TABLE 表名;
  ```

### 2.3 视图（View）
- 逻辑上的虚拟表，基于SQL查询结果。
- 创建视图：
  ```sql
  CREATE VIEW 视图名 AS SELECT 字段 FROM 表名 WHERE 条件;
  ```
- 删除视图：
  ```sql
  DROP VIEW 视图名;
  ```

### 2.4 索引（Index）
- 提高查询效率，常用于主键、唯一约束等字段。
- 创建索引：
  ```sql
  CREATE INDEX 索引名 ON 表名(字段);
  ```
- 删除索引：
  ```sql
  DROP INDEX 索引名;
  ```

### 2.5 约束（Constraint）
- 保证数据完整性和一致性。
- 常见约束类型：
  - PRIMARY KEY：主键，唯一且非空。
  - UNIQUE：唯一约束。
  - NOT NULL：非空约束。
  - FOREIGN KEY：外键约束。
  - CHECK：检查约束。
- 示例：
  ```sql
  CREATE TABLE student (
    id NUMBER PRIMARY KEY,
    name VARCHAR2(20) NOT NULL,
    age NUMBER CHECK (age >= 0),
    class_id NUMBER,
    CONSTRAINT fk_class FOREIGN KEY (class_id) REFERENCES class(id)
  );
  ```

## SQL基础语法

### 3.1 查询（SELECT）
- 基本查询：
  ```sql
  SELECT * FROM 表名;
  SELECT 字段1 || 字段2 FROM 表名； 拼接操作
  ```
- 指定字段查询：
  ```sql
  SELECT 字段1, 字段2 FROM 表名;
  ```
- 去重查询：
  ```sql
  SELECT DISTINCT 字段 FROM 表名;
  ```

### 3.2 条件与排序
- 条件查询：
  ```sql
  SELECT * FROM 表名 WHERE 条件;
  ```
- 排序：
  ```sql
  SELECT * FROM 表名 ORDER BY 字段 [ASC|DESC];
  ```
- 组合条件（AND/OR）：
  ```sql
  SELECT * FROM 表名 WHERE 条件1 AND/OR 条件2;
  ```

### 3.3 分组与聚合
- 分组：
  ```sql
  SELECT 字段, COUNT(*) FROM 表名 GROUP BY 字段;
  ```
- 常用聚合函数：SUM, AVG, MAX, MIN, COUNT
- 分组后筛选（HAVING）：
  ```sql
  SELECT 字段, COUNT(*) FROM 表名 GROUP BY 字段 HAVING COUNT(*) > 1;
  ```

### 3.4 连接查询（JOIN）
- 内连接（INNER JOIN）：
  ```sql
  SELECT * FROM 表1 INNER JOIN 表2 ON 表1.字段 = 表2.字段;
  ```
- 左连接（LEFT JOIN）：
  ```sql
  SELECT * FROM 表1 LEFT JOIN 表2 ON 表1.字段 = 表2.字段;
  ```
- 右连接（RIGHT JOIN）：
  ```sql
  SELECT * FROM 表1 RIGHT JOIN 表2 ON 表1.字段 = 表2.字段;
  ```

### 3.5 子查询
- 标量子查询、IN/EXISTS 子查询：
  ```sql
  SELECT * FROM 表名 WHERE 字段 IN (SELECT 字段 FROM 其他表 WHERE ...);
  ```

### 3.6 其他常用语法
- 别名：
  ```sql
  SELECT 字段 AS 别名 FROM 表名;
  ```
- 限制返回行数（Oracle 12c+）：
  ```sql
  SELECT * FROM 表名 FETCH FIRST 10 ROWS ONLY;
  ```

---

## PL/SQL编程

### 4.1 块结构与变量
- PL/SQL程序由块（Block）组成，每个块包含：
  - 声明部分（可选）：定义变量、常量、游标等。
  - 可执行部分：包含SQL语句和PL/SQL语句。
  - 异常处理部分（可选）：处理运行时错误。
它是ORACLE数据库专有的过程化编程语言。
- 示例：
  ```sql
  DECLARE
    v_name VARCHAR2(50);
  BEGIN
    SELECT name INTO v_name FROM 表名 WHERE id = 1;
    DBMS_OUTPUT.PUT_LINE('Name: ' || v_name);
  EXCEPTION
    WHEN NO_DATA_FOUND THEN
      DBMS_OUTPUT.PUT_LINE('没有找到数据');
  END;
  ```

### 4.2 流程控制
- 条件语句：IF...THEN...ELSE
- 循环语句：FOR、WHILE、LOOP
- 示例：
  ```sql
  DECLARE
    v_count NUMBER := 0;
  BEGIN
    FOR rec IN (SELECT * FROM 表名) LOOP
      v_count := v_count + 1;
    END LOOP;
    DBMS_OUTPUT.PUT_LINE('总记录数: ' || v_count);
  END;
  ```

### 4.3 游标与异常处理
- 游标：用于处理查询结果的指针。
  - 显式游标：需要手动打开、关闭。
  - 隐式游标：系统自动管理。
- 异常处理：使用EXCEPTION关键字处理异常。
- 示例：
  ```sql
  DECLARE
    CURSOR c1 IS SELECT * FROM 表名;
    v_id 表名.id%TYPE;
  BEGIN
    OPEN c1;
    LOOP
      FETCH c1 INTO v_id;
      EXIT WHEN c1%NOTFOUND;
      DBMS_OUTPUT.PUT_LINE('ID: ' || v_id);
    END LOOP;
    CLOSE c1;
  EXCEPTION
    WHEN OTHERS THEN
      DBMS_OUTPUT.PUT_LINE('发生错误：' || SQLERRM);
  END;
  ```

### 4.4 存储过程、函数、触发器
- 存储过程（Procedure）：一组预编译的SQL语句和PL/SQL语句的集合。
- 函数（Function）：类似于存储过程，但有返回值。
- 触发器（Trigger）：在特定事件发生时自动执行的PL/SQL块。
- 示例：
  ```sql
  CREATE OR REPLACE PROCEDURE proc_name IS
  BEGIN
    -- 过程逻辑
  END;
  ```
  ```sql
  CREATE OR REPLACE FUNCTION func_name RETURN NUMBER IS
  BEGIN
    RETURN 1;
  END;
  ```
  ```sql
  CREATE OR REPLACE TRIGGER trg_name
  BEFORE INSERT ON 表名
  FOR EACH ROW
  BEGIN
    :NEW.created_at := SYSDATE;
  END;
  ```

---

## 性能优化与管理

### 5.1 索引与执行计划
- 索引：提高查询性能的数据结构。
- 执行计划：查询语句的执行步骤，使用EXPLAIN PLAN查看。
- 示例：
  ```sql
  EXPLAIN PLAN FOR SELECT * FROM 表名 WHERE 条件;
  SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY());
  ```

### 5.2 常用视图与工具
- DBA_TAB_PRIVS：用户权限
- DBA_INDEXES：索引信息
- DBA_SEGMENTS：段信息
- V$SESSION：当前会话
- V$SQL：SQL语句

### 5.3 备份与恢复
- 备份：数据的完整副本，使用RMAN、数据泵等工具。
- 恢复：从备份中恢复数据。
- 示例：
  ```sql
  RMAN> BACKUP DATABASE;
  RMAN> RESTORE DATABASE;
  ```

### 5.4 故障排查
- 常见问题：连接失败、查询慢、数据不一致等。
- 排查步骤：
  - 检查网络连接
  - 检查数据库状态
  - 检查用户权限
  - 检查SQL语句
  - 检查执行计划
  - 检查日志文件
  - 使用DBA工具监控数据库性能。
