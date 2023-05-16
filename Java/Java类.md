[toc]

# 类

## 构造器 （constructor）

**构造方法幼教构造器， 是类的一种特殊的方法， 他的主要作用是完成新对象的初始化。**

**【】表示可写可不写**

### 语法

【修饰符】方法名(形参列表) {

​	方法体

}

### 说明

1. 构造器的修饰符可以默认  => public protected private
2. 构造器没有返回值
3. 方法名和类名称必须一样
4. 参数列表和 成员方法 一样的规则
5. 构造器的调用系统完成

### 使用细节

1. 一个类可以定义多个不同的构造器， 即构造器重载

   比如：我们可以再给Person类定义一个构造器， 用来创建对象的时候， 只指定人名， 不需要指定年龄

2. 构造器名要和类名相同

3. 构造器没有返回值

4. 构造器是完成对象的初始化，并不是创建对象

5. 在创建对象时，系统自动的调用该类的构造方法

6. 如果程序员没有定义构造方法， 系统会自动给类生成一个默认无参构造方法(也叫默认构造方法)， 如如Person(){}，使用javap指令反编译试试

7. 一旦定义了自己的构造器， 默认的构造器就覆盖了， 就不能再使用默认无参构造器， 除非显示的定义一下， 即： Person(){}

---

## 对象创建流程

~~~ java
class Person{
    String name;
    int age = 90;
    Person(String n, int a){
        name = n;
        age = a
    }
}
Person p = new Person("小明", 18)
~~~

### 流程分析

1. 加载Person类信息（Person.class），只会加载一次（在方法区）
2. 在堆中分配空间（地址）
3. 完成对象默认初始化  => name = null      age = 0
4. 显示初始化  =>  name =  null      age = 90
5. 构造器的初始化  =>  name = 小明      age = 18
6. 对象在堆中的地址，返回给p（p是对象名 => 也可以叫对象的引用）

---

## 引出this

~~~java
class Person {
    String name;
    int age;
    public Person(String dName, int dAge){
        name = dName;
        age = dAge;
    }
}
~~~

上面的类初始化是没有问题的，只是在构造器中的  name = dName 很不优雅，这样会起很多没必要的名字，但是如果直接把形参换成name然后通过 name = name 的话，这个name又取不到Person上的属性， 这时候我们就可以使用this

~~~java
class Person {
    String name;
    int age;
    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }
}
~~~

这样写代码就比较优雅了

### 什么是this

java虚拟机会给每个对象分配this， 这个this代表当前对象。

简单来说， 哪个对象调用， this就指向哪个对象

**可以通过hashCode来返回该对象的内存地址转换成的整数**

### this的使用细节

1. this关键字可以用来访问本类的属性、方法、构造器
2. this用于区分当前类的属性和局部变量
3. 访问成员方法的语法： this.方法名（参数列表）
4. 访问构造器语法：this（参数列表） => **注意稚嫩恶搞在构造器中使用（即只能在构造器中访问另一个构造器， 且必须放在第一条语句， 这和继承有关系）**
5. this不能再类定义的外部使用， 只能在类定义的方法中使用

---

## 访问修饰符

#### 访问权限

Java提供四种访问控制修饰符号，用于控制方法和属性（成员变量）的访问权限

1. 公开级别：用 **public** 修饰，对外公开
2. 受保护级别：用 **protected** 修饰，对子类和同一个包中的类 公开
3. 默认级别：没有修饰符号，向同一个包的类公开，子类不可用
4. 私有级别：用 **private** 修饰，只有类本身可以访问，不对外公开

#### 使用访问修饰符的注意事项

1. 修饰符可以用来修饰类中的属性、成员方法以及类

2. **只有默认的和 public 才能修饰类！**并且遵循上述访问权限的特点

3. 子类（待补充）

4. 成员方法的访问规则和属性完全一样

   // com.hspedu.modifier: 需要很多文件来说明（A类、B类、Test类）

---

## 面向对象

属性如果不赋值, 有默认值, 与数组保持一致

### 封装（encapsulation）

**封装就是把抽象出的数据和对数据 [ 属性 ] 的操作 [ 方法 ] 封装在一起，数据被保护在内部，程序的其他部分只有通过被授权的操作 [ 方法 ] ，才能对数据进行操作**

#### 好处

1. 隐藏实现细节	

   可以写好方法，使用时直接调用就可以

2. 可以对数据进行验证，保存安全合理

#### 实现步骤

1. 将属性进行私有化 private	=>	不能再外部直接修改属性
2. 提供一个公共的（public）set方法，用于对属性判断并赋值

~~~java
public void xxx(类型 参数名){
    //加入数据验证的业务逻辑
    属性 = 参数名
}
~~~

3. 提供一个公共的（public）get方法，用于获取属性的值

~~~java
public 数据类型 getXxx(){//权限判断，Xxx某个属性
    return XX
}
~~~

### 继承（extends）

**在两个类的属性和方法很多都是相同的时候，我们可以不必每次都重新写过，这样的话代码的冗余度太高，可以通过继承来创建另一个类**

**继承可以解决代码复用，让我们的编程更加靠近人类思维，当多个类存在相同的属性（变量）和方法时，可以从这些类中抽象出父类，在父类中定义这些相同的属性和方法，所有的子类不需要重新定义这些属性和方法，只需要通过 extends 来声明继承父类即可**

#### 基本语法

~~~java
class 子类 extends 父类 {

}
~~~

1. 子类就会自动拥有父类定义的属性和方法
2. 父类又叫超类、基类
3. 子类又叫派生类

#### 好处

1. 代码的复用性提高了
2. 代码的扩展性和维护性提高了

#### 细节

1. 子类继承了所有的属性和方法，但是私有属性不能再子类直接访问，要通过公共的方法区访问

2. 子类必须调用父类的构造器，完成父类的初始化

   **先调用父类的构造器，然后在调用子类的构造器**

3. 当创建子类对象时，不管使用子类的哪个构造器，默认情况下总会去调用父类的无参构造器，如果父类没有提供无参构造器，则必须在子类的构造器中使用 super 去指定使用父类的哪个构造器完成对父类的初始化工作，否则，编译不会通过

4. 如果希望指定去调用父类的某个构造器，则显式的调用一下：super(参数列表)

5. super() 在使用时，必须放在构造器第一行

   **super() 只能在构造器中使用, 但是super可以在非构造器中使用**

   **super() 是访问父类的构造器**

6. super() 和 this() 都只能放在构造器第一行， 因此这两个方法不能共存在一个构造器

7. java所有类都是Object类的子类（派生类），Object是所有类的父类（基类）

8. **父类构造器的调用不限于直接父类！将一直往上追溯直到Object类（顶级父类）**

9. 子类最多只能继承一个父类(指直接继承)，即java中是单继承机制。

   思考：如何让A类继承B类和C类

   **可以让B类先去继承C类，然后再用A类继承B类， 就实现了A类同时继承B类和C类**

10. 不能滥用继承，子类和父类之间必须满足 is-a 的逻辑关系

    is-a: Person is a Music ? 

    ​		Person 和 Music 并没有直接关系 所以 如果想要 Music extends Person 是不合理的

    ​		Animal

    ​		Cat extends Animal 	Cat 属于 Animal 所以是合理的

#### 本质 

##### new 一个继承过的类时 内存中发生了什么， 内存布局

 	1. 首先加载 Object 类信息（在方法区加载），因为 Object 是顶级父类，最先加载，然后从上到下一级一级加载，形成类的继承关系
 	2. 只会在堆中分配一块空间, 从上到下 一级一级初始化属性到这一块空间内，重复属性不覆盖，因为它们属于不同的类，而是创建不同空间存起来

##### 当我们通过new出来的子类去访问属性时 

	1. 首先看子类是否有改属性
	2. 如果子类有这个属性，并且可以访问，则返回信息
	3. 如果子类有这个属性，但是不能访问，就会报错
	4. 如果子类没有这个属性，就看父类有没有这个属性，如果父类有该属性并且可以访问，就返回信息
	5. 如果父类没有就按照(4)的规则，继续找上级父类，直到Object...  
	6. 如果都没有就会报错

![](E:\Study\Java\Java\images\Java继承-new子类时内存分布.png)

#### super关键字

**super 代表父类的引用，用于访问父类的属性、方法、构造器**

##### 基本语法

1. 访问父类的属性，但不能访问父类的 private 属性

   super.属性名、

2. 访问父类的方法，不能访问父类的 private 方法

   super.方法名（参数列表）

3. 访问父类的构造器（这点前面用过）

   super(参数列表)；只能放在构造器的第一句，且只能出现一句

##### super给编程带来的便利/细节

1. 调用父类的构造器的好处（分工明确，父类属性由父类初始化，子类的属性由子类初始化）
2. 当子类中有和父类中的成员（属性和方法）重名时，问了访问父类的成员，必须通过 super。如果没有重名，使用 super、this、直接访问是一样的效果！
3. super 的访问不限于直接父类，如果爷爷类和本类中有同名的成员，也可以使用 super 去访问爷爷类的成员；如果多个基类（上级类）中都有同名的成员，使用 super 访问遵循就近原则。

##### super 和 this 的比较

1. 访问属性
   - this：访问本类中的属性，如果本类没有此属性则从父类中继续查找
   - super：从父类开始查找属性
2. 调用方法
   - this：访问本类中的方法，如果本类没有此方法则从父类继续查找
   - super：从父类开始查找方法
3. 调用构造器
   - this：调用本类构造器，必须放在构造器的首行
   - super：调用父类构造器，必须放在子类构造器的首行
4. 特殊
   - this：表示当前对象
   - super：子类中访问父类对象

#### 方法重写/覆盖(override)

##### 概述

方法覆盖就是子类有一个方法,和弗雷的某个方法的名称、返回类型、参数一样, 那么我们就说子类的这个方法覆盖了父类的方法

##### 细节

1. 子类的方法的参数,方法名称,要和父类方法的参数,方法名称完全一样
2. 子类方法的返回类型和父类方法返回类型一样, 或者是父类返回类型的子类

​	比如: 父类的返回类型是Object, 子类方法返回类型是String

~~~java
//父类
class Parent {
    public Object getInfo(){}
}

//子类
class Children extends Parent {
    public String getInfo(){}
}

//这样会触发重写 因为String是Object的子类
//如果children里的getInfo的返回类型也是Object也会触发重写
//如果parent的getInfo返回类型是String, 而children的getInfo的返回类型是是Object会编译错误
~~~



3. 子类方法不能缩小父类方法的访问权限

~~~java
//父类
class Parent {
    public void eat(){}
}

//子类
class Children extends Parent {
    protected void eat(){} // 报错, 缩小了父类方法的访问权限
}
~~~

##### 重载和重写

1. 发生范围

   - 重载: 本类
   - 重写: 父子类

2. 方法名

- 重载: 必须一样
- 重写: 必须一样

3. 形参列表

- 重载: 类型,个数或者顺序至少有一个不同
- 重写: 相同

4. 返回类型

- 重载: 无要求
- 重写: 子类重写的方法返回的类型和父类方法返回的类型一样或者是其子类

5. 修饰符

- 重载: 无要求
- 重写: 子类方法不能缩小父类方法的访问范围

### 多态

#### 概述

我的 => 调用一个类的方法时传的参数类型不一样每次都要对方法进行一次重载, 这就导致了代码的冗余,这样的代码复用性不高,而且不利于代码维护

基本介绍: 方法或对象具有多种形态,是面向对象的第三大特征,多态是简历在封装和继承基础之上的

#### 具体体现

1. 方法的多态

   重载和重写就体现多态

2. 对象的多态

   1. 一个对象的编译类型和运行类型可以不一致
   2. 编译类型在定义对象时就确定了,不能改变
   3. 运行类型是可以变化的
   4. 编译类型看定义时 = 号的左边, 运行类型看= 号的右边

   **父类的对象引用可以指向子类的对象, 并且在运行时以运行类型为主, 如下**

   ```java
   Animal animal = new Dog(); // animal 编译类型是Animal, 运行类型是Dog
   animal = new Cat(); // animal运行类型变成了Cat, 编译类型仍然是Aniaml
   ```


#### 注意事项和细节

**多态的前提是：两个对象（类）存在继承关系**

##### 多态的向上转型

1. 本质：父类的引用指向了子类的对象；

2. 语法：父类类型 引用名 = new 子类类型（）；

3. 特点：编译类型看左边，运行类型看右边。

   1. 可以调用父类中的所有成员（需遵循访问权限）

   2. 不能调用子类中特有成员； 

      **因为在编译阶段，能调用哪些成员，是由编译类型来决定的**

   3. 最终运行效果看子类的具体实现

      **即调用方法时，按照从子类开始查找方法，然后按照就近原则调用**

~~~java
//父类
class Parent {
    public void eat(){}
    public void setInfo(){}
}

//子类 
class Children extends Parent {
    public void eat(){}
    public void getInfo(){}
}

Parent parent = new Children();

parent.setInfo(); // 可以
parent.eat(); // 可以
parent.getInfo(); // 不可以

// 多态的向上转型可以调用父类中的所有成员（遵循访问权限），但是不能调用子类的特有成员
// 因为在编译阶段，能调用哪些成员是由编译类型来决定的
// 最终运行效果看子类的具体实现，即调用方法时，按照从子类开始查找方法，然后按照就近原则调用
~~~

##### 多态的向下转型

###### 语法：子类类型 引用名 = （子类类型）父类引用

1. 只能强转父类的引用，不能强转父类的对象
2. 要求父类的引用必须指向是当前目标类型的对象
3. 当向下转型后，就可以调用子类类型中所有的成员

~~~java
//父类
class Parent {
    public void eat(){}
    public void setInfo(){}
}

//子类 
class Children extends Parent {
    public void eat(){}
    public void getInfo(){}
}

Parent parent = new Children();
//多态的向下转型
Children children = (Children) parent;
//!只能把指向Children的parent强转为Children
children.getInfo() //可以了

~~~

##### 属性没有重写，属性的值看编译类型

~~~java
//父类
class Parent {
   	int count = 10
}

//子类 
class Children extends Parent {
    int count = 20
}

Parent parent = new Children();
parent.count = ? //10

~~~

#### instansOf 比较操作符

**用于判断对象的运行类型是否为XX类型或XX类型的子类型**

~~~java
class Parent {
    
}

class Children extends Parent {
    
}

Children children = new Children();
System.ou.println(children instanceof Children); // true
System.out.println(chidlren instanceof Parent); // true

Parent parent = new Children();
System.out.println(parent instanceof Parent); // true
System.out.println(parent instanceof Children); // true

Object obj = new Object();
System.out.println(obj instanceof Parent); // false
String str = "hello"
System.out.println(str instanceof Parent); // false
System.out.println(str instanceof Object); // true
~~~

#### java的动态绑定机制（非常重要）

1. 当调用对象方法的时候，**该方法会和该对象的内存地址/运行类型绑定**
2. 当调用对象属性时，没有动态绑定机制，哪里声明，哪里使用

~~~java
class A {
    public int i = 10;
    public int sum(){
        return getI() + 10;
    }
    public int sum1(){
        return i + 10;
    }
    public int sum2(){
        return getI() + 20
    }
    public int sum3(){
        return i + 20
    }
    public int getI(){
        return i;
    }
}

class B extends A {
    public int i = 20;
    public int sum(){
        return i + 20;
    }
    public int getI(){
        return i;
    }
    public int sum1(){
        return i + 10;
    }
}

A a = new B(); // 向上转型
System.out.println(a.sum()); // 40
System.out.println(a.sum1()); // 30

System.out.println(a.sum2()); // 20 + 20 = 30
// 调用 sum2 时，与运行类型绑定，a 的运行类型是 B，所以调用的 getI 是B类里的 getI
System.out.println(a.sum3()); // 10 + 20 = 30
// 调用 sum3 时，因为属性没有动态绑定机制，所以在当前类中找到 i = 10， 所以结果是10 + 20 = 30
~~~

#### 多态的应用

##### 多态数组

数组的定义类型为父类类型， 里面保存的实际元素类型为子类类型

~~~java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
    public String say(){
        return "Hello World my name is " + this.name + "age=" + this.age ;
    }

}

public class Student extends Person {
    private double score;

    public Student(String name, int age, double score) {
        super(name, age);
        this.score = score;
    }

    public double getScore() {
        return score;
    }

    public void setScore(double score) {
        this.score = score;
    }

    @Override
    public String say() {
        return super.say() + "score" + this.score;
    }

    public void study(){
        System.out.println("学生" + getName() + "正在学java");
    }
}


public class Teacher extends Person {
    double salary;

    public Teacher(String name, int age, double salary) {
        super(name, age);
        this.salary = salary;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    @Override
    public String say() {
        return super.say() + "salary=" + this.salary;
    }

    public void teach(){
        System.out.println("老师" + getName() + "正在授课");
    }
}


public class PolyArray {
    public static void main(String[] args) {
        //现有一个继承结构如下：要求创建一个Person对象
        //Person person = new Person("余杰",18);
        //Student stu1 = new Student("xx",20, 91.1);
        //Student stu2 = new Student("xxx", 19, 59.9);
        //Teacher teacher1 = new Teacher("猪毛", 55, 2010);
        //Teacher teacher2 = new Teacher("雷哥", 66, 3210);

        //Person[] persons = {person, stu1, stu2, teacher1, teacher2};
		
        Person[] persons = new Person[5];
        persons[0] = new Person("余杰",18);
        persons[1] = new Student("xx",20, 91.1);
        persons[2] = new Student("xxx", 19, 59.9);
        persons[3] = new Teacher("猪毛", 55, 2010);
        persons[4] = new Teacher("雷哥", 66, 3210);
        
        for(int i = 0; i < persons.length; i++){
            if(persons[i] instanceof Student){
                Student stu3 = (Student) persons[i];
                stu3.study();
            }else if(persons[i] instanceof Teacher){
                Teacher teacher3 = (Teacher) persons[i];
                teacher3.teach();
            }
            System.out.println(persons[i].say());
        }
    }
}
~~~

##### 多态参数

方法定义的形参类型为父类类型，实参类型允许为子类类型

### Object类详解

#### equals和==对比

- ==

1. 既可以判断基本类型，又可以判断引用类型

2. 如果判断基本类型，判断的是值是否相等

3. 如果判断引用类型，判断的是地址是否相等，即判定是不是同一个对象

   ~~~java
   class B {}
   class A {}
   
   A a = new A();
   A b = a;
   A c = b;
   System.out.println(a == c); //true
   System.out.println(b == c); //true
   B newB = a;
   System.out.println(newB == c); //true 
   // 虽然编译类型不一样了，但是他们保存的地址指向的还是同一个对象
   
   int num1 = 10;
   double num2 = 10.0;
   System.out.println(num1 == num2); // true 基本类型判 => 断值相等
   ~~~

- equals
  1. 只能判断引用类型
  2. 默认判断的是地址是否相等，子类中往往重写该方法。用于判断内容是否相等，比如Integer，String

~~~java
//子类重写equals
class Person {
    String name;
    int age;
    String genner;
    public boolean equals(Object obj){
        if(this == obj){ //如果穿进来的对象和当前对象是同一个对象直接返回true
            return true;
        }
        //如果穿进来的对象 是 Person 类型或它的子类型
        if(obj instansof Person){
            //进行向下转型,这里的向下转型是为了拿到obj上的属性方法，因为这时候obj的编译类型还是Object
            Person p = (Person)obj;
            return this.name.equals(p.name) && this.age == p.age && this.genner == p.genner
        }
        return false
    }
}
~~~

#### hashCode方法

1. 提高具有哈希结构的容器的效率
2. 两个引用,如果指向的是同一个对象,则哈希值肯定是一样的
3. 两个引用,如果指向的是不同对象,则哈希值是不一样的
4. 哈希值主要根据地址来计算的,不能完全将哈希值等价与地址

~~~java
class AA {}

AA a = new AA();
AA b = new AA();
AA c = a

System.out.println(a.hashCode() != b.hashCode()); true
System.out.println(c.hashCode() == b.hashCode()); true
~~~

5. 在集合只不过,如果需要的话,会重写hashCode方法

#### toString方法

##### 基本介绍

- 默认返回: 全类名 + @ + 哈希值的十六进制

  Object源码: getClass().getName() + '@' + Integer.toHexString(hashCode())

  - 全类名: 包名+类名
  - @
  - Integer.toHexString(hashCode()): 将对象的hashCode值转成16进制的字符串

- 子类往往重写toString方法,用于返回对象的属性信息

- 重写toString方法,打印对象或拼接对象时,都会自动调用该对象的toString形式

- 当直接输出一个对象时,toString方法会被默认的调用

#### finalize方法

概述: 当垃圾回收器确定不存在对该对象的更多引用时,由对象的垃圾回收器调用此方法

1. 当对象被回收时,系统自动调用该对象的finalize方法.子类可以重写该方法,做一些释放资源的操作
2. **什么时候回收: **当某个对象没有任何引用,则jvm就任务这个对象是一个垃圾对象,就会使用垃圾回收机制来销毁对象,在销毁该对象前,会先调用finalize方法
3. 垃圾回收机制的调用,是由系统来决定,也可以通过System.gc()主动触发垃圾回收机制

~~~java
class Finalize_ {
    public static void main(String[] args){
        Car bm = new Car("宝马");
        bm = null;
        //这时new Car("宝马")这个对象并没有任何引用,所以jvm认为这是一个垃圾对象要进行回收,在回收之前,会先调用该对象的finalize方法,我们就可以在 finalize 方法中写自己的业务逻辑代码(比如释放资源,数据库连接,或者打开文件), 如果我们不重写该 finalize 方法,就会调用Object类的 finalize 方法
    }
}

class Car {
    public Car(String name){
        this.name = name
    }
    
}
~~~

### 断点调试

开发中查找错误时可用

#### 重点

在断点调试过程中, 是运行状态,是以对象的运行类型来执行的

~~~java
class A {};
class B extends A {};
A a = new A();
a.xx();
~~~

#### 快捷键

看关键字带Force代表强制

- f7: 跳入 => 跳入方法体内执行

  新版: f5, 看关键字: step Into

- f8: 跳过 => 逐行执行

  新版: f6,  看关键字: step Over

- shift + f8: 跳出 => 跳出方法

  新版: f7, 看关键字: step Out

- f9: resume,执行到下一个断点

  新版: f8, 看关键字: resume Program

## 类变量和类方法(静态变量和静态方法)

### 类变量(static变量,静态变量)

**jdk8以前一般认为存放在方法区的静态域中, jdk8之后一般认为在堆的class对象的末尾**

1. 静态变量是同一个类所有对象共享的
2. 静态变量在类加载的时候就生成了

#### 概述

 类变量也叫静态变量/静态属性,是该类的所有对象共享的变量,任何一个该类的对象去访问它时,取到的都是相同的值,同样任何一个该类的对象去修改它时,修改的也是同一个变量.

#### 语法

##### 如何创建

- 访问修饰符 static 数据类型 变量名; [推荐使用]

- static 访问修饰符 数据类型 变量名;

##### 如何访问

静态变量的访问修饰符的访问权限和范围和普通属性是一样的

- 类名.类变量名;[推荐使用]
- 对象名.类变量名;

#### 使用细节

1. 什么时候需要用类变量

   当我们需要让某个类的所有对象都共享一个变量时,就可以考虑使用类变量

2. 类变量和实例变量(普通变量)区别

   类变量是该类的所有对象共享的,而实例变量是每个对象独享的

3. 加上static成为类变量或静态变量,否则成为实例变量/普通变量/非静态变量

4. 类变量可以通过类名.类变量名 或者 对象名.类变量名来访问, 但java设计者推荐我们使用类名.类变量名方式访问   [但是前提是满足访问修饰符的访问权限和范围]

5. 实例变量不能通过类名.类变量名方式访问

6. 类变量是在类记在时就初始化了, 也就是说,即使你没有创建对象, 只要类加载了,就可以使用类变量了

7. 类变量的生命周期是随类的加载开始,随着类的消亡而销毁

### 类方法(静态方法)

静态方法才可以去访问静态属性/变量

#### 语法

- 访问修饰符 static 数据返回类型 方法名(){} [推荐使用]
- static 访问修饰符 数据返回类型 方法名(){}

#### 使用方式

前提是满足访问修饰符的访问权限和范围

- 类名.类方法名
- 对象名.类方法名 

#### 使用场景

当方法中不涉及到任何和对象相关的成员,则可以将方法设计成静态方法,提高开发效率

#### 注意事项

1. 类方法和普通方法都是随着类的加载而加载,将结构信息存储在方法区, 
   - 类方法中无this的参数,
   - 普通方法中隐含着this的参数
2. 类方法可以通过类名调用,也可以通过对象名调用
3. 普通方法和对象有关,需要通过对象名调用,比如对象名.方法名(参数), 不能通过类名调用
4. 类方法中不允许使用和对象有关的关键字,比如this和super, 普通方法(成员方法)可以
5. 类方法(静态方法)中,只能访问静态变量或静态方法
6. 普通成员方法,既可以访问普通变量(方法), 也可以访问静态变量(方法)

小结: **静态方法只能访问静态的成员,非静态的方法,可以访问静态成员和非静态成员(必须遵循访问权限)**

### 理解main方法的语法

1. java 虚拟机需要调用类的 main() 方法, 所以该方法的访问权限必须是 public

2. java 虚拟机在执行 main() 方法时不必创建对象, 所以该方法必须是 static

3. 该方法接收 String 类型的数组参数, 该数组中保存执行 java 命令时传递给所运行的类的参数

4. java 执行的城西 参数1 参数2 参数3 ..

   ~~~java
   class Person {
       public static void main(Stirng[] args){
           for(int i = 0; i < args.length; i++){
               System.out.println("第" + i + "个参数是" + args[i])
           }
       }
   }
   ~~~

   这个java文件首先javac Person.java

   然后 java Person 参数1 参数2 参数3 ... 这里传参数

#### 特别提示

1. 在 main 方法中,我们可以看直接调用 main 方法所在类的静态方法或静态属性
2. 但是, 不能直接访问该类中的非静态成员, 必须创建该类的一个实例对象后, 才能通过这个对象去访问类中的非静态成员

#### 使用案例

main 方法传递参数

## 代码块

### 基本介绍

代码化块又称为初始化块, 属于类中的成员[即 是类的一部分], 类似于方法, 将逻辑语句封装在方法体中, 通过{}包围起来.

但是和方法不同, 没有方法名, 没有返回, 没有参数, 只有方法体, 而且不能通过对象或类显示调用, 而是加载类时, 或创建对象时隐式调用

### 基本语法

~~~java
修饰符 {
	//代码
}
~~~

### 说明注意

1. 修饰符: 可选, 要写的话, 只能写 static
2. 代码块分为两类, 使用 static 修饰的叫静态代码块, 没有 static 修饰的叫做普通代码块/非静态代码块
3. 逻辑语句可以为任何逻辑语句(输入、输出、方法调用、循环、判断等)
4. 分号可以协商, 也可以省略

### 代码块的好处

1. 相当于另外一种形式的构造器(对构造器的补充机制, 可以做初始化的操作)
2. 如果多个构造器中都有重复语句, 可以抽取到初始化块中, 提高代码的重用性

~~~java
class Movie {
    private String name;
    private double price;
    private String director;
    
    public Movie(String name){
        System.out.println("电影屏幕打开...")
        System.out.println("广告开始...")
        System.out.println("电影正式开始...")
        this.name = name;
    }
    
    public Movie(String name, double price){
        System.out.println("电影屏幕打开...")
        System.out.println("广告开始...")
        System.out.println("电影正式开始...")
        this.name = name;
        this.price = price;
    }
    
    public Movie(String name, double price, String director){
        System.out.println("电影屏幕打开...")
        System.out.println("广告开始...")
        System.out.println("电影正式开始...")
        this.name = name;
        this.price = price;
        this.directior = director
    }
}
~~~

因为三个构造器都有相同的语句, 代码比较冗余, 可以将相同的语句放入一个代码块中

~~~java
class Movie {
    private String name;
    private double price;
    private String director;
    
    {
        System.out.println("电影屏幕打开...")
        System.out.println("广告开始...")
        System.out.println("电影正式开始...")
    }
    public Movie(String name){
        this.name = name;
    }
    
    public Movie(String name, double price){
        this.name = name;
        this.price = price;
    }
    
    public Movie(String name, double price, String director){
        this.name = name;
        this.price = price;
        this.directior = director
    }
}
~~~

3. 当我们不管调用哪个构造器创建对象, 都会先调用代码块的内容
4. 代码块调用的顺序优先于构造器...

### 代码块的使用细节

1. static 代码块也叫静态代码块, 作用就是对类进行初始化, 而且它随着类的加载而执行, 并且只会执行一次. 如果是普通代码块, 每创建一个对象就执行一次

   **类什么时候被加载**

   1. 创建对象实例时(new)
   2. 创建子类对象实例, 父类也会被加载, 而且父类先被加载
   3. 使用类的静态成员时(静态属性, 静态方法)

2. 普通的代码块, 在创建对象实例时, 会被隐式的调用, 被创建一次, 就会调用一次

   如果只是使用类的静态成员, 普通代码块并不会执行

3. 创建一个对象时, 在一个类的调用顺序是

   1. 调用静态代码块和静态属性初始化(注意: 静态代码块和静态属性初始化调用的优先级一样, 如果有多个静态代码块和多个静态变量初始化, 则按他们定义的顺序调用)

   ~~~java
   class A {
       private static int n1 = getN1();
       
       static {
           System.out.println("A 静态代码块01")
       }
       
       public static int getN1(){
           System.out.println("getN1被调用")
           return 100;
       }
   }
   
   A a = new A();
   //先输出getN1被调用
   //再输出A 静态代码块01
   ~~~

   

   2. 调用普通代码块和普通属性的初始化(注意: 普通代码块和普通属性初始化调用的优先级一样, 如果有多个普通代码块和多个普通属性初始化, 则按定义顺序调用)

   ~~~java
   class A {
       private int n1 = getN1();
       
       static {
           System.out.println("A 普通代码块01")
       }
       
       public int getN1(){
           System.out.println("getN1被调用")
           return 100;
       }
   }
   
   A a = new A();
   //先输出getN1被调用
   //再输出A 普通代码块01
   ~~~

   

   3. 调用构造方法

4. 构造器 的最前面其实隐含了 super() 和 调用普通代码块

   静态相关的代码块, 属性初始化, 在类加载时, 就执行完毕, 因此是优先于 构造器和普通代码块执行的

   ~~~java
   class A {
   	public A(){
   		//这里有隐藏的执行要求
           //1. super(); 先调用父类
           //2. 调用普通代码块
           System.out.println("ok")
   	}
   }
   
   class Parent {
       {
           System.out.println("Parent 的普通代码块被调用")
       }
       public Parent(){
           System.out.println("Parent 的构造器被调用")
       }
   }
   
   class Children extends Parent {
       {
           System.out.println("Children 的普通代码块被执行")
       }
       public Children(){
           //1. super(); 先调用父类
           //2. 调用本类的普通代码块
           System.out.println("Children 的构造器被执行")
       }
   }
   ~~~

   		1. 输出 => Parent 的普通代码块被执行
   		2. 输出 => Parent 的构造器被执行
   		3. 输出 => Children 的普通代码块被执行
   		4. 输出 => Children 的构造器被执行

5. 创建一个子类时, 他们的静态代码块, 静态属性初始化, 普通代码快, 普通通属性初始化, 构造方法的调用顺序

   1. 父类的静态代码块和静态属性(优先级一样, 按定义顺序执行)
   2. 子类的静态代码快和静态属性(优先级一样, 按定义顺序执行)
   3. 父类的普通代码块和普通属性初始化(优先级一样, 按定义顺序执行)
   4. 父类的构造方法
   5. 子类的普通代码块和普通属性初始化(优先级一样, 按定义顺序执行)
   6. 子类的构造方法

6. 静态代码块只能直接调用静态成员(静态属性和静态方法), 普通通代码块可以调用任意成员

## final关键字

final可以修饰类、属性、方法和局部变量

在某些情况下,程序员可能有一下需求吗就会使用到final:

1. 当不希望类被继承时, 可以用final修饰
2. 当不希望父类的某个方法被子类覆盖/重写( override )时, 可以用 final 关键字修饰
3. 当不希望类的某个属性的值被修改, 可以用 final 修饰
4. 当不希望某个局部变量被修改, 可以使用 final 修饰

### 使用细节

1. final 修饰的属性又叫常量, 一般用 XX_XX_XX 来命名

2. final 修饰的属性在定义时, 必须赋初始值, 并且以后不能再修改, 赋值可以在如下位置之一

   - 定义时: 如public final double TAX_RATE = 0.08;
   - 在构造器中
   - 在代码块中

3. 如果 final 修饰的属性是静态的, 则初始化的位置只能是

   - 定义式
   - 在静态代码快, 不能在构造器中赋值

4. final 类不能继承, 但是可以实例化对象

5. 如果类不是 final 类, 但是含有 final 方法, 则该方法虽然不能重写, 但是可以被继承

6. 一般来说, 如果一个类已经是 final 类了, 就没有必要再将方法修饰成 final 方法

7. final 不能修饰构造方法(即构造器)

8. final 和 static 往往搭配使用, 效率更高, 底层编译器做了优化处理

   **因为 final 和 static 搭配使用时调用这个属性不会导致类加载**

9. 包装类(Integer, Double, Float, Boolean等都是 final), String也是 final 类

## 抽象类

当父类的一些方法不能确定时, 可以是用 abstract 关键字来修饰该方法, 这个方法就是抽象方法, 用 abstract 来修饰类就是抽象类

**一般来说, 抽象类会被继承, 由其子类来实现抽象方法**

1. 用 abstract 关键字来修饰一个类时, 这个类就叫抽象类

   访问修饰符 abstract 类名 {}

2. 用 abstract 关键字来修饰一个方法时, 这个方法就是抽象方法

   访问修饰符 abstract 返回类型 方法名(参数列表); **没有方法体**

3. 抽象类的假值更多作用是在于设计, 是设计者设计好后, 让子类继承并实现抽象类

4. 抽象类, 是考官比较爱问的知识点, 在框架和设计模式使用较多

### 使用细节

1. 抽象类不能被实例化

2. 抽象类不一定要包含 abstract 方法. 也就是说, 抽象类可以没有 abstract 方法

3. 一旦类包含了 abstract 方法, 则这个类必须声明为 abstract

4. abstract 只能修饰类和方法, 不能修饰属性和其他的

5. 抽象类可以有任一成员(因为抽象类还是类)

   比如: 非抽象方法、构造器、静态属性等等

6. 抽象方法不能有主体, 即不能实现

7. 如果一个类继承了抽象类, 则它必须实现抽象类的所有抽象方法, 除非它自己也声明为 abstract 类

### 题目

~~~ java
abstract final class A {} // 编译不通过, 因为 final 不能继承
abstract public static void test2(); //编译不通过, 因为static关键字和方法重写无关,了简单来说就是这样方法不能重写
abstract private void test3(); // 编译不通过, private 方法不能重写
~~~

## 内部类

一个类的内部又完整的嵌套了另一个类接口.被嵌套的类成为内部类, 嵌套其他类的类成为外部类, 是类的第五大成员, 内部类最大的特点就是可以直接访问私有属性, 并且可以提现类和类之间的包含关系

五大成员: 

1. 属性
2. 方法
3. 构造器
4. 代码块
5. 内部类

### 基本语法

~~~java
class Outer { // 外部类
    class Inner { // 内部类
        
    }
}

class Other { // 外部其他类
    
}
~~~

### 四种内部类

#### 局部内部类

- 定义在外部类局部位置上 ( 比如方法内 ), 有两种
  - 局部内部类 ( 有类名 )
  
    特性:
  
    1. 可以直接访问外部类的所有成员, 包含私有的
  
    2. 不能添加访问修饰符, 因为它的低位就是一个局部变量. 局部变量是不能使用修饰符的. 但是可以使用 final 修饰, 因为局部变量也可以使用 final
  
    3. 作用域: 仅仅在定义它的方法或代码块中
  
    4. 局部内部类 --- 访问 ----> 外部类的成员 [ 访问方式: 直接访问 ]
  
    5. 外部类 --- 访问 ----> 局部内部类的方式
  
    6. 外部其他类 --- 不能访问 ---> 局部内部类( 因为局部内部类低位是一个局部变量 )
  
    7. 如果外部类和局部内部类的成员重名时, 默认遵循就近原则, 如果想访问外部类的成员, 则可以使用 ( 外部类名.this.成员 ) 去访问
  
       **此时外部类名.this  指向外部类的调用对象, 就是外部类的this指向**
  
  ~~~java
  class Outer {
      private int n = 100;
      private void m2(){};
      public void m1(){
          //1. 局部内部类是定义在外部类的局部位置, 通常在方法
          //3. 不能添加访问修饰符,但是可以使用 final 修饰
          //4. 作用域: 仅仅在定义它的方法或代码块中
          final class Inner { // 局部内部类(本质仍然是一个类)
              //2. 可以直接访问外部类的所有成员, 包含私有的
              public void f(){
                  //5. 局部内部类可以直接访问外部类的成员, 比如下面
                  System.out.println("n=" + n) // 可以访问到外部类的私有成员
              }
          }
          // 外部类能
      }
  }
  ~~~
  
  
  
  - **匿名内部类 ( 没有类名, 重点!!!!!!! )**
    1. 本质是类
    2. 内部类
    3. 该类没有名字 ( 其实有名字,但是是系统自动分配, 看不到的 )
    4. 同时还是一个对象
  
    ~~~java
    interface IA {
        cry();
    }
    
    //基于接口的匿名内部类
    //编译类型是 IA
    //运行类型是 外部类$1
    IA tiger = new IA() {
    	try(){
            System.out.println("xxx");
        }
    }
    //打印xxx
    tiger.try();
    
    class Father {
        name:String;
    	Father(String name){
        	this.name = name;
        }
        
    }
    //基于类的匿名内部类
    //编译类型 Father
    //运行类型 外部类$2
    //后面加上大括号就是匿名内部类
    //不加大括号就是正常new出来一个对象
    Father father = new Father("jack"){
        
    }
    ~~~
    
    
  
- 定义在外部类的成员位置上
  - 成员内部类 ( 没用static修饰 )
  - 静态内部类 ( 使用static修饰 )

