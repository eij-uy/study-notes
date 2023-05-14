[toc]

# 设计模式

什么是设计模式

1. 静态方法和属性的经典使用
2. 设计模式是在大量的时间中总结和理论化之后优选的代码结构、编程风格、以及解决问题的思考方式. 设计模式就想经典的棋谱, 不同的棋局, 我们要用不同的棋谱, 免去我们自己再思考和摸索

## 单例模式

1. 所谓类的单例设计模式, 就是采取一定的方法保证在整个软件系统中, 对某个类只能存在一个对象实例, 并且该类只提供一个取得其对象实例的方法

2. 单例模式有两种方法

   - 饿汉式

     1. 构造器私有化 => 防止用户直接 new
     2. 类的内部创建对象
     3. 向外暴露一个静态的公共方法. getInstance
     4. 代码实现

     ~~~java
     public class _single {
         public static void main (Stirng[] args){
             GirlFriend instance1 = GirlFriend.getInstance();
             
             GirlFriend instance2 = GirlFriend.getInstance();
             //这时instance1 和 instance2 指向的是同一个对象
             
             System.out.println(instance1 == instance2)//true
         }
     }
     
     //1. 构造器私有化
     class GirlFriend {
         private String name;
         //!!!
         private GrilFriend(String name){
             this.name = name;
         }
     }
     
     //2.在类的内部直接创建
     class GirlFriend {
         private String name;
         //为了能够在静态方法中, 返回gf对象, 需要将其修饰为静态对象
         //!!!
         private static GirlFriend gf = new GirlFriend("小红");
         private GrilFriend(String name){
             this.name = name;
         }
     }
     
     //3.提供一个公共的static方法, 返回gf对象
     class GirlFriend {
         private String name;
         //为了能够在静态方法中, 返回gf对象, 需要将其修饰为静态对象
         private GirlFriend gf = new GirlFriend("小红");
             
         private GrilFriend(String name){
             this.name = name;
         }
         //!!!
         public static GirlFriend getInstance(){
             return gf;
         }
     }
     ~~~

     **为什么叫饿汉式**

     有时候可能会调用这个类上的静态属性, 这会使类信息加载, 导致GirlFriend对象的创建, 但是这是没有必要的, 因为我可能没有使用到这个对象, 所以叫做饿汉式

   - 懒汉式

     ~~~java
     //1. 私有化构造器
     class Cat {
         private String name;
         //!!!
         private Cat(String name){
             this.name = name;
         }
     }
     
     //2. 定义一个 static 属性对象
     class Cat {
         private String name;
         
         //!!!
         private static Cat cat;
         
         private Cat(String name){
             this.name = name;
         }
     }
     
     //3. 定义一个 static 方法, 可以返回一个Cat对象
     class Cat {
         private String name;
         
         private static Cat cat;
         
         private Cat(String name){
             this.name = name;
         }
         
         //!!!
         public static Cat getInstance(){
             if(cat == null){//如果cat还没有创建
                 cat = new Cat("小白");
             }
             return cat
         }
     }
     ~~~

   饿汉式VS懒汉式

   1. 二者最主要的区别在于创建对象的时机不同, 饿汉式是在类加载就创建了对象实例, 而懒汉式在使用时才创建
   2. 饿汉式不存在线程安全问题, 懒汉式存在线程安全问题
   3. 饿汉式存在浪费资源的可能, 因为如果程序员一个对象实例都没有使用, 那么饿汉式创建的对象就浪费了, 懒汉式是使用时才创建, 就不存在这个问题
   4. 在我们javaSE标准类中, java.lang.Runtime就是经典的单例模式

