[toc]

# 注解

**使用 Annotation 时要在其面前增加 @ 符号, 并把该 Annotation 当成一个修饰符使用. 用于修饰它支持的程序元素**

1. 注解( Annotation )也被称为元数据( metadata ),用于修饰解释, 包、类、方法、属性、构造器、局部变量等数据信息
2. 和注释一样, 注解不影响程序逻辑, 但注解可以被编译或运行, 相当于嵌入在代码中的补充信息
3. 在 JavaSE 中, 注解的使用目的比较简单, 比如标记过时的功能, 忽略警告等, 在 JavaEE 中注解占据了更重要的角色, 例如用来配置应用程序的任何切面, 代替 JavaEE旧版中所一流的繁冗代码和XML配置等

## 三个基本的 Annotation

1. @Override

   限定某个方法, 是重写父类方法, 该注解只能用于方法

   - @ Override 表示指定重写父类的方法( 从编译层面验证 ), 如果父类没有该方法, 则会报错
   - 如果不写@Override 注解, 而父类仍有该方法, 仍然构成重写
   - @Override 只能修饰方法, 不能修饰其他类, 包, 属性等等
   - 查看 @Override 注解源码为 @Target( ElementType.METHOD ),  说明只能修饰方法
   - @Target 是修饰注解的注解, 成为元注解

2. @Deprecated

   用于表示某个程序元素( 类, 方法等 )已过时

   - 用于表示某个程序元素( 类, 方法等 )已过时
   - 可以修饰方法,类,字段,包,参数 等等
   - @ Target(value = { CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE })
   - @Deprecated 的作用可以做到新旧版本的兼容和过渡

3. @SuppressWarnings

   抑制编译器警告

   - 说明各种值
     1. unchecked 是忽略没有检查的警告
     2. rawtypes 是忽略没有指定泛型的警告(传参时没有指定泛型的警告错误)
     3. unused 是忽略没有使用某个变量的警告错误
     4. @SuppressWarnings 可以修饰的程序元素为,查看@Target
     5. 生成@SuppressWarnings 时, 不用背, 直接点击左侧的黄色提示, 就可以选择, 注意可以指定生成的位置

   ~~~java
   @SuppressWarnings({ "属性" })
   // 属性: 1.all 抑制所有警告
   //		2.boxing 抑制与封装/拆装作业相关的警告
   //		3.cast 抑制与强制转型作业相关的警告
   //		4.dep-ann 抑制与淘汰注释相关的警告
   //		5.deprecation 抑制与淘汰的相关警告
   //		6.fallthrough 抑制与switch陈述式中一楼break相关的警告
   //		7.finally 抑制与未传回finally区快相关的警告
   //		8.hiding 抑制与隐藏变数的区域变数相关的警告
   //		9.incomplete-switch 抑制与switch陈述式(enum case)中一楼项目相关的警告
   //		10.javadoc 抑制与javadoc相关的警告
   //		11.nls 抑制与非nls字串文字相关的警告
   //		12.null 抑制与空值分析相关的警告
   //		13.rawtypes 抑制与使用raw类型相关的警告
   //		14.resource 抑制与使用Closeable类型的资源相关的警告
   //		15.restriction 抑制与使用不建议或禁止参照相关的警告
   //		16.serial 抑制与可序列化的类别遗漏serialVersionUID栏位相关的警告
   //		17.static-access 抑制与静态存取不正确相关的警告
   //		18.static-method 抑制与可能宣告为static的方法相关的警告
   //		19.super 抑制与置换方法相关但不含super呼叫的警告
   //		20.synthetic-access 抑制与内部类别的存取未最佳化相关的警告
   //		21.sync-override 抑制因为置换同步方法而遗漏同步化的警告
   //		22.unchecked 抑制与未检查的作业相关的警告
   //		23.unqualified-field-access 抑制与栏位存取不合格相关的警告
   //		24.unused 抑制与未用的程序码及停用的程序码相关的警告
   ~~~

   

**补充说明: @interface 的说明, @interface 不是 interface , 是注解类, 是jdk5.0 加进去的**

## 元注解

**JDK的元 Annotation 用于修饰其他的 Annotation**

**元注解本身作用不大, 了解这个在看源码的时候会有帮助**

### 种类

1. Retention 

   指定注解的作用范围,

   三种值:

   - RetentionPolicy.SOURCE 编译器使用后, 直接丢弃这种策略的注释
   - RetentionPolicy.CLASS 编译器将把主食记录在 class 文件中, 当运行 Java 程序时, JVM 不会保留注解, 这是默认值
   - RetentionPolicy.RUNTIME 编译器将把注释记录在 class 文件中, 当运行 Java 程序时, JVM 会保留注解, 程序可以通过反射获取该注解

2. Target

   指定注解可以在哪些地方使用

3. Documented

   指定该注解是否会在 javadoc 体现

4. Inherited

   子类会继承父类注解