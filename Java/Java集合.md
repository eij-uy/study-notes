[toc]

# Java 集合

## 为什么要用集合

**之前保存多个数据使用的是数组,但是数组有很多不足之处**

- 长度开始时必须指定,而且一旦指定,不能更改
- 保存的必须为同一类型的元素
- 使用数组进行增加/删除元素的任意代码比较麻烦

~~~java
Person[] pers = new Person[1]; //创建一个大小是 1 的数组
pers[0] = neW Person();

// 数组扩容
Person[] pers1 = new Person[pers.length() + 1];//创建一个新的数组
for(int i = 0; i < pers.length(); i++){
    pers1[i] = pers[i] //循环拷贝 pers 数组的元素到 pers1
}
pers1[pers1.length - 1] = new Person(); // 添加新的对象
~~~

## 集合的好处

1. 可以动态保存任意多个对象,使用比较方便
2. 提供了一系列方便的操作对象的方法: add、remove、set、get等
3. 使用集合添加, 删除新元素的代码比较简洁

## Collection 接口实现类的特点

public interface Collection<E> extends Iterable<E>

1. collection 实现子类可以存放多个元素,每个元素可以使Object
2. 有些 Collecttion 的实现类,可以存放重复的元素,有些不可以
3. 有些 Collection 的实现类,有些是有序的 ( List ), 有些不是有序的 ( Set )
4. Collection 接口没有直接的实现子类,是通过它的子接口 Set 和 List 来实现的 

### Collection 接口的常用方法

**因为接口是不可以被实例化的, 所以以实现了这个接口的子类ArrayList 来演示**

1. add	添加单个元素

2. remove    删除指定元素

3. contains   查找元素是否存在

4. size   获取元素个数

5. isEmpty   判断是否为空

6. clear   清空

7. addAll   添加多个元素

8. containsAll   查找多个元素是否都存在

9. removeAll   删除多个元素

   ~~~java
   List list = new ArrayList();
   
   //1.增加单个元素
   list.add("jack");
   list.add(10);
   list.add(true);
   //这是的 list 输出后是[jack,10,true],这些每一个都是值对象形式, 例如 10 就是 new Integer(10) 
   
   //2.删除指定元素
   list.remove(0);// 参数可以放索引,例如 0 就是删除第一个
   list.remove(true); // 参数也可以放具体某个元素, 这回删除掉 true
   
   //3.查找元素是否存在
   list.contains("jack"); // 这会返回 true
   
   //4.获取元素个数
    list.size();// 这会返回 2, 返回的是索引最后一位
   
   //5.判断是否为空
   list.isEmpty(); // 返回 false,因为现在不为空
   
   //6.清空
   list.clear(); //会清空 list 中的数据
   
   //7.添加多个元素
   ArrayList list1 = new ArrayList();
   list1.add("红楼梦");
   list1.add("三国演绎");
   list.addAll(list2); //参数是一个集合
   
   //8.查找多个元素是否都存在
   list.containsAll(list2); //因为前面添加了, 所以返回 true
   
   //9.删除多个元素
   list.removeAll(list2); //这时会在 list 中删除掉 list2 的数据
   ~~~

### Collection 接口遍历元素方法

**两种方式**

- 使用 iterator 迭代器

  1. Iterator 对象成为迭代器,主要用于遍历 Collection 集合中的元素
  2. 所有实现了 Collection 接口的集合类都有一个 Iterator 方法, 用以返回一个实现了 Iterator 接口的对象, 即可以返回一个迭代器
  3. Iterator 的接口
  4. Iterator 仅用于遍历集合, Iterator 本身并不存放对象

  ~~~java
  Collection col = new ArrayList();
  col.add(new Book("三国演义"));
  col.add(new Book("小李飞刀"));
  col.add(new Book("红楼梦"));
  
  //1.首先得到 col 对应的迭代器
  Iterator iterator = col.iterator();
  
  //2.使用 while 循环遍历
  while(iterator.hasNext()){ //判断下一个是否还有数据
      //返回下一个元素,类型是Object
    Object obj = iterator.next();// 这个obj 编译类型是 Object,运行类型是 Book
      System.out.println("obj=" + obj);
  }
  
  // IDEA 快速生成 while iterator 循环的短语是 itit
  // IDEA 查看快捷键的快捷键 ctrl + j
  // 3.当退出 while 循环后,这时 iterator 迭代器,指向最后的元素,不能再使用 iterator.next() 如果想要再次遍历,需要重置迭代器
  iterator = col.iterator(); //重置迭代器,这就可以进行第二次遍历了
  
  ~~~

- for 循环增强

  **增强 for 循环,可以代替 iterator 迭代器**

  **特点: ** 增强 for 就是简化版的 iterator 本质一样,只能用于遍历集合或数组

  **基本语法: ** for ( 元素类型 元素名: 集合名或数组名 ){

  ​							//访问元素

  ​                   }

  ~~~java
  Collection col = new ArrayList();
  col.add(new Book("三国演义"));
  col.add(new Book("小李飞刀"));
  col.add(new Book("红楼梦"));
  
  //1. 使用增强 for 循环
  //2. 增强 for 的底层仍然是在调用 iterator 迭代器
  //3. 所以增强 for 就是简化版的 迭代器遍历
  for(Object book : col){
      //这里就能拿到每一个 book
  }
  // IDEA 快捷键 I
  ~~~

## List 接口  

**List 接口实现了 Collection 接口, 是 Collection 接口的子接口**

### 特点

1. List 集合类中元素有序【即添加顺序和取出顺序一致】 且可以重复

2. List 集合中的每个元素都有其对应的顺序索引,即支持索引

3. List 容器中的元素都对应一个整数型的序号记在其在容器中的位置,可以根据序号存取容器中的元素

   ~~~java
   List list = new ArrayList();
   list.add("jack");
   list.add("tom");
   list.add("mary");
   list.add("xxx");
   list.add("tom");
   //这时 list 输出是 [jack,tom,mary,xxx,tom]
   list.get(3);// 获取到的就是 xxx,通过 get 方法以索引取值
   ~~~

   

4. JDK API中 List接口的实现类有很多,常用的有

   - ArrayList
   - LinkedList 
   - Vector

### 常用方法

1. void add  ( int index, Object ele ); 在 index 位置 插入 ele 元素

2. boolean addAll (int index, Collection eles); 在 index 位置开始将 eles 中的所有元素添加进来

3. Object get（int index）获取指定 index 位置的元素

4. int indexOf ( Object obj ) 返回 obj 在集合中首次出现的位置

5. int lastIndexOf ( Object obj ) 返回 obj 在当前集合中末次出现的位置

6.   Object remove(int index) 移除指定 index 位置的元素，并返回该元素

7. Object set ( int index, Object ele ) 设置指定 index 位置的元素为 ele

8. List subList ( int fromIndex, int toIndex )  返回从 fromIndex 到 toIndex 位置的子集，包括 fromIndex， 不包括 toIndex

   ~~~java
   List list = new ArrayList();
   list.add("张三丰");
   list.add("贾宝玉");
   
   //在 index = 1 位置插入一个对象
   list.add(1, "xxx");// 这时 list 输出就是 [张三丰,xxx,贾宝玉]
   
   List list2 = new ArrayList();
   list2.add("jack");
   list2.add("tom");
   list.addAll(1,list2);// 在张三丰后面添加 list2 中所有元素,剩余元素往后移
   
   ~~~

### ArrayList 注意事项

1. 可以存放所有元素，包括空元素
2. ArrayList 是由数组来实现数据存储的
3. ArrayList 基本等同于 Vector，**除了 ArrayList 是线程不安全 ( 执行效率高 ), 在多线程情况下，不建议使用 ArrayList**

### ArrayList 底层结构和源码分析

1. ArrayList 维护了一个 Object 类型的数组 elementData

   transient Object[] elementData;

   **transient  ---> 表示瞬间，短暂的，表示该属性不会被序列化**

2. 当创建 ArrayList 对象时，如果使用的是无参构造器，则初始 elementData 容量为 0，第一次添加，则扩容 elementData 为 10，如需要再次扩容，则扩容 elementData 为 1.5 倍

3. 如果使用的是指定大小的构造器，则初始化 elementData 容量为指定大小，如果需要扩容，则直接扩容 elementData 为 1.5 倍

   ![](D:\study\markdown\Java\images\Java-IDEA工具Debug时默认显示简化数据，展示全部数据的设置.png)


### Vector 

#### Vector的基本介绍

1. Vector 类的定义说明

   ~~~java
   public class Vector<E> 
       extends AbstractList<E>
       implements List<E>,RandomAccess,Cloneable,Serializable
   ~~~

2. Vector 底层也是一个对象数组, protected Object[] elementData

3. Vector 是线程同步的,即线程安全的, Vector类的操作方法带有 synchronized

   ~~~java
   public synchronized E get(int index){
       if(index >= elementCount)
           throw bew ArrayIndexOutOfBoundsException(index);
       return elementData[index];
   }
   ~~~

4. 在开发中,需要线程同步安全时,考虑使用 Vector

### Vector 和 ArrayList 的比较

|           | 底层结构 | 版本   | 线程安全(同步)效率 | 扩容倍数                                                     |
| :-------: | :------: | ------ | ------------------ | ------------------------------------------------------------ |
| ArrayList | 可变数组 | jdk1.2 | 不安全,效率高 | 如果有参构造1.5倍,如果是无参,第一次是10,从第二次开始安1.5倍  |
|  Vector   | 可变数组Object[] | jdk1.0 | 安全,效率不搞 | 如果是无参,默认10,满后,就按2倍扩容,如果指定大小,则每次直接按2倍扩容 |

### LinkedList

#### LinkedList说明

1. LinkedList 底层实现了双向链表和双端队列特点
2. 可以添加任意元素 ( 元素可以重复 ), 包括 null
3. 线程不安全, 没有实现同步 

#### LinkedList的底层操作机制

1. LinkedList 底层维护了一个双向链表
2. LinkedList 中维护了两个属性 first 和 last 分别指向首节点和尾节点
3. 每个节点 ( Node对象 ), 里面又维护了 prev 、next、item 三个属性, 其中通过 prev指向前一个节点, 通过 next 指向后一个节点. 最终实现双向链表
4. 所以 LinkedList 的元素的添加和删除, 不是通过数组完成的, 相对来说效率较高.

#### 模拟一个简单的双向链表

~~~java
//定义一个 Node 类, 表示双向列表的一个节点
class Node {
    public Object item; // 真正存放数据
    public Node next; // 指向后一个节点
    public Node prev; // 指向前一个节点
    public Node(Object name){
        this,item = item
    }
    public String toString(){
        return "Node name=" + item;
    }
}

Node jack = new Node("jack");
Node tom = new Node("tom");
Node huihui = new Node("huihui");

//连接三个节点, 形成双向链表
// jack -> tom -> huihui
jack.next = tom;
tom.next = huihui;
// huihui -> tom -> jack
huihui.prev = tom;
tom.prev = jack;

Node first = jack // 让 first 引用指向 jack, 就是双向链表的头节点
Node last = huihui;// 让 last 引用指向 huihui, 就是双向相链表的尾节点

//遍历 -> 从头到尾
while(true){
    if(fist == null){
        break;
    }
    System.out.println(first);
    first = first.next;
}

//遍历 -> 从尾到头
while(true){
    if(last == null){
        break;
    }
    System.out.println(last);
    last = last.prev;
}

// 演示链表的添加/删除
// 在 tom 之后插入一个对象 smith
Node smith = new Node("smith");
smith.next = huihui;
smith.prev = tom;
tom.next = smith;
huihui.prev = smith;
~~~

#### LinkedList 源码阅读

~~~java
LinkedList linkedList = new LinkedList(); //这是 linkList 的属性 first = null, last = null
//add 操作源码流程
linkedList.add(1); // 调用 linkLast;
 
//这是源码
void linkLast(E e){
    final Node<E> l = last; //首先将 last 赋给 l
    final Node<E> newNode = new Node<>(l, e, null); // 创建一个新节点, 并将新节点的 prev 指向 last
    last = newNode; //last 指向 新节点
    if(l == null) // 如果现在 last 是 null 的话, 代表目前是个空链表, 所以直接将first 也指向新节点
        first = newNode;
    else // 如果现在 last 不是 null 的话, 代表现在是个有值的链表, 所以将 add 之前的 last 的 next 指向新节点
        l.next = newNode;
    size++; //大小
    modCount++; //修改次数
}

//默认删除第一个, 可以传第几个(索引)也可以传具体 Object
linkedList.remove(); //默认调用 removeFirst
// 源码
public E removeFirst(){
    final Node<E> f = first; // 拿到当前链表中第一个元素
    if(f == null) //如果第一个元素是空, 那么这就是一个空链表,空链表不能删除,索引抛出异常
        throw new NoSuchElementException();
    return unlinkFirst(f); 
}
private E unlinkFirst(Node<E> f){
    final E element = f.item; // 拿到当前的对象
    final Node<E> next = f.next; // 拿到下一个对象
    f.item = null; // 将第一个当前对象至空
    f,next = null; // 将第一个的 next 至空
    first = next; // 现在 first 原先的指向变成了 null, 所以要重新指向 first 到 next
    if(next == null) // 如果 next 是空的话,就代表链表删除这一个元素之后变成了空链表,如果是这样,上面已经把 first 至空, 所以这里要再把 last 至空
        last = null;
    else
        next.prev = null; // 如果删除第一个之后,下一个还有值的话,就将下一个的 上一个引用指向空,因为这时的下一个已经是第一个了
    size--; //大小
    modeCount++; // 修改次数
    return element;
}

// 后面大同小异
// 某个节点对象
linkedList.set(1, 999);
// 得到某个节点的值
Object obj = linkList.get(1);
//因为 LinkedList 实现了 List 接口, 所以它可以使用迭代器遍历,自然也可以使用增强 for 循环, 同时因为 size 方法可以获取具体大小, 所以也可以使用 普通 for 循环遍历
~~~

### ArrayList 和 LinkedList 的比较

|            | 底层结构 | 增删的效率        | 改查的效率 | 线程安全 |
| ---------- | -------- | ----------------- | ---------- | -------- |
| ArrayList  | 可变数组 | 较低,数组扩容     | 较高       | 不安全   |
| LinkedList | 双向链表 | 较高,通过链表追加 | 较低       | 不安全   |

#### 如何选择 ArrayList 和 LinkedList

1. 如果我们改查的操作多,选择 ArrayList
2. 如果我们增删的操作多,选择 LinkedList
3. 一般来说,在程序中, 80% - 90% 都是查询,因此大部分情况下会选择 ArrayList
4. 在一个项目中,根据业务灵活选择,也可能这样,一个模块使用的是 ArrayList,另外一个模块是 LinkedList  , 要根据业务来进行选择

## Set 接口

**和 List 接口一样, Set 接口也是 Collection 的子接口 **

### Set 接口基本介绍

1. 无序 ( 添加和取出的顺序不一致 ), 没有索引
2. 不允许重复元素, 所以最多包含一个 null
3. JDK API 中 Set 接口的实现类很多, 常用的有:
   - HashSet
   - TreeSet

### Set 接口常用方法

**和 List 接口一样, Set 接口也是 Collection 的子接口, 因此, 常用方法和 Collection 接口一样**

### Set 接口的遍历方式

**同 Collection 的遍历方法一样, 因为 Set 接口是 Collection 接口的子接口**

1. 可以使用迭代器
2. 增强 for 循环
3. 不能使用索引的方式来获取

~~~java
//这里使用 HashSet 来描述 Set 接口的常用方法
// 1. Set 接口的实现类的对象(Set接口对象),不能存放重复的元素,可以添加 null
// 2. Set 接口对象存放数据是无序的(即添加的顺序和取出的顺序不一致)
// 3. 注意: 取出的顺序虽然不是添加的顺序,但是他是固定的,底层有自己的算法
Set set = ne wHashSet();
set.add("john");
set.add("lucy");
set.add("john");
set.add("jack");
set.add(null);

//遍历 
//1. 迭代器
Iterator iterator = set.iterator();
while(iterator.hasNext()){
	Object obj = iterator.next();
    System.out.println("obj=" + obj)
}

//2. 增强 for 循环
for(Object obj : set){
    System.out.println("obj=" + obj)
}

//3. 不能使用传统 for 循环来进行遍历, 因为 Set 虽然可以获取到 size, 但是 Set 接口对象没有 get 方法
~~~

### HashSet 的全面说明

1. HashSet 实现了 Set 接口

2. HashSet 实际上是 HashMap,

   ~~~java
   public HashSet(){
       map = new HashMap();
   }
   ~~~

3. 可以存放 null 值, 但是只能有一个 null

4. HashSet 不保证元素是有序的, 取决于 hash 后, 再确定索引的结果

5. 不能有重复元素/对象

~~~java
Set set = new HashSet();
set.add("john"); // 返回T
set.add("lucy"); // 返回T
set.add("john"); // 返回F
set.add("jack"); // 返回T
set.add("Rose"); // 返回T
// 这时 set = [Rose,john,jack,lucy]
set.remove("john"); 
// 这时 set = [Rose,jack,lucy]

set = new HashSet(); // 这时 set = []
set.add("lucy"); // 返回T
set.add("lucy"); // 返回F
set.add(new Dog("tom"));// 返回T
set.add(new Dog("tom"));// 返回T 因为两个 tom 不是同一个对象
//这时 set = [lucy, Dog{name='tom'}]

set.add(new String("xxx")); // 返回T
set.add(new String("xxx")); // 返回F
//这里等学了 HashMap 再重新来过
~~~

### HashMap 

**HashMap 的底层是 数组 + 链表 + 红黑树**

**就是数组的每一项都是一个链表, 也有可能是一个红黑树, 之所以这样是为了性能考虑**

1. 添加一个元素时, 先得到 hash 值 --- 会转成 ---> 索引值
2. 找到存储数据表 table ( 自定义名称 ),看到这个索引位置是否已经存放的有元素
3. 如果没有, 直接加入
4. 如果有, 调用 equals 比较, 如果相同, 就放弃添加, 如果不相同, 则添加到最后
5. 在 Java8 中, 如果一条链表的元素个数超过 TREEIFY_THRESHOLD ( 默认是 8 ), 并且 table 的大小 (  自定义名称 ) >= MIN_TREEIFY_CAPACITY ( 默认64 ), 就会进行树化 ( 红黑树 )

#### 添加一个元素的流程

**先进行 hash 运算得到它的 hash 值, 然后转成索引, 判断这个 HashMap 中对应索引的链表中有没有这个 元素, 调用这个元素的 equals 方法比较, 如果有这个元素, 就放弃添加, 如果没有这个元素, 就放入这个链表最后**

#### 源码解析

~~~java
// 源码调用流程分析
// 调用 add 方法 
public boolean add(E e) {
    return map.put(e, PRESENT)==null;
}
// add 中 调用的 put 方法
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

// 真正操作的方法
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i; //定义辅助变量
    // table 就是 HashMap 的一个属性, 类型是Node[]
    // if 语句表示如果当前 table 是 null, 或者大小==0
    // 就是第一次扩容, 到 16 空间
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    // (1) 根据 key 得到的 hash, 去计算该 key 应该存放到 table 表的哪个索引位置并把这个位置的对象, 赋给 p
    // (2) 判断 p 是否为 null
    // (2.1) 如果 p 为 null, 表示还没有存放元素, 就创建一个 Node ( key='传进来的值', value=PRESENT ) 就放在该位置 tab[i] = newNode(hash, key, value, null);
    
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        //(2.2) 如果 p 不为 null, 并且满足下面两个条件之一, 就不能加入
        Node<K,V> e; K k;
        //(2.2.1) 如果当前索引位置对应链表第一个元素和转呗添加的 key 的 hash 值一样同时准备加入的 key 和当前链表第一个的 key 是同一个对象
        //(2.2.2) 或者 key 内容不为空, 并且和当前链表第一个的 key 调用 equals 方法返回相同 ( 即eauals方法返回 true )
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        //(2.3) 判断 p 是不是一颗红黑树
        //(2.3.1) 如果是一颗红黑树, 就调用 putTreeVal, 来进行添加
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        //(2.4) 如果 table 对应索引位置已经是一个链表了, 就是用 for 循环从链表第二个开始依次和该链表的每一个元素进行比较,因为上面第一个 if 已经比较过第一个元素了
        //(2.4.1) 如果都不相同,说明添加的元素没有重复,就会加入到链表的最后,添加到链表后,会立即判断该链表是否已经达到了 8 个节点, 如果达到了 8 个节点, 就会调用 treeifybin 方法对当前链表进行树化(转成红黑树), 注意: 在转成红黑树时, 还进行了一层判断, 判断了当前tbale 数组为空, 或者小于等于64的话, 并不会马上变成树, 而是先对 table 数组进行扩容,只有当表的数量大于 64 的时候才会进行红黑树的转换
        //(2.4.2) 如果有相同情况, 就直接 break
        else {
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ++modCount;
    // size 就是没加入一个节点 Node(k,v,h,next), size++
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    return null;
}
~~~

### HashSet ( HashMap ) 底层机制说明

- 扩容机制

1. HashSet 底层是 HashMap, 第一次添加时, table 数组扩容到 16, 临界值( threshold ) 是 16 * 加载因子 ( loadFactor = 0.75 ) = 12
2. 如果 table 数组使用到了临界值 12, 就会扩容到 16 * 2 = 32, 新的临界值就是 32 * 0.75 = 24, 以此类推
3. 在 Java8 中, 如果一条链表的元素个数到达 TREEIFY_THRESHOLD ( 默认是8 )的时候, 如果 table 的大小 >= MIN_TREEIFY_CAPACITY ( 默认是 64 ), 就会进行树化 ( 红黑树 ), 否则会采用数组扩容的机制, 先去扩容数组

### LinkedHashSet

#### LinkedHashSet 的介绍

1. LinkedHashSet 是 HashSet 的子类
2. LinkedHashSet 底层是一个 LinkedHashMap, 底层维护了一个 数组 + 双向链表
3. LinkedHashSet 根据元素的 hashCode 值来决定元素的存储位置, 同时使用链表维护元素的次序, 这使得元素看起来是以插入顺序保存的
4. LinkedHashSet 不允许添加重复元素

##### LinkedHashSet 的全面说明

1. 在 LinkedHashSet 中维护了一个 hash 表和双向链表 ( LinedHashSet 有 head 和 tail )

2. 每一个节点有 before 和 after 属性, 这样可以形成双向链表

3. 在添加一个元素时, 先求 hash 值, 在求索引, 确定该元素在 table 的位置, 然后将添加的元素加入到双向链表 ( 如果已经存在, 不添加 [ 原则和 hashset 一样 ] )

   ~~~java
   tail.next = newElement; // 简单指定
   newElement.pre = tail;
   tail = newElement;
   ~~~

4. 这样的话, 我们遍历 LinkedHashSet 也能确保插入顺序和遍历顺序一致

##### 解读

1. LinkedHashSet 加入顺序和取出元素/数据的顺序一致

2. LinkedHashSet 底层维护的是一个 LinkedHashSet ( 是 HashMap 的子类 )

3. LinkedHashSet 底层结构 ( 数组 table + 双向链表 )

4. 添加第一次时, 直接将 数组 table 扩容到 16, 存放的节点类型是 LinkedHashMap$Entry

5. 数组是 HashMap$Node[], 但是存放的元素/数据是 LinkedHashMap$Entry 类型, 这说明 LinkedHashMap$Entry 类型应该是 HashMap$Node 的子类

   **LinkedHashMap$Entry 的三个参数**

   - before ---> prev
   - after ---> next
   - entry ---> item

   **源码调用的还是 HashSet 的一样**

   ### TreeSet

   #### 解读

   1. 当我们使用无参构造器创建 TreeSet 的时候仍然是无序的

   2. 当我们需要添加的元素按照字符串大小来排序的话，可以使用 TreeSet 的另一个构造器，可以传入一个  Comparator 比较器，并指定排序规则

      1. 构造器吧传入的比较器对象，赋给了 TreeMap 的属性 comparator
      2. 在调用 add 方法时，在底层会执行这个匿名内部类的 compare 方法 

      ~~~java
      TreeSet treeSet = new TreeSet(new Comparator(){
          @Override
          public int compare(Object o1, Object o2){
              return ((String) o1).compareTo((String) o2)
          }
      })
      ~~~

   3. TreeSet 的底层调用的是 TreeMap

## Map 接口

### Map的特点

**注意: 这里讲的是 JDK8 的 Map 接口特点**

1. Map 与 Collection 并列存在. 用于保存具有映射关系的数据, Key - Value
2. Map 中的 key 和 value 可以是任何引用类型的数据, 会封装到 HashMap$Node 对象中
3. Map 中的 key 不允许重复. 原因和 HashSet 一样, 前面分析过源码
4. Map 中的 value 可以重复
5. Map 中的 key 可以为 null, value 也可以为 null, 注意 key 为 null, 只能有一个, value 为 null, 可以多个
6. 常用 String 类作为 Map 的 key
7. key 和 value 之间存在单向一对一关系, 即通过指定的 key 总能找到对应的 value
8. Map 存放数据的 key - value 示意图, 一对 k - v 是放在一个 Node 中的, 又因为 Node 实现了 Entry 接口, 也可以说, 一对 k - v 就是一个 Entry

### 解读

1. k - v 最后是 HashMap$Node node = newNode ( hash, key, value, null )

2. k - v 为了方便程序员的遍历,还会创建 EbtrySet 集合, 该集合放的元素类型是 Entry, 而一个 Entry 对象就有 k, v, EntrySet<Entry<K, V>>, transient Set<Map.Entry<K, V>> entrySet

3. ebtrySet 中, 定义的类型是 Map.Entry, 但是实际上存放的还是 HashMap$Node, 这是因为 HashMap$Node implements Map.Entry

4. 当把 HashMap$Node 对象存放到 entrySet 就方便我们的遍历, 因为 Map.Entry 提供了重要方法 K getKey ( ); V getVakye ( );

   ~~~java
   Map map = new HashMap();
   map.put("no1", "xx");
   map.put("no2", "xxx")
   
   Set set = map.entrySet(); // HashMap$EntrySet
   for(Object obj : set){
       Map.Entry entry = (Map.Entry) obj;
       entry.getKey(); // no1, no2
       entry.getValue(); // xx, xxx 
   }
   
   ~~~


### Map 的常用方法

1. put    添加
2. remove    根据键删除映射关系
3. get    根据键获取值
4. size    获取元素个数
5. isEmpty    判断个数是否为0
6. clear    清楚
7. containsKey    查找键是否存在

### Map 的遍历方法

2. keySet:    获取所有的键
3. entrySet:    获取所有关系 k - v
4. values:    获取所有的值

**是用这三种方法, 每个方法都有两种方式**

~~~java
Map map = new HashMap();
map.put("no1", "xx");
map.put("no2", "xxx");
    
//1. 先取出所有的 key, 然后通过 key 去除对应的 value
Set keyset = map.keySet();
//1.1 增强 for
for(Object key : keyset){
    key; // 这个就是每一个 key
    map.get(key); // 这个就是每一个 value
}

//1.2 迭代器
Iterator iterator keyset.interator();
while(iterator.hasNext()){
    Object key = iterator.next();
    map.getKey(key);
}

//2. 把所有的 value 取出
Collection values = map.values();
// 这里可以使用所有的 Collection 使用的遍历方法
// 2.1. 增强 for  2.2. 迭代器

//3. 通过 EntrySet 来获取 k - v
Set entrySet = map.entrySet(); // EntrySet<Map.Entry<K, V>>
// 3.1 增强 for
for(Object entry : entrySet){
    //将 entry 转成 Map.Entry
    Map.Entry m = (Map.Entry) entry;
    m.getKey();
    m.getValue();
}

// 3.2 迭代器
interator interator2 = entrySet.iterator();
while(iterator2.hasNext()) {
    Object next = iterator2.next();
    // 这里的 next 运行类型是 HashMap$Node 类型, 因为其实现了 Map.Entry, 所以可以使用 getKey 和 getValue 方法
    // 因为 Object 没有 getKey 和 getValue 方法, 所以要进行向下转型
    Map.Entry m = (Map.Entry) entry;
}

~~~

### TreeMap

#### 解读

1. 使用默认构造器创建 TreeMap 是无序的
2. 其他和 TreeSet 基本一致， 这里的比较器比较的是 key

### 小结

1. Map 接口的常用实现类 HashMap、HashTable、Properties
2. HashMap 是 Map 接口使用频率最高的实现类
3. HashMap 是以 key - value 对的方式来存储数据
4. key 不能重复, 但是是值可以重复, 允许使用 null 键 和 null 值
5. 如果添加相同的 key, 则会覆盖原来的 key - value, 等同于修改, ( key 不会替换, value 会替换 )
6. 与 HashSet 一样, 不保证映射的顺序, 因为底层是以 hash 表的方式来存储的
7. HashMap 没有实现同步, 因此是线程不安全的

### HashTable 

#### HashTable  的基本介绍

1. 存放的元素是键值对： 即 k -v
2. HashTable 的键和值都不能为 null，否则会抛出 NullPointerException
3. HashTable 使用方法基本上和 HashMap 一样
4. HashTable 是线程安全的， HashMap 是线程不安全的

#### HashTable  的底层介绍

1. 底层有数组 HashTable$Entry[] 初始化大小为 11

2. 临界值 threshold 8 = 11 * 0.75

3. 扩容 按照自己的扩容机制来进行即可

   ~~~java
   //3.1 执行方法 addEntry(hash, key, value, index);
   //3.2 如果大于临界值 就按照两倍加一的方式扩容
   ~~~
   

#### Properties

##### Properties 的基本介绍

1. Properties 类继承自 HashTable 类并且实现了 Map 接口， 也是使用一种键值对的形式来保存数据
2. 它的使用特点和 HashTable 类似
3. Properties 还可以用于从 xxx.properties 文件中， 加载数据到 Properties 类对象， 并进行读取和修改
4. 说明：工作后 xxx.properties 文件通常作为配置文件

##### 解读

1. Properties 继承自 HashTable
2. 可以通过 k - v 存放数据，当然 key 和 value 不能为 null

### HashTable 和 HashMap 对比

|          | 版本 | 线程安全 ( 同步 ) | 效率 | 允许 null |
| -------- | ---- | ----------------- | ---- | --------- |
| HashMap  | 1.2  | 不安全            | 高   | 可以      |
| Hshtable | 1.0  | 安全              | 较低 | 不可以    |

   ## 集合选型规则

1. 判断存储的类型（ 一组对象【单列】或一组键值对【双列】 ）
   * 一组对象【单列】：Collection 接口
     - 允许重复：List
       - 增删多：LinedList【底层维护了一个双向链表】
       - 改查多：ArrayList【底层维护 Object 类型的可变数组】
     - 不允许重复：Set
       - 无序：HashSet【底层是 HashMap，维护了一个哈希表，即 ( 数组 + 链表 + 红黑树 ) 】
       - 排序：TreeSet
       - 插入和取出顺序一致：LinkedHashSet【底层是 LinkedHashMap】，维护数组 + 双向链表
   * 一组键值对【双列】：Map
     - 键无序：HashMap【底层是：哈希表，jdk1：数组 + 链表， jdk8：数组 + 链表 + 红黑树】
     - 键排序：TreeMap
     - 键插入和取出顺序一致：LinkedHashMap【底层是 HashMap】
     - 读取文件：Properties

## 工具类

### Collections 

#### Collections 介绍

1. Collections 是一个操作 Set、List 和 Map 等集合的工具类
2. Collections 中提供了一系列静态的方法对集合元素进行排序、查询和修改等操作

#### 排序操作：此处均为 static 方法

1. reverse ( List )：翻转 List 中元素的顺序
2. shuffle ( List )：对 List 集合元素进行随机排序
3. sort ( List )：根据元素的自然顺序对指定 List 集合元素按升序排序
4. sort ( List, Comparator )：根据指定的 Comparator 产生的顺序对 List 排序
5. swap ( List, int, int )：将指定 List 集合中的 i 处元素和 j 处元素进行交换
6. Object max ( Collection )：根据元素的自然顺序，返回给定集合中的最大元素
7. Object max ( Collection, Comparator )：根据 Comparator 指定的顺序，返回给定集合中的最大元素
8. Object min ( Collection )：同上，返回最小元素
9. Object min ( Collection, Comparator )：同上
10. int fequency ( Collection， Object )：返回指定集合中指定元素的出现次数
11. void copy ( List dest, List src )：将 src 中的内容复制到 dest 中
12. boolean replaceAll ( List list, Object oldVal, Object newVal )：使用新值替换 List 对象的所有旧值


## 分析

1. HashSet 的去重机制: hashCode + equals, 底层先通过存入对象