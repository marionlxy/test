# springboot源码解读

## 1、spring几个重要的发展时期

1.1 Spring 1.0的出现彻底改变了我们开发企业级Java应用程序的方式。**Spring的依赖注入与声明式事务**意味着组件之间再也不存在紧耦合，再也不用重量级的EJB了

1.2 Spring 2.0，我们可以在配置里使用自定义的XML命名空间，更小、更简单易懂的配置文件让Spring本身更便于使用. Spring 2.5让我们有了更优雅的面向注解的依赖注入模型（即@Component和@Autowired注解），以及面向注解的Spring MVC编程模型。不用再去显式地声明应用程序组件了，也不再需要去继承某个基础的控制器类了

1.3 到了Spring 3.0，我们有了一套基于Java的全新配置，它能够取代XML。在Spring 3.1里，一系列以@Enable开头的注解进一步完善了这一特性。终于，我们第一次可以写出一个没有任何XML配置的Spring应用程序了。这玩意儿不能更好了

1.4 Spring 4.0对条件化配置提供了支持，根据应用程序的Classpath、环境和其他因素，运行时决策将决定使用哪些配置，忽略哪些配置。那些决策不需要在构建时通过编写脚本确定了；

Spring的生态圈里正在出现很多让人激动的新鲜事物，涉及的领域涵盖云计算、大数据、无模式的数据持久化、响应式编程以及客户端应用程序开发,Spring Boot提供了一种新的编程范式，能在最小的阻力下开发Spring应用程序。有了它，你可以更加敏捷地开发Spring应用程序，专注于应用程序的功能，不用在Spring的配置上多花功夫，甚至完全不用配置。

## 2、springboot起步

>  前言：`一个项目结构，其中有一个包含必要依赖的Maven或者Gradle构建文件，最起码要有Spring`
>
> `MVC和Servlet API这些依赖。`
>
>  `一个web.xml文件（或者一个WebApplicationInitializer实现），其中声明了Spring`
>
> `的DispatcherServlet。`
>
> `一个启用了Spring MVC的Spring配置。`
>
>  `一个控制器类，以“Hello World”响应HTTP请求。`
>
>  `一个用于部署应用程序的Web应用服务器，比如Tomcat .`
>
> ？？？
>
> Spring Boot 精要
>
> 1.  自动配置：针对很多Spring应用程序常见的应用功能，Spring Boot能自动提供相关配置。
>
> 2.  起步依赖：告诉Spring Boot需要什么功能，它就能引入需要的库。
>
> 3.  命令行界面：这是Spring Boot的可选特性，借此你只需写代码就能完成完整的应用程序，无需传统项目构建。
>
> 4.  Actuator：让你能够深入运行中的Spring Boot应用程序，一探究竟。每一个特性都在通过自己的方式简化Spring应用程序的开发
>
>    

### 2.1 springboot创建项目

```
<!--springboot工程需要继承的父工程-->
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>2.1.5.RELEASE</version>
</parent>
```

```
<!--依赖web启动器-->
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

 pom依赖传递父parent声明，依赖传递管理，比如查看依赖树

```
mvn dependency:tree
```

## 3、springboot配置

### 3.1 首先了解@SpringBootApplication注解

### 3.2 内部配置文件加载顺序

- file:./config/：当前项目下的/config目录下

- file:./ ：当前项目的根目录

- classpath:/config/：classpath的/config目录

- classpath:/ ：classpath的根目录

  

### 3.3 外部配置文件加载顺序

https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-f
eatures-external-config

1.命令行

```
java -jar icoding-init-1.0-SNAPSHOT.jar --server.port=9000 --
server.servlet.context-path=/icoding01
```

2.指定配置文件位置

```
java -jar icoding-init-1.0-SNAPSHOT.jar --
spring.config.location=C://icoding//springboot//application.yml
```

3.外部不带profile的yml文件

```
classpath:/config/application.yml
classpath:/application.yml
```

## 4.springboot自动配置原理解析

### 4.1Condition(条件)

Condition是Spring4.0后引入的条件化配置接口，通过实现Condition接口可以完成有条件的加载相应
的Bean。

总结：
@ConditionalOnBean（仅仅在当前上下文中存在某个对象时，才会实例化一个Bean）
@ConditionalOnClass（某个class位于类路径上，才会实例化一个Bean）
@ConditionalOnExpression（当表达式为true的时候，才会实例化一个Bean）
@ConditionalOnMissingBean（仅仅在当前上下文中不存在某个对象时，才会实例化一个Bean）
@ConditionalOnMissingClass（某个class类路径上不存在的时候，才会实例化一个Bean）
@ConditionalOnNotWebApplication（不是web应用）

### 4.2Enable注解原理

## 5.springboot监听机制

### 5.1java事件监听角色

Java中的事件监听机制定义了以下几个角色：
事件：Event，继承 java.util.EventObject 类的对象
事件源：Source ，任意对象Object
监听器：Listener，实现 java.util.EventListener 接口 的对象

### 5.2启动时回调的4个监听器

SpringBoot 在项目启动时，会对几个监听器进行回调，我们可以实现这些监听器接口，在项目启动时
完成一些操作。
ApplicationContextInitializer、
SpringApplicationRunListener、
CommandLineRunner、
ApplicationRunner
创建包：listener
自定义监听器的启动时机：MyApplicationRunner和MyCommandLineRunner都是当项目启动后执
行，使用@Component放入容器即可使用

![image-20200423211049906](C:\Users\My\AppData\Roaming\Typora\typora-user-images\image-20200423211049906.png)



## 6.自定义starter



# 7.springboot常规使用

## 7.1 spring跨域问题

## 7.2 集成swagger

### 7.3 多数据源问题

