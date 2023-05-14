[toc]

# 基本介绍

接口就是给出一些没有实现的方法, 封装到一起, 到某个类要使用的时候, 在根据具体情况把这些方法写出来.

1. 如果一个类 implements实现 接口
2. 需要将该接口的所有抽象方法都实现

# 语法

~~~java
interface 接口名 {
    //属性
    //方法
    	//1. 抽象方法
    	//2. 默认实现方法
    	//3. 静态方法
}

class 类名 implements 接口 {
	//自己属性
    //自己方法
    //必须实现接口的抽象方法
}
~~~

# 小结 

1. 在 jdk7.0 前, 接口中的所有方法都没有方法体; ( 即都是抽象方法 )
2. jdk8.0 后接口可以有静态方法, 默认方法, 也就是说接口中可以有方法的具体实现
   - 在 jdk8 后, 可以有默认实现方法, 需要使用 default 关键字修饰
   - 在 jdk8 后, 可以有静态方法, ( 因为是静态方法所以不是抽象的了 )

# 细节

1. 接口不能被实例化
2. 接口中所有的方法是 public 方法, 接口中抽象方法, 可以不用 abstract 修饰
3. 一个普通类实现接口, 就必须将该接口的所有方法都实现
4. 抽象类实现接口, 可以不用实现接口的方法
5. 一个类同时可以实现多个接口
6. 接口中的属性, 只能是 final 的, 而且是 public static final 修饰符. 比如 int a = 1; 实际上是 public static final int a = 1; ( 必须初始化 )
7. 接口中属性的访问形式: 接口名.属性名
8. 一个接口不能集成其他的类, 但是可以集成多个别的接口
9. 接口的修饰符只能是 public 和默认, 这点和类的修饰符是一样的

# 实现接口和继承类的区别

- 接口和继承解决的问题不同

  - 继承的价值主要在于: 解决代码的复用性和可维护性
  - 接口的价值主要在于: 设计, 设计好各种规范(方法), 让其他类去实现这些方法

- 接口比继承更加灵活

  接口比继承更加灵活, 继承是满足 is - a 的关系, 而接口只需要满足 like - a 的关系

- 接口在一定程度上实现代码解耦 [ 即 接口规范性 + 动态绑定 ]

# 接口的多态特性

1. 多态参数

   ~~~java
   class Computer {
       //1. UseInterface useInterface 形参是接口类型 UsbInterface
       //2. 接收 实现了 UsbInterface 接口的类的对象实例
       public void work(UsbInterface usbInterface){
           
           usbInterface.start();
           usbInterface.close();
       }
   }
   
   interface UsbInterface {
   	void start();
       void close();
   }
   
   class Phone implements UsbInterface {
       public void start(){
           //xxx
       }
       public void close(){
           //xxx
       }
   }
   
   Computer computer = new Computer();
   Phone phone = new Phone();
   computer.work(phone)
   ~~~

2. 多态数组

   ~~~java
   interface USB {
       void work();
   }
   
   class Phone implements USB {
       public void call(){
           //xxx
       }
       public void work(){
           //xxx
       }
   }
   
   class Camera implements USB {
       public void work(){
           //xxx
       }
   }
   
   USB[] usbs = new USB[2];
   usbs[0] = new Phone();
   usbs[1] = new Carema();
   
   for(int i = 0; i < usbs.length; i++){
       if(usbs[i] instanceof Phone){
           //((Phone) usbs[i]).call(); //简写
           Phone ph = (Phone) usbs[i];
           ph.call();
       }
       usbs[i].work();
   }
~~~
   
3. 多态传递

   ~~~java
   public class InterfacePolyPass {
       public static void main(String[] args){
           IG ig = new Teacher();
           IH ih = new Teacher(); // 如果 IG 没有继承 IH 那么这一句会报错
           // 也就是说 如果 IG 继承了 IH 接口, 而Teacher 类实现了IG接口 那么,实际上就相当于 Teacher 类也实现了 IH 接口
           // 这就是多态传递
       }
   }
   
   class IH {}
   class IG extends IH {}
   class Teacher implements IG {}
   ~~~

   