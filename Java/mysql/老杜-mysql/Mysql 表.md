

[toc]

# 表

## 创建表的语法格式：DDL语句

`create table 表名(字段名1 数据类型，字段名2 数据类型...)`

​	表名建议以 `t_` 或者 `tb_` 开始，可读性强，见名知意，字段名也要见名知意

### 约束：constraint，非常重要

#### 什么是约束

在创建表的时候，我们可以给表中的字段加上一些约束，来保证这个表中数据的完整性，有效性，约束的作用就是为了保证表中的数据有效

#### 常见的约束

- 非空约束：`not null`

  **非空约束 not null 约束的字段不能为 null **

  **not null 没有表级约束，只有列级约束**

  ~~~sql
  create table t_vip (
  	id int,
      name varchar(255) not null,
      email varchar(255)
  )
  
  # 这样写之后 name 在插入数据的时候为空就会报错
  # 报错
  insert into t_vip(id) values(1);
  ~~~

- 唯一性约束：`unique`

  **唯一性约束 unique 约束的字段不能重复，但是可以为 NULL，多个也可以为 NULL**

  ~~~sql
  # 单个字段唯一
  create table t_vip1 (
  	id int unique,
      name varchar(255),
      email: varchar(255)
  )
  insert into t_vip(id) values(1);
  # 这里会报错，以为有两个同样的 id，但是 id 是不能重复的
  insert into t_vip(id) values(1);
  
  # 多个字段联合唯一
  create table t_vip2 (
  	id int,
      name varchar(255),
      email: varchar(255),
      unique(name,email)
  )
  # 这里就是 name 和 email 两个字段联合起来唯一
  # 这种约束没有添加在列的后面的约束被称为 表级约束
  
  # unique 和 not null 联合
  create table t_vip3(
  	id int not null unique,
      name varchar(255),
      email: varchar(255)
  )
  # 在 mysql 当中，如果一个字段同时被 not null 和 unique 约束了，该字段会自动变成主键字段 
  ~~~

- 主键约束：`primary key(简称 PK)`

  相关术语：

  - 主键约束：就是一种约束
  - 主键字段：该字段上添加了主键约束，那么这个字段就是主键字段
  - 主键值：主键字段中的每一个值都叫主键值

##### 什么是主键

  - 主键值是每一行记录的唯一标识
  - 主键值是每一行记录的身份证号

  **任何一张表都应该有主键，没有主键的话，表无效**

##### 主键的特征

  - `not null + unique`：主键值不能是 NULL，同时也不能重复

  - 主键只能有一个

  - 主键值建议使用 `int,bigint,char 等类型`

    不建议使用 varchar 来做主键，主键值一般都是数字，一般都是定长的

##### 主键分类

  - 自然主键：用的多

    主键值是一个自然数，和业务没关系

  - 业务主键

    主键值和业务紧密关联，例如拿银行卡账号做主键值，这就是业务主键

  **自然主键使用较多，因为主键只要做到不重复就行，不需要有意义，业务主键不好，因为主键一旦和业务挂钩，那么当业务发生变动的时候可能会影响到主键值，所以业务主键不建议使用，尽量使用自然主键**

  ~~~~sql
  # 单一主键
  create table t_vip1 (
  	id int primary key,
      name varchar(255)
  )
  
  insert into t_vip1(id,name) values(1,'zs');
  insert into t_vip1(id,name) values(2,'ls');
  insert into t_vip1(id,name) values(2,'ww'); # 报错，主键值不能相同 
  insert into t_vip1(name) values('zl'); # 报错，主键值不能为 NULL
  
  # 复合主键
  create table t_vip2 (
  	id int,
      name varchar(255),
      primary key(id,name)
  )
  # 这是可以的，因为这是符复合主键
  # 不建议使用，因为主键存在的意义就是这行记录的身份证号，只要意义达到即可，单一主键可以做到，复合主键比较复杂不建议使用
  insert into t_vip2(id,name) values(1,'zs');
  insert into t_vip2(id,name) values(1,'ls');
  
  # 自动维护主键值
  create table t_vip3 (
  	id int primary key auto_increment,
     name varchar(255)
  )
  
  insert into t_vip(name) values('zs');
  insert into t_vip(name) values('zs');  
  insert into t_vip(name) values('zs');  
  insert into t_vip(name) values('zs');  
  # 这时 id 会自动自增
  # auto_increment 表示从 1 开始自增
  ~~~~

- 外键约束：`foreign key(简称 FK)`

  相关术语：

  - 外键约束：一种约束
  - 外键字段：该字段上添加了外键约束
  - 外键值：外键字段当中的每一个值

  业务背景：设计数据库表，来描述 班级和学生 的信息

  ##### 外键的思考

  - 外键值可以为 NULL
  - 子表的外键引用父表中的某个字段，被引用的这个字段不一定非要是主键，但是至少要具有 unique 约束

  ~~~sql
  # 第一种方案 班级和学生存储在一张表中
  create table t_student(
  	id int,
      name varchar(255),
      classno int,
      className: varchar(32)
  )
  # 缺点：数据冗余，空间浪费
  
  # 第二种方案：班级一张表，学生一张表
  # t_class 班级表
  # t_student 学生表
  create table t_class(
      classno int primary key,
      className varchar(255)
  )
  create table t_student(
  	classno int primary key auto_increment,
      name varchar(255),
      classno int
  )
  # 这里的 classno 如果没有任何约束的话，可能会出现无效的 classno 比如说 t_class 中没有的 classno
  # 所以为了保证 classno 字段中的值是有效的，需要给 classno 字段添加外键约束
  # 那么：classno 字段就是外键字段，classno 字段中每一个值都是外键值
  # 所以 t_student 表应该这样创建
  create table t_student(
  	classno int primary key auto_increment,
      name varchar(255),
      classno int，
      foreign key(classno) references t_class(classno)
  )
  
  insert into t_class(classno,className) values(100,'aaa');
  insert into t_class(classno,className) values(101,'bbb');
  
  insert into t_student(name,classno) values('jack',100);
  insert into t_student(name,classno) values('lucy',100);
  insert into t_student(name,classno) values('lilei',100);
  insert into t_student(name,classno) values('hanmeimei',101);
  insert into t_student(name,classno) values('zhangsan',101);
  insert into t_student(name,classno) values('lisi',101);
  ~~~

- 检查约束：`check(mysql 不支持，oracle 支持)`

### 数据类型

- varchar：可变长度的字符串，**最长 255**

  **比较智能，节省空间，会根据实际的数据长度动态分配空间**

  - 优点：节省空间
  - 缺点：需要动态分配空间，速度慢

- char：定长字符串，**最长 255**

  **不管实际的数据长度是多少，分配固定长度的空间取存储数据，**

  **使用不恰当的时候，可能会导致空间的浪费**

  - 优点：不需要动态分配空间，速度快
  - 缺点：使用不当可能会导致空间的浪费

- int：数字中的整数型，等同于 java 的 int，**最长 11**

- bigint：数字中的长整型，等同于 java 中的 long

- float：单精度浮点型数据

- double：双精度浮点型数据

- date：短日期类型

- datetime：长日期类型

- clob：字符大对象

  **最多可以存储 4G 的字符串 **

  **比如：存储一篇文章，存储一个说明**

  **超过 255 字符的都要采用 CLOB 字符大对象来存储**

- blob：二进制大对象

  **专门用来存储图片、声明、视频等流媒体数据 **

  **往 BLOB 类型的字段上插入数据的时候，例如插入一个图片、视频等 需要使用 IO 流才行**

#### date 和 datetime 的区别

- date 是短日期：只包括年月日信息
- datetime是长日期：包括年月日时分秒信息

**通过 now 函数获取系统当前时间**

#### 测试

- 创建 `t_move` 表 ( 专门用来存储电影信息的 )
  - 编号：no ( bigint )
  - 名字：name( varchar )
  - 描述信息：description ( clob )
  - 上映日期：playtime ( date )
  - 时长：time ( double )
  - 海报：image ( blob )
  - 类型：type ( char )
- 创建一个学生表
  - no ( int ( 3 ) ) 
  - name ( varchar ( 32 ) )
  - sex ( char ( 1 ) ) default 'm'
  - age ( int ( 3 ) )
  - email ( varchar ( 255 ) )

**在后面添加 default 意思是创建数据时如果不穿的话显示的不是 NULL，而是 m**

## 删除表

### 语法

- `drop table 表名`：这样删当这张表不存在时会报错
- `drop table if exists 表名`：如果这张表存在的话就删除

## 快速复制表

### 全部复制

`create table emp2 as select * from emp;`

**将后面的查询结果当做一个表新建，创建表的同时，数据也同步过来**

### 选择复制

`create table mytable as select empno,ename from emp where jon = 'MANAGER'`

### 将查询结果插入到表中：很少用

1. `create table dept_bak as select * from dept`

   创建 dept_bak 表

2. `insert into dept_bak as select * from dept`

### 对表结构的增删改：用的较少

#### 添加字段

`alter table 表名 add 字段名 字段类型`

#### 修改字段

`alter table 表名 modify 字段名 字段类型`

#### 删除字段

`alter table 表名 drop 字段名`

## 存储引擎 ( 了解即可 )

### 什么是存储引擎

存储引擎是 MySql 中特有的一个术语，其他数据库中没有 ( Oracle 中也有，但是不叫这个名字 )

存储引擎是一个表 存储 / 组织数据的方式

不同存储引擎，表存储数据的方式不同

### 怎么给表指定存储引擎

在建表的时候可以在最后 给表指定存储引擎、

- 使用 ENGINE 来指定存储引擎
- 使用 CHARSET 来指定这张表的字符编码方式

**mysql 默认的存储引擎是： InnoDB **

**mysql默认的字符编码方式是：utf8**

~~~sql
# 建表时指定存储引擎和字符编码方式
create table t_product(
	id int primary key,
    name varchar(255)
) engine=InnoDB default charset=gbk
~~~

### mysql 支持的存储引擎

#### 查看命令：`show engines \G`

### mysql 常用存储引擎

- MyISAM

  它管理的表具有以下特征

  - 使用三个文件表示每个表
    - 格式文件 ---> 存储表结构的定义 ( mytable.frm )

    - 数据文件 ---> 存储表行的内容 ( mytable.MYD )

    - 索引文件 ---> 存储表上索引 ( mytable.MYI )

      索引是一本书的目录，可以减小扫描的范围，提高检索效率

      对于一张表来说，只要是主键，或者加有 unique 约束的字段上会自动创建索引

  - 可以被转换为压缩、只读表来节省空间，这是 MyISAM 引擎的优势

  MyISAM 不支持事务机制，安全性低

- InnoDB

  - 这是 mysql 默认的存储引擎，同时也是一个重量级的存储引擎

  - InnoDB 支持实物，支持数据库崩溃后自动恢复机制
  - 最主要的特点就是非常安全

  它管理的表具有以下特征

  - 每个 InnoDB 表在数据库目录中以 .frm 格式文件表示
  - InnoDB 表空间 tablespace 被用于存储表的内容 ( 表空间是一个逻辑名称。表空间存储数据 + 索引 )
  - 提供一组用来记录事务性活动的日志文件
  - 用 COMMIT ( 提交 )、SAVEPOINT 及 ROLLBACK ( 回滚 ) 支持事务处理
  - 提供全 ACID 兼容
  - 在 MySQL 服务器崩溃后提供自动恢复
  - 多版本 ( MVCC ) 和行级锁定
  - 支持外键及引用的完整性，包括级联删除和更新

  InnoDB 最大的特点就是支持事务，以保证数据的安全。效率不是很高，并且也不能压缩，不能转换为只读，不能很好的节省存储空间

- MEMORY

  使用 MEMORY 存储引擎的表，其数据存储在内存中，且行的长度固定，这两个特点使得 MEMORY 存储引擎非常快

  它管理的表具有以下特征

  - 在数据库目录内，每个表均以 .frm 格式的问价你表示
  - 表数据及索引被存储在内容中
  - 表级锁机制
  - 不能包含 TEXT 或 BLOB 字段

  MEMORY 存储引擎以前被称为 HEAP 引擎

  优点：查询效率是最高的，不需要和硬盘交互

  缺点：不安全，关机之后数据消失，因为数据和索引都是在内存当中