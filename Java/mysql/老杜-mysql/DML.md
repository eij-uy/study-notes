[toc]

# DML：操作语句

## 插入：insert

### 语法

`insert into 表名(字段名1，字段名2，字段名3...) values(值1，值2，值3)`

注意：字段名和值要一一对应，即数量要对应，数据类型要对应

- `insert into t_student(no,name,sex,age,email) values (1,'张三','n',20,'zhang@123.com')`

- `insert into t_student(email,age,sex,name,no) values ('lisi@123.com',21,'f','李四',2)`

- `insert into t_student(no) values (3)`

- `insrt into t_student() values (2)`

  **insert 语句但凡执行成功了，那么必然会多一条记录 **

  **如果没有给其他字段指定值的话，默认值是 NULL, 默认值可以在创建表时指定，不指定默认是 NULL**

  **insert 语句中的 字段名 可以省略, 但是省略了字段名相当于都写上了，所以值也要都写上**

### 插入多条记录

#### 语法

`insrt into t_user(id,name,birth) values(1,'zs','1980-10-11'),(2,'ls','1980-10-11')...`

## 修改：update

### 语法

`update 表名 set 字段名=值1，字段名2=值2，字段名=值3... where 条件`

注意：没有条件限制会导致所有数据全部更新

- `update t_user set name='jack',birth='2000-10-11' where  id=2`

## 删除：delete

### 语法

`delete from 表名 where 条件`

注意：没有条件，整张表数据会全部删除

* `delete from t_user where id = 2`
* `insert into t_user(id) values(2)`
* `delete from t_user`：删除所有

### delete 删除原理

表中的数据被删除了，但是这个数据在硬盘上的真实存在存储空间不会被释放

- 优点：支持回滚，后悔了可以再恢复数据
- 缺点：删除效率比较低

### 快速删除：DDL 语句

### 语法

`truncate table dept_bak`

### truncate 语句删除数据的原理

物理删除

- 优点：删除效率比较高，表被一次截断，物理删除
- 缺点：不支持回滚