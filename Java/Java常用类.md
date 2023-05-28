[toc]

# Java常用类

## 包装类 --- Wrapper

**判断相等的时候，只要有基本数据类型，那么判断的就是值是否相同**

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
//Integer.valueOf() 源码中判断在-128 ~ 127 之间(包括两者)的数会直接返回值，不在这个范围内才会去 new 一个对象
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

## String 类

**字符串是不可变的，一个字符串对象一旦被分配，其内容是不可变的。详情见练习第三题**

### String 类的理解和创建对象

1. String 对象用于保存字符串，也就是一组字符序列

2. 字符串常亮对象是用双引号括起来的字符序列。例如： "你好"、"12.97"、"boy"等

3. 字符串的字符使用 Unicode 字符编码，一个字符( 不区分字母还是汉字 )占两个字节

4. String 类较常用构造方法

   - String str1 = new String();
   - String str2 = new String(String original);
   - String str3 = new String(char[] a);
   - String str4 = new String(char[] a, int startIndex, int count)

5. String 类实现了 Serializable 接口，说明 String 可以串行化【可以网络传输, 也可以保存到文件】

6. String 类实现了 Comparable 接口，说明 String 对象可以比较

7. String 是一个 final 类，不可以被其他类继承

8. String 有属性 private final char value[]; 用于存储字符串内容

   **这个 value 是一个 final 类型，赋值后不可修改，指的是 value[] 存储的地址不能修改，不是值不能修改**

### 创建 String 对象的两种方式

1. 直接赋值 String str = "xxxx"

   **先从常量池查看是否有 xxxx 数据空间， 如果有，直接指向；如果没有则重新创建，然后指向。str 最终指向的是常量池的空间地址**

2. 调用构造器 String str = new String("xxxx")

   **先在堆中创建空间，里面维护了 value 属性，指向常量池的 xxxx 空间，如果常量池没有 xxxx， 重新创建， 如果有，直接通过 value 指向。最终指向的是堆中的空间地址**

### 练习

1. 

~~~java
String a = "xx"; // 执行这一句时，首先在常量池看是否有 xx 的数据空间，没有就创建一个，并将 a 指向这个数据空间
String b = new String("xx"); // 执行这一句时，首先在堆中创建空间，里面维护了 value 属性，指向常量池的 xx 空间，此时常量池有 xx 空间，直接把 value 指向 xx 空间

System.out.println(a.equals(b));// T 判断 a 和 b 的字面量是否相等，a 和 b 的字面量都是 xx，所以相等，是 true
System.out.println(a == b);// F 判断的是 a 和 b 存储的地址是否相等，此时 a 存储的是变量池中 xx 的地址，而 b 中存储的是堆中的地址，所以不相等，是 false
// String 类的 intern 方法是在调用时先在池中查看是否有一个等于此 String 对象字面量的字符串(用 equals 方法判断),如果有，则返回池中的字符串的地址，如果没有，则将此 String 对象添加到池中，并返回此 String 对象的引用
System.out.println(a == b.intern()); // T 这里 a 存储的是常量池中 xx 的地址，b 因为常量池中有 xx 和 b 的字面量相等，所以返回的也是 xx 的地址，所以相等，是 true
System.out.println(b == b.intern()); // F 这里 b 存储的是堆中的地址，而 b.intern 因为常量池中有 xx 和 b 的字面量相等，所以返回的是常量池中 xx 的地址，所以不相等，是 false
~~~

2. 

~~~java
class Person {
    private String name;
}// 首先先加载类信息

Person p1 = new Person(); //先在堆中创建一块新的空间, 给 name 赋初始值 null，然后 p1 指向这块空间
p1.name = "xx"; // 先在常量池中创建 xx 的空间，然后将堆中空间的 name 指向这个 xx 的地址
Person p2 = new Person(); //先在堆中创建一个新的空间，给 name 赋初始值 null，然后 p2 指向这快空间
p2.name = "xx"; // 因为常量池中已经有了 xx 的空间，所以直接将堆中空间的 name 指向这个 xx 的地址

System.out.println(p1.name.equals(p2.name)); // T 这里比较的是字面量，xx 和 xx 相等，所以是 true
System.out.println(p1.name == p2.name); // F p1.name 指向的是 p1 指向的空间中的 name的地址，p2.name 指向的是 p2 指向的空间中的 name 的地址，两者保存的都是常量池中 xx 的地址，所以相等，是 true
System.out.println(p1.name == "xx"); // T p1.name 拿到的是常量池中 xx 的地址，而 xx 就是常量池中 xx 的引用，所以指向的也是 常量池中 xx 的地址，所以相等， 是 true

String str1 = new String("xxx"); // 首先在堆中创建空间，里面维护 value 属性，因为在常量池没有 xxx 的空间，所以要在常量池创建 xxx 的空间，然后将堆中空间的 value 属性指向常量池中 xxx 的地址
String str2 = new String("xxx"); // 首先在堆中创建空间，里面维护 value 属性，因为在常量池已经有 xxx 的空间，所以直接将堆中空间的 value 属性指向常量池中 xxx 的地址
System.out.println(str1 == str2); // 判断的是 str1 在堆中的地址 和 str2 在堆中的地址，两者不一样，所以是 F
~~~

3. 判断以下语句创建了几个对象

~~~java
String s1 = "hello"; // 首先在常量池创建了 hello 的空间， 然后 s1 指向常量池中 hello 的地址
s1 = "haha"; // 在常量池创建 haha 的空间， 然后将 s1 重新指向常量池中 haha 的地址
// 一共创建了 2 个对象， 一个 hello 一个 haha
// 这里不会将 hello 的值改变成 haha，因为字符串是不可变的，一个字符串对象一旦被分配，其内容是不可变的
~~~

---

~~~java
//这里底层做了优化，不会创建三个对象 hello abc helloabc，而是直接创建了 helloabc
String a = "hello" + "abc";// 这个优化等价于 ==> String a = "helloabc"
//所以这个创建了一个对象
~~~

---

~~~java
String a = "hello";// 首先在常量池创建 hello 的空间，然后 a 指向这个地址
String b = "abc";// 常量池创建 abc 的空间，然后 b 指向这个地址
String c = a + b;
// 1. 首先创建一个 StringBuilder sb = StringBuilder();
// 2. 执行 sb.append("hello");
// 3. 再执行 sb.apend("abc");
// 4. 最后执行 sb.toString 方法，是 new String("字符串");
// 所以最后其实 c 指向的是堆中的对象(String) value[], 然后这个指向池中的 "helloabc" 空间
// 所以是三个对象 一个是 a，一个是 b，一个是 c

String d = "helloabc"; // 此时常量池中已经有了 helloabc 的空间了，所以直接将 d 指向常量池中 helloabc 的地址
System.out.println(c == d)// 这里的 c 指向的是堆中对象的地址，因为 String c = a + b 会执行 StringBuilder 对象的 toString 方法，而这个方法会调用 new String()；生成一个对象. d 指向的是常量池中的地址，所以不相等，是 false
~~~

**总结：常量相加，看的是池。变量相加，是在堆中**

~~~java
public class Test {
    String str = new String("xx");
    final char[] ch = { "j","a","v","a" };
    public void change(String str, char ch[]){
        str = "java";
        ch[0] = "h";
    }
    public static void main(String[] args){
        Test1 ex = new Text1(); //首先在堆中创建一块空间，然后给 str 和 ch 初始化赋值，然后再堆中创建另一块空间，然后再常量池中创建 xx 的空间，堆空间中的 str 指向现在堆中的另一块空间，这块空间指向 xx，再在堆中创建一块空间，存储j，a，v，a，因为这里是 char，所以不用保存在常量池，然后在栈中创建 ex 指向第一个创建的堆空间
        ex.change(ex.str, ex.ch);// 运行方法时会重新创建一个栈，这里传入的 ex.str 是指向堆空间中 String 创建的地址，change 方法中的 又在常量池创建了 java 的空间，然后将这个新的栈里的 str 指向了常量池中的 java 地址，但是原来的 ex.str 并没有变化，因为这个引用保存在堆里，然后将堆空间中 ch 的引用地址赋值给了新的栈中的 ch，然后将堆中原先 ch 的引用第零个 j 改成了 h，同样是因为这里是 char 类型，所以不用再常量池创建 h，直接在堆中替换
        System.out.println(ex.str + "and");//所以这里的 ex.str 并没有变化，还是 xx，结果是 xxand
        System.out.println(ex.ch);//所以这里的 ex.ch 就是改过了的 { "h","a","v","a" }
    }
} 
~~~

### String 的常用方法

- equals  区分大小写，判断内容是否相等

- equalslgnoreCase 忽略大小写，判断内容是否相等

- length 获取字符的个数，字符串的长度

- indexOf 获取字符在字符串中第 1 次出现的索引，索引从 0 开始，如果找不到，就返回 -1

- lastIndexOf 获取字符在字符串中最后 1 次出现的索引，索引从 0 开始，如找不到，返回 -1

- substring 截取指定范围的子串 

  **两个参数，比如 str.substring(0,5) 就表示从索引 0 开始截取，截取到索引 5 - 1 位置， 包括 5 - 1**

  **如果只填一个参数 比如 str.substring(6) 表示从索引 6 开始截取后面素有的内容**

- trim 去前后空格

- charAt 获取某索引处的字符，注意不能使用 Str[index] 这种方式

- toUpperCase 把字符串转换成大写

- toLowerCase 把字符串转换成小写

- concat 拼接字符串

- replace 替换字符串中的字符

  **str.replace("xx","yy") 是将 str 中 所有 的 xx 替换成 yy，然后返回新值，对 str 本身没有影响**

- split 分割字符串

- toCharArray 转换成字符数组

- compareTo 比较两个字符串的大小，如果前者大，返回整数，如果后者大，返回负数，如果相等，返回 0

  **比较首先比较长度，如果长度不一样就返回长度差，如果长度一样，就比较每个字符，只要有一个不一样，就返回这个字符在 ASCII 码中对应的数字的差值，如果都一样就返回 0**

- format 格式化字符串

  ~~~java
  String name = "john";
  int age = 10;
  double score = 98.3 / 3;
  char gender = '男';
  
  // 想要拼接字符串，笨方法就是一个一个拼接
  String info1 = "我的姓名是" + name + "年龄是" + age + "成绩是" + score + "性别是" + gender;
  // 字符串的 format 可以方便的完成拼接
  String info2 = String.format("我的名字是%s 年龄是%d 成绩是%.2f 性别是%c",name,age,score,gender);
  // %s %d %.2f %c 称为占位符，这些占位符由后面的变量来替换
  // %s 表示后面由字符串替换
  // %d 表示整数替换
  // %.2f 表示使用一个小数来替换，替换后，.2 表示只会保留小数点后两位，并且进行四舍五入
  // %c 表示使用 char 类型替换
  ~~~

## StringBuffer 类

- java.lang.StringBuffer 代表可变的字符序列，可以对字符串内容进行增删
- 很多方法与 String 相同，但StringBuffer 是可变长度的
- StringBuffer 是一个容器

### 解读

1. StringBuffer 的直接父类是 AbstractStringBuilder
2. StringBuffer 实现了 Serializable，即 StringBuffer 的对象可以串行化
3. 在父类中 AbstractStringBuilder 有属性 char[] value， 不是 final，该 value 数组存放字符串内容， 因此是存放在堆中的
4. StringBuffer 是一个 final 类，不能被继承
5. 因为 StringBuffer 字符内容是存在 char[] value 中， 所以在变化【增加或删除】时，不用每次都更换地址【即不是每次都创建新的对象】，所以效率高于 String

---

### String 和 StringBuffer 的对比

1. String 保存的是字符串常量，里面的值不能更改，每次 String 类的更新实际上就是更改地址，效率很低
2. StringBuffer 保存的是字符串变量，里面的值可以更改，每次 StringBuffer 的更新实际上可以更新内容，不用更新地址，效率较高 【 char[] value; 保存在堆中】

### StringBuffer 的构造器

- 无参

  ~~~java
  StringBuffer stringBuffer = new StringBuffer();
  //会创建一个大小为 16 的 char[]，用于存放字符内容
  ~~~

- 直接传入一个 char[]，通过这个 char[]，直接构造一个字符串缓冲区，和这个 char[] 有着相同的字符

  **用得少**

- 通过构造器指定 char[] 大小

  ~~~java
  StringBuffer stringBuffer = new StringBuffer(100);
  //会创建一个大小为 100 的 char[]
  ~~~

- 通过给一个 String 创建 StringBuffer

  ~~~java
  StringBuffer stringBuffer = new StringBuffer("hello");
  // 这时候 char[] 数组大小就是字符串长度加上 16
  ~~~

  ---

### String 和 StringBuffer 的相互转换

- String --> StringBuffer

  ~~~java
  String str = "Hello World";
  // 方式一: 使用构造器
  // 注意: 返回的才是 StringBuffer，对 str 本身没有影响
  StringBuffer stringBuffer = new StringBuffer(str);
  
  // 方式二: 使用 append 方法
  StringBuffer stringBuffer1 = new StringBuffer();
  stringBuffer1 = stringBuffer1.append(str);
  ~~~

- StringBuffer --> String

  ~~~java
  StringBuffer stringBuffer = new StringBuffer("xxxx");
  //方式一: 使用 StringBuffer 提供的 toString 方法
  String str1 = stringBuffer.toString();
  
  //方式二: 使用构造器
  String str2 = NEW String(stringBuffer)
  ~~~

### StringBuffer 的常用方法

1. append 增 单个参数

   **调用 append 方法传入的不管什么类型都会转换成 StringBuffer**

2. delete 删 两个参数（ start, end ）

   删除索引 **大于等于 start** 并且 **小于 end **的字符

3. replace 改 三个参数 ( start, end, str )

   使用 str 替换 **大于等于 start ** 并且 **小于 end ** 处的字符，直接更改原字符串

4. indexOf 查 

   **查找指定的子串在字符串中第一次出现的索引，没有则返回 -1**

5. insert  插 两个参数 ( index, str )

   **在索引为 index 的位置插入 str**

6. length 长度

   **返回 StringBuffer 的长度**

### 练习

~~~java
String str = null;
StringBuffer sb = new StringBuffer();
sb.append(str);
System.out.println(sb.length()); // 4, 因为 append 方法会把传入的值转成字符串

System.out.println(sb); // StringBuffer 类型的 null
StringBuffer sb1 = new StringBuffer(str); // 这里会抛出空指针异常，因为构造器中传入了 null，在StringBuffer 的源码中会调用传入值的 length 方法，null 没有，所以报错
System.out.println(sb1);
~~~

## StringBuilder 类

**StringBuilder 和 StringBuffer 均代表可变的字符序列,方法是一样的,所以使用和 StringBuffer 一样**

1. 一个可变的字符序列，此类提供一个与 StringBuffer 兼容的 API，但不保证同步( StringBuilder 不是线程安全的【即存在多线程问题】 )。该类被设计用作 StringBuffer 的一个简单替换，**用在字符串缓冲区被单个线程使用的时候。如果可能，建议优先采用该类**，因为在大多数实现中，它比 StringBuffer 要快
2. 在 StringBuilder 上的主要操作是 append 和 insert 方法，可重载这些方法，以接受任意类型的数据

### 解读

1. StringBuilder 继承 AbstractStringBuilder 类
2. 实现了 Serializable, 说明 StringBuilder 对象是可以串行化( 对象可以网络传输, 也可以保存到本地 )
3. StringBuilder 是 final 类, 不能被继承
4. StringBuilder 对象字符序列仍然是存放在其父类 AbstractStringBuilder的 char[] value; 因此,字符序列在堆中
5. **StringBuilder 的方法没有互斥的处理, 即没有 synchronized 关键字, 因此在单线程的情况下使用 StringBuilder**

## String 和 StringBuffer 和 StringBuilder 比较

1. StringBuilder 和 StringBuffer 非常类似, 均代表可变的字符序列,而且方法也一样

2. String: 不可变字符序列, 效率低, 但是复用率高

3. StringBuffer: 可变字符序列, 效率高(增删), 线程安全

4. StringBuilder: 可变字符序列, 效率最高, 线程不安全

5. String 使用注意说明

   ~~~java
   String s = "a"; //创建了一个字符串"a"
   s += "b"; // 实际上原来的 "a" 字符串对象已经丢弃了,现在又产生了一个字符串 s + "b" (也就是 "ab" ).如果多次执行这些改变串内容的操作,会导致大量副本字符串对象存留在内存中,降低效率.如果这样的操作放到循环中,会极大影响程序的性能
   // 结论: 如果我们对 String 做大量修改, 不要使用 String
   ~~~

   ## 使用规则

   1. 如果字符串存在大量的修改操作，一般使用 StringBuffer 或 StringBuilder
   2. 如果字符串存在大量的修改操作，并在单线程的情况下，使用 StringBuilder
   3. 如果字符串存在大量的修改操作，并在多线程的情况下，使用 StringBuffer
   4. 如果我们字符串很少修改，被多个对象引用，使用 String，比如配置信息等

## Math 类

### 常用方法

1. abs 绝对值 **返回值是 int 类型**
2. pow 求幂 **返回值是 double 类型**
3. ceil 向上取整 **返回值是 double 类型**
4. floor 向下取整 **返回值是 double 类型**
5. round 四舍五入 **返回值是 long 类型**
6. sqrt 求开方 **返回值是 double 类型** 
7. random 求随机数 **返回值是 double 类型**
8. max 求两个数的最大值 **有函数重载， 传入什么类型就返回什么类型**
9. min 求两个数的最小值 **有函数重载， 传入什么类型就返回什么类型**

~~~java
//获取一个 a - b 之间的一个随机整数(包括 a 也包括 b)
Math.random() * (b - a + 1) + a;
~~~

## Arrays 类

### Arrays 常用方法

1. toString 返回数组的字符串

2. sort 排序（ 自然排序和定制排序 ）

   ~~~java
   Integer[] arr = { 1,-1,7,0,89 };
   //默认排序
   // 1.因为数组是引用类型，所以通过 sort 排序后，会直接影响到实参 arr
   // 2.sort 重载的，也可以通过传入一个接口 Comparator 实现定制排序
   Arrays.sort(arr); //{ -1,0,1,7,89 }
   // 定制排序
   // 1. 调用定制排序时，传入两个参数
   // (1) 排序的数组 arr
   // (2) 实现了 Comparator 接口的匿名内部类，要求实现 compare 方法
   Arrays.sort(arr, new Comparator() {
       @Override
       public int compare(Object o1, Object o2){
           Integer i1 = (Integer) o1;
           Integer i2 = (Integer) o2;
           return i2 - i1
       }
   });
   // 这时 arr 为 {  }
   ~~~

3. binarySearch 通过二分搜索法进行查找，要求必须排好序

   ~~~java
   // 1. 使用 binarySearch 二叉查找
   // 2. 要求该数组是有序的(升序)，如果该数组是无序的，不能使用这种二分查找法
   // 3. 如果数组中不存在该元素，就返回 这个元素应该存在的索引加一的负数
   Integer[] arr = { 1,2,90,123,567 };
   int index = Arrays.binarySearch(arr, 95);
   // index = -(3 + 1) = -4
   ~~~

4. copyOf 数组元素的复制

   ~~~java
   Integer[] arr = { 1,2,90,126,11 };
   // 1. 从 arr 数组中，拷贝 arr.length 个元素到 newArr 数组中
   // 2. 如果拷贝的长度 > arr.length 就在新数组的后面增加 null
   // 3. 如果拷贝长度 < 0 就会抛出异常 NegativeArraySizeException 
   // 4. 该方法的底层使用的是 System.arraycopr();
   Integer newArr = Arrays.copyOf(arr, arr.length)
   ~~~

5. fill 数组元素的填充

   ~~~java
   Integer integers = new Integer[]{ 9,3,2 };
   Arrays.fill(integers,99);// 值会变成{ 99,99,99 }
   ~~~

6. equals 比较两个数组元素内容是否完全一致

7. asList 将一组值，转换成 list 

   ~~~java
   // 1. asList 方法，会将 (2,3,4,5,6,1) 数据转成一个 list 集合
   // 2. 返回的 asList 的变异类型 List(接口)
   // 3. asList 运行类型 java.util.Arrays#ArrayList, 是 Arrays 类的静态内部类
   List<Integer> asList = Arrays.asList(2,3,4,5,6,1);
   ~~~

## System 类

1. exit 退出当前程序

   ~~~java
   System.out.println("ok1");
   System.exit(0);
   System.out.println("ok2");
   // 1. 0表示一个状态， 正常的状态
   // 2. ok2 不会输出
   ~~~

   

2. arraycopy 赋值数组元素，比较适合底层调用，一般使用 Arrays.copeOf 完成赋值数组

   ~~~java
   int[] arr = {1,2,3};
   int[] newArr = new int[3];
   System.arraycope(arr, 0, newArr, 0, 3);
   // 1. 第一个参数是原数组
   // 2. 原数组开始拷贝处的索引
   // 3. 目标数组，即把原数组的数据拷贝到哪个数组
   // 4. 目标数组的开始索引
   // 5. 从原数组拷贝多少个数据到目标数组，超出原数组范围报错
   ~~~

3. currentTimeMillens 返回当前时间举例 1970-1-1 的毫秒数( 时间戳 )

4. gc 运行垃圾回收机制

## BigInteger 和 BigDecimal 类

### 应用场景

1. BigInteger 适合保存比较大的整型  ---> new 时使用字符串
2. BigDecimal 适合保存精度更高的浮点型( 小数 )

### 常用方法

- BigInteger  ---> 不能直接加减乘除，需要依赖方法

  1. add 加
  2. subtract 减
  3. multiply 乘
  4. divide 除

- BigDecimal

  **方法同上，只是这个类的 divide 方法可能会抛出异常，因为除了之后的值可能会是一个无限循环小数，**

  **解决方案：在调用divide 方法时，指定精度即可**

  ~~~java
  BigDecimal bigDecimal1 = new BigDecimal("19.54534415321351321325456");
  BigDecimal bigDecimal2 = new BigDecimal("1.1");
  
  //BigDecimal.ROUND_CEILING: 表示如果有无限循环小数，就会保留 分子 的精度
  System.out.println(bigDevimal1.divice(bigDecimal2, BigDecimal.ROUND_CEILING))
  ~~~

## 日期类

### 第一代日期类 Date

1. Date  精确到毫秒，代表特定的瞬间

2. SimpleDateFormat  

   格式和解析日期的类 SimpleDateFormat 格式化和解析日期的具体类。它允许进行格式化( 日期 -> 文本 )、解析( 文本 -> 日期 ) 和规范化

~~~java
Date d1 = new Date(); // 虎丘当前系统时间
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 hh:mm:ss E"); // E代表星期几
String format = sdf.format(d1);// format 就是格式化之后的日期

String s = "1996年01月01日 10:20:30 星期一";
Date parse = sdf.parse(s);// 这里的 parse 会有个 parseException 异常， 可以跑出去
// 这里的 parse 就是原本的格式
~~~

### 第二代日期类 Calendar

**它为特定瞬间与一组诸如 YEAR、MONTH、DAY_OF_MONTH、HOUR 等日期字符之间的转换提供了一些方法，并为操作日历字段 ( 例如获得下星期的日期 ) 提供了一些方法**

1. Calendar 类是一个抽象类
2. 可以通过 getInstance 方法来获取实例
3. 提供大量的方法和字段提供诶程序员
4. Calendar 没有提供对应的格式化的类，因此需要程序员自己组合来输出
5. 如果我们需要按照 24 小时进制来获取时间，Calendar.HOUR 改成 Calendar.HOUR_OF_DAY

~~~java
Calendar c = Calendar.getInstance();//创建日历类对象
System.out.println("年:" + c.get(Calendar.YEAR));
//这里之所以要加一，是因为 Calendar 在返回月的时候，是按照 0 开始编号的
System.out.println("月:" + (c.get(Calendar.MONTH) + 1));
System.out.println("日:" + c.get(Calendar.DAY_OF_MONTH));
System.out.println("小时:" + c.get(Calendar.HOUR));
System.out.println("分钟:" + c.get(Calendar.MINUTE);
System.out.println("秒:" + c.get(Calendar.SECOND));
~~~

### 第三代日期类

#### 前两代日期类的不足分析

JDK1.0 中包含了一个 java.util.Date 类，但是它的大多数方法已经在 JDK1.1 中引入 Calendar 类之后被弃用了。而 Calendar 也存在问题是: 

1. 可变性: 像日期和时间这样的类应该是不可变的
2. 偏移性: Date中的年份是从 1900 开始的，而月份都是从 0 开始
3. 格式化: 格式化只对 Date 有用，Calendar 则不行
4. 此外，它们也不是线程安全的；不能处理润秒等（ 每隔两天会多出 1s ）

**在 JDK8加入了 LocalDate (日期/年月日) 、LocalTime (时间/时分秒) 、LocalDateTime (日期时间/年月日时分秒) **

- LocalDate 只包含日期，可以获取日期字段
- LocalTime 只包含时间，可以获取时间字段
- LocalDateTime 包含日期 + 时间，可以获取日期和时间字段

~~~java
LocalDateTime ldt = LocalDateTime.now(); // 使用 now 方法返回表示当前日期时间的 对象
// LocalDate.now() 只返回日期 -> 年月日
// LocalTime.now() 只返回时间 -> 时分秒
System.out.println("年:" + ldt.getYear());
System.out.println("月:" + ldt.getMonth()); //返回对应月份的中文
System.out.println("月:" + ldt.getMonthValue()); //返回对应月份的数字
System.out.println("日:" + ldt.getDayOfMonth());
System.out.println("小时:" + ldt.getHour());
System.out.println("分钟:" + ldt.getMinute());
System.out.println("秒:" + ldt.getSecond());
~~~

#### 格式日期类 DateTimeFromatter 

~~~java
LocalDateTime ldt = LocalDateTime.now();
DateTimeFormatter dtf = DateTimeFormatter。ofPattern("yyyy年MM月dd日 HH小时mm分钟ss时间");
String format = dtf.format(ldt) //format 就是格式化之后的日期
~~~

#### Instant 时间戳

~~~java
//1. 通过静态方法 now 获取表示当前时间戳的对象
Instant now = Instant.now();
// 2. 通过 from 方法可以把 Instant 转成 Date
Date date = Date.form(now);
// 3. 通过 date 的 toInstant 方法可以把 Date 转成 Instant 对象
Instant instant = date.toInstant();
~~~

#### 更多方法

- LocalDateTime 类
- MonthDay 类 ---> 检查重复时间
- 是否是闰年
- 增加日期的某个部分
- 使用 plus 方法测试增加时间的某个部分
- 使用 minus 方法测试减少时间的某个部分
- ...

~~~java
LocalDateTime ldt = LocalDateTime.now();
xxxxxxxxxx //获取800天后的时间
LocalDateTime ldt2 = ldt.plusDays(800); //这时未格式化的 800 天后的时间
DateTimeFormatter dtf = DateTimeFormatter。ofPattern("yyyy年MM月dd日 HH小时mm分钟ss时间");
dtf.format(ldt2); //这就是格式化之后的 800 天后的时间

// 3456 分钟前的时间，格式化输出
LocalDateTime ldt3 = ldt.minusMinutes(3456); //未格式化的 3456 分钟之前的日期
dtf.format(ldt3); // 格式化后的 3456 分钟之前的日期
~~~