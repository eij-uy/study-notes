[toc]

# Java包

## 应用场景

现在又两个程序员共同开发一个java项目， 程序员小明希望定义一个类， 取名叫做Person， 程序员小青也想定义一个类叫Person， 这个时候就可以用到包

## 基本语法

package com.hspedu

说明

1. package 关键字， 表示打包
2. com.hspedu 表示报名

## 作用

1. 区分相同名字的类
2. 当类很多时， 可以很好的管理类 => 看java api文档
3. 控制访问范围

## 原理

包的本质就是创建不同的文件夹来保存类文件

## 命名

### 命名规则  => 只能这样写

只能包含数字、字母、下划线、小圆点， 但是不能用数字开头， 不能是关键字或者保留字

### 命名规范 => 推荐这样写

使用小写 + 小圆点    =>    com.公司名.项目名.业务模块名

比如： com.hspedu.oa.model;	com.hspedu.oa.controller

举例：

1. com.sina.crm.user //用户模块
2. com.sina.crm.order //订单模块
3. com.sina.crm.utils //工具类

## 常用的包

- java.lang.*

  lang 包是基本包，默认引入，不需要再引入

- java.util.*

  util 包，系统提供的工具包、工具类	=>	使用Scanner

- java.net.*

  网络包，网络开发

- java.awt

  是做java的界面开发，GUI

## 使用细节

### 如何引入包

#### 语法： import  包；

- 我们如一个包的主要目的是要使用该包下的类

- 比如import java.utils.Scanner; 就只是引入一个类Scanner      =>      建议使用这个方式

- import Java.util.*	=>	表示将java.util 包所有都引入

案例：

~~~java
import java.util.*;

public class Person {
    public static void main(String args){
        System.out.println(Math.PI);
        int arr[] = {1, -1, 100, 90, 70};
        Arrays.sort(arr);
        for(int i = 0; i < arr.length; i++){
            System.out,print(arr[i] + "\t");
        }
    }
}
~~~

### 注意事项

1. package 的作用是生命当前类所在的包， 需要放在类的最上面， 一个类最多只有一句package
2. import 指令位置放在package的下面， 在类定义前面可以有多句且没有顺序要求