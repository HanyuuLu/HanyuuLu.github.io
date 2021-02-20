---
title: Spring生态学习笔记
date: 2020-07-13 00:00:00
tags:
  - Spring
  - Java
---

[toc]

# 概览

## 体系结构

![img](springLearn/arch1.png)

## 核心容器

核心容器由核心，Bean，上下文和表达式语言模块组成，它们的细节如下： 

- **核心**模块提供了框架的基本组成部分，包括 IoC 和依赖注入功能。
- **Bean** 模块提供 BeanFactory，它是一个工厂模式的复杂实现。
- **上下文**模块建立在由核心和 Bean 模块提供的坚实基础上，它是访问定义和配置的任何对象的媒介。ApplicationContext 接口是上下文模块的重点。
- **表达式语言**模块在运行时提供了查询和操作一个对象图的强大的表达式语言。

## 数据访问/集成

数据访问/集成层包括 JDBC，ORM，OXM，JMS 和事务处理模块，它们的细节如下： 

- **JDBC** 模块提供了删除冗余的 JDBC 相关编码的 JDBC 抽象层。
- **ORM** 模块为流行的对象关系映射 API，包括 JPA，JDO，Hibernate 和 iBatis，提供了集成层。
- **OXM** 模块提供了抽象层，它支持对 JAXB，Castor，XMLBeans，JiBX 和 XStream 的对象/XML 映射实现。
- Java 消息服务 **JMS** 模块包含生产和消费的信息的功能。
- **事务**模块为实现特殊接口的类及所有的 POJO 支持编程式和声明式事务管理。

## Web

Web 层由 Web，Web-MVC，Web-Socket 和 Web-Portlet 组成，它们的细节如下： 

- **Web** 模块提供了基本的面向 web 的集成功能，例如多个文件上传的功能和使用 servlet 监听器和面向 web 应用程序的上下文来初始化 IoC 容器。
- **Web-MVC** 模块包含 Spring 的模型-视图-控制器（MVC），实现了 web 应用程序。
- **Web-Socket** 模块为 WebSocket-based 提供了支持，而且在 web 应用程序中提供了客户端和服务器端之间通信的两种方式。
- **Web-Portlet** 模块提供了在 portlet 环境中实现 MVC，并且反映了 Web-Servlet 模块的功能。

## 其他

还有其他一些重要的模块，像 AOP，Aspects，Instrumentation，Web 和测试模块，它们的细节如下：

- **AOP** 模块提供了面向方面的编程实现，允许你定义方法拦截器和切入点对代码进行干净地解耦，它实现了应该分离的功能。
- **Aspects** 模块提供了与 **AspectJ** 的集成，这是一个功能强大且成熟的面向切面编程（AOP）框架。
- **Instrumentation** 模块在一定的应用服务器中提供了类 instrumentation 的支持和类加载器的实现。
- **Messaging** 模块为 STOMP 提供了支持作为在应用程序中 WebSocket 子协议的使用。它也支持一个注解编程模型，它是为了选路和处理来自 WebSocket 客户端的 STOMP 信息。
- **测试**模块支持对具有 JUnit 或 TestNG 框架的 Spring 组件的测试。

# IOC

## BeanFactory容器

`HelloWorld.java`

```java
package com.hanyuu;
public class HelloWorld {
   private String message;
   public void setMessage(String message){
    this.message  = message;
   }
   public void getMessage(){
    System.out.println("Your Message : " + message);
   }
}
```

`Main.java`

```java
package com.hanyuu;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.xml.XmlBeanFactory;
import org.springframework.core.io.ClassPathResource;
public class MainApp {
   public static void main(String[] args) {
      XmlBeanFactory factory = new XmlBeanFactory
                             (new ClassPathResource("Beans.xml"));
      HelloWorld obj = (HelloWorld) factory.getBean("helloWorld");
      obj.getMessage();
   }
}
```

`Beans.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <bean id="helloWorld" class="com.hanyuu.HelloWorld">
       <property name="message" value="Hello World!"/>
   </bean>

</beans>
```



## ApplicationContent容器

* ApplicationContent 接口实现
  * **FileSystemXmlApplicationContext**：使用文件完整路径加载bean
  * **ClassPathXmlApplicationContext**：在ClassPath中搜索（只需要提供文件名）
  * **WebXmlApplicationContext**：该容器会在一个 web 应用程序的范围内加载在 XML 文件中已被定义的 bean。

`Student.java`

``` java
package com.hanyuu.test;

import lombok.Data;

@Data
public class Student {

   private Integer age;
   private String name;
}
```

`DemoApplication.java`

``` java
package com.hanyuu.test;

import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.util.Date;

import javax.sound.midi.SysexMessage;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.context.support.FileSystemXmlApplicationContext;

@SpringBootApplication
public class DemoApplication {
	public static void main(final String[] args) {		
		final ApplicationContext applicationContext = new FileSystemXmlApplicationContext("beans.xml");
		final Student student = (Student) applicationContext.getBean("student");

		System.out.println("Name : " + student.getName());
		System.out.println("Age : " + student.getAge());
	}

```

`beans.xml`

``` xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.0.xsd">

   <context:annotation-config/>
   <!-- bean definitions go here -->

   <!-- Definition for student bean -->
   <bean id="student" class="com.hanyuu.test.Student">
      <property name="name"  value="Hanyuu" />
      <property name="age"  value="3"/>
   </bean>
</beans>
```

# Bean 定义

| 属性                     | 描述                                                                                                 |
| ------------------------ | ---------------------------------------------------------------------------------------------------- |
| class                    | 强制性，并且指定用来创建 bean 的 bean 类。                                                           |
| name                     | 指定唯一的 bean 标识符。在基于 XML 的配置元数据中，你可以使用 ID 和/或 name 属性来指定 bean 标识符。 |
| scope                    | 指定由特定的 bean 定义创建的对象的作用域                                                             |
| constructor-arg          | 注入依赖关系                                                                                         |
| properties               | 注入依赖关系                                                                                         |
| autowiring mode          | 注入依赖关系                                                                                         |
| lazy-initialization mode | 延迟初始化                                                                                           |
| initialization 方法      | 在 bean 的所有必需的属性被容器设置之后，调用回调方法                                                 |
| destruction 方法         | 当包含该 bean 的容器被销毁时，使用回调方法                                                           |

## 配置元数据来源

- 基于 XML 的配置文件。
- 基于注解的配置
- 基于 Java 的配置 

## xml样例

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <!-- A simple bean definition -->
   <bean id="..." class="...">
       <!-- collaborators and configuration for this bean go here -->
   </bean>

   <!-- A bean definition with lazy init set on -->
   <bean id="..." class="..." lazy-init="true">
       <!-- collaborators and configuration for this bean go here -->
   </bean>

   <!-- A bean definition with initialization method -->
   <bean id="..." class="..." init-method="...">
       <!-- collaborators and configuration for this bean go here -->
   </bean>

   <!-- A bean definition with destruction method -->
   <bean id="..." class="..." destroy-method="...">
       <!-- collaborators and configuration for this bean go here -->
   </bean>

   <!-- more bean definitions go here -->

</beans>
```



## 区别

1. 功能
   * BeanFactory提供了最简单的功能（实例化对象和获取对象）
   * ApplicationContent提供了MessageSource（国际化）、ResourceLoader（资源访问）、载入多个上下文、ApplicationEventPublisher（消息发送、响应机制）和AOP（拦截器）

2. 加载机制
   * BeanFactory懒加载
   * ApplicationContext启动时全部加载

## 作用域（Scope）

| 作用域         | 描述                                                                                                  |
| -------------- | ----------------------------------------------------------------------------------------------------- |
| singleton      | 该作用域将 bean 的定义的限制在每一个 Spring IoC 容器中的一个单一实例(默认)。                          |
| prototype      | 该作用域将单一 bean 的定义限制在任意数量的对象实例。                                                  |
| request        | 该作用域将 bean 的定义限制为 HTTP 请求。只在 web-aware Spring ApplicationContext 的上下文中有效。     |
| session        | 该作用域将 bean 的定义限制为 HTTP 会话。 只在web-aware Spring ApplicationContext的上下文中有效。      |
| global-session | 该作用域将 bean 的定义限制为全局 HTTP 会话。只在 web-aware Spring ApplicationContext 的上下文中有效。 |

# Reference

* https://wiki.jikexueyuan.com/project/spring/

* https://blog.csdn.net/pythias_/article/details/82752881