[toc]

# Java 泛型

~~~java
// 1. 当我们 ArrayList<Dog> 表示存放到 ArrayList 集合中的元素是 Dog 类型
// 2. 如果编译器发现添加的类型，不满足要求，就会报错
// 3. 在便利的时候，可以直接取出 Dog 类型而不是 Object
ArrayList<Dog> arrayList = new ArrayList<Dog>();
arrayList.add(new Dog("x",10));
arrayList.add(new Dog("xx",100));
arrayList.add(new Dog("xxx",1000));
//这时如果不小心添加了一个别的类型， 就会报错
//arrayList.add(new Cat("xxxx",10000)) // 报错

for(Dog dog : arrayList){
    //这里就不用使用 Object 向下转型成 Dog 了
}
~~~

## 泛型的好处

1. 编译时，检查添加元素的类型，提高了安全性
2. 减少了类型转换的次数，提高效率

## 泛型的说明

1. 泛型又称参数化类型，是 jdk5.0 出现的新特性，解决数据类型的安全性问题
2. 在类声明或实例化时只要指定好需要的具体的类型即可
3. Java 泛型可以保证如果程序在编译时没有发出警告，运行时就不会产生 ClassCastException 异常。同时，代码更加简洁、健壮
4. 泛型的作用是：可以再类声明时通过一个标识表示类中某个属性的类型，或者某个方法的返回值的类型，或者是参数类型

## 泛型的声明

~~~java
interface 接口<T>{};
class 类 <K,V>{};
// 1. 其中，T、K、V不代表值，而是表示类型
// 2. 任意字母都可以。常用 T 表示，是 Tyoe 的缩写
~~~

## 泛型的实例化

要在类名后面指定类型参数的值 ( 类型 )。如：

~~~java
List<String> strList = new ArrayList<String>();
Iterator<Customer> iterator = customers.iterator;
~~~

## 泛型使用细节

1. 泛型只能是引用类型

   ~~~java
   interface List<T>{};
   public class HashSet<E>{};
   // 这个里的 T,E 都只能是引用类型
   // List<Integer> list = new ArrayList<Integer>(); //这个可以，因为 Integer 是引用类型
   // List<int> list1 = new ArrayList<int>(); //这个不可以，因为 int 是原始(基本数据)类型
   ~~~

2. 在给泛型指定具体类型后，可以传入该类型或者其子类类型

   ~~~java
   class Parent {};
   class Children extends Parent {};
   class Other<T> {
       T e;
       public C(T e){
           this.e = e;
       }
   }
   
   // 因为 T 指定了 Parent 类型， 所以可以传入 Parent 类型
   Other<Parent> list = new Other<Parent>(new Parent());
   
   // 在给泛型指定具体类型后，可以传入该类型或者其子类类型
   Other<Parent> list = new Other<Parent>(new Children());
   
   ~~~

3. 泛型使用形式

   ~~~java
   // 1. 传统形式
   ArrayList<Integer> list1 = new ArrayList<Integer>();
   List<Integer> list2 = new ArrayList<Integer>();
   // 2. 简写，实际开发中一版都这样写
   ArrayList<Integer> list3 = new ArrayList<>();
   List<Integer> list4 = new ArrayList<>();
   ~~~

4. 泛型默认是 Object

   ~~~java
   ArrayList arrayList = new ArrayList();
   //这样写等价于 ArrayList<Object> arrayList = new ArrayList<>();
   ~~~

## 自定义泛型

### 自定义泛型类

#### 基本语法

~~~java
class 类名<T,E...> {
    成员...
}
~~~

#### 注意细节

1. 普通成员可以使用泛型 ( 属性、方法 )
2. 使用泛型的数组，不能初始化【因为数组在 new 的时候不能确定 T 的类型，就无法在内存开辟空间】
3. 静态方法/静态属性 中不能使用类的泛型【因为静态是和类相关的，在类加载时，对象还没有创建，而泛型是在类创建时指定的，所以，如果静态方法和静态属性使用了泛型，JVM 就无法完成初始化】
4. 泛型类的类型，是在创建对象时确定的 ( 因为创建对象时，需要指定确定类型 )
5. 如果在创建对象时，没有指定类型，默认为 Object 

#### 应用实例

~~~java
// T = Double E = String R = Integer
Tiger<Double, String, Integer> tiger = new Tiger<>();
g.setT(10.9); // OK 因为 T 是 Double 类型，而 10.9 也是 Double 类型
g.setT("yy"); // 错误 因为 yy 不是 Double 类型
Tiger tiger1 = new Tiger(); // OK 没有指定泛型，所以 T,E,R 都是默认的 Object 类型
tiger1.setT("yy"); // OK 因为 String 是 Object 的子类



class Tiger<T,E,R> {
    private T t;
    private E e;
    private R r;
    public Tiger(){};
    public T getT(){
        return this.t;
    }
    public void setT(T t){
        this.t = t;
    }
    public E getE(){
        return this.e;
    }
    public void setE(E e){
        this.e = e;
    }
    public R getR(){
        return this.r;
    }
    public void setT(R r){
        this.r = r;
    }
}
~~~

### 自定义泛型接口

#### 基本语法

~~~java
interface 接口名<T,E...> {
    //...
}
~~~

#### 注意细节

1. 接口中，静态成员也不能使用泛型 ( 这个和泛型类规定一样 )
2. 泛型接口的类型，在继承接口或者实现接口时确定
3. 没有指定类型，默认为 Object

#### 应用实例

~~~java
class IUsb<T, E> {
    int n = 10;
    // U name = "hsp"; // 不能这样使用，静态成员不能使用泛型
    // 接口中属性是 public static final 
    // 接口中方法是 public abstract
    E get(T t);
    void hi(E e);
    void run(E e1, E e2, T t1, T t2);
    default E method(T t){
        return null;
    }
}

// 在继承接口时指定泛型接口的类型
interface IA extends IUsb<String, Double> {
    
}

class AA implements IA {
    // 这里实现这个接口的方法时，会自动使用 String 和 Double 替换 T，E
    // 因为在 IA 继承 IUsb 时，指定了 T = String E = Double
}
class BB implements IUsb(Integer, Float){
    // 同上
}
class CC implements IUsb {
    // 这样不指定类型 泛型默认为 Object T = Object E = Object
}
~~~

### 自定义泛型方法

#### 基本语法

~~~java
修饰符<T,E...> 返回类型 方法名(参数列表) {
	//...    
}
~~~

#### 注意细节

1. 泛型方法，可以定义在普通类中，也可以定义在泛型类中
2. 当泛型方法被调用，类型会确定
3. public void eat (E e) {}, 修饰符后没有<T,E...> eat 方法就不是泛型方法，而是使用了泛型
4. 泛型方法可以使用类声明的泛型，也可以使用自己声明的泛型

~~~java
class Car { //普通类
    // 普通方法
    public void run(){}; 
    // 泛型方法
    // 1. <T,E> 就是泛型
    // 2. 是提供给 fly 使用的
    public <T,E> void fly(T t, E e){}; 
}

Car car = new Car();
// 当我们调用方法时，传入的参数，编译器就会确定对应的类型
// 这里的 T 就是 String， 而 E 因为泛型不能是原始类型，所以这里会自动装箱成 Integer
car.fly("宝马", 100); 

class Fish<T,E> { // 泛型类
    // 普通方法
    public void run(){};
    // 泛型方法
    // 1. <U,M> 就是泛型，最好和类的 <T,E> 区分开
    public <U,M> void eat(U u, M m){};
    // 1. 下面 hi 方法不是泛型方法
    // 2. 是 hi 方法使用了类声明的泛型
    public void hi(T t){}; 
    // 泛型方法，可以使用类声明的泛型，也可以使用自己声明的泛型
    public<K> void hello(R r, K k){}
}
~~~

#### 应用实例

~~~java
class Apple<T,R,M> {
    public void fly(E e) { //泛型方法
        // 这里如果只写 getClass 输出的是包名加上运行类型，如果加上 getSimpleName 就会只显示出运行类型
        System.out.println(e.getClass().getSimpleName());
    }
    public void eat(U u){}; // 错误，因为 U 没有声明
    public void run(M m){}; // OK
}

class Dog {};
// T -> String R -> Integer M -> Double
Apple<String, Integer, Double> apple = new Apple<>();
apple.fly(10); // 因为泛型不能是原始类型，所以 10 会被自动装箱成 Integer， 所以这里输出 Integer
apple.fly(new Dog()); // 这里输出 Dog
~~~

## 泛型的继承和通配符

1. 泛型不具备继承性
2. <?>：支持任意泛型类型
3. <? extends A>：支持 A 类以及 A 类的子类，规定了泛型的上限
4. <? super A>：支持 A 类以及 A 类的父类，不限于父类，规定了泛型的下限

### 应用实例

~~~java
public class Test {
    public static void main(Stringp[] args) {
        // 泛型没有继承性
		List<Object> list = new ArrayList<String>();
        
        //举例说明下面三个方法的使用
        List<Object> list1 = new ArrayList<>();
        List<String> list2 = new ArrayList<>();
        List<AA> list3 = new ArrayList<>();
        List<BB> list4 = new ArrayList<>();
        List<CC> list5 = new ArrayList<>();
        
        // 如果是 List<?> c, 可以接受任意的泛型类型
        printCollection1(list1);
        printCollection1(list2);
        printCollection1(list3);
        printCollection1(list4);
        printCollection1(list5);
        
        printCollection2(list1); // 错，因为 Object 不是 AA 的子类
        printCollection2(list2); // 错，因为 String 不是 AA 的子类
        printCollection2(list3); // 对，因为这里就是 AA 类型
        printCollection2(list4); // 对，因为这里的类型是 BB，而 BB 是 AA 的子类
        printCollection2(list5); // 对，因为这里的类型是 CC，而 CC 也是 AA 的子类
        
        printCollection3(list1); // 对，因为 Object 是 AA 的父类
        printCollection3(list2); // 错，因为 String 不是 AA 的父类
        printCollection3(list3); // 对，因为这里的类型就是 AA
        printCollection3(list4); // 错，因为 BB 不是 AA 的父类
        printCollection3(list5); // 错，因为 CC 不是 AA 的父类
        
    }
    // 表示任意的泛型类型都可以接受
    public static void printCollection1(List<?> c){
        for(Object obj : c){ // 通配符，取出时，就是 Object
            System.out.println(obj);
        }
    }
    //<? extends AA> 表示上限，可以接受 AA 或者 AA 子类
    public static void printCollection2(List<? extends AA> c){
        for(Object obj : c){
            System.out.println(obj);
        }
    }
    // <? super AA> 表示下限，可以接受 AA 类以及 AA 类的父类，不限于直接父类
    public static void printCollection3(List<? super AA> c){
        for(Object obj : c){
            System.out.println(obj);
        }
    }
}

class AA {};
class BB extends AA {};
class CC extends BB {];
~~~

## 练习

~~~java
package com.yujie.homework;
import org.junit.jupiter.api.Test;

import java.util.*;

public class GenericityTest {
  public static void main(String[] args) {

  }
  @Test
  public void TestDAO() {
    DAO<User> userDAO = new DAO<>();
    userDAO.save("001", new User(1,10,"jack"));
    userDAO.save("002", new User(2,18,"king"));
    userDAO.save("003", new User(3,38,"smith"));

    List<User> list = userDAO.list();
    System.out.println(list);

    userDAO.update("003", new User(3,58,"milan"));
    List<User> list1 = userDAO.list();
    System.out.println(list1);

    userDAO.delete("001");

    List<User> list2 = userDAO.list();
    System.out.println(list2);

    System.out.println(userDAO.get("003"));
  }
}

class DAO<T> {
  Map<String, T> map = new HashMap<>();
  public void save(String id, T entry){
    map.put(id, entry);
  }
  public T get(String id){
    return map.get(id);
  }
  public void update(String id, T entity){
    map.put(id, entity);
  }
  public List<T> list() {
    List<T> ts = new ArrayList<>();
    Set<String> strings = map.keySet();
    for(String str : strings){
      ts.add(get(str));
    }
    return ts;
  }
  public void delete(String id){
    map.remove(id);
  }
}

class User {
  private int id;
  private int age;
  private String name;

  public User(int id, int age, String name) {
    this.id = id;
    this.age = age;
    this.name = name;
  }

  @Override
  public String toString() {
    return "User{" +
        "id=" + id +
        ", age=" + age +
        ", name='" + name + '\'' +
        '}';
  }
}

~~~

