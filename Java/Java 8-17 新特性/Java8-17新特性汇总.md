[toc]

# Java8 - 17 新特性汇总

## 新语法结构

新的语法结构，为我们勾勒出了 java 语法进行的一个趋势，将开发者从 复杂，繁琐 的低层次抽象中逐渐解放出来，以更高层次、更优雅的抽象，即 降低代码里，有避免意外编程错误的出现，进而提高代码质量和开发效率

### Java的 REPL 工具：jShell 命令  --->  jdk9

可以在命令行中使用 `jshell` 命令执行 `java` 代码，比较方便

首先在命令行通过指令 `jshell`，进入 `jshell` 环境

#### 常用指令

- `/help`：获取有关使用 `jshell` 工具的信息
- `/help intro`：`jshell` 工具的简介
- `/list`：列出当前 `session` 里所有有效的代码片段
- `/vars`：查看当前 `session` 下所有创建过的变量
- `/methods`：查看当前 `session` 下所有创建过的方法
- `/imports`：列出导入的包
- `/history`：键入的内容的历史记录
- `/edit`：使用外部代码编辑器来编写 `Java` 代码
- `exit`：退出 `jshell` 工具

### 异常处理之 try - catch 资源关闭   --->   jdk7   --->   jdk9 

在 jdk7 之前，我们这样处理资源的关闭

~~~java
// jdk7 之前处理资源关闭的方式
FileWriter fw = null;
BufferdWtier bw = null;
try {
  fw = new FileWriter("test.txt");
  bw = new BufferedWriter(fw);
  
  bw.write("hello,世界");
} catch(IOException e) {
	e.printStackTrace();
} finally {
  try {
    if(bw != null){
      bw.close();
    } catch(IOException e) {
      e.printStackTrace();
    }
  }
}

// jdk7 以后，这样写 try 后小括号内的资源会自动实现关闭操作
// try(资源对象的生命和初始化){
//   业务逻辑代码，可能会造成异常
// } catch(异常类型1 e) {
//   处理异常代码
// } catch(异常类型2 e) {
//   处理异常代码
// }
try(
	FileWriter fw = new FileWriter("test.txt");
  BufferdWtier bw = new BufferedWriter(fw);
) {
  bw.write("hello,世界");
} catch(IOException e) {
	e.printStackTrace();
}

// jdk9 新特性, try 前面可以定义流对象， try 后面的 () 中可以直接引用流对象的名称，在 try 代码执行完毕后，流对象也可以自动释放
FileWriter fw = new FileWriter("test.txt");
BufferdWtier bw = new BufferedWriter(fw);
try(fw;bw) {
  bw.write("hello,世界");
} catch(IOException e) {
	e.printStackTrace();
}
~~~

### 局部变量的类型推断   --->   jdk10

#### 可以使用的场景

- 局部变量的实例化
- 增强 for 循环中的索引
- 传统 for 循环中
- 返回值类型含复杂泛型结构

~~~java
// 1. 局部变量的实例化
var list = new ArrayList<String>();
var set = new LinkedHashSet<Integer>();
// 2. 增强 for 循环中的索引
for(var v : list){
  System.out.println(v);
}
// 3. 传统 for 循环中
for(var i = 0; i < 100; i++){
  System.out.println(i);
}

HashMap<String, Integer> map = new HashMap();
// 4. 返回值类型含复杂泛型结构
// 老版本 Set<Map.Entry<String, Integer>> entries = map.entryMap();
var entries = map.entryMap();
~~~

#### 不可以使用的场景

- 声明一个成员变量
- 声明一个数组变量，并为数组静态初始化 ( 省略 new 的情况下 )
- 方法的返回值类型
- 方法的参数类型
- 没有初始化的方法内的局部变量声明
- 作为 catch 块中异常类型
- Lambda 表达式中函数式接口的类型
- 方法引用中函数式接口的类型

### instanceof 自动匹配模式 ---> JDK14预览特性 ---> JDK15第二次预览 ---> JDK16转正特性

~~~java
Object obj = mew String("hello,java");

// jdk14 之前 这样写
if(obj instanceof String) {
    // 然后转换
    String s = (String) obj;
    // 然后才可以使用 s
}

// jdk14 之后
if(obj instanceof String s){
    // 直接可以使用 s
}
~~~

### switch

#### switch 表达式 ---> JDK12预览特性 ---> JDK13第二次预览 ---> JDK14转正

##### 传统 switch 声明语句的弊端

- 匹配是自上而下的，如果忘记写 break，后面的 case 语句不论匹配与否都会执行
- 所有的 case 语句共用一个块范围，在不同的 case 语句定义的变量名不能重复
- 不能在一个 case 里写多个执行结果一致的条件
- 整个 switch 不能作为表达式返回值

~~~java
// 传统 switch 表达式
switch(month){
    case 3|4|5: // 3|4|5 用了位运算符，11 | 100 | 101 结果是 111 是 7
        System.out.println("春季");
        break;
    case 6|7|8: // 6|7|8 用了位运算符，110 | 111 | 1000 结果是 1111 是 15
        System.out.println("夏季");
        break; 
    case 9|10|11: // 9|10|11 用了位运算符，1001 | 1010 | 1011 结果是 1011 是 11
        System.out.println("秋季");
        break;
    case 12|1|2: // 12|1|2 用了位运算符，1100 | 1 | 10 结果是 1111 是 15
        System.out.println("冬季");
        break;
}
~~~

##### java 12 中预览特性

- java 12 将会对 switch 声明语句进行扩展，使用 `case L ->` 来代替以前的 `break;` 省去了 break 语句，避免了因少些 break 而出错

- 同时将多个 case 合并到一行，显得简洁、清晰、也更加优雅的表达逻辑分支

- 为了保持兼容性，case 条件语句中依然可以使用字符 `:`，但是同一个 switch 结构里不能混用 `->` 和 `:`，否则编译错误

  ~~~java
  enum Week {
      MONDAY,
      THESDAY,
      WEDNESDAY,
      THURSDAY,
      FRIDAY,
      SATURDAY,
      SUNDAY
  }
  Week day = Week.FRIDAY;
  switch (day) {
      case MONDAY -> System.out.println(1);
      case THESDAY,WEDNESDAY,THURSDAY -> System.out.println(2);
      case FRIDAY -> System.out.println(3);
      case SATURDAY,SUNDAY -> System.out.println(4);
      default -> throw new RuntimeException("What day is today?" + day);
  }
  
  // java12 中还可以使用变量接收 switch 表达式的结果
  int result switch (day) {
      case MONDAY -> 1;
      case THESDAY,WEDNESDAY,THURSDAY -> 2;
      case FRIDAY -> 3;
      case SATURDAY,SUNDAY -> 4;
      default -> throw new RuntimeException("What day is today?" + day);
  };
  System.out.println(result); 
  ~~~

  ##### JDK13 中引入了 yield 关键字

  `yield` 关键字用于返回指定的数据，结束 switch 结构。

  这意味着，`switch ` 表达式 ( 返回值 ) 应该使用 `yield`，`switch` 语句 ( 不返回值 ) 应该使用 `break`

  和 `return` 关键字的区别在于：`return` 会直接跳出当前方法，而 `yield` 只会跳出当前 `switch` 块

  ~~~java
  // 方式1：
  int result switch (day) {
      case MONDAY -> {
          yeild 1;
      }
      case THESDAY,WEDNESDAY,THURSDAY -> {
          yeild 2;
      }
      case FRIDAY -> {
          yeild 3;
      }
      case SATURDAY,SUNDAY -> {
      	yeild 4;
      }
      default -> throw new RuntimeException("What day is today?" + day);
  };
  
  // 方式二：
  int result switch (day) {
      case MONDAY: 
          yield 1;
      case THESDAY,WEDNESDAY,THURSDAY: 
          yield 2;
      case FRIDAY: 
          yield 3;
      case SATURDAY,SUNDAY: 
          yield 4;
      default: 
          yield 5;
  };
  System.out.println(result); 
  ~~~

#### switch 模式匹配 ---> JDK17预览特性

~~~java
// jdk17 之前
static String formatter(Object o) {
    String formatted = "unknown";
    if(o instanceof Integer i){
        formatted = "int " + i;
    } else if(o instanceof Long l){
        formatted = "long " + l;
    } else if(o instanceof Double d) {
        formatted = "double " + d;
    } else if(o instanceof String s) {
        formatted = "String " + s;
    }
    return formatted;
}

// JDK17 之后 switch 的模式匹配
String formatted = switch(o){
    case Integer i:
        yield "int " + i;
    case Long l:
        yield "long " + l;
    case Double d:
        yield "double " + d;
    case String s:
        yield "String " + s;
};
return formatted;
~~~

### 文本块的使用 ---> JDK13预览特性 ---> JDK14二次预览 ---> JDK15转正

~~~java
// 传统文本块
String info = "<html>\n" +
    "  <body>\n" +
    "     <p>hello，java</p>\n" +
    "  </body>\n" +
    "</html>";
String myJson = "{\n" +
    "	\"name\":\"Yu Jie\",\n" +
    "	\"gender\":\"男",\n" + 
    "	\"address\":\"xxx\"\n" +
    "}";

// jdk13以后的文本块
String info1 = """
    <html>
    	<body>
    		<p>hello,java</p>
    	</body>
    </html>
    """;
String myJson1 = """
    {
		"name":"Yu Jie",
    	"gender":"男",
    	"address":"xxx"
    }""";
~~~

#### JDK14 新增特性

- `\`：取消换行操作
- `\s`：表示一个空格

### record 的使用 ---> JDK14预览特性 ---> JDK15第二次预览 ---> JDK16转正

#### 介绍

当我们把一个类定义为 `record` 时，就避免了编写：构造函数，访问器，`equals()`、`hashCode()`、`toString()`等

`record` 的设计目标是提供一种将数据建模为数据的好方法。它也不是 `JavaBeans` 的直接替代品，因为 `record` 的方法不符合 `JavaBeans` 的 `get` 标准。另外 `JavaBeans` 通常是可变的，而记录是不可变的。尽管他们的用途有点像，但记录并不会以某种方式取代 `JavaBeans`

#### 具体说明

当你用 `record` 声明一个类时，该类将自动拥有以下功能：

- 获取成员变量的简单方法，比如例题中的 `name()` 和 `partner()`。注意区别于我们平常 `getter()` 的写法
- 一个 `equals` 方法的实现，执行比较时会比较该类的所有成员属性
- 重写 `hashCode()` 方法
- 一个可以打印该类所有成员属性的 `toString` 方法
- 只有一个构造方法

~~~java
public record Order(int orderId, String orderName) {
}
// 这样定义一个 record 类，就会自动生成构造器,equals,hashCode,toString 等
~~~

此外：

- 还可以在 `record` 声明的类中定义静态字段、静态方法、构造器或实例方法
- 不能在 `record` 声明的类中定义实例字段，类不能声明为 `abstract`，不能声明显示的父类等

~~~java
// 还可以在 `record` 声明的类中定义静态字段、静态方法、构造器或实例方法
public record Person(int id, String name) {
    static String info = "我是一个人";
    public static void show(){
        System.out.println("我是一个人");
    }
    public Person(){
        this(0,null);
    }
    public void eat(){
        System.out.println("人吃饭");
    }
}
~~~

### 密封类 ---> JDK15预览特性 ---> JDK16二次预览 ---> JDK17转正

在 java 中如果想让一个类不能被继承和修改，这时我们应该使用 `final` 关键字对类进行修饰，不过这种要么可以继承，要么不能继承的机制不够灵活，有些时候我们可能想让某个类可以被某些类型继承，但是又不能随意继承，是做不到的，java15 尝试解决这个问题，引入了 `sealed` 类，被 `sealed` 修饰的类可以指定子类。这样这个类就只能被指定的类继承

~~~java
// Person 类只能被指定的 Student，Teacher， Worker 三个类继承
// 而且要求指定的子类必须是 final，sealed，non-sealed 三者之一
public sealed class Person permits Student,Teacher,Worker {}

class Student extends Person {} // Student 类不能被继承了
class Teacehr extends Person permits SeaiorTeacher{} // Teacher 类只能被指定的子类继承 
class Worker extends Person {} // Worker 类在被继承时，没有任何的限制

// 这里会报错
// class Farmer extends Person {}
~~~

## API 的变化

### Optional 类 ---> JDK8的新特性

到目前为止，臭名昭著的空指针异常是导致 Java 应用程序失败的最常见原因。以前，为了解决空指针异常，Google 在注明的 Guava 项目引入了 Optional 类，通过检查空值的方式避免空指针异常。收到 Google 的启发，Optional 类已经成为 Java8 类库的一部分

#### 介绍

`Optional<T>` 类 ( java.util.Optional ) 是一个容器类，他可以保存类型 T 的值，代表这个值存在，或者仅仅报错 null，表示这个值不存在，如果值存在，则 `isPresent` 方法会返回 true，调用 get 方法会返回该对象

**Optional 提供很多有用的方法**

- 创建 `Optional` 类对象的方法：
  - `static<T> Optional<T> empty()`：用来创建一个空的 `Optional` 实例
  - `static<T> Optional<T> of(T value)`：用来创建一个 `Optional` 实例，value 必须非空
  - `static<T> Optional<T> ofNullable(T value)`：用来创建一个 `Optional` 实例， vaule 肯呢个是空，也可能费控
- 判断 `Optional` 容器中是否包含对象
  - `boolean isPresent()`：判断 `Optional` 容器中的值是否存在
  - `void ifPresent(Consumer<? super T> consumer)`：判断 `Optional` 容器中的值是否存在，如果存在就将他进行 `Comnsumer` 指定的操作，如果不存在就不做
- 获取 `Optional` 容器的对象
  - `T get()`：如果调用对象包含值，返回该值。否则抛异常。`T get() 与 of(T value) ` 配合使用
  - `T orElse(T other)`：`orElse(T other)` 与 `ofNullable(T value)` 配合使用，如果 `Optional` 容器中非空，就返回所包装值，如果为空，就用 `orElse(T other) other` 指定的默认值代替
  - `T orElseGet(Supplier<? extends T> other)`：如果 `Optional` 容器中非空，就返回所包装值，如果为空，就用 `Supplier` 接口的 `Lambda` 表达式提供的值代替
  - `T orElseThrow(Supplier<? extends X> exceptionSypplier)`：如果 `Optional` 容器中非空，就返回所包装值，如果为空，就抛出你指定的异常类型代替原来的 `NoSuchElementException`

#### 为什么需要 Optional 类

为了避免代码中出现空指针异常

#### 如何实例化 Optional

~~~java
static <T> Optional<T> ofNullable(T value) // 用来创建一个 Optional 实例，value 可能是空，也可能非空
~~~

#### 常用方法

~~~java
String star = "迪丽热巴";
star = null;

// 使用 Optional 避免空指针的问题
// 1. 实例化
// 此时 value(star) 可能是 null，也可能不是 null
Optional<String> optional = Optional.ofNullable(star);
// get()：取出内部的 value 值，如果是 null 就会抛异常
optional.get(star);

// orElse(T other)：如果 Optional 实例内部的 value 属性不为 null，则返回value，如果 value 位null，则返回 other
String otherStar = "杨幂";
String finalStar = optional.orElse(otherStar);

// 因为 star 为 null，调用 toString 就会抛出空指针异常
System.out.prinln(finalStar.toString());

~~~

### String 存储结构和 API 变更 ---> JDK9

**内部从原本的 char 类型的数组改为了 byte 类型的数组**

~~~java
public final class String
    implements java.io.Serializable,Comparable<String>, CharSequence {
    @Stable
    private final byte[] value;
    ...
}
~~~

#### StringBuffer 和 StringBuilder

**内部同样改写成了 byte 类型的数组**

#### jdk11新特性：新增了一系列字符串处理方法

| 描述                 | 举例                                            |
| -------------------- | ----------------------------------------------- |
| 判断字符串是否为空白 | `" ".isBlank --> true`                          |
| 去除首尾空白         | `" Javastack".strip() -->  "Javastack"`         |
| 去除尾部空格         | `" Javastack".stripTrailing() -->  "Javastack"` |
| 去除首部空格         | `" Javastack".stripLeading() -->  "Javastack"`  |
| 复制字符串           | `"Java".repeat(3) --> "JavaJavaJava"`           |
| 行数统计             | `"A\nB\nC".lines().count() --> 3`               |

#### jdk12新特性

##### String 实现了 Constable 接口

~~~java
public final class string implements java.io.Serializable, Comparable<String>, CharSequence, Constable, ConstantDesc {}
~~~

`java.lang.constant.Constable` 接口定义了抽象方法

~~~java
public interface Constable {
    Optional<? extends ConstantDesc> describeConstable();
}

// 实现源码
// 其实就是调用 Optional.of 方法返回一个 Optional 类型
@Override
public Optional<String> describeConstable() {
    return Optional.of(this);
}

// 举例
private static void testDescribeConstable() {
    String name = "xxx";
    Optional<String> optional = name.describeConstable();
    System.out.priintln(optional.get()); // 输出 xxx
}
~~~

##### String 新增方法

`String 的 transform(Funtion)`

~~~java
var result = "foo".transform(input -> input + " bar");
System.out.println(result); //foo bar

var result1 = "foo".transform(input -> input + " bar").transform(String::toUpperCase);
System.out.println(result); //FOO BAR

// 对应源码
public <R> R transform(Funtion<? super String, ? extends R> f) {
    return f.apply(this);
}
~~~

### JDK17：标记删除 Applet API

Applet API 提供了一种将 Java AWT/Swing 空间嵌入到浏览器网页中的方法。不过，目前 Applet 已经被淘汰。

## 其他结构的变化

### JDK9：UnderScore ( 下划线 ) 使用的限制

在 java8 中，标识符可以独立使用 "-" 来命名

~~~java
String _ = "hello";
System.out.println(_);
~~~

但是在 java9 中规定 "_" 不再可以单独命名标识符了，如果使用，会报错

### JDK11：更简化的编译运行程序

看下面的代码

~~~java
// 在我们的认知力，要运行一个 Java 源代码必须先编译，再运行
// 编译
javac JavaStack.java;

// 运行
java JavaStack;

// 而java11 版本中，通过一个 java 命令就直接搞定了
java JavaStack.java;
~~~

### GC 方面新特性

GC 是 Java 主要优势之一。然而，当 GC 停顿太长，就会开始影响应用的响应时间。随着现代系统中内存不断增长，用户和程序员希望 JVM 能够以高效的方式充分利用这些内存，并且无需长时间的 GC 暂停时间

#### G1 GC

JDK9 以后得默认的垃圾回收期是 G1GC

##### JDK10：为 G1 提供并行的 Full GC

G1 最大的两点就是可以尽量的避免 full gc，但是毕竟是尽量，在有些情况下， G1就要进行 full gc 了，比如如果它无法足够快的回收内存的时候，他就会强制停止所有的应用线程然后清理

在 java10 之前，一个单线程版的 **标记 - 清除 - 压缩算法** 被用于 full gc，为了尽量减少 full gc 带来的影响，在 java10 中，就把之前的那个单线程版的标记 - 清除 - 压缩的 full gc 算法改成了支持多个线程同时 full gc，这样也算是减少了 full gc 所带来的停顿，从而提高新能

可以通过 `-XX:ParallelGCThreads` 参数来指定用于并行 GC 的线程数

##### JDK12：可中断的 G1 Mixed GC

##### JDK12：增强 G1，自动返回未用堆内存给操作系统

#### Shenandoah GC

##### JDK12：Shenandoah GC：低停顿时间的 GC

##### JDK15：Shenandoah 垃圾回收算法转正

### 革命性的 ZGC

#### JDK11：引入革命性的 ZGC

ZGC 是 JDK11 最为瞩目的特性，没有之一

ZGC 是一个并发、基于 region、压缩性的垃圾回收期

ZGC 的设计目标是：支持 TB 级内存容量，暂停时间低 ( < 10ms )，对整个程序吞吐量的影响小于 15%，将来还可以扩展实现机制，以支持不少令人兴奋的功能，例如多层堆 ( 即热对象置于 DRAM 和冷对象置于 NVME 闪存 )，或压缩堆

#### JDK13：ZGC：将未使用的堆内存归还操作系统

#### JDK14：ZGC on macOS 和 windows

- 在 jdk14 之前，ZGC 仅 Linux 才支持。现在 mac 或 windows 上也能使用 ZGC 了

  ~~~java
  -XX:+UnlickExperimentalVMOptions -XX:+UseZGC
  ~~~

- ZGC 与 Shenandoah 目标高度相似，在尽可能对吞吐量影响不大的前提下，实现在任意堆内存大小都可以把垃圾收集的停顿时间限制在 **十毫秒以内** 的低延迟

#### JDK15：ZGC 功能转正

ZGC 是 java11 引入的新的垃圾收集器，经历了多个实验阶段，自此终于成为正式特性

但是这并不是替换默认的 GC，默认的 GC 仍然还是 G1；之前通过 `-XX:+UnlockExperimentalVMOptions`、`-XX:+UseZGC` 来启用 ZGC，现在只需要 `-XX:+UseZGC` 就可以。相信不就得将来它比较成为默认的垃圾回收器

##### Shenandoah 和 ZGC 的关系

- 相同点：性能几乎可认为是相同的
- 不同点：ZGC 是 Oracle JDK 的，根正苗红，而 Shenandoah 只存在于 Open JDK 中，因此使用时需要注意 JDK 版本

#### JDK16：ZGC 并发线程处理

在县城的堆栈处理过程中，总有一个制约因素就是 `safepoints` 在 `safepoints` 这个点，Java 的线程是要暂停执行的，从而限制了 GC 的效率