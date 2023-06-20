[toc]

# Spring6

## 容器：IoC

是 `Inversion of Control` 的简写，译为 控制反转

### IoC 容器

`Spring` **通过 IoC 容器** 来**管理所有 Java 对象的实例化和初始化，控制对象与对象之间的依赖关系**，我们将由 IoC 容器管理的 java 对象成为 `Spring bean`，它与使用关键字 `new` 创建的 Java 对象没有任何区别

#### 控制反转

- 控制反转是一种思想
- 控制反转是为了降低程序耦合度，提高程序扩展力
- 控制反转，反转的是什么？
  - 将对象的创建权利交出去，交给第三方容器负责
  - 将对象和对象之间的关系的维护全交出去，交给第三方容器负责
- 控制反转这种思想如何实现
  - `DI (Dependency Injection):` 依赖注入

#### 依赖注入

> DI ( Dependency Injection )：依赖注入，依赖注入实现了控制反转的思想

- 指 Spring 创建对象的过程中，将对象依赖属性通过配置进行注入
- 依赖注入常见的实现方式包括两种
  - set 注入
  - 构造注入

所以结论是：IOC 就是一种控制反转的思想，而 DI 是对 IoC 的一种具体实现

**Bean管理说的是：Bean对象的创建以及 Bean 对象中属性的赋值 ( 或者叫做 Bean 对象之间关系的维护 )**

#### IoC 容器在 Spring 的实现

- `BeanFactory`

  > 这时 Ioc 容器的基本实现，是 Spring 内部使用的接口，面向 Spring 本身

- `ApplicationContext`

  > BeanFactory 的子接口，提供了很多高级特性，面向 Spring 的使用者

- `ApplicationContext 的主要实现类`

  | 类型名                            | 简介                                                         |
  | --------------------------------- | ------------------------------------------------------------ |
  | `ClassPathXmlApplicationContext`  | 通过读取类路径下的 XML 格式的配置文件创建 IOC 容器对象       |
  | `FileSystemXmlApplicationContext` | 通过文件系统路径读取 XML 格式的配置文件创建 IOC 容器对象     |
  | `ConfigurableApplicationContext`  | `ApplicationContext` 的子接口，包含一些扩展方法 `refresh` 和 `close` 让 `ApplicationContext` 具有启动、关闭和刷新上下文的能力 |
  | `WebApplicationContext`           | 专门为 web 应用准备，基于 Web 环境创建 IOC 容器对象，并将对象引入存入 ServletContext 域中 |

### 基于 XML 管理 bean

#### 获取 Bean

##### 获取 Bean 的三个方式

- 根据 id 获取

  ~~~java
  ApplicationContext context = new ClassPathXmlApplicationContext("xml文件路径");
  User user = (User)context.getBean('user');
  
  // bean.xml
  <bean id="user" class="全包名+类名" />
  ~~~

- 根据类型获取

  ~~~java
  // 注意：这种方式获取 Bean 时，要求 IOC 容器中指定类型的 bean 有且只能有一个，如果有两个将会抛出异常
  User user = context.getBean(User.class);
  ~~~

- 根据 id 和 类型

  ~~~java
  User user = context.getBean("user",User.class);
  ~~~

##### 扩展

- 如果组件类实现了接口，根据接口类型可以获取 bean 吗?

  > 可以，通过类型获取，前提是 bean 唯一

- 如果一个接口有多个实现类，这些实现类都配置了 bean，根据接口类型可以获取 bean 吗?

  > 不可以，因为 bean 不唯一

##### 结论

> 根据类型来获取 bean 时，在满足 bean 唯一性的前提下，其实只是看：[ 对象 instanceof 指定的类型 ] 的返回结果，只要返回的是 true 就可以认定为和类型匹配，能够获取到
>
> java 中，instanceof 运算符用于判断前面的对象是否是后面的类，或其子类、实现类的实例。如果是返回 true，否则返回 false。也就是说，用 instanceof 关键字做判断时，instanceof 操作符的左右操作必须有继承或实现关系

#### 依赖注入之 setter 注入

> 创建对象过程中,向属性设置值

1. 创建类,定义树形,生成属性方法

   ~~~java
   class Book {
     private String name;
     private String author;
     public String getName(){
       return name;
     }
     public void setName(String name){
       this.name = name
     }
     public String getAuthor(){
     	return author;
     }
     public void setAuthor(String author){
       this.author = author
     }
   }
   ~~~

2. 在 Spring 配置文件配置

   ~~~xml
   <bean id="book" class="全包名 + 类名" >
     <property name="name" value="前端开发" />
     <property name="author" value="xxx" />
   </bean>
   ~~~

#### 依赖注入之构造器注入

1. 创建类,定义属性,生成有参构造器

   ~~~java
   class Book {
     private String name;
     private String author;
     public String getName(){
       return name;
     }
     public Book(String name, String author){
       this.name = name;
       this.author = author;
     }
     public void setName(String name){
       this.name = name
     }
     public String getAuthor(){
     	return author;
     }
     public void setAuthor(String author){
       this.author = author
     }
   }
   ~~~

2. 在 Spring 配置文件配置

   ~~~xml
   <bean id="book" class="全包名 + 类名" >
     <constructor-arg name="name" values="java开发" />
     <constructor-arg index="1" values="xxx" />
   </bean>
   ~~~

#### 特殊类型的注入

##### 字面量赋值

> 就是上面的写法

##### null值

~~~xml
<bean id="book" class="全包名 + 类名" >
  <constructor-arg name="name" values="java开发" />
  <constructor-arg name="author">
  	<null/>
  </constructor-arg>
</bean>
~~~

##### xml 实体

~~~xml
<bean id="book" class="全包名 + 类名" >
  <constructor-arg name="name" values="java开发" />
  // 这种方式 <> 会和标签冲突
  // <constructor-arg name="author" value="<>"></constructor-arg>
	<constructor-arg name="author" value="&lt;&gt;"></constructor-arg>
</bean>
~~~

##### CDATA 节

~~~xml
<bean id="book" class="全包名 + 类名" >
  <constructor-arg name="name" values="java开发" />
	<constructor-arg name="author">
  	<value>[!CDATA[/*这里可以写特殊符号*/a < b]]</value>
  </constructor-arg>
</bean>
~~~

##### 对象类型

~~~xml
public class Dept {
  private List<Emp> empList;
  private String dname;
  public String getEmpList() {
    return dname;
  }

  public void setEmpList(String empList) {
    this.empList = empList;
  }

  public String getDname() {
    return dname;
  }

  public void setDname(String dname) {
    this.dname = dname;
  }

  public void info(){
    System.out.println("部门名称=" + dname);
  }
}

public class Emp {
  // 员工属于某个部门
  private Dept dept;
  private String ename;
  private Integer age;

  public Dept getDept() {
    return dept;
  }

  public void setDept(Dept dept) {
    this.dept = dept;
  }

  public String getEname() {
    return ename;
  }

  public void setEname(String ename) {
    this.ename = ename;
  }

  public Integer getAge() {
    return age;
  }

  public void setAge(Integer age) {
    this.age = age;
  }

  public void work(){
    System.out.println(ename + "emp working......" + age);
    dept.info();
  }
}

// 方式一：引用外部 bean
<bean id="dept" class="com.yujie.spring6.iocxml.ditest.Dept">
    <property name="dname" value="安保部" />
</bean>
<bean id="emp" class="com.yujie.spring6.iocxml.ditest.Emp" >
    <!--  普通属性注入-->
    <property name="ename" value="lucy" />
    <property name="age" value="50" />
    <!--   注入对象类型     -->
    <property name="dept" ref="dept" />
</bean>
// 方式二：内部 bean
<bean id="emp2" class="com.yujie.spring6.iocxml.ditest.Emp" >
    <!--  普通属性注入-->
    <property name="ename" value="mary" />
    <property name="age" value="20" />
    <!--  内部 bean -->
    <property name="dept">
    	<bean id="dept2" class="com.yujie.spring6.iocxml.ditest.Dept">
        	<property name="dname" value="财务部" />
		</bean>
	</property>
</bean>
// 方式三：级联属性赋值
<bean id="dept3" class="com.yujie.spring6.iocxml.ditest.Dept">
	<property name="dname" value="技术研发部" />
</bean>
<bean id="emp3" class="com.yujie.spring6.iocxml.ditest.Emp" >
    <!--  普通属性注入-->
    <property name="ename" value="tom" />
    <property name="age" value="30" />
    <!--  级联赋值 -->
    <property name="dept.dname" value="测试部" />
</bean>
~~~

##### 数组类型

~~~xml
<bean id="dept4" class="com.yujie.spring6.iocxml.ditest.Dept">
        <property name="dname" value="技术研发部" />
    </bean>
    <bean id="emp4" class="com.yujie.spring6.iocxml.ditest.Emp" >
        <!--  普通属性注入-->
        <property name="ename" value="tom" />
        <property name="age" value="30" />
        <!--  级联赋值 -->
        <property name="dept" ref="dept" />
        <property name="loves" >
            <array>
                <value>吃饭</value>
                <value>睡觉</value>
                <value>敲代码</value>
            </array>
        </property>

    </bean>
~~~

##### 集合类型

###### List 集合

~~~xml
<bean id="emp" class="com.yujie.spring6.iocxml.ditest.Emp" >
    <property name="ename" value="lucy" />
    <property name="age" value="20" />
</bean>
<bean id="dept" class="com.yujie.spring6.iocxml.ditest.Dept" >
    <property name="dname" value="技术部" />
    <property name="empList">
    	<list>
    		<ref bean="empone"></ref>
    		<ref bean="emptow"></ref>
    		<ref bean="empthree"></ref>
    	</list>
    </property>
</bean>
~~~

###### Map 集合

~~~xml
// 1. 创建两个对象
// 2. 创建普通类型属性
// 3. 在学生 bean 注入 map 集合类型属性
<bean id="teacherone" class="com.yujie.spring6.iocxml.dimap.Teacher">
    <property name="teacherId" value="100" />
    <property name="teacherName" value="西门讲师" />
</bean>
<bean id="teachertwo" class="com.yujie.spring6.iocxml.dimap.Teacher">
    <property name="teacherId" value="100" />
    <property name="teacherName" value="西门讲师" />
</bean>
<bean id="student" class="com.yujie.spring6.iocxml.dimap.Student">
    <property name="sid" value="2000" />
    <property name="sname" value="张三" />
    <property name="teacherMap">
    	<map>
    		<entry>
    			<key>
    				<value>10010</value>
    			</key>
    			<ref bean="teacherone" />
    		</entry>
    		<entry>
    			<key>
    				<value>10086</value>
    			</key>
    			<ref bean="teachertwo" />
    		</entry>
    	</map>
    </property>
</bean>
~~~

##### 引用集合类型

~~~xml
// List 
// 1. 创建三个对象
// 2. 注入普通类型的属性
// 3. 使用 util：类型 定义
// 4. 在学生 bean 引入 util：类型定义 bean，完成 list、mao 类型属性的注入

// 这里需要加上注释的这三行才可以使用 util
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       //xmlns:util="http://www.springframework.org/schema/util"
       //xsi:schemaLocation="http://www.springframework.org/schema/util
       //http://www.springframework.org/schema/beans/spring-util.xsd
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

<bean id="student" class="com.yujie.spring6.iocxml.dimap.Student>
    <property name="sid" value="10000" />
    <property name="sname" value="lucy" />
    <!-- 注入list、map类型属性 -->
    <property name="lessonList" ref="lessonList" />
    <property name="teacherMap" ref="teacherMap" />
</bean>
    
<util:list id="lessonList">
    <ref bean="lessonone" />
    <ref bean="lessontwo" />
</util:list>
    
<util:map id="teacherMap">
    <entry>
    	<key>
    		<value>10010</value>
    	</key>
    	<ref bean="teacherone" />
    </entry>
    <entry>
    	<key>
    		<value>10086</value>
    	</key>
    	<ref bean="teachertwo" />
    </entry>
</util:map>
    
<bean id="lessonone" class="com.yujie.spring6.iocxml.dimap.Lesson >
	<property name="lessonName" value="java开发" />
</bean>
<bean id="lessontwo" class="com.yujie.spring6.iocxml.dimap.Lesson >
	<property name="lessonName" value="前端开发" />
</bean>
<bean id="teacherone" class="com.yujie.spring6.iocxml.dimap.Teacher">
    <property name="teacherId" value="100" />
    <property name="teacherName" value="西门讲师" />
</bean>
<bean id="teachertwo" class="com.yujie.spring6.iocxml.dimap.Teacher">
    <property name="teacherId" value="100" />
    <property name="teacherName" value="欧阳讲师" />
</bean>
~~~

##### P 命名空间

~~~xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       //xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

// p 命名空间注入
<bean id="studentp" class="com.yujie.spring6.iocxml.dimap.Student">
	p:sid="100" p:sname="mary" p:lessonList-ref="lessonList" p:teacherMap-ref="teacherMap"
</bean>
~~~

##### 引入外部配置文件

1. 加入依赖

   ~~~xml
   <dependencies>
       // MySQL 驱动
       <dependency>
       	<groupId>mysql</groupId>
       	<artigactId>mysql-connector-java</artifactId>
       	<version>8.0.30</version>
       </dependency>
       // 数据源
       <dependency>
       	<groupId>mysql</groupId>
       	<artigactId>mysql-connector-java</artifactId>
       	<version>8.0.30</version>
       </dependency>
   </dependencies>
   ~~~

2. 创建外部属性文件 `jdbc.properties`

   ~~~
   // main 下的 resources 下的 jdbc.properties
   jdbc.user=root
   jdbc.password=319612
   jdbc.url=jdbc:mysql://localhost:3306/my_test?serverTimezone=UTC
   jdbc.driver=com.mysql.cj.jdbc.Driver
   ~~~

3. 引入 context 命名空间

   ~~~xml
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          //xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd
          http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
       <!-- 引入外部属性文件 -->
       <context:property-placeholder location="classpath:jdnc.properties"></context:property-placeholder>
       <!-- 完成数据库信息注入 -->
       <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
           <property name="url" value="${jdbc.url}" />
           <property name="username" value="${jdbc.user}" />
           <property name="password" value="${jdbc.password}" />
           <property name="druiverClassName" value="${jdbc.driver}" />
       </bean>
   </beans>
   
   // 测试
   ApplicationContext context = new ClassPathXmlApplicationContext("bean-jdbc.xml");
   DruidDataSource dataSource = context.getBean(DruidDataSource.class);
   System.out.println(dataSource.getUrl());
   ~~~

#### Bean 的作用域

##### 概念

> 在 Spring 中可以通过配置 bean 标签的 scope 属性来指定 bean 的作用域范围，各取值含义参如下表

| 取值              | 含义                                                         | 创建对象的时机   |
| ----------------- | ------------------------------------------------------------ | ---------------- |
| `singleton(默认)` | 在 IOC 容器中，这个 bean 的对象始终为单实例                  | IOC 容器初始化时 |
| `prototype`       | 这个 bean 在 IOC 容器中有多个实例                            | 获取 bean 时     |
|                   | 如果是在 `WebApplicationContext` 环境下还会有另外几个作用域 ( 不常用 ) |                  |
| `request`         | 在一个请求范围内有效                                         |                  |
| `session`         | 在一个回话范围内有效                                         |                  |

#### Bean 的生命周期

##### 具体的生命周期过程

- bean 对象创建 ( 调用无参构造器 )
- 给 bean 对象设置属性
- bean 的后置处理器 ( 初始化之前 )
- bean 对象初始化 ( 需在配置 bean 时指定初始化方法 )
- bean 的后置处理器 ( 初始化之后 )
- bean 对象就绪可以使用
- bean 对象销毁 ( 需在配置 bean 时指定销毁方法 )
- IOC 容器关闭

~~~xml
<bean id="user" class="com.yujie.xxx.xx.xx" scope="singleton" init-method="initMethod" destory-method="destoryMethod">
	<property name="name" value="lucy" />
</bean>

// ClassPathXmlApplicationContext 类的实例有一个 close 方法可以销毁 bean
~~~

##### 后置处理器

~~~java
// 这会对所有的 bean 生效
public class MyBeanTest implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println(beanName + "::" + bean);
        return bean;
    }
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println(beanName + "::" + bean);
        return bean;
    }
}

// bean 的后置处理器要放入 IOC 容器才能生效
<bean id="myBeanTest" class="com.yujie.xxx.xx.xx.MyBeanTest" scope="singleton" >
	<property name="name" value="lucy" />
</bean>
~~~

#### FactoryBean

##### 简介

> FactoryBean 是 Spring 提供的一种整合第三方框架的常用机制。和普通的 bean 不同，配置一个 FactoryBean 类型的 bean，在获取 bean 的时候得到的并不是 class 属性中配置的这个类的对象，而是 getObject 方法的返回值。通过这种机制，Spring 可以帮我们把复杂组件创建的详细过程和繁琐细节都拼比起来，只把最简洁的使用界面展示给我们

#### 基于 xml 自动装配

