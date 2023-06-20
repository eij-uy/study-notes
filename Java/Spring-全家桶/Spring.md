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

   ~~~java
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

   ~~~java
   <bean id="book" class="全包名 + 类名" >
     <constructor-arg name="name" values="java开发" />
     <constructor-arg index="1" values="xxx" />
   </bean>
   ~~~

#### 特殊类型的注入

##### 字面量赋值

> 就是上面的写法

##### null值

~~~java
<bean id="book" class="全包名 + 类名" >
  <constructor-arg name="name" values="java开发" />
  <constructor-arg name="author">
  	<null/>
  </constructor-arg>
</bean>
~~~

##### xml 实体

~~~java
<bean id="book" class="全包名 + 类名" >
  <constructor-arg name="name" values="java开发" />
  // 这种方式 <> 会和标签冲突
  // <constructor-arg name="author" value="<>"></constructor-arg>
	<constructor-arg name="author" value="&lt;&gt;"></constructor-arg>
</bean>
~~~

##### CDATA 节

~~~java
<bean id="book" class="全包名 + 类名" >
  <constructor-arg name="name" values="java开发" />
	<constructor-arg name="author">
  	<value>[!CDATA[/*这里可以写特殊符号*/a < b]]</value>
  </constructor-arg>
</bean>
~~~

##### 对象类型

~~~java
public class Dept {
  private String dname;

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
            
~~~

##### 数组类型

~~~java
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

~~~java
 
~~~



#### Bean 的作用域

#### Bean 的生命周期

#### 基于 xml 自动装配

