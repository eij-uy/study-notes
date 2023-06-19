[toc]

### 使用 statement

~~~java
// 1. 注册驱动
/**
     * TODO:
     *    依赖：
     *      驱动版本 8+ com.mysql.cj.jdbc.Driver
     *       驱动版本 5+ com.mysql.jdbc.Driver
     */
DriverManager.registerDriver(new Driver());
// 2. 获取连接
/**
     * TODO:
     *    java 程序要和数据库创建连接
     *    java 程序要连接数据库，肯定是调用某个方法，方法也需要填入连接数据库的连接信息：
     *        1. 数据库的ip地址
     *        2. 数据库端口号
     *        3. 账号
     *        4. 密码
     *        5. 连接数据库的名称：yyj
     */
/**
     * 参数1：url：数据库的url
     *      jdbc:数据库厂商名://ip地址:port/数据库名
     *      jdbc:mysql://127.0.0.1:3306/my_test
     * 参数2：userName：数据库的账号
     * 参数3：password 数据库的密码
     */
// java.sql  接口 = 实现类
Connection connection = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/my_test","root","319612");
// 3. 创建 statement
Statement statement = connection.createStatement();
// 4. 发送 SQL 语句，并获取返回结果
String sql = "SELECT * FROM t_user";
ResultSet resultSet = statement.executeQuery(sql);
// 5. 结果集解析
// 查看有没有下一行数据，有就获取
while (resultSet.next()){
    int id = resultSet.getInt("id");
    String account = resultSet.getString("account");
    String password = resultSet.getString("password");
    String nickname = resultSet.getString("nickname");
    System.out.println(id + "--" + account + "--" + password + "--" + nickname);
}
// 6. 关闭资源
resultSet.close();
statement.close();
connection.close();
~~~

#### 例子

- 模拟 用户登录

  ~~~java
  /**
       * TODO：要做的事
       *    1. 输入账号和密码
       *    2. 进行数据库信息查询
       *    3. 反馈登陆成功还是登录失败
       */
  /**
       * TODO：详细
       *    1. 键盘输入时间，收集账号和密码信息
       *    2. 注册驱动
       *    3. 获取连接
       *    4. 创建 statement
       *    5. 发送查询 SQL 语句，并获取返回结果
       *    6. 结果判断，显示登录成功还是失败
       *    7. 关闭资源
       */
  // 1. 获取用户输入信息
  Scanner scanner = new Scanner(System.in);
  System.out.println("请输入账号:");
  String account = scanner.nextLine();
  System.out.println("请输入密码");
  String password = scanner.nextLine();
  
  // 2. 注册驱动
  /**
       * 方案1：
       *    DriverManager.registerDriver(ne com.mysql.cj.jdbc.Driver())
       *    注意：8+ com.mysql.cj.jdbc.Driver
       *         5+ com.mysql.jdbc.Driver
       *    问题：注册两次驱动
       *      1. DriverManager.registerDriver() 方法本身会注册一次
       *      2. Driver.static{ DriverManager.registerDriver() } 静态代码块，也会注册一次
       *     解释
       *      支出法静态代码块即可
       *     触发静态代码块
       *      类加载机制：类加载的机制，会触发静态代码块
       *          加载【class文件加载到 jvm 虚拟机的 class 对象】
       *          连接【验证(检查文件类型)  --->  准备(静态变量默认值)  --->  解析(触发静态代码块)】
       *          初始化【静态属性赋真实值】
       *      触发类加载的方案
       *        1. new 关键字
       *        2. 调用静态方法
       *        3. 调用静态属性
       *        4. 接口 1.8 default 默认实现
       *        5. 反射
       *        6. 子类触发父类
       *        7. 程序的入口 main
       */
  // 方案一：会注册两次
  // DriverManager.registerDriver(new Driver());
  // 方案二：这种方案很不灵活，比如说切换数据库(mysql ---> oracle)的时候，需要自己来改动代码
  // new Driver();
  // 方案三：这也会触发类加载，触发静态代码块的调用
  // 这种方案可以将字符串提取到外部的配置文件，所以这种方案比较好，比较灵活
  // 可以再不改变代码的情况下，完成数据库驱动的切换
  Class.forName("com.mysql.cj.jdbc.Driver");
  
  // 3. 获取数据库连接
  /**
       * getConnection 方法是一个重载方法！
       *  它允许开发者，用不同的形式传入数据库连接的核心参数！
       *  核心属性：
       *    1. 数据库软件所在的主机的 ip 地址
       *    2. 数据库软件所在的主机的端口号
       *    3. 连接的具体库
       *    4. 连接的账号
       *    5. 连接的密码
       *    6. 可选的信息
       *
       *   三个参数：
       *      1. String url           数据库软件所在的信息，连接的具体库，以及其他可选信息
       *                                具体格式：jdbc:数据库管理软件名称(mysql,oracle...)://ip地址|主机名:端口号/数据库名?key=value
       *                                &key=value 的形式填入一些可选信息
       *                              本机的省略写法：如果你的数据库软件安装到本机，并且端口号是3306，可以进行一些省略
       *                                jdbc:数据库管理软件名称:///数据库名
       *      2. String user          数据库的账号
       *      3. String password      数据库的密码
       *   两个参数
       *      String url              此 url 和三个参数的 url 的作用一样，数据库 ip，端口号，具体的数据库和可选信息
       *      Properties info         存储账号和密码
       *                              Properties 类似于 Map，只不过 key 和 value 都是字符串形式的
       *                              key user：账号信息
       *                              key password：密码信息
       *   一个参数
       *      String urk              数据库ip，端口号，具体的数据库 可选信息(账号密码)
       *                              jdbc:数据库软件名://ip:port/数据库名?user=xxx&password=xxx
       *                              jdbc:mysql://127.0.0.1:3306/my_test?user=root&password=319612
       *   url的路径属性可选信息：
       *      url?user=账号&password=密码
       *
       *      8.0.27版本驱动，下面都是一些可选属性
       *          8.0.25 以后，自动识别时区，之前的版本要添加
       *          8 版本以后，默认使用的就是 utf-8 格式，剩下三个都可以省略
       *
       *      serverTimezone=Asia/shanghai    时区，影响数据库事件类型的时间
       *      useUnicode=true                 是否使用 unicode 编码格式
       *      characterEncoding=utf8          编码格式是不是 utf-8
       *      useSSL=true                     是不是忽略 SSL 验证
       */
  // 方案一：三个参数
  // DriverManager.getConnection("jdbc:mysql:///my_test","root","319612");
  // 方案二：两个参数
  // Properties info = new Properties();
  // info.put("user", "root");
  // info.put("password", "319612");
  // DriverManager.getConnection("jdbc:mysql:///my_test", info);
  // 方案三：一个参数
  Connection connection = DriverManager.getConnection("jdbc:///my_test?user=root&password=319612");
  
  // 4. 创建发送 SQL 语句的 statement 对象
  // statement 可以发送 SQL 语句到数据库，并且获取返回结果
  Statement statement = connection.createStatement();
  
  // 5. 发送 SQL 语句
  // 5.1 编写 SQL 语句
  String sql = "SELECT * FROM t_user WHERE account = '"+ account +"' AND password = '" + password + ";'";
  // 5.2 发送 SQL 语句
  /**
       * SQL 分类：DDL(容器创建、修改、删除) DML(插入、修改、删除数据) DQL(查询数据) DCL(权限创建语句) TPL(事务控制语句)
       *
       *  executeUpdate(sql)
       *    参数：sql 非 DQL 语句
       *        情况1：DML 语句的话会返回影响的行数
       *        情况2：非 DML return 0
       *  executeQuery(sql)
       *    参数：sql DQL
       *    返回：resultSet：结果封装对象
       */
  // int i = statement.executeUpdate(sql);
  ResultSet resultSet = statement.executeQuery(sql);
  
  // 6. 结果集解析：resultSet
  /**
       * java 是一种面向对象的思维，将查询结果封装成了 resultSet 对象，我们应该理解，内部一定也是有行有列的，和可视化的数据是一样的
       *
       * resultSet  ->   逐行获取数据，行 -> 行的列的数据
       *
       * 想要进行数据解析，我们需要进行两件事情
       *    1. 移动游标志指定获取数据行
       *    2. 获取指定数据行的列数据即可
       *  问题
       *    1. 游标移动问题
       *      resultSet 内部包含一个游标，指定当前行数据
       *      默认游标指定的是第一行数据之前
       *      我们可以调用 next 方法向后移动一行游标
       *      如果我们有狠毒行数据，我们可以使用 while(resultSet.next()){ 获取每一行的数据 }
       *      boolean = next() true：有更多行数据，并且向下移动一行
       *                       false: 没有更多行数据，不一定
       *      TODO: 移动光标的方法有很多，只需要记 next 即可，配合 while 循环获取全部数据
       *     2. 获取列的数据问题(获取光标在指定的行的列的数据)
       *        resultSet.get类型(String columnLabel | int columnIndex);
       *          columnLabel: 别名，如果有别名 写别名 select id as aid, account as ac from xxx;
       *          columnIndex: 列的下角标获取，从左到右，从 1 开始
       */
  //    while (resultSet.next()){
  //      // 现在已经指向当前行了
  //      int id = resultSet.getInt(1);
  //      String account = resultSet.getString("account");
  //      String password = resultSet.getString(3);
  //      String nickname = resultSet.getString("nickname");
  //      System.out.println(id + " " + account + " " + password + " " + nickname);
  //    }
  
  // 移动一次光标，只要有数据，就代表登录成功
  if(resultSet.next()){
      System.out.println("登陆成功");
  } else {
      System.out.println("登录失败");
  }
  
  // 6. 关闭资源
  resultSet.close();
  statement.close();
  connection.close();
  ~~~

#### 存在的问题

- SQL 语句需要字符串拼接，比较麻烦

- 只能凭借字符串类型，其他数据库类型无法处理

- 可能发送注入攻击

  > 动态值充当了 SQL 语句结构，影响了原有的查询结果

### PreparedStatement

#### 重写登录

~~~java
 /**
     * TODO: 防止注入攻击 | 演示 ps 的使用流程
     */

    // 1. 收集用户信息
    Scanner scanner = new Scanner(System.in);
    System.out.println("请输入用户名：");
    String account = scanner.nextLine();
    System.out.println("请输入密码：");
    String password = scanner.nextLine();

    // 2. ps 的数据库流程
    // 1.注册驱动
    Class.forName("com.mysql.cj.jdbc.Driver");
    // 2. 获取连接
    Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/my_test", "root", "319612");

    /**
     * statement
     *    1. 创建 statement
     *    2. 拼接 SQL 语句
     *    3. 发送 SQL 语句，并且获取返回结果
     *
     *  prepareStatement
     *    1. 编写 SQL 语句结果，不包含动态值部分的语句，动态值部分使用占位符 ? 替代
     *        注意：? 只能替代动态值
     *    2. 创建 PreparedStatement，并传入动态值
     *    3. 动态值 占位置 复制 ？ 单独赋值即可
     *    4. 发送 SQL 语句即可，并获取发送结果
     */
    // 3. 编写 SQL 语句结果
    String sql = "SELECT * FROM t_user WHERE account = ? and password = ?";

    // 4. 创建预编译 statement 并且设置 SQL 语句结束
    PreparedStatement preparedStatement = connection.prepareStatement(sql);

    // 5. 单独的占位符进行赋值
    /**
     * 参数1：index 占位符的位置 从左向右 从 1 开始
     * 参数2：object 占位符的值，可以设置为任何值
     */
    preparedStatement.setObject(1, account);
    preparedStatement.setObject(2, password);

    // 6. 发送 SQL 语句并获取结果
    // statement.executeUpdate | executeQuery(String sql)
    // preparedStatement 不需要传入 sql 语句，因为它已经知道语句动态值
    ResultSet resultSet = preparedStatement.executeQuery();

    // 7. 结果集解析
    if(resultSet.next()){
      System.out.println("登陆成功");
    } else {
      System.out.println("登陆失败");
    }

    // 8. 关闭资源
    resultSet.close();
    preparedStatement.close();
    connection.close();
~~~

#### 增删改查案例

~~~java
@Test
  public void testInsert() throws ClassNotFoundException, SQLException {
    /**
     * t_user表插入一条数据
     *    account  test
     *    password test
     *    nickname 黑皮
     */
    // 1. 注册驱动
    Class.forName("com.mysql.cj.jdbc.Driver");
    // 2. 获取连接
    Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/my_test", "root", "319612");
    // 3. 编写 SQL 语句结果，动态值的部分使用 ? 代替
    String sql = "INSERT INTO t_user(account,password,nickname) values(?,?,?)";
    // 4. 创建 preparedStatement，并且传入 SQL 语句结果
    PreparedStatement preparedStatement = connection.prepareStatement(sql);
    // 5. 占位符赋值
    preparedStatement.setObject(1, "test");
    preparedStatement.setObject(2, "test");
    preparedStatement.setObject(3, "黑皮");
    // 6. 发送 SQL 语句
    // 这时是 DML 语句
    int i = preparedStatement.executeUpdate();
    // 7. 输出结果
    if(i > 0){
      System.out.println("数据插入成功");
    } else {
      System.out.println("数据插入失败");
    }
    // 8. 关闭资源
    preparedStatement.close();
    connection.close();
  }

  @Test
  public void testUpdate() throws ClassNotFoundException, SQLException {
    /**
     *  修改 id = 3 的用户 nickname 为 擦拔掉
      */
    // 1. 注册驱动
    Class.forName("com.mysql.cj.jdbc.Driver");
    // 2. 获取连接
    Connection connection = DriverManager.getConnection("jdbc:mysql:///my_test", "root", "319612");
    // 3. 编写 SQL 语句结果，动态值的部分使用 ? 代替
    String sql = "UPDATE t_user SET nickname = ? WHERE id = ?";
    // 4. 创建 preparedStatement，并且传入 SQL 语句结果
    PreparedStatement preparedStatement = connection.prepareStatement(sql);
    // 5. 占位符赋值
    preparedStatement.setObject(1, "擦拔掉");
    preparedStatement.setObject(2, 3);
    // 6. 发送 SQL 语句
    int i = preparedStatement.executeUpdate();
    // 7. 输出结果
    if(i > 0){
      System.out.println("修改成功");
    } else {
      System.out.println("修改失败");
    }
    // 8. 关闭资源
    preparedStatement.close();
    connection.close();
  }

  @Test
  public void testDelete() throws SQLException, ClassNotFoundException {
    /**
     * 删除 id = 3 的用户数据
     */
    // 1. 注册驱动
    Class.forName("com.mysql.cj.jdbc.Driver");
    // 2. 获取连接
    Connection connection = DriverManager.getConnection("jdbc:mysql:///my_test", "root", "319612");
    // 3. 编写 SQL 语句结果，动态值的部分使用 ? 代替
    String sql = "DELETE FROM t_user WHERE id = ?";
    // 4. 创建 preparedStatement，并且传入 SQL 语句结果
    PreparedStatement preparedStatement = connection.prepareStatement(sql);
    // 5. 占位符赋值
    preparedStatement.setObject(1, 3);
    // 6. 发送 SQL 语句
    int i = preparedStatement.executeUpdate();
    // 7. 输出结果
    if(i > 0){
      System.out.println("删除成功");
    } else {
      System.out.println("删除失败");
    }
    // 8. 关闭资源
    preparedStatement.close();
    connection.close();
  }

  @Test
  public void testSelect() throws ClassNotFoundException, SQLException {
    /**
     * 目标：查询所有用户数据，并且封装到一个 List<Map> list 集合中
     *  实现思路：
     *      便利行数据，一行对应一个 Map，获取一行的列名和对应的列的属性，装配即可
     *      将 Map 撞到一个集合就可以了
     */
    // 1. 注册驱动
    Class.forName("com.mysql.cj.jdbc.Driver");
    // 2. 获取连接
    Connection connection = DriverManager.getConnection("jdbc:mysql:///my_test", "root", "319612");
    // 3. 编写 SQL 语句结果，动态值的部分使用 ? 代替
    String sql = "SELECT * FROM t_user";
    // 4. 创建 statement
    Statement statement = connection.createStatement();
    // 6. 发送 SQL 语句
    ResultSet resultSet = statement.executeQuery(sql);
    // 7. 结果及解析
    /**
     * TODO: 回顾
     *    resultSet: 有行和列，获取数据的时候，一行一行数据
     *                内部有一个游标，默认值想数据的第一行之前
     *                我们可以使用 next() 方法移动游标指向数据行
     *                获取行中列的数据
     */
    List<Map> list = new ArrayList<>();
    while (resultSet.next()){
      Map hashMap = new HashMap();
      // 方案一：不智能，不推荐
//      int id = resultSet.getInt("id");
//      String account = resultSet.getString("account");
//      String password = resultSet.getString("password");
//      String nickname = resultSet.getString("nickname");
//      hashMap.put("account", account);
//      hashMap.put("password", password);
//      hashMap.put("nickname", nickname);
      // 方案二：自动便利列
      // 获取列的信息对象
      // TODO: metaData 装的是当前结果集列的信息对象(他可以获取列的名称根据下角标，可以获取列的数据)
      ResultSetMetaData metaData = resultSet.getMetaData();
      // 获取列的数量，有了列的数量就可以遍历列了
      int columnCount = metaData.getColumnCount();
      // 这里注意从 1 开始，因为数据库取值时是从 1 开始的
      for(int i = 1; i <= columnCount; i++){
        // 获取指定列下角标的值
        Object object = resultSet.getObject(i);
        // 获取指定列下角标的列的名称
        //    这里之所以使用 getColumnLabel 是因为这个方法会获取别名，如果没有写别名才会去获取列的名称
        //                  而 getColumnName 获取的只是列的名称
        String columnLabel = metaData.getColumnLabel(i);
        hashMap.put(columnLabel, object);
      }

      list.add(hashMap);
    }
    System.out.println("list=" + list);
    // 8. 关闭资源
    resultSet.close();
    statement.close();
    connection.close();
  }
~~~

#### 使用步骤总结

1. 注册驱动
2. 获取连接
3. 编写 SQL 语句
4. 创建 preparedStatement 并且传入 SQL 语句结构
5. 占位符赋值
6. 发送 SQL 语句，并且获取结果
7. 结果集解析
8. 关闭资源

#### 使用 API 总结

1. 注册驱动

   - 调用静态方式，但是会注册两次

     `DriverManager .registerDriver(new com.mysql.cj.jdbc.Driver());`

   - 反射触发：推荐这种方法

     `Class.forName("com.mysql.cj.jdbc.Driver()");`

   因为 Driver 类中有静态代码块会自动注册，所以只需要加载 Driver 这个类就可以

2. 获取连接

   `Connection connection = DriverManager.getConnection()`

   重载参数：

   - `String url, String user, String password`
   - `String url,Properties info(user,password)`
   - `String url 后面拼接上账号密码参数`

3. 创建 statement

   - 静态

     `Statement statement = connection.createStatement()`

   - 动态

     `PreparedStatement preparedStatement = connection.preparedStatement()`

4. 占位符赋值 ( preparedStatement 才有 )

   `preparedStatement.setObject(?的位置，从左到右，从 1 开始，值)`

5. 发送 SQL 语句获取结果

   - `int executeQuery()`：非 DQL
   - `ResultSet executeUpdate()`：DQL

6. 查询结果集解析

   - 移动光标指向行数据

     - `next()`
     - `if(next())`
     - `while(next())`

   - 获取列的数据

     - `get类型(int 列的下角标 从 1 开始 | int 列的 label (别名或者列名))`

   - 获取列的信息

     - `ResultMetaData getMetaData`：ResultMetaData 对象 包含的就是列的信息
       - `getClolumnCount`：获取列的数量
       - `getColumnLabel(index)`：获取列的别名，如果没有别名就获取列名
       - `getColumnName(index)`：获取列的列名

   - 关闭资源

     一系列的 close