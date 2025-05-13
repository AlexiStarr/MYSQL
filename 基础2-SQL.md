# SQL通用语法
1. 可单行或多行书写，以分号结尾；
2. 可用空格/缩进增强可读性；
3. 不分大小写，关键字建议大写；
4. 注释：
   - 单行：--/#（#是mysql特有）
   - 多行：/**/
# SQL分类
1. DDL
   definiton,数据定义语言，定义数据库、表、字段
2. DML
   manipulation,数据操作语言，对表中数据进行增删改
3. DQL
   query,数据查询语言，对表中数据进行查询
4. DCL
   control,数据控制语言，创建用户、控制用户的权限
## DDL
### 数据库操作
1. 查询
  ```
  --查询所有数据库
  SHOW DATABASES;
  --查询当前数据库
  SELECT DATABASE();
  ```
2. 创建
  ```
  CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则];
  ```
3. 删除
  ```
  DROP DATABASE [IF EXISTS] 数据库名;
  ```
4. 使用
  ```
  USE 数据库名;
  ```
### 操纵表结构
1. 查询表
  ```
  --查询当前数据库所有表，前提是要先进入数据库，使用use命令
  SHOW TABLES;
  --查询表结构
  DESC 表名;
  --查询指定表的建表语句
  SHOW CREATE TABLE 表名;
  ```
2. 创建表
  ```
  CREATE TABLE 表名(
        字段一 字段一类型 [COMMENT 字段一注释],
        字段二 字段二类型 [COMMENT 字段二注释],
        ....
        字段三 字段三类型 [COMMENT 字段三注释]
  )[COMMENT 表注释];
  ```
3. 数据类型
   1. 数值类型
     ![image](https://github.com/user-attachments/assets/6e3bb064-4f9d-4045-a298-63a41bb486cb)
     (int) float double decimal
   2. 字符串类型
     ![image](https://github.com/user-attachments/assets/4a4113ce-74b5-4f07-89f7-f81593f1c2ce)
     char varchar (blob text)
     - char vs varchar
       
           char(10)固定分配十单位空间，但是可以只输入一个字符；
           varchar(10)最高分配十单位空间；
           两者相比较char性能更好，因为后者需要动态分配空间。
   3. 日期类型
     ![image](https://github.com/user-attachments/assets/804a1757-880d-45bf-b35d-5f165b2fe32e)
     date（年月日） time（时分秒） year datetime（年月日时分秒） timestamp（同，最大到2038年）
4. 修改表
  - 添加字段
    ```
    ALTER TABLE 表名 ADD 字段名 类型（长度）[COMMENT 注释] [约束];
    ```
  - 修改数据（字段）类型
    ```
    ALTER TABLE 表名 MODIFY 字段名 新数据类型（长度）;
    ```
  - 修改字段名和字段类型
    ```
    ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型（长度） [COMMENT 注释] [约束];
    ```
  - 删除字段
    ```
    ALTER TABLE 表名 DROP 字段名;
    ```
  - 修改表名
    ```
    ALTER TABLE 表名 RENAME TO 新表名;
    ```
  - 删除表
    ```
    --删除表
    DROP TABLE [IF EXISTS] 表名;
    --删除指定表，并重新创建该表
    TRUNCATE TABLE 表名;
    ```
    TRUNCATE TABLE的作用就是清空表中数据，留下空表。
## DML
对表中记录进行增（insert）删（delete）改（update）。
### 添加数据
1. 给指定字段添加数据
  ```
  INSERT INTO 表名 (字段名1,字段名2,....) VALUES(值1,值2,...);
  ```
2. 给全部字段添加数据
  ```
  INSERT INTO 表名 VALUES(值1,值2,...);
  ```
3. 批量添加数据
  ```
  INSERT INTO 表名 (字段名1,字段名2,...) VALUES(值1,值2,..),(值1,值2,..),(值1,值2,...);
  INSERT INTO 表名 VALUES(值1,值2,...),(值1,值2,...),(值1,值2,...);
  ```
  PS:
   1. 插入数据时，字段顺序与值需一一对应；
   2. 字符串和日期型数据应包含在单引号中；
   3. 插入数据的大小应在字段规定范围内。
### 修改数据
```
UPDATE 表名 SET 字段名1 = 值1 , 字段名2 = 值2, ....[WHERE 条件];
```
条件可有可无，但没有条件会修改整张表中的所有数据。
### 删除数据
```
DELETE FROM 表名 [WHERE 条件]
```
注意：
1. 若没有条件会删除整张表的所有数据；
2. delete语句不能删除某一字段的值（可以用update）
## DQL
查询数据（select）  
基本语法
```
SELECT 字段列表
FROM 表名列表
WHERE 条件列表
GROUP BY 分组字段列表
HAVING 分组后条件列表
ORDER BY 排序字段列表
LIMIT 分页参数
```
### 基本查询（select from）
1. 查询多个字段
   ```
   SELECT 字段1,字段2,... FROM 表名;
   SELECT * FROM 表名;#返回所有字段
   ```
   实际开发中最好不写*，不直观。
2. 设置别名
   ```
   SELECT 字段1[AS 别名1],字段2[AS 别名2],... FROM 表名[AS 别名];
   ```
   as可以省略，直接``select 字段 别名 from 表名;``
3. 去除重复记录
   ```
   SELECT DISTINCT 字段列表 FROM 表名;
   ```
### 条件查询（where）
1. 语法
   ```
   SELECT 字段列表 FROM 表名 WHERE 条件列表;
   ```
2. 条件
   ![image](https://github.com/user-attachments/assets/4f0ba0d5-a511-4be2-bf29-5ea479de5d69)
   - 比较算符&逻辑算符 in like between and is null
   - 注意between A and B（A <= B），从小到大的顺序，范围是[A,B]。
   - in演示
     ![image](https://github.com/user-attachments/assets/88624706-4e4f-4f4a-92ec-a658078610b1)
   - like演示
     - _用法——表示单个字符
        ![image](https://github.com/user-attachments/assets/04071fda-1230-4ec3-944a-c0e4c60cb749)
     - %用法——表示任意字符（长度也任意）
       ![image](https://github.com/user-attachments/assets/8317ac80-aaaa-4b48-879e-093ad200b596)
### 聚合函数（count max min avg sum）
1. 介绍
   将一列数据作为一个整体，进行纵向计算。
2. 常见聚合函数
    ![image](https://github.com/user-attachments/assets/98819e7a-ca82-47e9-aeaa-57ca980db5e0)
   count、max、min、avg、sum
   - 注意：如果聚合函数内的是条件 ， 那么就不能用count而要用sum
     - 例如：count(peoplesex='女')返回的是总行数（因为返回值是true还是false它都计入）；sum(peoplesex='女')则返回的是女性行数（计true为1，false为0）
4. 语法
   ```
   SELECT 聚合函数(字段列表) FROM 表名;
   ```
   注意：所有的NULL值不参与聚合函数运算。
### 分组查询（group by having）
1. 语法
   ```
   SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件];
   ```
2. WHERE & HAVING 区别
   - 时间不同：where过滤分组前，having过滤分组后；
   - 条件不同：where不能判断聚合函数，having可以。
3. 例
   ![image](https://github.com/user-attachments/assets/996d0eb4-8d88-415c-896a-911d998f9523)
   ![image](https://github.com/user-attachments/assets/cf9c9d18-45c2-4b6c-873b-ea8ee0c27f4c)
   注意：
   1. 执行顺序：where > 聚合函数 > having
   2. 分组后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义。![026FDBD3](https://github.com/user-attachments/assets/ec95a0f0-bdea-411d-bc4e-44d8f8bc1a5e)（这个大猩猩笑鼠我了）
### 排序查询（order by）
1. 语法
   ```
   SELECT 字段列表 FROM 表名 ORDER BY 字段一 排序方式1,字段2 排序方式2..;
   ```
   支持多字段排序。
2. 排序方式
   - ASC 升序（默认）
   - DESC 降序
   如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序。
### 分页查询（limit）
- 语法
  ```
  SELECT 字段列表 FROM 表名 LIMIT 起始索引,每页记录数;
  ```
  ![image](https://github.com/user-attachments/assets/3474cfd4-9e2d-48e6-858a-cc3f105a3439)
  注意：
  1. 起始索引从0开始，起始索引 = （查询页码 - 1）* 每页显示记录数；
  2. 分页查询是数据库的方言，mysql中是limit；
  3. 若查询的是第一页数据，起始索引可省略，直接简写为limit 10。
### 执行顺序
1. 编写顺序是： select from where group by having order by limit
2. 执行顺序是：
   ![image](https://github.com/user-attachments/assets/a81df2da-d84d-4a75-9789-c4723411891d)
   通过起别名可以验证：
   ![image](https://github.com/user-attachments/assets/85b84a32-26a2-4b1b-81a9-79b142df2635)
## DCL
管理数据库用户，控制用户访问权限。
### 管理用户
1. 查询用户
   ```
   USE MYSQL;
   SELECT * FROM USER;
   ```
2. 创建用户
   ```
   CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
   ```
   主机名为localhost该用户只能在本地访问数据库，要想在任意主机访问，使用%。
3. 修改用户密码
   ```
   ALTER USER '用户名'@'主机名' IDENTIFIED WITH MYSQL_NATIVE_PASSWORD BY '新密码';
   ```
4. 删除用户
   ```
   DROP USER '用户名'@'主机名';
   ```
5. 注意
   - 主机名可用%通配
   - DCL开发人员使用较少，主要是DBA（数据管理员）在使用。
### 权限控制
![image](https://github.com/user-attachments/assets/9351b607-b79d-4baa-b34d-ca18c97201d1)
1. 查询权限
   ```
   SHOW GRANTS FOR '用户名'@'主机名';
   ```
   USAGE 表示没有权限，仅仅能连接mysql并登录
2. 授予权限
   ```
   GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
   ```
3. 撤销权限
   ```
   REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
   ```
4. 注意
   - 多个权限间需用逗号分隔
   - 授权时，数据库名和表名可用*通配

   

   

     
