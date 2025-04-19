## 数据库相关概念
1. 数据库（DB）：存储数据的仓库，使数据有组织地进行存储。
2. 数据库管理系统：操纵管理数据库的大型软件。
3. sql：操作关系型数据库的编程语言，是一套标准。
4. 主流关系型数据库管理系统：
   oracle、mysql、SQLserver、postgresql、sqlite（嵌入式微型数据库）...
## mysql安装及基本操作
- 手动打开关闭mysql
  ```
  net start mysql
  net stop mysql
  ```
  注意要以管理员身份打开命令行运行。  
  也可以通过win+r输入services.msc进行打开关闭。
- 操作mysql--连接到客户端
  1. 使用mysql自带的命令行客户端（两个都可以）
  2. 用win的命令行进行连接
     ```
     mysql [-h 127.0.0.1] [-P 3306] -u root -p
     ```
     括号中的可省。
## mysql数据库的数据模型
DBMS数据库管理系统，是一个软件，用来创建、操作数据库。
