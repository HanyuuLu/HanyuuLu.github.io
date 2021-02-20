---
title: Java反射机制备忘
date: 2020-03-29 17:14
categories:
	- programming
tags:
	- Java
	- reflection
---

# 概述

> 反射的概念是由Smith在1982年首次提出的，主要是指程序可以访问、检测和修改它本身状态或行为的一种能力，通过反射可以调用私有方法和私有属性，大部分框架也都是运用反射原理的。java通常是先有类再有对象，有对象就可以调用方法或者属性，java中的反射其实是通过Class对象来调用类里面的方法。
>
> 主要是指程序可以访问，检测和修改它本身状态或行为的一种能力，并能根据自身行为的状态和结果，调整或修改应用所描述行为的状态和相关的语义。
>
> 一个类有多个组成部分，例如：成员变量、方法、构造方法等，反射就是加载类,并解剖出类的各个组成部分。

* 反射主要提供的功能

  * 在运行时判断任意一个对象所属的类
  * 在运行时构造任意一个类的对象
  * 在运行时判断任意一个类所具有的成员变量和方法
  * 在运行时调用任意一个对象的方法
  * 生成动态代理

* 优点

  * 能够运行时动态获取类的实例，大大提高系统的灵活性和扩展性
  * 与Java动态编译相结合，可以更多功能

* 缺点

  * 性能较低
  * 不安全
  * 破坏了类的封装性

* 相关类

  ``` java
  java.lang.Class;
  java.lang.reflect.Constructor;
  java.lang.reflect.Field;
  java.lang.reflect.Method;
  java.lang.reflect.Modifier;
  ```

## 通过对象获取保证的包名和类名

文件结构

```
C:.
└─Hanyuu
        Demo.java
```

Demo.java

``` java
package Hanyuu;
class Demo{
    public static void main(String[] args) {
        Demo demo = new Demo();
        System.out.println(demo.getClass().getName());
    }
}
```

输出

```
Hanyuu.Demo
```

## 实例化类对象

Demo.java

``` java
package Hanyuu;
class Demo{
    public static void main(String[] args) throws ClassNotFoundException {
        Demo demo = new Demo();
        Class<?> fornameClass = null;
        Class<?> getClass = null;
        Class<?> dotClass = null;
        // throws ClassNotFoundException
        fornameClass = Class.forName("Hanyuu.Demo");
        getClass = new Demo().getClass();
        dotClass = Demo.class;
        System.out.println(fornameClass.getName());
        System.out.println(getClass.getName());
        System.out.println(dotClass.getName());
    }
}
```

输出

```
Hanyuu.Demo
Hanyuu.Demo
Hanyuu.Demo
```

## 获取父类对象



# Reference

* [JAVA反射](https://www.jianshu.com/p/c615ebdefc75)
* [java反射机制深入理解剖析](https://www.w3cschool.cn/java/java-reflex.html)

