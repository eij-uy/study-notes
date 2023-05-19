[toc]



# 异常

## 异常的概念

- 基本概念

Java 语言中, 将程序执行中发生的不正常情况成为异常, ( 开发过程中的语法错误和逻辑错误不是异常 )

- 执行过程中所发生的的异常事件可分为两大类

  1. Error

     Java 虚拟机无法解决的严重问题, 如: JVM 系统内部错误, 资源耗尽等严重情况. 比如: StackOverflowError[栈溢出] 和 OOM( out of memory ), Error 是严重错误, 程序会崩溃

  2. Exception

     其他因变成错误或偶然的外在因素导致的一般性问题. 可以使用针对性的代码进行处理. 例如空指针访问, 视图读取不存在的文件, 网络连接中断等等, Exception 分为两大类: 运行时异常[] 和 编译时异常[]

     - 编译异常

       在 Java 源程序进行 javac 编译的时候出现的异常

     - 运行异常

       在字节码文件进行 java 运行的时候出现的异常

## 异常体系图

1. 异常分为两大类, 运行时异常和编译时异常
2. 运行时异常, 编译器不要求强制处置的异常. 一般是指编译时的逻辑错误, 是程序员应该避免其出现的异常, java.lang.runtimeException 类及它的子类都是运行时异常
3. 对于运行时异常, 可以不作处理, 因为这类异常很普通, 若全处理可能会对程序的可读性和运行效率产生影响
4. 编译时异常, 是编译器要求必须处置的异常

## 常见的运行时异常

1. NullPointerException 空指针异常

   当应用程序视图咋急需要对象的地方使用 null 时, 抛出该异常

   ~~~java
   String name = "xxxx";
   System.out.println(name.length()); //这时候是正常运行
   
   String name1 = null;
   System.out.println(name1.length()); 
   //这时候抛出 NullPointerException 异常
   ~~~

   

2. ArithmeticException 数学运算异常

   当出现异常的运算条件时, 掏出此异常, 例如, 一个正数除以零时, 抛出此类的一个实例

   ~~~java
   int num1 = 10;
   int num2 = 0;
   
   int num3 = num1 / num2 // 抛出 ArithmeticException 异常
   ~~~

   

3. ArrayIndexOutOfBoundsException 数组下标越界异常

   用非法索引访问数组时抛出的异常, 如果索引为负或大于等于数组大小, 则该索引为非法索引

   ~~~java
   int arr = { 1,2,3,4 }
   for(int i = 0; i < arr.length; i++){
       System.out.ptintln(arr[i]) // 正常运行
   }
   
   int arr1 = { 1,2,3,4 }
   for(int i = 0; i <= arr1.length; i++){
       System.out.ptintln(arr[i]) 
           // 抛出 ArrayIndexOutOfBoundsException 异常
   }
   ~~~

   

4. ClassCastException 类型转换异常

   当视图将对象强制转换为不是实例的子类时, 抛出该异常

   ~~~java
   class A {}
   class B extends A {}
   class C extends B {}
   
   A b = new B(); //向上转型 OK
   B b2 = (B)b; //向下转型 OK
   C c2 = (C)b; // B 和 C 都是 A 的子类, 但是彼此没有关系, 这时候强转会抛出 ClassCastException 异常
   
   ~~~

   

5. NumberFormatException 数字格式不正确异常

   当应用程序视图将字符串转换成一种数组类型, 但该字符串不能转换为适当格式时, 抛出该异常 => 使用异常我们可以确保输入是满足条件数字

   ~~~java
   String name = "1234";
   int num = Integer.parseInt(name); // String => int 正确的
   
   String name1 = "xxx";
   int num1 = Integer.parseInt(name); 
   // 抛出 NumberFormatException 异常, 因为 xxx 不能转换为 int
   ~~~

   

## 常见编译异常

1. SQLException 操作数据库时, 查询表可能发生异常
2. IOException 操作文件时, 发生的异常
3. FileNotFoundException 当操作一个不存在的文件时, 发生异常
4. ClassNotFoundException 加载类, 而该类不存在时, 异常
5. EOFEException 操作文件,到文件末尾,发生异常
6. IIIegalArguementException 参数异常

## 异常处理的方式

1. try-catch-finally

   程序员在代码中捕获发生的异常, 自行处理

2. throws

   将发生的异常抛出, 交给调用者(方法)来处理, 最顶级的处理者就是JVM

   ~~~java
   public void f1() {
       // 创建一个文件流对象
       // 1. 这里的异常是一个 FileNotFoundException 编译异常
       FileInputStream fis = new FileInputStram("d://aa.txt");
   }
   
   public void f2() {
       // 2. 使用try-catch-finally 捕获
       try {
       	FileInputStream fis = new FileInputStram("d://aa.txt");
       } catch(e) {
           System.out.println(e.getMessage());
       }
   }
   
   public void f3() throws FileNotFoundException {
       // 3. 使用 throws 抛出异常, 让调用 f3 方法的调用者 (方法) 处理
       // 这里抛出的异常只能是 FileNotFoundException 或者它的父类 Exception, Throwable...
       // 在方法声明中用 throws 语句可以声明抛出异常的列表, throws 后面的异常类型可以是方法中产生的异常类型, 也可以是它的父类
       // throws 关键字后也可以是 异常列表, 即可以抛出多个异常
       FileInputStream fis = new FileInputStram("d://aa.txt");
   }
   
   public void f4() throws FileNotFoundException,NullPointerException,ArithmeticException {
       // throws 关键字后也可以是 异常列表, 即可以抛出多个异常
       FileInputStream fis = new FileInputStram("d://aa.txt");
   }
   ~~~

   细节: 

   1. 对于编译异常, 程序中必须处理, 比如 try-catch 或者 throws

   2. 对于运行时异常, 程序中如果没有处理, 默认就是 throws 的方式处理

   3. 子类重写父类的方法时, 对抛出异常的规定: 

      类重写的方法,所抛出的异常类型要么和父类抛出的异常一直, 要么为父类抛出的异常的类型的子类型

      ~~~java
      class Father {
          public void f1() throws RuntimeException {}
      }
      
      class Son extends Father {
          // 这个 NullException 异常必须是 RuntimeException, 或者它的子类
          @Override
          public void f1() throws NullException {}
      }
      ~~~

      

   4. 在 throws 过程中, 如果有方法 try-catch, 就相当于处理异常, 就可以不比 throws

## 自定义异常

**一般来说, 我们自定义异常是 继承 RuntimeException , 即把自定义异常做成 运行时异常, 好处是我们可以使用默认的处理机制**

### 基本概念

当程序中出现了某些错误, 但该错误信息并没有在 Throwable 子类中描述处理, 这个时候可以自己设计异常类, 用于描述该错误信息

### 步骤

1. 定义类

   自定义异常类名( 程序员自己写 )继承 Exception 或 RuntimeException

2. 如果继承 Exception, 属于编译异常

3. 如果继承 RuntimeException, 属于运行异常( 一般来说, 继承RuntimeException )

~~~java
class AgeException extends RuntimeException {
    public AgeException(String message){
        super(message)
    }
}

int age = 80;
if(!(age >= 18 && age <= 120)){
    throw new AgeException("年龄需要在18~120之间")
}
System.out.println("你的年龄范围正确")
~~~

## throw 和 throws 的区别

|        | 意义                     | 位置       | 后面跟的东西 |
| ------ | ------------------------ | ---------- | ------------ |
| throws | 异常处理的一种方式       | 方法声明处 | 异常类型     |
| throw  | 手动生成异常对象的关键字 | 方法体中   | 异常对象     |

## 练习

~~~java
public static void main(String[] args){
    if(args[4].equals("join")){ // 可能发生 ArrayIndexOutOfBoundsException 数组下标越界异常 或者 NullPointerException, 空指针异常,
        System.out.println("AA")
    }else{
        System.out.println("BB")
    }
    Object o = args[2]; //String -> Object 向上转型 正确
    integer i = (integer)o // 错误, 这里一定会抛出 ClassCastException 异常
}
~~~

