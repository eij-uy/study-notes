[toc]

# 实现枚举的两种方式

## 自定义类实现枚举

1. 不需要提供setXxx方法, 因为枚举对象值通常为只读
2. 对枚举对象/树形使用 final + static 共同修饰, 实现底层优化
3. 枚举对象名通常使用全部大写, 常亮的命名规范
4. 枚举对象根据需要,也可以有多个属性

### 特点

1. 构造器私有化
2. 本类内部创建一组对象
3. 对外暴露对象 ( 通过为对象添加 public final static 修饰符 )
4. 可以提供 get 方法, 但是不要提供 set

~~~java
package com.yujie.enums;

/**
 * @author 余杰
 * @version 1.0
 */
public class Enumeration02 {
  public static void main(String[] args) {
    System.out.println(Season02.AUYUMN);
    System.out.println(Season02.SPRING);
    System.out.println(Season02.SUMMER);
    System.out.println(Season02.WINTER);
  }
}

/**
 * 自定义类实现枚举
 * 1. 私有化构造器,防止直接 new
 * 2. 去掉相关set的方法, 防止属性被修改
 * 3. 在内部创建固定的值
 * 4. 可以在加入一个 final 修饰符
 */
class Season02 {
  private String name;
  private String desc; // 描述

  // 定义了四个对象
  public final static Season02 SPRING = new Season02("春天", "温暖");
  public final static Season02 WINTER = new Season02("冬天", "寒冷");
  public final static Season02 SUMMER = new Season02("夏天", "炎热");
  public final static Season02 AUYUMN = new Season02("秋天", "凉爽");

  private Season02(String name, String desc) {
    this.name = name;
    this.desc = desc;
  }
  public void setName(String name) {
    this.name = name;
  }
  public String getDesc() {
    return desc;
  }

  @Override
  public String toString() {
    return "Season02{" +
        "name='" + name + '\'' +
        ", desc='" + desc + '\'' +
        '}';
  }
}
~~~

## 使用 enum 关键字 实现枚举类

1. 当我们使用 enum 关键字开发一个枚举类时, 默认会继承 Enum 类, 而且是一个 final, 可以使用 javap 工具来掩饰
2. 传统的 public static final Season SPRING = new Season("春天","温暖") 简化成 SPRING("春天","温暖"), 这里必须知道, 他调用的是哪个构造器
3. 如果使用无参构造器创建枚举对象, 则实参列表和小括号都可以省略
4. 当有多个枚举对象时, 使用 都好( , ) 间隔, 最后一个分号结尾
5. 枚举对象必须放在枚举类的行首

~~~java
package com.yujie.enums;

/**
 * @author 余杰
 * @version 1.0
 */
public class Enumeration03 {
  public static void main(String[] args) {

  }
}

enum Season03 {
  //1.使用 enum 关键字 替代 class
  //2.public static final Season SPRING = new Season("春天","温暖") 直接使用 SPRING("春天","温暖") 解读 常量名(是参列表)
  //3.如果有多个常量(对象), 使用 , 间隔即可
  //4.如果使用 enum 来实现枚举, 需要将定义的常亮对象卸载最前面
  SPRING("春天","温暖"),WINTER("冬天","寒冷"),SUMMER("夏天","炎热"),AUTUMN("秋天","凉爽");
  private String name;
  private String desc;

  private Season03(String name, String desc) {
    this.name = name;
    this.desc = desc;
  }

  public String getName() {
    return name;
  }

  public String getDesc() {
    return desc;
  }
}
~~~

练习

~~~java
enum Gender {
    BOY,GIRL;
}
//这是正确的, 这里调用的是Gender的无参构造器

Gender boy = Gender.BOY; //OK
Gender boy2 = Gender.BOY; //OK
System.out.println("boy"); //因为Gender没有toString方法,所以这里本质就是调用Gender父类的 toString 方法
//就是 public String toString(){
// return name
//}

~~~

## Enum 类的方法

1. toString

   Enum 类已经重写过了, 返回的是当前对象名, 子类可以重写该方法, 用于返回对象的属性信息

2. name

   返回当前对象名( 常量名 ), 子类中不能重写

3. ordinal

   返回当前对象的位置( 索引 ), 默认从0开始

4. values

   返回当前枚举类中所有的常亮

5. valueOf

   将字符串转换成枚举对象, 要求字符串必须为已有的常量名, 否则报异常

6. compareTo

   比较两个枚举常亮, 比较的就是位置( 索引 )

   ~~~java
   Season.AUTUMN.compareTo(Season.SUMMER);
   //返回的是 Season.AUTUMN 的索引 减去 Season.SUMMER 的 索引
   ~~~

## enum 实现接口

1. 使用 enum 关键字后, 就不能再继承其他类了, 因为 enum 会隐式继承 Enum, 而 Java 是单继承机制

2. 枚举类和普通类一样, 可以实现接口, 如下形式

   enum 类名 implements 接口1, 接口2 {}

