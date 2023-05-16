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



