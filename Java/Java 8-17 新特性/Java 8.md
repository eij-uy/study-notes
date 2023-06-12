[toc]

# Java 8

**针对于集合，java8 中增加了一个方法 forEach**

## Lambda 表达式

**适用于接口中只有一个方法**

### 格式

`lambda 形参列表 -> lambda 体`

- `->` ：lambda 操作符或箭头操作符
- `-> 的左边`：lambda 形参列表，对应着要重写的接口中的抽象方法的形参列表
- `-> 的右边`：lambda 体，对应着接口的实现类要重写的方法的方法体

### 使用

~~~java
Comparator<Integer> com1 = new Comparator<Integer>() {
	@Override
    public int compare(Integer o1, Integer o2) {
        return Integer.compare(o1, o2);
    }
}
// 使用 Lambda 表达式编写
// 1. 简写
Comparator<Integer> com2 = (Integer o1, Integer o2) -> {
    return Integer.compare(o1, o2)
};
// 2. 再简写
Comparator<Integer> com3 = (Integer o1, Integer o2) -> Integer.compare(o1, o2);
// 3. 还能简写
Comparator<Integer> com4 = (o1, o2) -> Integer.compare(o1, o2);
~~~

### 案例

**lambda 表达式书写的六种格式**

- 语法格式一：无参，无返回值

  ~~~java
  Runnable r1 = new Runnable() {
  	@Override
      public int run() {
          System.out.println("我爱天安门1");
      }
  }
  
  r1.run();
  // 使用 lambda 表达式
  Runnable r2 = () -> {
      System.out.println("我爱天安门2");
  }
  ~~~

- 语法格式二：Lambda 需要你个参数，但是没有返回值

  ~~~java
  Consumer<String> con = new Consumer<String>() {
  	@Override
      public int accept(String s) {
          System.out.println(s);
      }
  }
  con.accept("谎言和誓言的的区别是什么?");
      
  // 使用 lambda 表达式
  Consumer<String> con1 = (String s) -> {
      System.out.println(s);
  }
  con1.accept("一个是说的人当真了，一个是听得人当真了。")
  ~~~

- 语法格式三：数据类型可以省略，因为可由编译器推断得出，成为 **类型推断**

  ~~~java
  Consumer<String> con1 = (String s) -> {
      System.out.println(s);
  }
  con1.accept("如果大学可以重来，你最想重来的事是啥?");
  Consumer<String> con2 = (s) -> {
      System.out.println(s);
  }
  con1.accept("xxx);
  ~~~

- 语法格式四：Lambda 若只需要一个参数时，参数的小括号可以省略

  ~~~java
  Consumer<String> con1 = (s) -> {
      System.out.println(s);
  }
  con1.accept("世界那么大，我想去看看");
  
  Consumer<String> con2 = s -> {
      System.out.println(s);
  }
  con1.accept("世界那么大，我想去看看");
  ~~~

- 语法格式五：Lambda 需要两个或以上的参数，多条执行语句，并且可以有返回值

  ~~~java
  Comparator<Integer> com1 = new Comparator<Integer>() {
      @Override
      public int compare(Integer o1, Integer o2) {
          System.out.println(o1);
          System.out.println(o2);
          return o1.compareTo(o2);
      }
  }
  System.out.println(com1.compare(12,21));
  
  Comparator<Integer> com2 = (o1, o2) -> {
      System.out.println(o1);
      System.out.println(o2);
      return o1.compareTo(o2);
  };
  System.out.println(com2.compare(12,21));
  ~~~

- 语法格式六：当 Lambda 体只有一条语句时，return 与大括号若有，都可以省略

  ~~~java
  Comparator<Integer> com1 = (o1, o2) -> {
      return o1.compareTo(o2);
  }
  System.out.println(com2.compare(12,6));
  
  Comparator<Integer> com2 = (o1, o2) -> o1.compareTo(o2);
  System.out.println(com2.compare(12,16));
  ~~~

### 本质

- 一方面，lambda 表达式作为接口的实现类的对象
- 另一方面，lambda 表达式是一个匿名函数

### 函数式接口

#### 什么是函数式接口

如果接口中只声明有一个抽象方法，则此接口就成为函数式接口

#### 为什么需要函数式接口

因为只有给函数式接口提供实现类的对象时，我们才可以使用 lambda 表达式

#### api 中函数式接口所在的包

jdk8中声明的函数式接口都在 `java.util.function` 包下

#### 4 个基本的函数式接口

| 函数式接口      | 称谓       | 参数类型 | 用途                                                         |
| --------------- | ---------- | -------- | ------------------------------------------------------------ |
| `Consumer<T>`   | 消费型接口 | T        | 对类型为 T 的对象应用操作，包含方法：`void accept(T t)`      |
| `Supplier<T>`   | 供给型接口 | 无       | 返回类型为 T 的对象，包含方法： `T get()`                    |
| `Function<T,R>` | 函数型接口 | T        | 对类型为 T 的对象应用操作，并返回结果，结果是 R 类型的对象。包含方法：`R apply(T t)` |
| `Predicate<T>`  | 判断型接口 | T        | 确定类型为 T 的对象是否满足某约束，并返回 boolean 值。包含方法：`boolean test(T t)` |

### 总结

- `-> 的左边`：lambda 形参列表，参数的类型都可以省略。如果形参只有一个，则一对 () 也可以省略
- `-> 的右边`：lambda体，对应着重写的方法的方法体。如果方法体重只有一行执行语句，则一对 {} 可以省略，如果有 return 关键字，则必须一并省略

## 方法引用

### 对方法引用的理解

- 可以看做是基于 lambda 表达式的进一步刻画
- 当需要提供一个函数式接口的实例时，我们可以使用 lambda 表达式提供此实例
  - 当满足一定条件的情况下，我们还可以使用 **方法引用** 或 **构造器引用** 替换 lambda 表达式

### 本质

方法引用作为了函数式接口的实例

### 格式

`类(或对象) :: 方法名`

- 情况一：`对象 :: 实例方法`

  - 要求：**函数式接口中的抽象方法 a 与其内部实现时调用的对象的某个方法 b 的形参列表和返回值类型相同或满足多态场景，此时，可以考虑使用方法 b 实现对方法 a 的替换、覆盖，此替换或覆盖即为方法引用**
  - 注意：此方法 b 是非静态的方法，需要对象调用

  ~~~java
  // 1. 传统写法
  Consumer<String> con1 = new Consumer<String>() {
      @Overiride
      public void accept(String s) {
          System.out.println(s);
      }
  }
  
  Employee emp = new Employee(1001,"马化腾",34,6000.38);
  Supplier<String> sup1 = new  Supplier<String>() {
      @Override
      public String get(){
          return emp.getName();
      }
  }
  
  con1.accept("hello!");
  
  // 2. lambda 表达式
  Consumer<String> con2 = s -> System.out.println(s);
  Supplier<String> sup2 = () -> emp.getName();
  // 3. 方法引用
  Consumer<String> con3 = System.out :: println;
  Supplier<String> sup3 = emp :: getName;
  ~~~

- 情况二：`类 :: 静态方法`

  - 要求：**函数式接口中的抽象方法 a 与其内部实现时调用的类的的某个静态方法 b 的形参列表和返回值类型相同或满足多态场景，此时，可以考虑使用方法 b 实现对方法 a 的替换、覆盖，此替换或覆盖即为方法引用**
  - 注意：此方法是静态的方法，需要类调用

  ~~~java
  // 1. 传统写法
  Comparator<Integer> com1 = new Comparator<Integer>() {
      @Override
      public int compare(Integer o1, Integer o2) {
          return Integer.compare(o1. o2);
      }
  }
  
  Function<Double, Long> fun1 = new Function<Double, Long>() {
      @Override
      public Long apply(Double, aDouble) {
          return Math.round(aDouble);
      }
  }
  
  // 2. lambda 表达式
  Comparator<Integer> com2 = (o1, o2) -> Integer.compare(o1, o2);
  Function<Double, Long> fun2 = (aDouble) -> Math.round(aDouble);
  
  // 方法引用
  Comparator<Integer> com3 = Integer :: compare;
  Function<Double, Long> fun3 = Math :: round;
  ~~~

- 情况三：`类 :: 实例方法`

  - 要求：**函数式接口中的抽象方法 a 与其内部实现时调用的对象的某个方法 b 的 返回值类型相同或满足多态场景，同时，抽象方法 a 中有 n 个参数，方法 b 中有 n - 1 个参数，且抽象方法 a 的第一个参数作为方法 b 的调用者，且抽象方法 a 的后 n - 1 个参数与方法 b 的 n - 1 个参数的类型相同或满足多态场景，此时，可以考虑使用方法 b 实现对方法 a 的替换、覆盖，此替换或覆盖即为方法引用**
  - 注意：此方法 b 是非静态的方法，需要对象调用，但是形式上，写成对象 a 所属的类

  ~~~java
  Comparator<String> com1 = new Comparator<String>() {
      @Override
      public int compare(String o1, String o2){
          return o1.compareTo(o2);
      }
  }
  BiPredicate<String, String> biPre1 = new BiPredicate<String,String>() {
      @Override
      public boolean test(String s1, String s2) {
          return s1.equals(s2);
      }
  }
  Employee emp = new Employee(1001,"马化腾",34,6000.38);
  Function<Employee, String> fun1 = new Funtion<Employee, String>() {
      @Override
      public String apply(Employee emp){
          return emp.getName();
      }
  }
  
  // 2. lambda 表达式
  Comparator<String> com2 = (o1, o2) -> i1.compareTo(o2);
  BiPredicate<String, String> biPre2 = (s1, s2) -> si.equals(s2);
  Function<Employee, String> fun2 = employee -> employee.getName();
  
  // 3. 方法引用
  Comparator<String> com3 = String :: compareTo;
  BiPredicate<String, String> biPre3 = String :: equals;
  Function<Employee, String> fun3 = Employee :: getName;
  ~~~

## 构造器引用

### 格式

`类名 :: new`

~~~java
// 1. 传统写法
Supplier<Employee> sup1 = new Supplier<Employee>() {
    @Override
    public Employee get() {
        return new Employee();
    }
}
System.out.println(sup1.get());

Function<Integer, Employee> func1 = new Function<Integer, Employee>() {
    @Override
    public Employee apply(Integer id) {
        return new Employee(id);
    }
}
System.out.println(func1.apply());

BiFunction<Integer, String, Employee> biFunc1 = new BiFunction<Integer, String, Employee>() {
    @Override
    public Employee apply(Integer id, String name) {
        return new Employee(id, name);
    }
}

// 2. 构造器引用
Supplier<Employee> sup2 = Employee :: new;
sup2.get();
Function<Integer, Employee> func2 = Employee :: new; // 它调用的是 Employee 类中参数是 int 类型的构造器，这里有装箱拆箱的操作，因为泛型不能是基础类型
BiFunction<Integer, String, Employee> biFunc2 = Employee :: new; // 它调用的是 Employee 类中参数是 int 和 String 类型的构造器，这里有装箱拆箱的操作，因为泛型不能是基础类型
~~~

### 说明

- 调用了类名对应的类中某一个确定的构造器
- 具体调用的是类中的哪一个构造器，取决于函数式接口的抽象方法的形参列表

### 数组引用

#### 格式

`数组名[] :: new`

~~~java
// 1. 传统方法
Function<Integer, Employee[]> func1 = new Function<Integer, Employee[]>() {
    @Override
    public Employee[] apply(Integer length){
        return new Emplyee[length];
    }
}

// 2. 数组引用
Function<Integer, Employee[]> func2 = Employee[] :: new;
~~~

## Stream API

### 介绍

`Stream API(java.util.stream)` 把真正的函数式编程风格引入到 java 中。这是目前为止对 Java 类库 最好的补充，因为 `Stream API` 可以极大提供 Java 程序员的生产力，让程序员写出高效率、干净、简洁的代码。

### Stream API vs 集合

- `Stream API` 关注的是多个数据的计算 ( 排序、查找、过滤、映射、遍历等 )，面向 CPU 的，而集合关注的是数据的存储，向面向内存的。
- `Stream API` 之于集合，类似 SQL语句 之于数据表。

### 使用说明

1. `Stream` 自己不会储存元素
2. `Stream` 不会改变源对象。相反，他们会返回一个持有结果的新的 `Sreeam`
3. `Stream` 操作是延迟执行的，这意味着他们会等到需要结果的时候才执行，即一旦执行终止操作，就执行中间操作链，并产生结果
4. `Stream` 一旦执行了终止操作，就不能再调用其他中间操作或终止操作了

### Stream 执行流程

1. `Stream` 实例化
2. 一系列的中间操作
3. 执行终止操作

### 常用方法

- 中间操作

| 方法                              | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| 1. 筛选与切片                     |                                                              |
| `filter(Predicatep)`              | 接收 Lambda，从流中排除某些元素                              |
| `distinct()`                      | 筛选，通过流所生成元素的 `hashCode()` 和 `equals()` 取出重复元素 |
| `limit(long maxSize)`             | 截断流，使其元素不超过给定数量                               |
| `skip(long n)`                    | 跳过元素，返回一个扔掉前 n 个元素的流。<br>若流中元素不足 n 个，则返回一个空流。与 `limit(n)` 互补 |
| 2. 映射                           |                                                              |
| `map(Function f)`                 | 接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素 |
| `mapToDouble(ToDoubleFunction f)` | 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 `DoubleStream` |
| `mapToInt(ToIntFunction f)`       | 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 `IntStream` |
| `mapToLong(ToLongFunction f)`     | 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 `LongStream` |
| `flatMap(Function f)`             | 接收一个函数作为参数，将流中的每个值都转换成另一个流，然后把所有流连接成一个流 |
| 3. 排序                           |                                                              |
| `sorted()`                        | 产生一个新流，其中按自然顺序排序                             |
| `sorted(Comparator com)`          | 产生一个新流，其中按比较器顺序排序                           |

- 终止操作
  - 终止操作会从流的流水线生成结果。其结果可以是任何不是流的值，例如：List，Integer，甚至是 void
  - 流进行了终止操作后，不能再次使用

| 方法                                   | 描述                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| 1. 匹配与查找                          |                                                              |
| `allMatch(Predicate p)`                | 检查是否匹配所有元素                                         |
| `anyMatch(Predicate p)`                | 检查是否至少匹配一个元素                                     |
| `noneMatch(Predicate p)`               | 检查是否没有匹配所有元素                                     |
| `findFirst()`                          | 返回第一个元素                                               |
| `findAny()`                            | 返回当前流中的任意元素                                       |
| `count()`                              | 返回流中元素总数                                             |
| `max(Comparator c)`                    | 返回流中最大值                                               |
| `min(Comparator c)`                    | 返回流中最小值                                               |
| `forEach(Consumer c)`                  | 内部迭代 ( 使用 `Collection` 接口需要用户去做迭代，成为外部迭代。<br>相反，`Stream API` 使用内部迭代   --->   它帮你把迭代做了 ) |
| 2. 归约                                |                                                              |
| `reduce(T identity, BinaryOperator b)` | 可以将流中元素反复结合起来，得到一个值。返回 T               |
| `reduce(BinaryOperator b)`             | 可以将流中元素反复结合起来，得到一个值。返回 `Optional<T> `  |
| 3. 收集                                |                                                              |
| `collect(Collector c)`                 | 将流转换成其他形式。接收一个 `Collector ` 接口的实现，<br>用于给 `Stream` 中元素作汇总的方法 |

### 案例

1. 创建 Stream 实例

~~~java
// 创建 Stream 
// 1. 方式一：通过集合
List<Employee> list = EmployeeData.getEmployees();
// 1.1. default Stream<E> stream()：返回一个顺序流
Stream<Employee> stream = list.stream();
// 1.2. default Stream<E> parallelStream()：返回一个并行流
Stream<Employee> stream = list.parallelStream();
// 2. 方式二：通过数组
// 2.1 调用 Aarrays 类的 static <T> Stream<T> stream(T[] array)：返回一个流
Integer[] arr = new Integer[]{1,2,3,4,5};
Stream<Integer> stream = Arrays.stream(arr);
// 因为泛型不能是基础类型，所以 int 数组得到的 stream 流是 intStream，其他 double，float 等同上
int[] arr = new int[]{1,2,3,4};
IntStream stream1 = Arrays.stream(arr1);
// 3. 方式三：通过 Stream 的 of()
Stream <String> stream = Stream.of("aa","bb");
~~~

2. 中间操作 

~~~java
// 1. 筛选与切片

// 1.1. filter(Predicate p) 接收 Lambda，从流中排除某些元素 
// 练习：想查询员工表中薪资大于 7000 的员工信息
List<Employee> list = EmployeeData.getEmployees();
Stream<Employee> stream = list.stream();
// 这里的 forEach 就是一个终止操作，在获取到工资大于 7000 的员工后并将他们打印出来
stream.filter(emp -> emp.getSalary() > 7000).forEach(System.out::println);
// 1.2. limit(n) 截断流，使其元素不超过给定数量
// 这里会报错，因为上面执行了终止操作，这时就不能再执行其他的中间操作和终止操作了
// 要想再次调用就要先获取一个新的 Stream 实例
stream.limit(2).forEach(System.out::println); // 错误的
// 获取到前四个员工并打印出来
list.stream().limit(4).forEach(System.out::println); // 正确的
// 1.3 skip(n) 跳过元素，返回一个扔掉前 n 个元素的流。
list.stream().skip(5).forEach(System.out::println);
// 1.4 distinct() 筛选，通过流所生成元素的 hashCode 和 equals 取出重复元素
list.stream().distinct().forEach(System.out::println);

// 2. 映射
// 2.1 map(Function f) 接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素
// 练习：转换成大写
List<String> list = Arrays.asList("aa","bb","cc","dd");
list.stream().map(str -> str.toUpperCase()).forEach(System.out::println); // Lambda 表达式
list.stream().map(String :: toUpperCase).forEach(System.out::println); // 方法引用
// 练习：获取员工姓名长度大于 3 的员工的姓名
List<Employee> list1 = EmployeeData.getEmployees();
// 2.1.1 方式一
list1.stream()
    .filter(emp -> emp.getName().length() > 3)
    .map(emp -> emp.getName())
    .forEach(System.out::println);
// 2.1.2 方式二
list1.stream()
    .map(emp -> emp.getName())
    .filter(name -> name.length() > 3)
    .forEach(System.out::println);
// 2.1.3 方式三
list1.stream()
    .map(Employee :: getName)
    .filter(name -> name.length() > 3)
    .forEach(System.out::println);

// 3. 排序
Integer[] arr = new Integer[]{345,3,64,3,45,7,3,34,65,68};
String[] arr1 = new String[]{"GG","DD","MM","SS","JJ"};
// 3.1 socted() 自然排序
Arrays.stream(arr).sorted().forEach(System.out::println);
Arrays.stream(arr1).sorted().forEach(System.out::println);
// 3.2 socted(Comparator com) 定制排序
// 前一个是降序，第二个是升序
Arrays.stream(arr1).sorted((s1, s2) -> -s1.compareTo(s2)).forEach(System.out::println);
Arrays.stream(arr1).sorted(String :: compareTo).forEach(System.out::println);
~~~

3. 结束操作

~~~java
// 匹配与查找
// 1. allMatch(Predicate p)  检查是否匹配所有元素
// 1.1 是否所有员工的年龄都大于 18 岁
List<Employee> list = EmployeeData.getEmployees();
Boolean flg = list.stream().allMatch(emp -> emp.getAge() > 18);
// 1.2 是否存在年龄大于 18 岁的员工
Boolean flg = list.stream().anyMatch(emp -> emp.getAge() > 18);

// 2. findFirst() 返回第一个元素,是一个 Optional，可以通过 get 方法得到原本的类型
Optional option = list.stream().findFirst();
Employee emp = option.get();

// 3. count() 返回流中元素的个数
long count = list.stream().count(); 

// 4. max(Comparator c) 返回流中最大值
list.stream().map(Employee :: getSalary).max(Double :: compare).get();

// 5. min(Comparator c) 返回流中最小值，同上
// 6. forEach(Consumer c) 内部迭代
list.stream().forEach(System.out::println);

// 归约
// 1. reduce(T identity, BinaryOperator)  可以将流中元素反复结合起来，得到一个值，返回 T
// 1.1 计算 1 ~ 10 的自然数的和
List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8,9,10);
list.stream().reduce(0, (x1,x2) -> x1 + x2); // 55
list.stream().reduce(0, (x1,x2) -> Integer.sum(x1,x2)); // 55
list.stream().reduce(0, Integer :: sum); // 55

// 2. reduce(BinaryOperator)  可以将流中元素反复结合起来，得到一个值，返回Optional<T>
// 2.1 计算公司所有员工工资的综合
List<Integer> list1 = EmployeeData.getEmployees();
list1.stream().map(Employee::getSalary).reduce(Double::sum).get();

// 收集
// 1. collect 将流转换为其他形式，接收一个 Collector 接口的实现，用于 Stream 中元素做汇总的方法
// 1.1 查找工资大于 6000 的员工，结果返回一个 List 的 Set
List<Employee> list2 = list1.stream().collect().filter(Employee::getSalary).collect(Collectors.toList());
list2.forEach(System.out::println);
// 1.2 按照员工的年龄进行排序，返回到一个新的 list 中
List<Employee> list3 = lis1t.stream()
  .sorted((e1,e2) -> e1.getAge() - e2.getAge())
  .collect(Collectors.toList());
list3.forEach(System.out::println);
~~~

