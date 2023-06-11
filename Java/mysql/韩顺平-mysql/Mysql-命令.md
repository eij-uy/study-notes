[toc]

# Mysql 命令

## 前置服务命令

- 启动 mysql 服务：`net start mysql80`
- 停止 mysql 服务：`net stop mysql80`
- 连接 mysql 数据库：`mysql -h 主机名 -P 端口 -u 用户名 -p密码`
  - 登录前要保证 mysql 服务是运行状态
  - -p密码不要有空格
  - -p后面没有写密码，回车会要求输入密码
  - 如果没有写 -h 主机，默认就是主机
  - 如果没有写 -P 端口，默认就是 3306
  - 在实际工作中，3306 一般修改

## 操作命令

### 操作数据库命令

* 创建数据库

  ~~~java
  CREATE DATABASE [IF NOT EXISTS] 数据库名称 [create_specification [, create_specification]...];
  
  create_specification:
  [DEFAULT] CHARACTER SET charset_name;
  [DEFAULT] COLLATE collation_name;
  
  ~~~

// 例子
  // 创建一个使用 utf8 字符集，并带校对规则 utf8_bin 的 db01 数据库
  CREATE DATABASE db01 CHARACTER SET utf8 COLLATE utf8_bin;
  ~~~
  
  - CHARACTER SET：指定数据库采用的字符集，如果不指定字符集，默认utf8
  - COLLATE：指定数据库字符集的校对规则，默认是 `utf8_general_ci`, 常用的有：
    - `utf8_bin`：区分大小写
    - `utf8_general_ci`：不区分大小写
  
  **练习：**
  
  1. 创建一个名称为 yj_db01 的数据库
  2. 创建一个使用 utf8 字符集的 yj_db02 的数据库
  3. 创建一个使用 utf8 字符集，并带校对规则的 yj_db03 数据库
  
  ~~~sql
  # 1. 创建一个名称为 yj_db01 的数据库
  CREATE DATABASE yj_db01;
  # 2. 创建一个使用 utf8符集的 yj_db02 的数据库
  CREATE DATABASE yj_db02 CHARACTER SET utf8;
  # 3. 创建一个使用 utf8 字符集，并带校对规则的 yj_db03 数据库
  CREATE DATABASE yj_db02 CHARACTER SET utf8 COLLATE utf8_bin;
  ~~~

* 显示数据库：`SHOW DATABSES`

* 显示数据库的定义信息【包含创建语句，字符集等】：`SHOW CREATE DATABASE 数据库名称`

  **在创建数据库/表的时候，为了规避关键字，可以使用反引号解决**

  **比如说，想要创建一个 名字叫 CREATE 的数据库，直接创建会报错，要使用反引号 ( \` ) 引起来**

* 删除数据库：`DROP DATABASE [IF EXISTS] 数据库名称`

  **慎用**

* 备份数据库 ( 在 dos 下执行 )

  `mysqldump -u 用户名 -p密码 -B 数据库1 数据库2... 数据库n > 要保存的全路径，一个sql文件`  备份数据库

  `mysqldump -u 用户名 -p密码 数据库 表1 表2 ... 表n > 要保存的全路径，一个sql文件` 备份表

* 恢复数据库 ( 注意：进入 Mysql 命令行再执行 )

  `source 备份时的 sql 文件的路径`

  ~~~sql
  # 备份恢复数据库
  # 这个备份的文件，就是对应的 sql 语句
  mysqldump -u 用户名 -p密码 -B 数据库1 数据库2 ... > d:\\bak.sql; // 备份数据库 到 d 盘的 bak.sql 文件
  
  # 恢复数据库 (注意：要先进入 Mysql 命令行)
  # 第一个方法
  source d:\\bak.sql;
  # 第二个方法：直接将 bak.sql 的内容放到查询编辑器中执行即可
  ~~~

### 操作表命令

- 创建表

  ~~~sql
  CREATE TABLE 表名称
  (
  	field1 datatype,
      field2 datatype,
      field3 datatype
  ) character set 字符集 collate 校对规则 engine 存储引擎
  
  field：指定列名
  datatype：指定列类型 (字段类型)
  character：如不指定则为所在数据库字符集
  collate：如不指定则为所在数据库校对规则
  engine：引擎 (这个设计内容较多，后面单独说)
  
  # 案例
  # 创建 user 表，有 id，name，password，birthday 四个列
  CREATE TABLE `user` (
  	id INT,
      `name` VARCHAR(255),
      `password` VARCHAR(32),
      `birthday` DATE
  ) CHARACTER SET utf8 COLLATE utf8_bin ENGINE INNODB;
  ~~~

