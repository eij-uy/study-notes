[toc]

# Java 多线程
## 进程与线程的概念

- 进程

1. 进程是指运行中的程序,比如我们使用 QQ,就启动了一个进程,操作系统就会为该进程分配内存空间.当我们使用迅雷,又启动了一个进程,操作系统将为迅雷分配新的内存空间
2. 进程是程序的一次执行过程,或是正在运行的一个程序.是动态过程:有它资深的产生、存在和消亡的过程

- 线程

1. 线程由进程创建,是进程的一个实体
2. 一个进程可以拥有多个线程

## 线程

- 单线程

  同一个时刻,只允许执行一个线程

- 多线程

  同一个时刻,可移植性多个线程, 比如: 一个 QQ 进程, 可以同时打开多个聊天窗口, 一个迅雷进程,可以同时下载多个文件

- 并发

  同一个时刻, 多个任务交替执行, 造成一种 貌似同时 的错觉, 简单的说, 单核 cpu 实现的多任务就是并发

- 并行

  同一个时刻, 多个任务同时执行, 多核 cpu 可以实现并行

### 创建线程的两种方式

1. 继承 Thread 类, 重写 run 方法

   ~~~java
   package com.yujie.homework;
   
   /**
    * @author 余杰
    * @version 1.0
    */
   public class ThreadTest01 {
     public static void main(String[] args) throws InterruptedException {
       // 创建 Cat01 对象, 可以当做线程使用
       Cat01 cat01 = new Cat01();
       // 启动线程
       cat01.start();
       // 说明: 当 main 线程启动了一个子线程 Thread-0, 主线程不会阻塞, 会继续执行
       System.out.println("主线程名称" + Thread.currentThread().getName());
       for(int i = 0; i < 100; i++){
         System.out.println("主线程i=" + i);
         Thread.sleep(1000);
       }
     }
   }
   
   // 1. 当一个类继承了 Thread 类, 该类就可以当做线程使用
   // 2. 我们会重写 run 方法, 写上自己的业务代码
   // 3. run Thread 类实现了 Runnable 接口的 run 方法
   class Cat01 extends Thread {
     @Override
     public void run() {
       // 该线程每隔一秒输出 "xlsx"
       while (true){
         System.out.println("xlsx 子线程名称" + Thread.currentThread().getName());
         try {
           Thread.sleep(1000);
         } catch (InterruptedException e) {
           throw new RuntimeException(e);
         }
       }
     }
   }
   
   ~~~

2. 实现 Runnable 接口, 重写 run 方法

   **说明**

   1. java 是单继承的, 在某些情况下一个类可能已经继承了某个父类, 这时在用继承 Thread 类方法来创建线程显然不可能了
   2. java 设计者们提供了另外一个方式创建线程, 就是通过实现 Runnable 接口来创建线程

   ~~~java
   package com.yujie.homework;
   
   /**
    * @author 余杰
    * @version 1.0
    */
   public class RunnableTest {
     public static void main(String[] args) {
       Dog01 dog01 = new Dog01();
       // 这里不能调用 Dog.start, 因为 Runnable 接口没有实现 start 方法
       // 要这样操作
       Thread thread = new Thread(dog01);
       thread.start();
     }
   }
   
   // 通过实现 Runnable 接口, 开发线程
   class Dog01 implements Runnable {
     int count = 0;
     @Override
     public void run() {
       while (true) {
         System.out.println("小狗汪汪叫...hi" + (++count) + Thread.currentThread().getName());
         try {
           Thread.sleep(1000);
         } catch (InterruptedException e) {
           throw new RuntimeException(e);
         }
       }
     }
   }
   ~~~

   **这里之所以可以直接把 dog01 对象传给 Thread 类, 是因为采用了静态代理的设计模式**

### 代理模式模拟

~~~java
// 线程代理类, 模拟了一个简单的 Thread 类
class ThreadProxy implements Runnable {
  private Runnable target = null;
  @Override
  public void run(){
    if(target != null){
      target.run();
    }
  }
  
  public ThreadProxy(Runnable target){
    this.target = target;
  }
  
  public void start(){
    start0(); // 这是底层中真正实现了 多线程 的方法
  }
  public void start0(){...}
}
~~~

### 继承 Thread 和 实现 Runnable 的区别

1.  从 java 的设计来看, 通过继承 Thread 或实现 Runnable 接口来创建线程本质上没有区别, 从 jdk 帮助文档我们可以看到 Thread 类本身就实现了 Runnable 接口 start -> start0()

2. 实现 Runnable 接口方式更加适合多个线程共享一个资源的情况, 并且避免了单继承的限制, 建议使用 Runnable 接口

      ~~~java
   T3 t3 = new T3();
   // t3可以被多个线程使用
   Thread thread1 = new Thread(t3);
   Thread thread2 = new Thread(t3);
   
   thread.start(); 
   thread.start();
   
   ~~~

### 线程终止

#### 基本说明

   1. 当线程完成任务后,线程自动退出
   2. 还可以通过使用变量来控制 run 方法退出的方式停止线程, 即通知方式
### 线程常用方法

| 方法名称    | 方法作用                                                  | 注意事项和细节                                               |
| ----------- | --------------------------------------------------------- | ------------------------------------------------------------ |
| setName     | 设置线程名称,使之与参数 name 相同                         |                                                              |
| getName     | 返回该线程的名称                                          |                                                              |
| start       | 使该线程开始执行; Java 虚拟机底层调用该线程的 start0 方法 | start底层会创建新的线程, 调用 run                            |
| run         | 调用线程对象 run 方法                                     | run 就是一个简单的方法调用, 不会启动新线程                   |
| setPriority | 更改线程的优先级                                          | 三个常用的优先级 MIN_PRIORITY, NORM_PRIORUTY, MAX_PRIORITY   |
| getPriority | 获取线程的优先级                                          | 三个常用的优先级 MIN_PRIORITY, NORM_PRIORUTY, MAX_PRIORITY   |
| sleep       | 在指定的毫秒数内让当前正在执行的线程休眠 ( 暂停执行 )     | 线程的静态方法, 使当前线程休眠                               |
| interrupt   | 中断线程                                                  | 中断线程, 但并没有真正的结束线程, 所以一般用于中断正在休眠的线程 |
| yield       | 线程的礼让,让出 cpu, 让其他线程执行                       | 礼让的时间不确定, 所以也不一定礼让成功                       |
| join        | 线程的插队                                                | 插队的线程一旦插队成功, 则肯定先执行完插入的线程所有的任务   |

### 用户线程和守护线程

- 用户线程: 也叫工作线程, 当线程的任务执行完或通知方式结束
- 守护线程: 一般是为工作线程服务的, 当所有的用户线程结束, 守护线程自动结束
- 常见的守护线程: 垃圾回收机制

**如何将一个线程设置成守护线程**

~~~java
class MyDaemonThread extends Thread {
  public void run(){
    for(;;) { // 无限循环
      try {
        Thread.sleep(100)
      } catch(InterruptedException e) {
        e.printStackTrack()
      }
      System.out.println("xx和xx 正在快乐的聊天, 哈哈哈")
    }
  }
}

MyDaemonThread daemon = MyDaemonThread();
// 如果我们虚妄 main 线程结束后, 子线程自动结束
// 只需要将子线程设为守护线程
daemon.setDaemon(true); // 设为守护线程
daemon.start();
~~~

### 线程的生命周期

#### 线程的七个状态

1. NEW 

   尚未启动的线程处于此状态

2. RUNNABLE 

   **在 Java 虚拟机中执行的线程处于此状态, 这个状态可以被细分为两个状态, 这里是内核态**

   2. Ready

      已经可以执行了, 什么时候执行看线程什么时候被调度器选中执行

   3. Running

      正在执行中

4. BLOCKED

   被阻塞等待监视器锁定的线程处于此状态

5. WAITING

   正在等待另一个线程执行特定动态的线程处于此状态

6. TIMED_WAITING

   正在等待另一个线程执行动作达到制定等待时间的线程处于此状态

7. TREMINATED

   已退出的线程处于此状态

#### 线程同步机制 

在多线程编程, 一些蜜柑数据不允许被多个线程同时访问, 此时就使用同步访问技术, 保证数据在任何同一时刻, 最多有一个线程访问, 以保证数据的完整性

线程同步: **即当有一个线程在对内存进行操作时, 其他线程都不可以对这个内存地址进行操作, 知道该线程完成操作, 其他线程才能对该内存地址进行操作**

##### 同步具体方法 - Synchronized

1. 同步代码块

   ~~~java
   synchronized ( 对象 ) { // 获取对象的锁, 才能操作同步方法
   	// 需要被同步的代码
   }
   ~~~

2. synchronized 还可以放到方法声明中, 表示整个方法-为同步方法

   ~~~java
   public synchronized void m (String name){
     // 需要被同步的代码
   }
   ~~~

### 互斥锁

#### 基本介绍

1. Java 语音中, 引入了对象互斥锁的概念, 来保证共享数据操作的完整性
2. 每个对象都对应于一个可称为互斥锁的标记, 这个标记用来保证在任一时刻, 只能有一个线程访问对象
3. 关键字 `synchronized` 来与对象的互斥锁联系, 当某个对象用 `synchronized` 修饰时, 表明该对象在任一时刻只能由一个线程访问
4. 同步的局限性: 导致程序的执行效率要降低
5. 同步方法 ( 非静态的 ) 的锁可以是 this, 也可以是其他对象 ( 要求是同一个对象 )
6. 同步方法 ( 静态 ) 的锁为当前类本身

~~~java
package com.yujie.homework;

/**
 * @author 余杰
 * @version 1.0
 */
public class SellTicket {
  public static void main(String[] args) {
    Sell sell = new Sell();
    Thread thread1 = new Thread(sell);
    Thread thread2 = new Thread(sell);
    Thread thread3 = new Thread(sell);
    thread1.start();
    thread2.start();
    thread3.start();
  }
}

class Sell implements Runnable {
  private static int tickets = 100; //票数
  //非静态对象
  // 1. 在方法上加 synchronized
  // @Override
  //  public synchronized void run() {
  //    while (true){
  //      if(tickets <= 0) {
  //        System.out.println("售票结束");
  //        break;
  //      };
  //      System.out.println("卖出一张,剩余票数" + (--tickets) + Thread.currentThread().getName());
  //      try {
  //        Thread.sleep(100);
  //      } catch (InterruptedException e) {
  //        e.printStackTrace();
  //      }
  //    }
  //  }
  // 2. 在代码块上加上 synchronized
  @Override
  public void run() {
    // 2.1 这里的 this 只要保证是同一个对象就可以
    synchronized (this){
      while (true){
        if(tickets <= 0) {
          System.out.println("售票结束");
          break;
        };
        System.out.println("卖出一张,剩余票数" + (--tickets) + Thread.currentThread().getName());
        try {
          Thread.sleep(100);
        } catch (InterruptedException e) {
          e.printStackTrace();
        }
      }
    }
  }

  // 如果是静态方法
  // 1. 在方法上增加 synchronized
  public synchronized static void ceshi1(){
    //...
  }
  // 2. 使用代码块, 代码块的()中放的对象可以是 Sell.class
  public static void ceshi2(){
    synchronized (Sell.class) {
      //...
    }
  }
}

~~~

#### 注意事项和细节

1. 同步方法如果没有使用 static 修饰: **默认锁对象为 this**
2. 如果方法使用 static 修饰: **默认锁对象为 当前类.class**
3. 实现的落地步骤:
   1. 需要先分析上锁的代码
   2. 选择同步代码块或同步方法
   3. 要求多个线程的锁对象为同一个即可

### 线程死锁

#### 基本介绍

**多个线程都占用了对方的锁资源, 但不肯相让, 导致了死锁, 在编程是一定要避免死锁的发生**

#### 案例

~~~java
class DeadLockDemo extends Thread {
  static Object o1 = new Object();
  static Object o2 = new Object();
  boolean flag;
  
  public DeadLockDemo(boolean flag){
    this.flag = flag;
  }
  
  public void run(){
    // 下面业务逻辑的分析
    // 1. 如果 flag 为 true, 线程A 就会先得到/持有 o1 对象锁, 然后尝试去获取 o2 对象锁
    // 2. 如果线程A 得不到 o2 对象锁, 就会 Blocked
    // 3. 如果 flag 为 false, 线程B 就会先得到/持有 o2 对象锁, 然后尝试去获取 o1 对象锁
    // 4. 如果线程B 得不到 o1 对象锁, 就会 Blocked
    if(flag){
      synchronized (o1){ //对象互斥锁, 下面就是同步代码
        System.out.println(Thread.currentThread().getName() + "进入1");
        synchronized (o2){ // 这里获得 li 对象的监视权
        	System.out.println(Thread.currentThread().getName() + "进入2");
        }
      }
    }else{
      synchronized (o2){
        System.out.println(Thread.currentThread().getName() + "进入3");
        synchronized (o1){ // 这里获得 li 对象的监视权
        	System.out.println(Thread.currentThread().getName() + "进入4");
        }
      }
    }
  }
}

// 模拟死锁现象
DeadLockDemo A = new DeadLockDemo(true);
A.setName("A");
DeadLockDemo B = new DeadLockDemo(false);
B.setName("B");
A.start();
B.start();
// 这样就会造成死锁 A线程想拿到 B线程拿到的锁, B线程想拿到 A线程拿到的锁
~~~

### 释放锁

1. 当前线程的同步方法、同步代码块执行结束
2. 当前线程在同步代码块、同步方法中遇到 break、return
3. 当前线程在同步代码块、同步方法中出现了未处理的 Error 或 Exception, 导致异常结束
4. 当前线程在同步代码快、同步方法中执行了线程对象的 wait 方法, 当前线程暂停, 并释放锁

#### 下面操作不会释放锁

1. 线程执行同步代码块或同步方法时, 程序调用 Thread.sleep、Thread.yield 方法暂停当前线程的执行, 不能释放锁
2. 线程执行同步代码块时, 其他线程调用了该线程的 suspend 方法将该线程挂起, 该线程不会释放锁

**应尽量避免使用 suspend 和 resume 来控制线程, 方法不再推荐使用**