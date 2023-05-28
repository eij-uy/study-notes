[toc]

# Java - JUnit单元测试类

## 为什么需要 JUnit

1. 一个类有很多功能代码需要测试，为了测试，就需要写入到 main 方法中
2. 如果有多个功能代码测试，就需要来回注销，切换很麻烦
3. 如果可以直接运行一个方法，就方便很多，并且可以给出相关信息就好了  ---> JUnit

## 基本介绍

1. JUnit 是一个 Java 语言的单元测试框架
2. 多数 Java 的开发环境都已经继承了 JUnit 作为单元测试的工具

~~~java
public class JUnitTest {
    public static void main(String[] args){
        // 传统方式
        new JUnitTest.m1();
        new JUnitTest.m2();
        
        
    }
    // 加上 Test 并使用快捷键 alt + enter 然后选择第二个
    @Test
    public void m1(){
        System.out.println("m1方法被调用");
    }
    public void m2(){
        System.out.println("m2方法被调用");
    }
}
~~~

