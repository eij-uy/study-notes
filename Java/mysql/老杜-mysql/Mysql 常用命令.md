[toc]

# Mysql 数据库

数据库当中最基本的单元是表：table，数据库最基本的单元是表，以表的形式表示数据，因为表比较直观

任何一张表都有行和列

- 行 ( row )：被称为数据 / 记录

- 列 ( column )：被称为字段

  每一个字段都有：字段名、数据类型、约束等属性
  
## Mysql 常用命令

- **命令不区分大小写**

- mysql 是不见 **分号** 不执行，**分号** 代表结束
- `\c`用来终止一条命令的输入

| 命令                                     | 含义                       |
| ---------------------------------------- | -------------------------- |
| `mysql -u 用户名 -p密码`                 | 登录 mysql                 |
| `select version()`                       | 查看 mysql 版本号          |
| `show databases`                         | 查看 mysql 中有哪些数据库  |
| `create database 数据库名`               | 创建数据库                 |
| `use 数据库名`                           | 使用某个数据库             |
| `select database()`                      | 查看当前使用的是哪个数据库 |
| `show tables`                            | 查看当前数据库下有哪些表   |
| `describe 表名`  可以缩写为  `desc 表名` | 查看表的结构               |
| `exit`                                   | 退出 mysql                 |


## SQL 语句的分类

SQL 语句有很多，最好进行分类，容易记忆

- DQL：数据查询语言 ( 凡是担忧 select 关键字的都是查询语句 )

  - select：查

- DML：数据操作语言 ( 凡是对表当中的数据进行增删改的都是 DML )

  - insert：增

  - delete：删

  - update：改

    这个主要是操作表中的数据 data

- DDL：数据定义语言 ( 凡是带有 create、drop、alter 的都是 DDL )

  DDL 主要操作的是表的结构，不是表中的数据

  - create：新建，等同于增

  - drop：删除

  - alter：修改

    这个增删改和 DML 不同，这个主要是对表结构进行操作

- TCL

  是事务控制语言

  - commit：事务提交
  - rollback：事务回滚

- DCL

  是数据控制语言

  - grant：授权

  - revoke：撤销权限

    ...

## SQL语句

强调：

- 对于 SQL 语句来说，是通用的
- 所有的 SQL 语句以 **分号** 结尾
- 另外 SQL 语句不区分大小写，都行
