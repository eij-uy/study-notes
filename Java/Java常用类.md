[toc]

# Java常用类

## 包装类 --- Wrapper

### 分类

1. 针对八中基本数据类型相应的引用类型 ---- 包装类
2. 有了类的特点, 就可以调用类中的方法

| 基本数据类型 | 包装类    |
| ------------ | --------- |
| boolean      | Boolean   |
| char         | Character |

| 基本数据类型 | 包装类    |
| ------------ | --------- |
| byte         | Byte      |
| short        | Short     |
| int          | Integer   |
| long         | Long      |
| float        | Float     |
| double       | Double    |

这六个的父类都是 Number

### 包装类和基本数据类型的转换

1. jdk5 前的手动装箱和拆箱方式, 装箱: 基本类型 -> 包装类型, 反之就是拆箱

2. jdk5 以后(包含jdk5) 的自动装箱和拆箱方式

3. 自动装箱底层调用的是 valueOf 方法, 比如 Integer.valueOf()

   **以 int 和 Integer 为例**

~~~java
//jdk5 之前 
// 手动装箱 int -> Integer
int n1 = 100;
Integer integer = new Integer(n1); //装箱
Integer integer1 = Integer.valueOf(n1); //同上

//手动拆箱 Integer -> int
int i1 = integer.intValue(); //拆箱

//jdk5 之后
// 自动装箱 int -> Integer
int n2 = 200;
Integer integer2 = n2; // 自动装箱, 底层调用的是 Integer.valueOf(n2)
//自动拆箱
int i2 = integer2; //自动拆箱, 底层调用的是 integer2.intValue()
~~~

### 练习

~~~java
Double d = 100d; //ok 自动装箱 Double.valueOf(100d);
Float f = 1.5f; //ok 自动装箱 Float.valueOf(1.5f);

Object obj1 = true ? new Integer(1) : new Double(2.0); //三元运算符[是一个整体] 提升了精度, 所以此时Obj1 = 1.0

Object obj2;
if(true){
    obj2 = new Integer(1);
}else {
    obj2 = new Double(2.0);
}
//此时 obj2 = 1, 因为 if-else 分别计算, 所以不会提升精度, 语句是独立的
~~~

### 包装类型和 String 类型相互转换

**以 Integer 和 String 转换为例**

~~~java
//Integer -> String
Integer i = 100; //自动装箱

//方式1
String str1 = i + ""; //得到一个以 i 为基本数值的 String 字符串
//方式2
String str2 = i.toStirng();
//方式3
String str3 = String.valueOf(i); 

//String -> Integer
String str = "12345";
//方式1
Integer i1 = Integer.parseInt(str); //使用了自动装箱
//方式2
Integer i2 = new Integer(str); //使用构造器也可以
//方式3
Integer i3 = Integer.valueOf(str); //
~~~

### 包装类的常用方法

**以 Integer 和 Character 为例**

- Integer
  - Integer.MIN_VALUE  返回最小值
  - Integer.MAX_VALUE 返回最大值
- Character
  - Character.isDigit("a") 判断是不是数字
  - Character.isLetter("a") 判断是不是字母
  - Character.isUpperCase("a") 判断是不是大写
  - Character.isLowerCase("a") 判断是不是小写
  - Character.isWhitespace("a") 判断是不是空格
  - Character.toUpperCase("a") 转成大写
  - Character.toLowerCase("A") 转成小写