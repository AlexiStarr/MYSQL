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
### 基本查询
1. 查询多个字段
   ```
   SELECT 字段1,字段2,... FROM 表名;
   SELECT * FROM 表名;#返回所有字段
   ```
   实际开发中最好不写*，不直观。
2. 设置别名
   ```
   SELECT 字段1[AS 别名1],字段2[AS 别名2],... FROM 表名;
   ```
   as可以省略，直接``select 字段 别名 from 表名;``
3. 去除重复记录
   ```
   SELECT DISTINCT 字段列表 FROM 表名;
   ```
### 条件查询
1. 语法
   ```
   SELECT 字段列表 FROM 表名 WHERE 条件列表;
   ```
2. 条件
   ![image](https://github.com/user-attachments/assets/4f0ba0d5-a511-4be2-bf29-5ea479de5d69)
   比较算符&逻辑算符
   
