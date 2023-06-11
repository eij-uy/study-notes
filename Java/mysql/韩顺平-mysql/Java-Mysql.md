[toc]

# Java 操作 Mysql

## 案例

1. 创建一个商品 yj_goods 表，选用适当的数据类型
2. 添加两条数据
3. 删除表 yj_goods

~~~java
// 记载类，得到 mysql 连接
Class.forName("com.yujie.jdbc.Driver");
Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306");

// 这里可以编写 sql 语句【create, select, insert, update, delete...】
// String sql = "";
// 1. 创建一个商品 yj_goods 表，选用适当的数据类型
// 创建的命令
// String sql = "create table yj_goods( id int, name varchar(32), price double, introduce text)"
// 2. 添加数据
// String sql = "insert into yj_goods values(1,'华为手机', 2000, '这时不错的一款手机')"
// 3. 删除表
// String sql = "drop table yj_goods"

// 得到 statement 对象, 把 sql 语句发送给 mysql 执行
Statement statement = connection.createStatement();
statement.executeUpdate(sql);

// 关闭连接
statement.close();
connection.close();
~~~