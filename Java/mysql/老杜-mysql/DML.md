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