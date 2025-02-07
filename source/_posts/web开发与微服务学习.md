---
title: web开发与微服务学习
date: 2024-10-13 15:03:53
categories: 学习
math:
tags:
---
# web开发部分
<!-- TOC -->

- [web开发部分](#web开发部分)
    - [SpringBoot的启动流程](#springboot的启动流程)
    - [Spring IOC](#spring-ioc)
        - [基础概念](#基础概念)
        - [IOC解决了什么问题？](#ioc解决了什么问题)
        - [IOC容器的初始化](#ioc容器的初始化)
    - [Spring Bean](#spring-bean)
        - [什么是Spring Bean？@Component 和 @Bean 的区别是什么？](#什么是spring-beancomponent-和-bean-的区别是什么)
        - [将一个类声明为 Bean 的注解有哪些?](#将一个类声明为-bean-的注解有哪些)
        - [Bean生命周期](#bean生命周期)
        - [Bean的循环依赖](#bean的循环依赖)
    - [SpringMVC](#springmvc)
        - [SpringMVC关键组件](#springmvc关键组件)
        - [SpringMVC执行流程](#springmvc执行流程)
        - [统一异常处理怎么做？](#统一异常处理怎么做)
    - [springboot的依赖注入](#springboot的依赖注入)
        - [构造函数注入](#构造函数注入)
        - [setter方法注入](#setter方法注入)
        - [字段注入](#字段注入)
    - [springboot3自动配置原理](#springboot3自动配置原理)
        - [主要步骤](#主要步骤)
        - [使用示例](#使用示例)
    - [自定义starter](#自定义starter)
        - [什么是springboot starter](#什么是springboot-starter)
        - [如何创建自定义starter](#如何创建自定义starter)
    - [设计模式](#设计模式)
        - [工厂模式](#工厂模式)
        - [单例模式](#单例模式)
        - [代理模式](#代理模式)
        - [适配器模式](#适配器模式)
        - [代理模式和适配器模式的区别？](#代理模式和适配器模式的区别)
    - [Redis](#redis)
        - [什么是Redis?](#什么是redis)
        - [Redis为什么快？](#redis为什么快)
    - [JWT令牌](#jwt令牌)
    - [ThreadLocal](#threadlocal)
        - [特点](#特点)
        - [使用](#使用)
    - [拦截器](#拦截器)
        - [作用](#作用)
        - [拦截器与过滤器的区别](#拦截器与过滤器的区别)
        - [实现](#实现)
        - [拦截器的执行顺序](#拦截器的执行顺序)
    - [Controller中的方法接收参数注解](#controller中的方法接收参数注解)
        - [@RequestParam](#requestparam)
        - [@PathVariable](#pathvariable)
        - [@RequestBody](#requestbody)
        - [@CookieValue](#cookievalue)
        - [@RequestHeader](#requestheader)
        - [@ModelAttribute](#modelattribute)
    - [参数校验](#参数校验)
        - [常用注解](#常用注解)
        - [开启参数校验](#开启参数校验)
        - [@Validated和@Valid区别](#validated和valid区别)
    - [自定义参数校验](#自定义参数校验)
        - [自定义State注解](#自定义state注解)
        - [自定义校验规则的类](#自定义校验规则的类)
    - [redis实现登陆主动失效](#redis实现登陆主动失效)
    - [JavaScript的导入导出](#javascript的导入导出)
        - [非默认导出](#非默认导出)
        - [默认导出](#默认导出)
    - [vue基本概念](#vue基本概念)
    - [vue常用指令](#vue常用指令)
        - [ref和reactive](#ref和reactive)
        - [v-for](#v-for)
        - [v-bind](#v-bind)
        - [v-if 和 v-show](#v-if-和-v-show)
        - [v-on](#v-on)
        - [v-model](#v-model)
    - [let和var的区别](#let和var的区别)
    - [vue生命周期](#vue生命周期)
    - [axios的使用](#axios的使用)
        - [基本用法](#基本用法)
        - [axios响应拦截器](#axios响应拦截器)
        - [axios请求拦截器](#axios请求拦截器)
    - [浏览器跨域问题](#浏览器跨域问题)
        - [问题描述](#问题描述)
        - [解决方法](#解决方法)
    - [vue的路由](#vue的路由)
        - [vue中路由的使用步骤](#vue中路由的使用步骤)
        - [路由主动切换组件](#路由主动切换组件)
    - [vue中使用pinia及其持久化](#vue中使用pinia及其持久化)
        - [pinia使用步骤](#pinia使用步骤)
        - [pinia持久化](#pinia持久化)
- [微服务部分](#微服务部分)
    - [注册中心](#注册中心)
        - [服务治理基础知识](#服务治理基础知识)
        - [nacos基础知识](#nacos基础知识)
        - [环境隔离](#环境隔离)
        - [服务分级模型](#服务分级模型)
    - [OpenFeign](#openfeign)
        - [基本用法](#基本用法-1)
        - [连接池](#连接池)
        - [负载均衡](#负载均衡)
    - [网关](#网关)
        - [基本用法](#基本用法-2)
        - [网关实现登陆校验](#网关实现登陆校验)
            - [如何在转发之前做登录校验？](#如何在转发之前做登录校验)
            - [网关登陆校验之后，如何将用户信息传递给微服务？](#网关登陆校验之后如何将用户信息传递给微服务)
            - [微服务之间也会相互调用，这种调用不经过网关，又该如何传递用户信息？](#微服务之间也会相互调用这种调用不经过网关又该如何传递用户信息)
        - [动态路由](#动态路由)
    - [微服务保护](#微服务保护)
        - [请求限流](#请求限流)
        - [线程隔离](#线程隔离)
        - [服务熔断](#服务熔断)
        - [不同线程隔离方案对比](#不同线程隔离方案对比)
        - [计数算法](#计数算法)
    - [分布式事务](#分布式事务)
        - [Seata的基本用法](#seata的基本用法)
        - [XA模式](#xa模式)
        - [AT模式](#at模式)
        - [AT模式的脏写问题](#at模式的脏写问题)
        - [TCC模式](#tcc模式)
    - [消息队列 (MQ)](#消息队列-mq)
        - [基础知识](#基础知识)
        - [RabbitMQ基础知识](#rabbitmq基础知识)
        - [RabbitMQ交换机类型](#rabbitmq交换机类型)
        - [声明队列和交换机](#声明队列和交换机)
        - [消息转换器](#消息转换器)
        - [发送者可靠性](#发送者可靠性)
        - [MQ可靠性](#mq可靠性)
        - [消费者可靠性](#消费者可靠性)
        - [延迟消息](#延迟消息)
    - [ElasticSearch](#elasticsearch)
        - [基础知识](#基础知识-1)
        - [Kibana中的增删改查](#kibana中的增删改查)
        - [DSL查询](#dsl查询)
        - [RestClient基础操作](#restclient基础操作)
    - [Redis主从](#redis主从)
    - [Redis哨兵](#redis哨兵)
    - [Redis分片集群](#redis分片集群)
        - [散列插槽](#散列插槽)
    - [Redis数据结构](#redis数据结构)
        - [SkipList](#skiplist)
        - [SortedSet](#sortedset)
        - [Bitmap](#bitmap)
    - [Redis内存回收](#redis内存回收)
        - [内存过期策略](#内存过期策略)
        - [内存淘汰策略](#内存淘汰策略)
    - [Redis缓存](#redis缓存)
        - [缓存一致性](#缓存一致性)
        - [缓存穿透](#缓存穿透)
        - [缓存雪崩](#缓存雪崩)
        - [缓存击穿](#缓存击穿)

<!-- /TOC -->
## SpringBoot的启动流程
SpringBoot的启动，其本质就是加载各种配置信息，然后初始化IOC容器并返回

首先，当我们在启动类执行SpringApplication.run这行代码的时候，在它的方法内部其实会做两个事情：
- 创建SpringApplication对象；
- 执行run方法。

在创建SpringApplication对象的时候，在它的构造方法内部主要做3个事情：
- 确认web应用类型，一般情况下是Servlet类型，这种类型的应用，将来会自动启动一个tomcat
- 从spring.factories配置文件中，加载默认的ApplicationContextInitializer和ApplicationListener
- 记录当前应用的主启动类，将来做包扫描使用

对象创建好了以后，再调用该对象的run方法，在run方法的内部主要做4个事情：
- 准备Environment对象，它里面会封装一些当前应用运行环境的参数，比如环境变量等等
- 实例化容器，这里仅仅是创建ApplicationContext对象
- 容器创建好了以后，会为容器做一些准备工作，比如为容器设置Environment、BeanFactoryPostProcessor后置处理器，并且加载主类对应的Definition
- 刷新容器，就是我们常说的refresh，在这里会真正的创建Bean实例

总结下来，SpringBoot启动的时候核心就两步，创建SpringApplication对象以及run方法的调用，在run方法中会真正的实例化容器，并创建容器中需要的Bean实例，最终返回

## Spring IOC
### 基础概念
IoC （Inversion of Control ）即控制反转/反转控制。它是一种思想不是一个技术实现。描述的是：Java 开发领域对象的创建以及管理的问题。
- 传统的开发方式 ：往往是在类 A 中手动通过 new 关键字来 new 一个 B 的对象出来使用 
- IoC 思想的开发方式 ：不通过 new 关键字来创建对象，而是通过 IoC 容器(Spring 框架) 来帮助我们实例化对象。我们需要哪个对象，直接从 IoC 容器里面去取即可。
- 控制 ：指的是对象创建（实例化、管理）的权力
- 反转 ：控制权交给外部环境（IoC 容器）
![](https://oss.javaguide.cn/github/javaguide/system-design/framework/spring/IoC&Aop-ioc-illustration.png)

### IOC解决了什么问题？
IoC 的思想就是两方之间不互相依赖，由第三方容器来管理相关资源。这样有什么好处呢？
- 对象之间的耦合度或者说依赖程度降低；
- 资源变的容易管理；比如你用 Spring 容器提供的话很容易就可以实现一个单例。
>将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。 IoC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。

### IOC容器的初始化
IOC容器初始化，核心步骤主要是在`AbstractApplicationContext`类中的`refresh`方法中完成的：
- 准备BeanFactory，在这一块需要给BeanFactory设置很多属性，比如类加载器、Environment等
- 执行BeanFactory后置处理器，这一阶段会扫描要放入到容器中的Bean信息，得到对应的BeanDefinition（注意，这里只扫描，不创建）
- 注册BeanPostProcessor，我们自定义的BeanPostProcessor就是在这一阶段被加载的，将来Bean对象实例化好后需要用到
- 启动tomcat
- 实例化容器中实例化非懒加载的单例Bean，这里需要说的是，多例Bean和懒加载的Bean不会在这个阶段实例化，将来用到的时候再创建
- 当容器初始化完毕后，再做一些收尾工作，比如清除缓存等

总结一下就是，在IOC容器初始化的过程中，首先得准备并执行BeanFactory后置处理器，其次得注册Bean后置处理器，并启动tomcat，最后需要借助于BeanFactory完成Bean的实例化

## Spring Bean

### 什么是Spring Bean？@Component 和 @Bean 的区别是什么？
Bean 代指的就是那些被 IoC 容器所管理的对象。
- @Component 注解作用于类，而@Bean注解作用于方法。
- @Component通常是通过类路径扫描来自动侦测以及自动装配到 Spring 容器中（SpringBoot默认只能扫描到启动类所在的包及其子包，我们可以使用 @ComponentScan 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 bean 容器中）。
- @Bean 注解通常是我们在标有该注解的方法中定义产生这个 bean, @Bean告诉了 Spring 这是某个类的实例，当我需要用它的时候还给我。
- @Bean 注解比 @Component 注解的自定义性更强，而且很多地方我们只能通过 @Bean 注解来注册 bean。比如当我们引用第三方库中的类需要装配到 Spring容器时，则只能通过 @Bean来实现。

### 将一个类声明为 Bean 的注解有哪些?
- @Component：通用的注解，可标注任意类为 Spring 组件。如果一个 Bean 不知道属于哪个层，可以使用@Component 注解标注。
- @Repository : 对应持久层即 Dao 层，主要用于数据库相关操作。
- @Service : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。
- @Controller : 对应 Spring MVC 控制层，主要用于接受用户请求并调用 Service 层返回数据给前端页面。


### Bean生命周期
Bean的生命周期总的来说有4个阶段，分别有创建对象、初始化对象、使用对象以及销毁对象，而且这些工作大部分是交给Bean工厂的doCreateBean方法完成的。

- 在创建对象阶段，先调用构造方法实例化对象，对象有了后会填充该对象的内容，其实就是处理依赖注入。

- 对象创建完毕后，需要做一些初始化的操作，在这里涉及到几个扩展点。
  - 执行Aware感知接口的回调方法
  - 执行Bean后置处理器的postProcessBeforeInitialization方法
  - 执行InitializingBean接口的回调，在这一步如果Bean中有标注了@PostConstruct注解的方法，会先执行它
  - 执行Bean后置处理器的postProcessAfterInitialization
  - 把这些扩展点都执行完，Bean的初始化就完成了。

- 在使用阶段就是程序员从容器中获取该Bean使用即可。
- 在容器销毁之前，会先销毁对象，此时会执行DisposableBean接口的回调，这一步如果Bean中有标注了@PreDestroy接口的函数，会先执行它。

简单总结一下，Bean的生命周期共包含四个阶段，其中初始化对象和销毁对象我们程序员可以通过一些扩展点执行自己的代码。

### Bean的循环依赖
Bean的循环依赖指的是A依赖B，B又依赖A这样的依赖闭环问题。

在Spring中，通过三个对象缓存区来解决循环依赖问题，这三个缓存区被定义到了DefaultSingletonBeanRegistry中，分别是singletonObjects用来存储创建完毕的Bean，earlySingletonObjects用来存储未完成依赖注入的Bean，还有SingletonFactories用来存储创建Bean的ObjectFactory。假如说现在A依赖B，B依赖A，整个Bean的创建过程是这样的

- 首先，调用A的构造方法实例化A，当前的A还没有处理依赖注入，暂且把它称为半成品，此时会把半成品A封装到一个ObjectFactory中，并存储到SingletonFactories缓存区。

- 接下来，要处理A的依赖注入了，由于此时还没有B，所以得先实例化一个B，同样的，半成品B也会被封装到ObjectFactory中，并存储到SingletonFactories缓存区。

- 紧接着，要处理B的依赖注入了，此时会找到SingletonFactories中A对应的ObjectFactory，调用它的getObject方法得到刚才实例化的半成品A（如果需要代理对象，则会自动创建代理对象，将来得到的就是代理对象），把得到的半成品A注入给B，并同时会把半成品A存入到earlySingletonObjects中，将来如果还有其他的类循环依赖了A，就可以直接从earlySingletonObjects中找到它了，那么此时SingletonFactories中创建A的ObjectFactory也可以删除了。

- 至此，B的依赖注入处理完了后，B就创建完毕了，就可以把B的对象存入到singletonObjects中了，并同时删除掉SingletonFactories中创建B的ObjectFactory。

- B创建完毕后，就可以继续处理A的依赖注入了，把B注入给A，此时A也创建完毕了，就可以把A的对象存储到singletonObjects中，并同时删除掉earlySingletonObjects中的半成品A。

- 至此，A和B对象全部创建完毕，并存储到了singletonObjects中，将来通过容器获取对象，都是从singletonObjects中获取。

总结起来还是一句话，借助于DefaultSingletonBeanRegistry的三个缓存区singletonObjects、earlySingletonObjects、singletonFactories可以解决循环依赖问题。

## SpringMVC
MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码。
Spring MVC 下我们一般把后端项目分为 Service 层（处理业务）、Dao 层（数据库操作）、Entity 层（实体类）、Controller 层(控制层，返回数据给前台页面)。

### SpringMVC关键组件
- DispatcherServlet：核心的中央处理器，负责接收请求、分发，并给予客户端响应。
- HandlerMapping：处理器映射器，根据 URL 去匹配查找能处理的 Handler ，并会将请求涉及到的拦截器和 Handler 一起封装。
- HandlerAdapter：处理器适配器，根据 HandlerMapping 找到的 Handler ，适配执行对应的 Handler；
- Handler：请求处理器，处理实际请求的处理器。
- ViewResolver：视图解析器，根据 Handler 返回的逻辑视图 / 视图，解析并渲染真正的视图，并传递给 DispatcherServlet 响应客户端
### SpringMVC执行流程
- 客户端（浏览器）发送请求， DispatcherServlet拦截请求。
- DispatcherServlet 根据请求信息调用 HandlerMapping 。
- HandlerMapping 根据 URL 去匹配查找能处理的 Handler（也就是我们平常说的 Controller 控制器） ，并会将请求涉及到的拦截器和 Handler 一起封装。
- DispatcherServlet 调用 HandlerAdapter适配器执行 Handler 。
- Handler 完成对用户请求的处理后，会返回一个 ModelAndView 对象给DispatcherServlet，ModelAndView 顾名思义，包含了数据模型以及相应的视图的信息。Model 是返回的数据对象，View 是个逻辑上的 View。
- ViewResolver 会根据逻辑 View 查找实际的 View。
- DispaterServlet 把返回的 Model 传给 View（视图渲染）。
- 把 View 返回给请求者（浏览器）

### 统一异常处理怎么做？
- 定义一个响应的实体类，用来封装异常信息
- 定义一个异常处理类，使用`@RestControllerAdvice + @ExceptionHandler`注解，被@ExceptionHandler注解修饰的方法处理异常。
```
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(Exception.class)
    public Result handleException(Exception e){
        e.printStackTrace();
        return Result.error(StringUtils.hasLength(e.getMessage()) ? e.getMessage(): "操作失败" );
    }
}
```
## springboot的依赖注入
>依赖注入允许对象在需要使用其他对象时，不用自己去创建，而是由 Spring 框架负责提供这些对象（依赖）。这不仅提高了代码的可测试性，还简化了对象之间的依赖关系管理。

springboot中，依赖注入主要有三种方式。

### 构造函数注入
构造器注入是通过类的构造函数将依赖传入。在实例化对象时，依赖会通过构造器传递进来。这种方式**最推荐**，原因是它确保了依赖在对象创建时被正确地初始化，并且使对象变得不可变（immutable）。

常见的用法是：
```
@RequiredArgsConstructor
public class MyService {
    private final DependencyService dependencyService;
}
```
这里的`@RequiredArgsConstructor`注解由lombok提供，作用是为`final`修饰的成员变量(依赖)自动生成一个构造器，避免手动编写代码。

优点：
- 保证依赖不可变。
- 提高了可测试性，便于使用构造器参数进行单元测试。
- 强制依赖的存在，避免空指针异常。
### setter方法注入
Setter方法注入通过 setter 方法将依赖注入到类中。Spring 在实例化对象后，会调用这些 setter 方法进行注入。这种方式适用于依赖是可选项或在运行时需要灵活设置的情况。

一般用法是：
```
@Component
public class MyService {
    private DependencyService dependencyService;

    // Setter方法注入
    @Autowired
    public void setDependencyService(DependencyService dependencyService) {
        this.dependencyService = dependencyService;
    }
}
```
优点：
- 灵活性高，可以在运行时动态注入
- 适合可选的依赖或需要后期修改的依赖
缺点：
- 依赖对象可能未初始化便被调用，可能导致空指针异常
- 类的状态可变
### 字段注入
字段注入直接在类的成员变量上使用 @Autowired 注解进行依赖注入，是最简单的方式。

一般用法是：
```
@Component
public class MyService {
    @Autowired
    private DependencyService dependencyService;
}
```

优点：
- 代码简单，注入逻辑隐式完成

缺点：
- 不利于单元测试，无法通过构造器或 setter 模拟依赖。
- 类的可维护性降低，依赖关系不明显，可能导致隐式的错误。
## springboot3自动配置原理

### 主要步骤

- 主启动类中，通常使用`@SpringBootApplication`注解，这个注解是一个组合注解，它包含了`@EnableAutoConfiguration`注解。

- `@EnableAutoConfiguration`注解中，通过`@Import`导入自动配置模块的导入选择器`AutoConfigurationImportSelector`，它的作用是在启动时扫描指定包路径下的所有自动配置类，并根据应用程序的依赖关系和环境变量等信息，自动地选择需要引入的自动配置类，并将其注册为 Bean

- `AutoConfigurationImportSelector`导入选择器中实现了selectImports方法，这个方法经过多层调用，最终在`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`文件中扫描候选的配置类，springboot2.7以前的版本是在`META-INF/spring.factories`文件中

- 通过自动配置类使用条件注解（如 `@ConditionalOnClass、@ConditionalOnBean、@ConditionalOnProperty` 等）来确保只有满足特定条件的Bean才能被注册。

### 使用示例

- 编写配置类
```
@Configuration
public class ThirdPartyConfig {

    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```

- 编写自动配置类，使用`@AutoConfiguration`注解，通过`@Import`导入所需要的自动配置类

**注意**：直接在配置类上使用`@AutoConfiguration`作为自动配置类，不额外创建一个自动配置类也是可以的

```
@AutoConfiguration
@Import(ThirdPartyConfig.class)
public class MyAutoConfiguration {
}
```

- 在`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`文件中添加自动配置类的全类名

```
com.example.config.MyAutoConfiguration
```

- 在pom文件中引入坐标，就可以通过依赖注入使用Bean了


## 自定义starter
### 什么是springboot starter
Spring Boot Starter 是 Spring Boot 提供的一系列依赖管理工具，旨在简化 Spring 应用程序的设置和配置。每个 Starter 都是一个预配置的依赖集合，包含了开发特定类型应用程序所需的所有依赖项和配置。
### 如何创建自定义starter
熟悉自动配置之后，自定义starter的步骤其实很简单，只有两步：
- 首先，创建一个模块，提供自动配置功能，即该模块包含自动配置类(定义了自动配置类并在`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`中声明)
- 创建另一个模块作为starter，在pom文件中引入上面的自定义自动配置类模块的坐标


## 设计模式
### 工厂模式
定义一个创建对象的接口，但由子类决定实例化哪个类。Spring 使用工厂模式可以通过 BeanFactory 或 ApplicationContext 创建 bean 对象。
两者对比：
- BeanFactory：延迟注入(使用到某个 bean 的时候才会注入),相比于ApplicationContext 来说会占用更少的内存，程序启动速度更快。
- ApplicationContext：容器启动的时候，不管你用没用到，一次性创建所有 bean 。BeanFactory 仅提供了最基本的依赖注入支持，ApplicationContext 扩展了 BeanFactory ,除了有BeanFactory的功能还有额外更多功能，所以一般开发人员使用ApplicationContext会更多。
### 单例模式
保证一个类仅有一个实例，并提供一个访问它的全局访问点。有一些对象其实我们只需要一个，比如说：线程池、缓存、对话框、注册表、日志对象、充当打印机、显卡等设备驱动程序的对象。事实上，这一类对象只能有一个实例，如果制造出多个实例就可能会导致一些问题的产生，比如：程序的行为异常、资源使用过量、或者不一致性的结果。
使用单例模式的好处 :
- 对于频繁使用的对象，可以省略创建对象所花费的时间，这对于那些重量级对象而言，是非常可观的一笔系统开销；
- 由于 new 操作的次数减少，因而对系统内存的使用频率也会降低，这将减轻 GC 压力，缩短 GC 停顿时间。
### 代理模式
为其他对象提供一种代理以控制对这个对象的访问。有些对象由于某些原因（比如对象创建开销很大，或者某些操作需要安全控制，或者需要进程外的访问），直接访问会给使用者或者系统结构带来很多麻烦，这时候我们可以提供一个代理对象来控制对这个对象的访问。
### 适配器模式
将一个类的接口转换成客户希望的另外一个接口。适配器模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。
主要解决在软件系统中，常常要将一些"现存的对象"放到新的环境中，而新环境要求的接口是现对象不能满足的。
### 代理模式和适配器模式的区别？
- 目的不同：代理模式主要关注对象的访问，适配器模式主要关注接口的转换，使不兼容的接口可以一起工作。
- 结构不同：代理模式一般包含抽象主题、真实主题、代理主题三个角色，适配器模式一般包含目标接口、适配器、被适配者三个角色。
- 应用场景不同：代理模式常用于通过代理控制对象的访问，适配器模式用于让不兼容的接口协同工作。
## Redis
### 什么是Redis?
Redis （REmote DIctionary Server）是一个基于 C 语言开发的开源 NoSQL 数据库（BSD 许可）。与传统数据库不同的是，Redis 的数据是保存在内存中的（内存数据库，支持持久化），因此读写速度非常快，被广泛应用于分布式缓存方向。并且，Redis 存储的是 KV 键值对数据。
### Redis为什么快？
- Redis 内部做了非常多的性能优化，比较重要的有：
- Redis 基于内存，内存的访问速度比磁盘快很多；
- Redis 基于 Reactor 模式设计开发了一套高效的事件处理模型，主要是单线程事件循环和 IO 多路复用（Redis 线程模式后面会详细介绍到）；
- Redis 内置了多种优化过后的数据类型/结构实现，性能非常高。
- Redis 通信协议实现简单且解析高效。
## JWT令牌

JWT全称为Json Web Token，定义了一种简洁的、自包含的格式，用于通信双方以json数据格式安全的传输信息。其形式是一个很长的字符串，中间用点`.`分隔成三个部分。这些字符串由JSON对象使用Base64URL算法转换得到。
分别是：
- 头部(Header)，记录令牌类型，加密算法等，如`{"alg": "HS256","typ": "JWT"}`
- 载荷(Payload)，包括默认信息和自定义信息，如可添加自定义信息`{"id":"1", "name":"tom"}`
- 签名(Signature)，防止JWT被篡改。需要指定一个密钥(secret)。这个密钥只有服务器才知道，不能泄露给用户。然后，使用Header里面指定的签名算法(默认是 HMAC SHA256)，按照下面的公式产生签名。
```
HMACSHA256( base64UrlEncode(header) + ".”"+base64UrlEncode(payload), secret)
```
- 算出签名以后，把 Header、Payload、Signature 三个部分拼成一个字符串，每个部分之间用"点"(.)分隔，就可以返回给用户。

以下是利用hutool工具封装的创建和解析JWT令牌的类:
```
@Component
public class JwtTool {
    private final JWTSigner jwtSigner;

    public JwtTool(KeyPair keyPair) {
        this.jwtSigner = JWTSignerUtil.createSigner("rs256", keyPair);
    }

    /**
     * 创建 access-token
     *
     * @param userDTO 用户信息
     * @return access-token
     */
    public String createToken(Long userId, Duration ttl) {
        // 1.生成jws
        return JWT.create()
                .setPayload("user", userId)
                .setExpiresAt(new Date(System.currentTimeMillis() + ttl.toMillis()))
                .setSigner(jwtSigner)
                .sign();
    }

    /**
     * 解析token
     *
     * @param token token
     * @return 解析刷新token得到的用户信息
     */
    public Long parseToken(String token) {
        // 1.校验token是否为空
        if (token == null) {
            throw new UnauthorizedException("未登录");
        }
        // 2.校验并解析jwt
        JWT jwt;
        try {
            jwt = JWT.of(token).setSigner(jwtSigner);
        } catch (Exception e) {
            throw new UnauthorizedException("无效的token", e);
        }
        // 2.校验jwt是否有效
        if (!jwt.verify()) {
            // 验证失败
            throw new UnauthorizedException("无效的token");
        }
        // 3.校验是否过期
        try {
            JWTValidator.of(jwt).validateDate();
        } catch (ValidateException e) {
            throw new UnauthorizedException("token已经过期");
        }
        // 4.数据格式校验
        Object userPayload = jwt.getPayload("user");
        if (userPayload == null) {
            // 数据为空
            throw new UnauthorizedException("无效的token");
        }

        // 5.数据解析
        try {
           return Long.valueOf(userPayload.toString());
        } catch (RuntimeException e) {
            // 数据格式有误
            throw new UnauthorizedException("无效的token");
        }
    }
}
```

Spring 在创建 JwtTool 的实例时(自动注入JwtTool时)，会自动提供一个 KeyPair 实例(KeyPair在配置类中被注册成Bean)，所以使用该工具类时，直接注入，然后调用ceate或者parse方法即可，例如：
```
private final JwtTool jwtTool;
String token = jwtTool.createToken(user.getId(), jwtProperties.getTokenTTL());
```

## ThreadLocal
ThreadLoal 变量，线程局部变量
ThreadLocal 提供了线程本地的实例。它与普通变量的区别在于，每个使用该变量的线程都会初始化一个完全独立的实例副本。ThreadLocal 变量通常被private static修饰。当一个线程结束时，它所使用的所有 ThreadLocal 相对的实例副本都可被回收。

>总的来说，ThreadLocal 适用于每个线程需要自己独立的实例且该实例需要在多个方法中被使用，也即变量在线程间隔离而在方法或类间共享的场景。如同一个用户进行不同的请求。

### 特点
- 线程隔离：每个线程都拥有自己的变量副本，线程之间的变量副本互不影响，从而避免了多线程操作共享资源造成的数据不一致问题。
- 线程安全：由于每个线程只能访问自己的变量副本，因此不需要额外的同步机制来保证线程安全。
- 减少参数传递：在复杂的业务逻辑中，使用ThreadLocal可以避免在多个方法之间频繁传递参数，从而简化代码。

### 使用

ThreadLocal的工具类模板：
```
public class ThreadLocalUtil {
    private static final ThreadLocal THREAD_LOCAL = new ThreadLocal();

    //    按键获取数据
    public static <T> T get() {
        return (T) THREAD_LOCAL.get();
    }

    //    存储数据
    public static void set(Object value) {
        THREAD_LOCAL.set(value);
    }

    //    清除线程
    public static void remove() {
        THREAD_LOCAL.remove();
    }
}
```




## 拦截器
拦截器（Interceptor）是一种特殊的组件，它可以在请求处理的过程中对请求和响应进行拦截和处理。拦截器可以在请求到达目标处理器之前、处理器处理请求之后以及视图渲染之前执行特定的操作。拦截器的主要目的是在不修改原有代码的情况下，实现对请求和响应的统一处理。

### 作用
- 权限控制：拦截器可以在请求到达处理器之前进行权限验证，从而实现对不同用户的访问控制。
- 日志记录：拦截器可以在请求处理过程中记录请求和响应的详细信息，便于后期分析和调试。
- 接口幂等性校验：拦截器可以在请求到达处理器之前进行幂等性校验，防止重复提交。
- 数据校验：拦截器可以在请求到达处理器之前对请求数据进行校验，确保数据的合法性。
- 缓存处理：拦截器可以在请求处理之后对响应数据进行缓存，提高系统性能。

### 拦截器与过滤器的区别
- 执行顺序：过滤器在拦截器之前执行，拦截器在处理器之前执行。
- 功能范围：过滤器可以对所有请求进行拦截，而拦截器只能对特定的请求进行拦截。
- 生命周期：过滤器由Servlet容器管理，拦截器由Spring容器管理。
- 使用场景：过滤器适用于对请求和响应的全局处理，拦截器适用于对特定请求的处理。

### 实现
1. 在SpringBoot中实现拦截器，首先需要创建一个类并实现HandlerInterceptor接口。HandlerInterceptor接口包含以下三个方法：
    - preHandle：在请求到达处理器之前执行，可以用于权限验证、数据校验等操作。如果返回true，则继续执行后续操作；如果返回false，则中断请求处理。
    - postHandle：在处理器处理请求之后执行，可以用于日志记录、缓存处理等操作。
    - afterCompletion：在视图渲染之后执行，可以用于资源清理等操作。
    以登录认证为例，代码如下：
```
@Component
public class LoginInterceptor implements HandlerInterceptor {
    @Autowired
    private StringRedisTemplate redisTemplate;

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String token = request.getHeader("Authorization");
        //        验证token
        try {
//            从redis中获取token
            ValueOperations<String, String> ops = redisTemplate.opsForValue();
            String redisToken = ops.get(token);
            if(redisToken==null){
                throw new RuntimeException();
            }
            if(!redisToken.equals(token)){
                throw new RuntimeException();
            }
            Map<String, Object> claims = JwtUtil.parseToken(token);
//            把用户信息存到threadlocal中
            ThreadLocalUtil.set(claims);
            return true;
        } catch (Exception e) {
            response.setStatus(401);
            return false;
        }
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        ThreadLocalUtil.remove();
    }
}
```

2. 创建一个配置类，实现WebMvcConfigurer接口，注册拦截器到InterceptorRegistry，并设置拦截规则
    以登录认证为例，代码如下：
```
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Autowired
    private LoginInterceptor loginInterceptor;
    @Override
    public void addInterceptors(InterceptorRegistry registry) {

//        拦截除了登录和注册的其他请求
        registry.addInterceptor(loginInterceptor).excludePathPatterns("/user/login","/user/register");
    }
}
```

### 拦截器的执行顺序
当有多个拦截器时，它们的执行顺序取决于注册顺序。先注册的拦截器先执行，后注册的拦截器后执行。在请求处理过程中，拦截器的preHandle方法按注册顺序执行，而postHandle和afterCompletion方法按注册顺序的逆序执行。

当有多个拦截器时，它们的执行流程如下：

- 执行所有拦截器的preHandle方法，按注册顺序执行。如果某个拦截器的preHandle方法返回false，则中断请求处理，直接执行已执行拦截器的afterCompletion方法。
- 执行处理器的处理方法。
- 执行所有拦截器的postHandle方法，按注册顺序的逆序执行。
- 渲染视图。
- 执行所有拦截器的afterCompletion方法，按注册顺序的逆序执行。

## Controller中的方法接收参数注解
### @RequestParam
用于将HTTP请求参数绑定到控制器的方法参数上。如果请求参数不存在，可以指定一个默认值。这个注解主要用于处理请求中的**查询参数（即URL中`?`后面的部分）**，使得你可以轻松地从请求中获取这些参数的值。

示例：
```
@GetMapping("/greeting")  
public String greeting(@RequestParam(value = "name", defaultValue = "World") String name) {  
    return String.format("Hello, %s!", name);  
}
```

### @PathVariable
用于将URL模板变量值绑定到控制器处理方法的参数上。
这个注解使得你可以**从URL的路径中提取变量值**，这在构建RESTful API时非常有用。

示例：
```
@GetMapping("/user/{id}")  
public String getUserById(@PathVariable("id") Long id) {  
    // 根据id获取用户信息  
    return "User ID is: " + id;  
}
```

### @RequestBody
用于处理HTTP请求的内容体（Body），**将请求体中的数据绑定到Java对象**上。

这个注解**通常用于处理POST和PUT请求**，特别是当请求的内容类型为application/json或application/xml时。它允许你直接将请求体中的数据映射到Java对象上，便于后续处理

示例：
```
@GetMapping("/cookie")  
public String readCookie(@CookieValue(name = "theme", defaultValue = "default") String theme) {  
    return "Theme is: " + theme;  
}
```

### @CookieValue
用于**将请求的Cookie数据绑定到控制器方法的参数**上。

这个注解允许你访问HTTP请求中的Cookie信息，这在**处理需要身份验证或会话管理的Web应用**时非常有用。

示例：
```
@GetMapping("/cookie")  
public String readCookie(@CookieValue(name = "theme", defaultValue = "default") String theme) {  
    return "Theme is: " + theme;  
}
```

### @RequestHeader
用于**将HTTP请求头信息绑定到控制器方法的参数**上。

这个注解**允许你访问HTTP请求中的头部信息**，比如User-Agent、Content-Type等，这对于日志记录、内容协商等场景非常有用。

示例：
```
@GetMapping("/header")  
public String readHeader(@RequestHeader(name = "User-Agent", defaultValue = "Unknown") String userAgent) {  
    return "User-Agent header is: " + userAgent;  
}
```

### @ModelAttribute

用于**将请求参数绑定到JavaBean上**，也可以用在方法上，表示该方法的返回值应该添加到模型（Model）中。

这个注解主要用于处理表单数据，它可以将请求中的参数自动绑定到一个或多个JavaBean上，便于后续处理。

示例：
```
@PostMapping("/addUser")  
public String addUser(@ModelAttribute User user) {  
    // 处理user对象  
    return "User added successfully";  
}
```
## 参数校验
一般对于提交的表单要进行参数校验，主要步骤如下：

### 常用注解
可以通过注解给对象的每个属性设置校验规则，常见的注解有

- @NotEmpty：检查集合或数组等对象是否为 null 或空。该注解通常用于检查字符串是否为空，集合是否为空等情况。
- @NotBlank：检查字符串是否不为空，并且去除首尾空格后长度大于 0。该注解通常用于检查用户输入的字符串是否为有效值。
- @NotNull：检查对象(包装类：Integer、Boolean等)是否不为 null。该注解通常用于检查对象是否已经被初始化。
- Size(max,min)：检查该字段的size是否在min和max之间，可以是字符串、数组、集合、Map等
- @Pattern(regexp = “[abc]”)：被注释的元素必须符合指定的正则表达式
- @Email：被注释的元素必须是电子邮件地址
- @URL：被注释的元素必须是URL地址

示例：
```
import jakarta.validation.constraints.*;
import lombok.Data;
/**
 * @author mijiupro
 */
@Data
public class UserLoginDTO {
    @NotBlank(message = "账号不能为空")
    private String userAccount;// 用户账号
    @Size(min = 6, max = 18, message = "用户密码长度需在6-18位")
    private String password;// 密码
    @NotBlank(message = "验证码id不能为空")
    private String captchaId;// 验证码id
    @NotBlank(message = "验证码内容不能为空")
    private String captcha;// 验证码内容
}
```

### 开启参数校验
通常在控制层进行参数校验，通过对请求参数使用@Validated注解或者@Valid注解来启用校验。

示例：
```
@RestController
@RequestMapping("/user")
@Tag(name = "用户管理", description = "用户管理")
public class UserController {
    private final UserService userService;
 
    public UserController(UserService userService) {
        this.userService = userService;
    }
 
    @PostMapping("/login")
    @Operation(summary = "用户登录")
    public UserLoginVO login(@RequestBody @Validated UserLoginDTO userLoginDTO) {
        return userService.login(userLoginDTO);
    }
 
}
```

### @Validated和@Valid区别

- @Validated注解是 Spring 框架提供的，主要用于在 Spring MVC 中对控制器的方法参数进行校验。
- @Valid注解是 Java 标准库提供的，用于在任何地方触发参数校验，包括 Spring MVC 的控制器方法、Spring Boot 的 REST 控制器方法、Spring Data JPA 的实体类等。

>在spring boot推荐用@Validated注解，因为它能够支持 Spring 提供的校验注解，并且具有更好的集成性。

## 自定义参数校验

已有的参数校验注解不能覆盖所有特殊情况，比如发布状态state要求必须是草稿或已发布状态，因此需要自定义校验规则。

### 自定义State注解
需要实现State接口

代码：

```
@Documented//元注解
@Target({FIELD})//元注解
@Retention(RUNTIME)//元注解
@Constraint(validatedBy = {StateValidation.class})//指定提供校验规则的类
public @interface State {
    //提供校验失败后的提示信息
    String message() default "state参数的值只能是已发布或者草稿";
    //指定分组
    Class<?>[] groups() default { };
    //负载  获取到State注解的附加信息
    Class<? extends Payload>[] payload() default { };
}
```

- @Documented: 这是一个元注解，用于指示该注解应该被包含在文档中
- @Target({FIELD}): 这也是一个元注解，指定了该注解可以应用在字段上
- @Retention(RUNTIME): 这是一个元注解，指定了该注解应该在运行时保留，以便在运行时可以通过反射获取注解信息。
- @Constraint(validatedBy = {StateValidation.class}): 这是一个约束注解，用于指定提供校验规则的类，即 StateValidation 类(自定义的)

### 自定义校验规则的类
下面实现StateValidation类，该类要实现ConstraintValidator接口：

```
public class StateValidation implements ConstraintValidator<State, String>{

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        if(value==null)
            return false;
        return value.equals("已发布") || value.equals("草稿");
    }
}
```
ConstraintValidator接口要传入两个参数，一个是提供校验规则的注解(State)，一个是校验的数据类型(String)，然后重写isValid方法。

经过以上步骤，自定义的@State注解就可以使用了。

## redis实现登陆主动失效

在普通的token颁发和校验中 当用户发现自己账号和密码被暴露了时修改了登录密码后，旧的token仍然可以通过系统校验直至token到达失效时间，这肯定是不安全的，所以系统需要利用redis实现token主动失效机制

实现这个机制的主要步骤如下：

- 在每次用户登录后颁发token的同时往redis数据库中存储一份颁发给用户的token，两个token过期时间要相同
```
    ValueOperations<String, String> ops = redisTemplate.opsForValue();
    ops.set(token, token, 30, TimeUnit.MINUTES);
```
- 每次每次用户请求时除了解析token外还需要查询redis中是否有当前token，有则校验通过，没有则校验失败，校验一般在拦截器执行
```
    //从redis中获取token
    ValueOperations<String, String> ops = redisTemplate.opsForValue();
    String redisToken = ops.get(token);
    if(redisToken==null){
        throw new RuntimeException();
    }
```
- 每次用户修改密码后删除redis中当前用所携带的token,从而使旧token无法通过token校验
```
    userService.updatePwd(Md5Util.getMD5String(newPwd));
    ValueOperations<String, String> ops = redisTemplate.opsForValue();
    ops.getOperations().delete(token);
```

## JavaScript的导入导出

### 非默认导出
可以导出多个变量、函数、类等，使用`export`关键字

示例：
```
##不带别名
export {test1, test2}; ##只有一个函数，可以不用{}
import {test1, test2} from './test.js'; 
##带别名
export {test1 as t1, test2 as t2};
import {t1, t2} from './test.js'; 
```

### 默认导出
每个模块只能有一个默认导出，使用`export default`关键字

示例：
```
export default {test1, test2};
import testMethods from './test.js'; ##不需要{}
testMethods.test1();
testMethods.test2();
```
>导入非默认导出的模块时，需要使用`{}`，导入默认导出的模块时，不需要使用`{}`


## vue基本概念

- 一般vue项目目录中，index.html是默认首页，在该文件中引入main.js文件，main.js是**vue项目的入口文件**

- npm下载的包在node_modules文件夹中，一般不需要手动引入，直接在main.js中引入即可

- main.js中引入App.vue文件，App.vue是根组件，其他组件都是在根组件中引入的

- App.vue文件中包含三个部分：template、script、style，分别对应html、js、css
    - template: 模板，定义页面结构，生成html
    - script: 页面逻辑，处理数据和事件
    - style: 样式，定义页面样式


## vue常用指令
### ref和reactive

>响应式数据是指当数据发生变化时，相关的视图会自动更新

ref 可以用来定义所有类型的数据，包括基本数据类型（如字符串、数字、布尔值）和引用类型（如对象、数组等）。ref 会返回一个具有 .value 属性的响应式对象，在模板中可以直接访问，而在脚本中需要通过 .value 来访问和更新其值。

特点:
- 基本数据类型必须使用ref进行修饰
- 在template引入响应式数据时，直接引入变量名即可
- 修改ref修饰的响应数据的值时，必须使用 "变量.value"才能修改

reactive 只能用来定义引用类型（如对象、数组等），它将整个对象进行深度代理，使其具有响应式特性。通常用于复杂的对象结构或嵌套对象，便于直接在对象属性上进行修改。

特点：
- reactive 如果重新进行赋值，那么原来的对象就会失去响应式
- reactive 修改值的内容不需要使用 .value

>如何选择：ref 更适合基本类型和简单的变量，而 reactive 更适合对象和结构复杂的数据。
### v-for

常用于列表渲染，用于遍历容器的元素或者对象的属性。

如渲染多行表格
示例：
```
<tr v-for="(article,index) in articleList">
    <td>{{article.title}}</td>
    <td>{{article.category}}</td>
    <td>{{article.time}}</td>
    <td>{{article.state}}</td>
    <td>
        <button>编辑</button>
        <button>删除</button>
    </td>
</tr>
```
其中，index可以省略，如`v-for="article in articleList"`

### v-bind

动态为HTML标签属性绑定属性，即让标签绑定一个变量

如为herf标签绑定url变量：
示例
```
<a v-bind:href="url">我的博客</a>

简写： <a :href="url">我的博客</a> 
```
其中，url可以是博客的网页链接

### v-if 和 v-show

这两个指令都用来控制元素的显示与隐藏

v-if 示例：
```
手链价格为: <span v-if="customer.level>=0 && customer.level<=1">9.9</span>
<span v-else-if="customer.level>1 && customer.level<=2">8.8</span>
<span v-else>7.7</span>
```

v-show 示例：
```
手链价格为: <span v-show="customer.level>=0 && customer.level<=1">9.9</span>
<span v-show="customer.level>1 && customer.level<=2">8.8</span>
<span v-show="customer.level>2">7.7</span>
```

**区别**

- v-if 基于条件来创建或移除元素
- v-show 基于css的display属性的切换，控制元素的显示和隐藏

### v-on

为html元素绑定事件

示例：
```
<button v-on:click="oneClick">点我有惊喜</button> &nbsp;

简写： <button @click="twoClick">再点更惊喜</button>
```

### v-model

在表单元素上创建双向数据绑定

如将表单中输入框和代码中的数据变量绑定
```
文章分类: <input type="text" v-model="searchConditions.category" />

发布状态: <input type="text" v-model="searchConditions.state" />
```
这样，当表单中填写的值改动，代码中的变量值也会改变，反之亦然

## let和var的区别

- 作用域（Scope）：
    - var 声明的变量具有函数作用域，即它在函数内部声明时，作用域限制在该函数内；如果在代码块（如if、for）中声明，它的作用域仍然是该代码块所在的函数或全局作用域。
    - let 声明的变量具有块作用域，即它的作用范围仅限于声明它的代码块内。因此在if、for等代码块中使用let，变量只在该代码块内有效。
```
function test() {
    if (true) {
        var a = 1; // 函数作用域
        let b = 2; // 块作用域
    }
    console.log(a); // 1
    console.log(b); // ReferenceError: b is not defined
}
```

- 变量提升（Hoisting）：

    - var 声明的变量会被提升到当前作用域的顶部，在声明之前可以访问到，但值为undefined。
    - let 声明的变量也会提升到作用域顶部，但存在暂时性死区（Temporal Dead Zone），即在变量声明之前访问会**报错**。
```
console.log(a); // undefined
var a = 1;

console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 2;
```

- 重复声明：
    - var 允许在同一作用域内重复声明相同变量名，而let不允许重复声明相同变量名。
```
var a = 1;
var a = 2; // 不会报错

let b = 1;
let b = 2; // SyntaxError: Identifier 'b' has already been declared
```

- 全局对象属性：

    - 在全局作用域中用var声明的变量会成为全局对象（如浏览器中的window对象）的属性，而let声明的全局变量不会添加到全局对象上。

## vue生命周期

vue 生命周期一共包含八个阶段，每个阶段会自动执行对应的钩子函数，这些钩子函数可以用来执行一些初始化操作、数据请求、事件监听等操作，具体如下：

| 钩子函数 | 阶段 |
| :---: | :---: |
|beforeCreate|创建前|
|created|创建后|
|beforeMount|挂载前|
|mounted|挂载后|
|beforeUpdate|数据更新前|
|updated|数据更新后|
|beforeUnmount|组件销毁前|
|unmounted|组件销毁后|

## axios的使用
### 基本用法
axios 是一个基于 promise 的 异步 ajax 请求库，前端最流行的 ajax 请求库。简单的讲就是可以发送get、post请求，负责与后端交互。

axios有2种基本使用方式：

1. axios({method:'方法'，url: '', baseURL: ''，data:{name: 'cc', sex: 'man'} })

示例：
```
axios({
    method: 'post',
    url: '/user',
    baseURL: 'http://localhost:8080',
    data: {
        name: 'cc',
        sex: 'man'
    }
}).then(res => {
    console.log(res)
}).catch(err => {
    console.log(err)
})
```
2. 直接调用axios.get()、axios.post()等方法

示例：
```
axios.post('/login',{name:'cc', sex:'man'}).then(res => {
    console.log(res)
}).catch(err => {
    console.log(err)
})
```
### axios响应拦截器

在响应被 then 或 catch 处理前拦截它们，可以对响应数据进行处理，如统一处理错误信息、统一处理loading等。

示例：
```
import axios from "axios";
const baseURL = 'http://localhost:8080'; //后端接口地址
const instance = axios.create({baseURL}); // 定义请求实例
// 添加响应拦截器
instance.interceptors.response.use(
    response => {//http状态码为2xx时触发
        return response.data; 
    },
    error => {//http状态码不为2xx时触发
        alert('请求错误');
        return Promise.reject(error);//异步的状态转为失败状态，抛出错误
    }
);
export default instance;
```
axios的拦截器是基于Promise的，use()方法接收两个参数，第一个是成功的回调函数，第二个是失败的回调函数。
由于该函数本身就是异步的，所以不需要async和await关键字，js文件调用时也不需要加，只需要在vue文件中调用时加上async和await关键字即可。

### axios请求拦截器

在请求被 then 或 catch 处理前拦截它们，可以对请求数据进行处理，如统一添加token、统一添加loading等。

用法与响应拦截器类似，下面是一个请求拦截器添加token的示例：
```
instance.interceptors.request.use(
  (config) => {
    let tokenStore = useTokenStore();
    if (tokenStore.token) {
      config.headers.Authorization = tokenStore.token;
    }
    return config;
  },
  (error) => {
    ElMessage({
      type: "error",
      message: "请求错误！",
      plain: true,
    });
    return Promise.reject(error);
  }
);
```

## 浏览器跨域问题

### 问题描述
由于浏览器的同源策略，不同源（不同协议、不同域名、不同端口）发送ajax请求时会被浏览器拦截，这就是跨域问题。

比如，前端页面的地址是 `http://localhost:5173`，后端接口的地址是 `http://localhost:8080`，当浏览器启动时，会先发送请求到前端地址，得到注册页面
当用户点击注册时，会发送请求到后端地址，这时浏览器会拦截这个请求，因为这是跨域请求。

### 解决方法
一般是用代理解决跨域问题，即在前端项目中配置代理，将请求转发到后端接口地址。步骤如下：

1. 在vite.config.js中配置代理
```
server: {
    proxy: {
      "/api": {//获取路径中包含/api的请求，将其代理到http://localhost:8080
        target: "http://localhost:8080",
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, ""), //重写路径，将路径中的/api替换为空
      },
    },
  }
```
2. 将axios的baseURL设置为`/api`，然后定义axios实例
```
const baseURL = "/api"; //前端代理地址
const instance = axios.create({ baseURL });
```

## vue的路由

路由的含义是根据不同的url地址，返回不同的内容给用户，他的实现原理是监听url地址的变化，然后根据url地址的变化返回不同的内容给用户
他的优点是当用户在应用中浏览不同页面时，URL 会随之更新，但页面不需要从服务器重新加载，这样就可以实现单页应用

### vue中路由的使用步骤

- 安装vue-router
- 在src/router/index.js中配置路由

示例:
```
import { createRouter, createWebHistory } from "vue-router";

//导入组件
import LoginVue from "@/views/Login.vue";
import LayoutVue from "@/views/Layout.vue";
import ArticleCategoryVue from "@/views/article/ArticleCategory.vue";
import ArticleManageVue from "@/views/article/ArticleManage.vue";
import UserAvatarVue from "@/views/user/UserAvatar.vue";
import UserInfoVue from "@/views/user/UserInfo.vue";
import UserResetPasswordVue from "@/views/user/UserResetPassword.vue";

//定义路由关系
const routes = [
  {
    path: "/login",
    component: LoginVue,
  },
  {
    path: "/",
    component: LayoutVue,
    redirect: "/article/category",
    //子路由，访问子路由时，父路由的内容会先被渲染，然后子路由的内容会被渲染到父路由的内容上
    //子路由的path不需要加/，会自动拼接到父路由的path后面
    children: [
      {
        path: "article/category",
        component: ArticleCategoryVue,
      },
      {
        path: "article/manage",
        component: ArticleManageVue,
      },
      {
        path: "user/avatar",
        component: UserAvatarVue,
      },
      {
        path: "user/info",
        component: UserInfoVue,
      },
      {
        path: "user/resetpwd",
        component: UserResetPasswordVue,
      },
    ],
  },
];

//创建路由
const router = createRouter({ history: createWebHistory(), routes: routes });
export default router;
```

- 在main.js中引入路由
```
import router from "@/router";
app.use(router);
```

- 在App.vue中使用路由
```
<template>
  <router-view></router-view>
</template>
```

### 路由主动切换组件

实际应用中，vue文件和js文件中路由主动跳转方式是不同的

- vue文件中

示例：
```
import { useRouter } from "vue-router";
const router = useRouter();
router.push("/login");
```

- js文件中，没有setup，说明js不是vue组件，因此不能用import {useRouter} from 'vue-router'来获取router

示例：
```
import router from "@/router";
router.push("/login");
```

## vue中使用pinia及其持久化

pinia是vue的专属状态管理库，它允许跨组件或页面共享状态，比如在登陆页面登陆成功后，可以将用户信息存储在pinia中，然后其他页面从pinia中获取用户信息。

### pinia使用步骤

- 在main.js中引入pinia

```
import { createPinia } from "pinia";
const pinia = createPinia();
app.use(pinia);
```

- 在src/stores/token.js中定义pinia的store

```
//定义关于counter的store
import {defineStore} from 'pinia'
const useCounter = defineStore("counter",{
    state:() => ({
        count:66,
    }),
    
    getters: {

  	},

  	actions: {

  	}
})

//导出这个useCounter模块
export default useCounter
```
defineStore 是需要传参数的，其中第一个参数是id，就是一个唯一的值，简单点说就可以理解成是一个命名空间。
第二个参数就是一个对象，里面有三个模块需要处理，第一个是 state，第二个是 getters，第三个是 actions。


### pinia持久化

>pinia默认是内存存储，当刷新浏览器时会丢失数据，所以需要利用persist插件实现持久化

- 引入pinia-persistedstate-plugin插件
```
import { createPersistedState } from "pinia-persistedstate-plugin"; 
pinia.use(createPersistedState());
```

- 设置persisted为true
```
import { defineStore } from "pinia";
//defineStore函数接受二个参数，第一个参数是store的名字，第二个参数是一个函数，定义了关于token的一些操作
export const useTokenStore = defineStore("token", {
  state: () => ({
    //state，一般定义要存储的数据，state的数据本身就是响应式的
    token: "",
  }),
  actions: {
    //actions，定义一些操作
    setToken(newToken) {
      this.token = newToken;
    },
    removeToken() {
      this.token = "";
    },
  },
  //persist为true，开启持久化
  persist: true,
});
```
# 微服务部分

## 注册中心

### 服务治理基础知识
服务治理中的三个角色：
- 服务提供者：暴露服务接口，供其他服务调用
- 服务消费者：调用其他服务提供的接口
- 注册中心：记录并监控微服务各实例的状态，推送服务变更信息

消费者如何知道服务提供者的地址？
- 服务提供者启动时向注册中心注册服务信息，消费者从注册中心订阅和拉取服务信息

消费者如何知道服务状态变更？
- 服务提供者通过心跳机制向注册中心报告健康状态，当“心跳”异常时注册中心会剔除异常服务，并通知订阅了该服务的消费者

当服务提供者有多个实例时，如何选择？
- 通过负载均衡算法选择一个服务提供者

### nacos基础知识

Nacos是阿里旗下的一款开源产品，它主要是针对微服务架构中的服务发现、配置管理、服务治理的综合型解决方案；简单来说 Nacos 就是注册中心 + 配置中心的组合，致力于帮助您发现、配置和管理微服务，提供简单易用的特性集，帮助您快速实现动态服务发现、服务配置、服务元数据及流量管理。

配置方式：
一般会把所有微服务的共享的配置，如jdbc、logger、mq等配置放在nacos中，然后由微服务拉取。

- 在nacos控制台新建共享配置，如shared-jdbc.yaml
- 接下来，要在微服务拉取共享配置。将拉取到的共享配置与本地的application.yaml配置合并，完成项目上下文的初始化。

需要注意的是，拉取Nacos配置是SpringCloud上下文（ApplicationContext）初始化时处理的。然后才会初始化SpringBoot上下文，去读取application.yaml。

如果将nacos配置放在application.yaml中，那么在项目引导阶段就无法拉取nacos中的配置了。

因此，将nacos地址配置到bootstrap.yaml中，那么在项目引导阶段就可以拉取nacos中的共享配置了。

配置示例：
```
spring:
  application:
    name: item-service # 服务名称
  profiles:
    active: dev
  cloud:
    nacos:
      server-addr: 192.168.56.101:8848 # nacos地址
      config:
        file-extension: yaml # 文件后缀名
        shared-configs: # 共享配置
          - dataId: shared-jdbc.yaml
          - dataId: shared-log.yaml
          - dataId: shared-swagger.yaml
          - dataId: shared-seata.yaml #
          - dataId: shared-mq.yaml
```


### 环境隔离
实际开发中，通常会有多个环境，如开发环境、测试环境、发布环境等，不同的环境需要隔离。

nacos中，主要分为namespace, group, service三级：
- namespace：用于隔离不同环境，默认为空（public）
- group：服务分组，用于区分不同的服务，默认为DEFAULT_GROUP
- service：服务名，用于标识服务

配置namespace：
- 在nacos控制台中创建namespace，如dev、test、prod
- 在nacos配置文件中config或discovery字段下指定namespace, 如`namespace: {namespaceId}`

### 服务分级模型
大厂的服务可能部署在多个机房，物理上被隔离为多个集群，nacos可以实现对集群的划分，实现服务分级模型。

nacos的分级分为服务，集群，实例三级。
比如，某个服务的实例有很多个，分布在全国各地，可以将这些实例划分为多个集群，如华北集群、华南集群、华东集群等，每个集群下有多个实例。

具体来说，nacos的注册表是一个Map结构`Map<String1, Map<String2, Map<String3, Cluster<Set<Instance>>>>>`
- String1：namespace名称
- String2：服务名称（含分组信息）
- String3：集群名称

## OpenFeign
利用Nacos实现服务治理和发现后，需要利用RestTemplate实现服务的远程调用，步骤如下：
- 获取服务名称
- 根据服务名称获取服务实例列表
- 利用负载均衡算法选择一个服务实例
- 重构请求的url，通过RestTemplate发送请求

但是远程调用的代码太复杂，OpenFeign可以简化远程调用的代码。

### 基本用法
OpenFeign的使用步骤如下：
- 引入OpenFeign依赖
```
  <!--openFeign-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-openfeign</artifactId>
  </dependency>
  <!--负载均衡器，早期是Ribbon，新版为loadbalancer--> 
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-loadbalancer</artifactId>
  </dependency>
```

- 在对应服务的启动类上添加`@EnableFeignClients`注解
- 编写OpenFeign客户端接口
```
@FeignClient(value = "item-service",fallbackFactory = ItemClientFallbackFactory.class)
public interface ItemClient {
    @GetMapping("/items")
    List<ItemDTO> queryItemByIds(@RequestParam("ids") Collection<Long> ids);
    @PutMapping("/items/stock/deduct")
    void deductStock(@RequestBody List<OrderDetailDTO> items);
    @PutMapping("/items/stock/restore")
    void restoreStock(@RequestBody List<OrderDetailDTO> items);
}
```
注意，接口内的方法和对应的Controller方法的参数、返回值、请求方式、请求路径等要一致
- 在需要的地方注入OpenFeign客户端接口，调用接口方法

### 连接池

Feign底层发起http请求，依赖于其它的框架。其底层支持的http客户端实现包括：
- HttpURLConnection：默认实现，不支持连接池
- Apache HttpClient ：支持连接池
- OKHttp：支持连接池

支持连接池的优点是可以复用连接，减少连接的创建和销毁，提高性能，一般会使用OKHttp。一般两步即可：
- 引入OKHttp依赖
```
<!--OK http 的依赖 -->
<dependency>
    <groupId>io.github.openfeign</groupId>
    <artifactId>feign-okhttp</artifactId>
</dependency>
```
- 在配置文件中配置使用OKHttp
```
feign:
  okhttp:
    enabled: true # 开启OKHttp功能
``` 
### 负载均衡
OpenFeign默认的负载均衡策略是轮询策略，在内部维护一个自增的计数器（达到INT最大值时归零），每次调用时，选择一个服务实例。
这样的缺点是，当服务实例的数量不均匀时，轮询策略可能会导致某些实例的负载过高，而某些实例的负载过低。

内置的负载均衡策略有三种：
- RoundRobinLoadBalancer：轮询策略
- RandomLoadBalancer：随机策略
- NacosLoadBalancer：优先在本集群内选择实例（访问快），根据不同实例的权重（nacos控制台设置权重），随机选择实例



修改默认的负载均衡策略，需要自定义一个返回ReactorLoadBalancer的Bean，具体步骤如下：
- 在对应的服务中创建一个配置类，返回ReactorLoadBalancer的Bean
```
public class OpenFeignConfig {
    ##修改为NacosLoadBalancer
    @Bean
    public ReactorLoadBalancer<ServiceInstance> reactorServiceInstanceLoadBalancer(
            Environment environment, NacosDiscoveryProperties properties,
            LoadBalancerClientFactory loadBalancerClientFactory) {
        String name = environment.getProperty(LoadBalancerClientFactory.PROPERTY_NAME);
        return new NacosLoadBalancer(
                loadBalancerClientFactory.getLazyProvider(name, ServiceInstanceListSupplier.class), name, properties);
    }

}
```
注意，这个配置类**不要加@Configuration注解**，也不要被SpringBootApplication扫描到。而是在启动类上通过注解来声明这个配置

全局配置：`@LoadBalancerClients(defaultConfiguration = OpenFeignConfig.class)`，对所有服务生效
局部配置：`@LoadBalancerClients({@LoadBalancerClient(value = "item-service", configuration = OpenFeignConfig.class)})`，仅对某个服务生效

## 网关

网关就是网络的关口。数据在网络间传输，从一个网络传输到另一网络时就需要经过网关来做数据的路由和转发以及数据安全的校验。

一般单体项目拆分成各种微服务后，每个服务都有自己的端口号，这样会导致前端请求时需要知道每个服务的端口号，这样不利于维护，因此需要一个网关来统一管理请求。

网关在微服务架构中的作用：
- 网关可以做安全控制，也就是**登录身份校验**，校验通过才放行
- 通过认证后，网关再根据请求判断应该访问哪个微服务，将请求转发过去

### 基本用法
利用网关实现请求路由。由于网关本身也是一个独立的微服务，因此也需要创建一个模块开发功能。大概步骤如下：
- 创建网关微服务，引入依赖
- 编写启动类
- 在application.yaml中配置网关路由
```
server:
  port: 8080
spring:
  application:
    name: gateway
  cloud:
    nacos:
      server-addr: 192.168.150.101:8848
    gateway:
      routes:
        - id: item # 路由规则id，自定义，唯一
          uri: lb://item-service # 路由的目标服务，lb代表负载均衡，会从注册中心拉取服务列表
          predicates: # 路由断言，判断当前请求是否符合当前规则，符合则路由到目标服务
            - Path=/items/**,/search/** # 这里是以请求路径作为判断规则
        - id: cart
          uri: lb://cart-service
          predicates:
            - Path=/carts/**
        - id: user
          uri: lb://user-service
          predicates:
            - Path=/users/**,/addresses/**
        - id: trade
          uri: lb://trade-service
          predicates:
            - Path=/orders/**
        - id: pay
          uri: lb://pay-service
          predicates:
            - Path=/pay-orders/**
```
四个属性含义如下：
- id：路由的唯一标示
- predicates：路由断言，其实就是匹配条件
- filters：路由过滤器，对请求或响应做特殊处理
- uri：路由目标地址，lb://代表负载均衡，从注册中心获取目标微服务的实例列表，并且负载均衡选择一个访问。

### 网关实现登陆校验
登录是基于JWT来实现的，校验JWT的算法复杂，而且需要用到秘钥。如果每个微服务都去做登录校验，这就存在着两大问题：
- 每个微服务都需要知道JWT的秘钥，不安全
- 每个微服务重复编写登录校验代码、权限校验代码，麻烦

网关是所有微服务的入口，一切请求都需要先经过网关。完全可以把登录校验的工作放到网关去做：
- 只需要在网关和用户服务保存秘钥
- 只需要在网关开发登录校验功能

不过，这里存在几个问题：

#### 如何在转发之前做登录校验？

网关处理请求的流程如下：

![](/img/gateway.png)

1. 客户端请求进入网关后由HandlerMapping对请求做判断，找到与当前请求匹配的路由规则（Route），然后将请求交给WebHandler去处理。
2. WebHandler则会加载当前路由下需要执行的过滤器链（Filter chain），然后按照顺序逐一执行过滤器（后面称为Filter）。
3. 图中Filter被虚线分为左右两部分，是因为Filter内部的逻辑分为pre和post两部分，分别会在请求路由到微服务之前和之后被执行。
4. 只有所有Filter的pre逻辑都依次顺序执行通过后，请求才会被路由到微服务。
5. 微服务返回结果后，再倒序执行Filter的post逻辑。
6. 最终把响应结果返回。

最终请求转发是有一个名为NettyRoutingFilter的过滤器来执行的，而且这个过滤器是整个过滤器链中顺序最靠后的一个。
如果我们能够定义一个过滤器，在其中实现登录校验逻辑，并且将过滤器执行顺序定义到NettyRoutingFilter之前，这就符合我们的需求了！

网关过滤器链中的过滤器有两种：
- GatewayFilter：路由过滤器，作用范围比较灵活，可以是任意指定的路由Route. 
- GlobalFilter：全局过滤器，作用范围是所有路由，不可配置。

接下来以GlobalFilter为例，实现登录校验功能:
```
@Component
@RequiredArgsConstructor
public class AuthGlobalFilter implements GlobalFilter, Ordered {

    private final AuthProperties authProperties;
    private final JwtTool jwtTool;
    private final AntPathMatcher antPathMatcher = new AntPathMatcher();

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        //获取request
        ServerHttpRequest request = exchange.getRequest();
        //判断是否需要登录校验
        if (isExclude(request.getPath().toString())) {
            return chain.filter(exchange);
        }
        //获取token
        String token = null;
        List<String> authorizations = request.getHeaders().get("authorization");
        if (authorizations != null && !authorizations.isEmpty()) {
            token = authorizations.get(0);
        }
        //校验解析token
        Long userId = null;
        try {
            userId = jwtTool.parseToken(token);
        } catch (Exception e) {
            ServerHttpResponse response = exchange.getResponse();
            response.setStatusCode(HttpStatus.UNAUTHORIZED);
            return response.setComplete();
        }
        //通过请求头传递用户信息
        String userInfo = userId.toString();
        ServerWebExchange ex = exchange.mutate()
                .request(builder -> builder.header("userInfo", userInfo))
                .build();
        return chain.filter(ex);
    }

    private boolean isExclude(String string) {
        for (String pathPattern : authProperties.getExcludePaths()) {
            if (antPathMatcher.match(pathPattern, string)) {
                return true;
            }
        }
        return false;
    }

    //设置优先级，确保该过滤器在NettyRoutingFilter之前执行，越小优先级越高
    @Override
    public int getOrder() {
        return 0;
    }
}
```
#### 网关登陆校验之后，如何将用户信息传递给微服务？
由于网关发送请求到微服务依然采用的是Http请求，因此我们可以将用户信息以请求头的方式传递到下游微服务。
然后微服务可以从请求头中获取登录用户信息。考虑到微服务内部可能很多地方都需要用到登录用户信息，因此我们可以利用SpringMVC的拦截器来实现登录用户信息获取，并存入ThreadLocal，方便后续使用。
具体步骤如下：
- 在网关的过滤器中将用户信息放入请求头
```
//通过请求头传递用户信息
        String userInfo = userId.toString();
        ServerWebExchange ex = exchange.mutate()
                .request(builder -> builder.header("userInfo", userInfo))
                .build();
        return chain.filter(ex);
```
- 在通用微服务中定义一个拦截器，从请求头中获取用户信息
```
public class UserInfoInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        //获取登录用户信息
        String userInfo = request.getHeader("userInfo");
        //判断是否为空，如果不为空存入ThreadLocal
        if(StrUtil.isNotBlank(userInfo)){
            UserContext.setUser(Long.valueOf(userInfo));
        }
        //因为网关已经做过了token校验，所以这里不需要再校验
        return true;
    }
    
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        //清除ThreadLocal中的用户信息
        UserContext.removeUser();
    }

}
```

- 在通用微服务配置类中，添加拦截器，为了只对微服务生效，仅在SpringMVC中添加拦截器，添加注解`@ConditionalOnClass(DispatcherServlet.class)`
```
@Configuration
@ConditionalOnClass(DispatcherServlet.class)
public class MvcConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new UserInfoInterceptor());
    }
}
```
- 在文件中设置自动配置（参考自动配置原理）

- 其他微服务引入通用微服务的依赖

#### 微服务之间也会相互调用，这种调用不经过网关，又该如何传递用户信息？
要想实现微服务之间的用户信息传递，就必须在微服务发起调用时把用户信息存入请求头。

为了让每一个由OpenFeign发起的请求自动携带登录用户信息，这里要借助Feign中提供的一个拦截器接口：feign.RequestInterceptor

配置类中添加一个Bean，将用户信息存入请求头：
```
@Bean
public RequestInterceptor userInfoRequestInterceptor(){
    return new RequestInterceptor() {
        @Override
        public void apply(RequestTemplate template) {
            // 获取登录用户
            Long userId = UserContext.getUser();
            if(userId == null) {
                // 如果为空则直接跳过
                return;
            }
            // 如果不为空则放入请求头中，传递给下游微服务
            template.header("user-info", userId.toString());
        }
    };
}
```
### 动态路由
网关的路由配置全部是在项目启动的时候加载，并且一经加载就会缓存到内存中的路由表内（一个Map），不会改变，也不会监听路由变更。要修改路由配置，需要重启网关。

实现动态路由，需要把路由信息保存到nacos，当路由配置更新时，推送最新路由信息到网关，实现实时更新路由表，步骤如下：
- 在nacos控制台中添加路由表，类型为json格式
- 在原始网关配置文件application.yml中，删除路由配置
- 引入nacos配置管理依赖
- 监听nacos配置变更，更新路由表
```
@Slf4j
@Component
@RequiredArgsConstructor
public class DynamicRouteLoader {
    private final RouteDefinitionWriter routeDefinitionWriter;
    private final NacosConfigManager nacosConfigManager;
    private final String dataId = "gateway-routes.json";
    private final String group = "DEFAULT_GROUP";
    private final Set<String> routeIds = new HashSet<>();

    @PostConstruct // 在构造函数执行完之后执行
    public void initRouteConfigListener() throws NacosException {
        //拉取配置，添加监听器
        String configInfo = nacosConfigManager.getConfigService()
                .getConfigAndSignListener(dataId, group, 5000, new Listener() {
                    @Override
                    public Executor getExecutor() {
                        return null;
                    }

                    @Override
                    public void receiveConfigInfo(String configInfo) {
                        //监听到配置变化后，更新路由表
                        updateRouteConfig(configInfo);
                    }
                });
        //第一次拉取配置后，更新路由表
        updateRouteConfig(configInfo);
    }

    //更新路由表
    public void updateRouteConfig(String configInfo) {
        log.debug("监听到配置变化，更新路由表{}", configInfo);
        //解析配置信息
        List<RouteDefinition> routeDefinitions = JSONUtil.toList(configInfo, RouteDefinition.class);
        //清空路由表
        for (String id : routeIds) {
            routeDefinitionWriter.delete(Mono.just(id)).subscribe();
        }
        routeIds.clear();
        //判断是否需要更新路由表
        if (CollUtil.isEmpty(routeDefinitions)) {
            return;
        }
        //更新路由表
        routeDefinitions.forEach(routeDefinition -> {
            // 3.1.更新路由
            routeDefinitionWriter.save(Mono.just(routeDefinition)).subscribe();
            // 3.2.记录路由id，方便将来删除
            routeIds.add(routeDefinition.getId());
        });
    }
}
```
## 微服务保护
雪崩问题：微服务调用链路中某个微服务出现故障，引起整个链路中所有微服务不可用，这就是雪崩
雪崩问题出现的原因主要有：
- 微服务相互调用，服务提供者出现故障
- 服务调用者没有做好异常处理，导致自身故障
- 调用链所有服务级联故障，最终整个链路不可用

微服务保护的方案有很多，比如：
- 请求限流
- 线程隔离
- 服务熔断

这些方案或多或少都会导致服务的体验上略有下降

比如请求限流，降低了并发上限；
线程隔离，降低了可用资源数量；
服务熔断，降低了服务的完整度，部分服务变的不可用或弱可用。
因此这些方案都属于服务降级的方案。但通过这些方案，服务的健壮性得到了提升，

### 请求限流
服务故障最重要原因，就是并发太高！当然，接口的并发不是一直很高，而是突发的。因此请求限流，就是**限制或控制接口访问的并发流量**，避免服务因流量激增而出现故障。

Sentinel是阿里巴巴开源的一款服务保护框架，目前已经加入SpringCloudAlibaba中。

下面利用Sentinel实现请求限流：
- 在相应的微服务中引入Sentinel依赖
- 在配置文件中添加Sentinel配置
```
spring:
  cloud:
    sentinel:
      transport:
        dashboard: localhost:8090
      http-method-specify: true # 开启请求方式前缀
```
- 打开Sentinel控制台，查看簇点链路，查看每个接口的访问情况。
- 在簇点链路中点击流控，设置流控规则，如QPS为10
### 线程隔离
限流可以降低服务器压力，尽量减少因并发流量引起的服务故障的概率，但并不能完全避免服务故障。一旦某个服务出现故障，我们必须隔离对这个服务的调用，避免发生雪崩。

比如，购物车服务运行中可能涉及到商品服务，商品会占用一些线程资源，为防止商品服务故障导致整个购物车服务不可用，可以限制商品服务可用的线程资源。

步骤如下：
- 开启feign对sentinel的支持
```
feign:
  sentinel:
    enabled: true # 开启feign对sentinel的支持
```
- 打开Sentinel控制台,打开簇点链路，点击流控
- 设置设置并发线程数，如10
### 服务熔断
线程隔离限定了线程数，导致接口吞吐能力有限，会产生以下问题：
1. 超出的QPS上限的请求就只能抛出异常，从而导致购物车的查询失败。但从业务角度来说，即便没有查询到最新的商品信息，购物车也应该展示给用户，用户体验更好。也就是给查询失败设置一个降级处理逻辑。
2. 由于查询商品的延迟较高（模拟的500ms），从而导致查询购物车的响应时间也变的很长。这样不仅拖慢了购物车服务，消耗了购物车服务的更多资源，而且用户体验也很差。对于商品服务这种不太健康的接口，我们应该直接停止调用，直接走降级逻辑，避免影响到当前服务。也就是将商品查询接口熔断。

编写降级逻辑一般通过FallbackFactory实现，步骤如下：
- 定义降级处理类，实现FallbackFactory接口
```
@Slf4j
public class ItemClientFallbackFactory implements FallbackFactory<ItemClient> {
    @Override
    public ItemClient create(Throwable cause) {
        return new ItemClient() {
            @Override
            public List<ItemDTO> queryItemByIds(Collection<Long> ids) {
                log.error("查询商品失败", cause);
                return CollUtils.emptyList();
            }

            @Override
            public void deductStock(List<OrderDetailDTO> items) {
                log.error("扣减库存失败", cause);
                throw new RuntimeException(cause);
            }

            @Override
            public void restoreStock(List<OrderDetailDTO> items) {
                log.error("恢复库存失败", cause);
                throw new RuntimeException(cause);
            }
        };
    }
}
```
- 在配置类中定义Bean
```
@Bean
    public ItemClientFallbackFactory itemClientFallbackFactory() {
        return new ItemClientFallbackFactory();
    }
```
- ItemClient接口中使用ItemClientFallbackFactory
```
@FeignClient(value = "item-service",fallbackFactory = ItemClientFallbackFactory.class)
public interface ItemClient {
    @GetMapping("/items")
    List<ItemDTO> queryItemByIds(@RequestParam("ids") Collection<Long> ids);
    @PutMapping("/items/stock/deduct")
    void deductStock(@RequestBody List<OrderDetailDTO> items);
    @PutMapping("/items/stock/restore")
    void restoreStock(@RequestBody List<OrderDetailDTO> items);
}
```
编写完降级逻辑后，还需要在Sentinel控制台中配置熔断规则

Sentinel中的断路器不仅可以统计某个接口的**慢请求比例**，还可以统计**异常请求**比例。
当这些比例超出阈值时，就会熔断该接口，即拦截访问该接口的一切请求，降级处理；当该接口恢复正常时，再放行对于该接口的请求。
具体规则在控制台中簇点链路中进行设置即可。

### 不同线程隔离方案对比
线程隔离有两种实现方式：
- 线程池隔离（Hystix默认采用）
- 信号量隔离（Sentinel默认采用）

线程池隔离会为每一个被隔离的业务创建一个线程池，从而确保每个业务分配的线程资源不会超出上限，具有更好的隔离性。缺点是带来了额外的CPU开销，性能一般
信号量隔离则是通过信号量来限制并发数，性能更好但是隔离性不如线程池隔离。

### 计数算法
在熔断功能中，需要统计异常请求或慢请求比例，也就是计数。在限流的时候，要统计每秒钟的QPS，同样是计数。可见计数算法在熔断限流中的应用非常多。常见的计数算法有以下几种：
- 固定窗口计数算法：将时间划分为固定的时间窗口，比如1s，如果某个窗口内的请求超过了阈值则拒绝新加入的请求。但是只能统计当前某1个时间窗的请求数量是否到达阈值，无法结合前后的时间窗的数据做综合统计。
所以仍然存在超过QPS的情况。
- 滑动窗口计数：滑动时间窗口算法中只包含1个固定跨度的窗口，但窗口是可移动的，窗口内可分为多个区间，每个区间的请求都会被统计，划分的区间越多，这种统计就越准确（否则容易超QPS）。
窗口会随着当前请求所在时间currentTime移动，窗口范围从currentTime-Interval时刻之后的第一个时区开始，到currentTime所在时区结束。
- 令牌桶算法：-以固定的速率生成令牌，存入令牌桶中，如果令牌桶满了以后，多余令牌丢弃。请求进入后，必须先尝试从桶中获取令牌，获取到令牌后才可以被处理。如果令牌桶中没有令牌，则请求等待或丢弃
注意，使用令牌桶算法时，尽量不要将令牌上限设定到服务能承受的QPS上限。而是预留一定的波动空间，这样才能应对突发流量。
- 漏桶算法：请求到达后不是直接处理，而是先放入一个队列。而后以固定的速率从队列中取出并处理请求。之所以叫漏桶算法，就是把请求看做水，队列看做是一个漏了的桶。如果桶满了则会拒绝新来的请求。
对突发流量的应对能力更好一些。
## 分布式事务

整个业务中，各个本地事务是有关联的。因此每个微服务的本地事务，也可以称为分支事务。多个有关联的分支事务一起就组成了全局事务。我们必须保证整个全局事务同时成功或失败。

比如商品添加购物车后，进入下单界面，此时如果将后台数据库中对应商品库存改为0，那么点击下单会下单失败，但是回到购物车界面，发现购物车中的商品已经清除了，说明购物服务对其他服务的异常没有感知，没有保证事务的一致性。

解决分布式事务一般采用Seata，Seata中有三个重要的角色：
-  TC (Transaction Coordinator) - 事务协调者：维护全局和分支事务的状态，协调全局事务提交或回滚。 
-  TM (Transaction Manager) - 事务管理器：定义全局事务的范围、开始全局事务、提交或回滚全局事务。 
-  RM (Resource Manager) - 资源管理器：管理分支事务，与TC交谈以注册分支事务和报告分支事务的状态，并驱动分支事务提交或回滚。

其中，TM和RM可以理解为Seata的客户端部分，引入到参与事务的微服务依赖中即可。将来TM和RM就会协助微服务，实现本地分支事务与TC之间交互，实现事务的提交或回滚。
而TC服务则是事务协调中心，是一个独立的微服务，需要单独部署。

### Seata的基本用法
- 首先，在服务端部署TC
- 在微服务中引入Seata的依赖，在配置文件中配置Seata
- seata的客户端在解决分布式事务的时候需要记录一些中间数据，因此还需要准备数据表。
- 在对应serviceimpl的方法上添加@GlobalTransactional注解，@GlobalTransactional注解就是在标记事务的起点，将来TM就会基于这个方法判断全局事务范围，初始化全局事务。

此外Seata支持四种不同的分布式事务解决方案：
- XA
- TCC
- AT
- SAGA

下面主要介绍XA和AT两种解决方案。

### XA模式
A是一种规范，目前主流数据库都实现了这种规范，实现的原理都是基于两阶段提交。

一阶段：
- TM发起并注册全局事务到TC
- TM调用分支事务
- RM注册分支事务到TC
- 本地事务执行完sql后**不提交**，而是报告事务执行状态给TC，继续持有数据库锁

二阶段：
- TC基于一阶段的报告检查事务执行状态
- 如果一阶段都成功，则通知所有事务参与者，提交事务
- 如果一阶段任意一个参与者失败，则通知所有事务参与者回滚事务

![](/img/seata1.png)

XA模式的优点是什么？
- 事务的强一致性，满足ACID原则
- 常用数据库都支持，实现简单，并且没有代码侵入

XA模式的缺点是什么？
- 因为一阶段需要锁定数据库资源，等待二阶段结束才释放，**性能较差**
- 依赖关系型数据库实现事务，有的数据库不支持XA

使用XA模式很简单，只需要在在配置文件中设置XA模式，然后添加@GlobalTransactional注解即可(上面已经用过)：
```
seata:
  data-source-proxy-mode: XA
```

### AT模式
AT模式是Seata的默认模式，AT模式是基于数据库的undo-log（数据快照）实现的，流程如下图：

![](/img/seata2.png)

与XA模式不同的是，AT模式**执行sql之前要保存数据快照**，然后执行sql，执行完**立刻提交**，并向TC报告状态
TC检查各事务状态，如果所有事务都成功则删除undo-log，如果有人失败则根据undo-log回滚事务

简述AT模式与XA模式最大的区别是什么？
- XA模式一阶段不提交事务，锁定资源；AT模式一阶段直接提交，不锁定资源。
- XA模式依赖数据库机制实现回滚；AT模式利用数据快照实现数据回滚。
- XA模式强一致；AT模式最终一致（会有短暂不一致）。

### AT模式的脏写问题
如果现在有两个事务，事务1保存快照后（此时i为100），修改i=90，然后提交并释放DB锁，此时事务2获取了DB锁，修改i=80，提交并释放DB锁，此时事务1回滚，i=100，那么事务2会**丢失更新**，也就是脏写。简单来说就是事务1回滚前，数据被事务2修改。

解决方法：
通过**全局锁**，在提交事务释放DB锁前，先获取全局锁，保证同一时间只有一个事务能够提交，这样就能避免脏写问题。

### TCC模式
TCC模式是Try-Confirm-Cancel的缩写，分为三个阶段：
- Try：检查和预留资源
- Confirm：执行业务和提交
- Cancel：释放预留资源

事务悬挂和空回滚：
假如一个分布式事务中包含两个分支事务，try阶段，一个分支成功执行，另一个分支事务阻塞，如果阻塞时间太长，可能导致全局事务超时而触发二阶段的cancel操作。两个分支事务都会执行cancel操作。

空回滚：其中一个分支是未执行try操作的，直接执行了cancel操作，反而会导致数据错误。因此，这种情况下，尽管cancel方法要执行，但其中不能做任何回滚操作，这就是空回滚。
事务悬挂：对于空回滚的分支事务，将来try方法阻塞结束依然会执行。但是整个全局事务其实已经结束了，因此永远不会再有confirm或cancel，也就是说这个事务执行了一半，处于悬挂状态，这就是业务悬挂问题。

以上问题都需要我们在编写try、cancel方法时处理。

TCC的优点是什么？
- 一阶段完成直接提交事务，释放数据库资源，性能好
- 相比AT模型，无需生成快照，无需使用全局锁，性能最强
- 不依赖数据库事务，而是依赖补偿操作，可以用于非事务型数据库

TCC的缺点是什么？
- 有代码侵入，需要人为编写try、Confirm和Cancel接口，太麻烦
- 软状态，事务是最终一致
- 需要考虑Confirm和Cancel的失败情况，做好幂等处理、事务悬挂和空回滚处理

## 消息队列 (MQ)

### 基础知识
引入MQ之前，微服务之间的调用都是基于OpenFeign的同步调用，存在以下问题：

- 拓展性差：每次有新的需求，现有支付逻辑都要跟着变化，拓展性不好
- 性能下降：调用者需要等待服务提供者执行完返回结果后，才能继续向下执行，也就是说每次远程调用，调用者都是阻塞等待状态。最终整个业务的响应时长就是每次远程调用的执行时长之和
- 级联失败：由于是基于OpenFeign调用交易服务、通知服务。当交易服务、通知服务出现故障时，整个事务都会回滚，交易失败。

异步调用方式其实就是基于消息通知的方式，一般包含三个角色：
- 消息发送者：投递消息的人，就是原来的调用方
- 消息Broker：管理、暂存、转发消息，你可以把它理解成微信服务器
- 消息接收者：接收和处理消息的人，就是原来的服务提供方

异步调用的优势包括：
- 耦合度更低
- 性能更好
- 业务拓展性强
- 故障隔离，避免级联失败

缺点是：
- 完全依赖于Broker的可靠性、安全性和性能
- 架构复杂，后期维护和调试麻烦

几种常见的MQ对比如下：
|               | RabbitMQ            | ActiveMQ                                                     | RocketMQ     | Kafka         |
| :------------: | :------------------: | :-----------------------------------------------------------: | :-----------: | :------------: |
| 公司/社区     | Rabbit              | Apache                                                       | 阿里         | Apache        |
| 开发语言      | Erlang              | Java                                                         | Java         | Scala&Java   |
| 协议支持      | AMQP, XMPP, SMTP, STOMP | OpenWire,STOMP, REST,XMPP,AMQP                            | 自定义协议   | 自定义协议    |
| 可用性        | 高                  | 一般                                                         | 高           | 高            |
| 单机吞吐量    | 一般                | 差                                                           | 高           | 非常高        |
| 消息延迟      | 微秒级              | 毫秒级                                                       | 毫秒级       | 毫秒以内      |
| 消息可靠性    | 高                  | 一般                                                         | 高           | 一般          |

### RabbitMQ基础知识

RabbitMQ主要有以下几个角色：
- publisher：生产者，也就是发送消息的一方
- consumer：消费者，也就是消费消息的一方
- queue：队列，存储消息。生产者投递的消息会暂存在消息队列中，等待消费者处理
- exchange：交换机，负责消息路由。生产者发送的消息由交换机决定投递到哪个队列。
- virtual host：虚拟主机，起到数据隔离的作用。每个虚拟主机相互独立，有各自的exchange、queue

上述内容都可以在RabbitMQ的管理控制台来管理

WorkQueues模型：
Work queues，任务模型。简单来说就是让多个消费者绑定到一个队列，共同消费队列中的消息。RabbitMQ**默认会将消息平均分发**给每个消费者。
这样的问题是处理消息快的消费者会快速消费完分配给自己所有消息，然后处于空闲状态，而处理慢的消费者则会消费的很慢。因此我们可以通过设置prefetch来限制每个消费者一次性获取的消息数量。
```
spring:
  rabbitmq:
    listener:
      simple:
        prefetch: 1 # 每次只能获取一条消息，处理完成才能获取下一个消息
```
这样设置以后，处理快的消费者会处理更多消息（能者多劳），总的处理时间会大大缩短。

### RabbitMQ交换机类型
交换机的作用是：
- 接收publisher发送的消息
- 将消息按照规则路由到与之绑定的队列
- 不能缓存消息，路由失败，消息丢失
- FanoutExchange的会将消息路由到每个绑定的队列


RabbitMQ的交换机类型有以下几种：
- Fanout：广播，将消息交给所有绑定到交换机的队列。
- Direct：订阅，基于RoutingKey（路由key）发送给订阅了消息的队列
- Topic：通配符订阅，与Direct类似，只不过RoutingKey可以使用通配符
- Headers：头匹配，基于MQ的消息头匹配，用的较少。

Direct交换机和Topic交换机的区别：
- Direct交换机是**完全匹配**，只有RoutingKey和BindingKey完全一致，才会将消息路由到队列，可以是多个单词、数字、符号等组成的字符串。
- Topic交换机接收的消息RoutingKey必须是多个单词，以 `.` 分割
- Topic交换机与队列绑定时的bindingKey可以指定通配符，`#`：代表0个或多个词，`*`：代表1个词

BindingKey和RoutingKey的区别：
- RoutingKey是**生产者发送消息时指定**的，用于交换机将消息路由到队列
- BindingKey是**消费者绑定队列时指定**的，用于交换机将消息路由到队列

### 声明队列和交换机
声明的方式有两种，通过Bean和通过注解。
通过Bean的方式比较麻烦，需要手动声明队列和交换机，然后绑定队列和交换机。
下面给出通过注解声明的例子：
使用前需要引入`spring-boot-starter-amqp`依赖
```
public class PayStatusListener {

    private final IOrderService orderService;

    @RabbitListener(bindings = @QueueBinding(
            value=@Queue(name = "trade.pay.success.queue",durable = "true"),
            exchange = @Exchange(name = "pay.direct",type = ExchangeTypes.DIRECT),
            key = {"pay.success"}
    ))
    public void listenPaySuccess(Long orderId) {
        //查询订单
        Order order = orderService.getById(orderId);
        //判断订单状态,仅处理未支付订单
        if(order==null || order.getStatus()!=1) {
            return;
        }
        //标记订单支付成功
        orderService.markOrderPaySuccess(orderId);
    }
}
```

发送消息：
```
rabbitTemplate.convertAndSend("pay.direct", "pay.success", po.getBizOrderNo());
```
### 消息转换器
SpringAMQP数据传输时，会把发送的消息序列化为字节发送给MQ，接收消息的时候，还会把字节反序列化为Java对象。
默认情况下Spring采用的序列化方式是JDK序列化。众所周知，JDK序列化存在下列问题：
- 数据体积过大
- 有安全漏洞
- 可读性差

我们希望消息体的体积更小、可读性更高，因此可以使用JSON方式来做序列化和反序列化。步骤如下：
- 在publisher和consumer中引入依赖
```
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
</dependency>
```
- 在publisher和consumer的启动类中添加Bean
```
@Bean
public MessageConverter messageConverter(){
    // 1.定义消息转换器
    Jackson2JsonMessageConverter jjmc = new Jackson2JsonMessageConverter();
    // 2.配置自动创建消息id，用于识别不同消息，也可以在业务中基于ID判断是否是重复消息
    jjmc.setCreateMessageIds(true);
    return jjmc;
}
```
### 发送者可靠性
RabbitMQ提供了发送者重连机制和发送者确认机制来保证发送者的可靠性。

发送者重连：当RabbitTemplate与MQ连接超时后，多次重试，可通过配置yaml文件启用。重试机制是阻塞式的重试，也就是说多次重试等待的过程中，当前线程是被阻塞的。
如果对于业务性能有要求，建议禁用重试机制。

发送者确认：生产者消息确认机制，包括Publisher Confirm和Publisher Return两种。在开启确认机制的情况下，当生产者发送消息给MQ后，MQ会根据消息处理的情况返回不同的回执。包括以下几种情况：
- 消息到达交换机，但是路由失败，会通过Publisher Return机制返回ACK，告知投递成功
- 临时消息到达交换机，并路由到队列，返回ACK，告知投递成功
- 持久消息到达交换机，并路由到队列完成持久化，返回ACK，告知投递成功
- 其他情况都会返回NACK，告知投递失败

总结就是只要消息到了交换机，就会返回ACK，但是不一定会投递到队列中。
开启生产者确认比较消耗MQ性能，一般不建议开启。

### MQ可靠性
默认情况下，MQ接收到的消息保存在内存中，如果MQ宕机，消息会丢失。为了保证可靠性，主要通过数据持久化和懒队列来实现。

数据持久化包括：
- 交换机持久化，默认就是持久化的
- 队列持久化，也是默认持久化的
- 消息持久化，需要在发送消息时设置

懒队列：
在默认情况下，RabbitMQ会将接收到的信息保存在内存中以降低消息收发的延迟。但在某些特殊情况下，这会导致消息积压，比如：
- 消费者宕机或出现网络故障
- 消息发送量激增，超过了消费者处理速度
- 消费者处理业务发生阻塞

一旦出现消息堆积问题，RabbitMQ的内存占用就会越来越高，直到触发内存预警上限。此时RabbitMQ会将内存消息刷到磁盘上，这个行为称为PageOut. 
PageOut会耗费一段时间，并且会阻塞队列进程。因此在这个过程中RabbitMQ不会再处理新的消息，生产者的所有请求都会被阻塞。

懒队列的特点是：
- 接收到消息后直接存入磁盘而非内存
- 消费者要消费消息时才会从磁盘中读取并加载到内存（也就是懒加载）
- 支持数百万条的消息存储

RabbitMQ 3.12版本之后，LazyQueue已经成为所有队列的默认格式。之前版本声明LazyQueue：
```
@RabbitListener(queuesToDeclare = @Queue(
        name = "lazy.queue",
        durable = "true",
        arguments = @Argument(name = "x-queue-mode", value = "lazy")
))
public void listenLazyQueue(String msg){
    log.info("接收到 lazy.queue的消息：{}", msg);
}
```
### 消费者可靠性

消费者确认机制：
为了确认消费者是否成功处理消息，RabbitMQ提供了消费者确认机制。当消费者处理消息结束后，应该向RabbitMQ发送一个回执，告知RabbitMQ自己消息处理状态。回执有三种可选值：
- ack：成功处理消息，RabbitMQ从队列中删除该消息
- nack：消息处理失败，RabbitMQ需要再次投递消息
- reject：消息处理失败并拒绝该消息，RabbitMQ从队列中删除该消息

一般reject方式用的较少，除非是消息格式有问题

SpringAMQP帮我们实现了消息确认。并允许我们通过配置文件设置ACK处理方式，有三种模式：
- none：不处理。即消息投递给消费者后立刻ack，消息会立刻从MQ删除。非常不安全，不建议使用
- manual：手动模式。需要自己在业务代码中调用api，发送ack或reject，存在业务入侵，但更灵活
- auto：自动模式。SpringAMQP利用AOP对我们的消息处理逻辑做了环绕增强，当业务正常执行时则自动返回ack. 当业务出现异常时，根据异常类型判断返回不同结果：
  - 如果是业务异常，会自动返回nack；
  - 如果是消息处理或校验异常，自动返回reject;


失败重试机制：
当消费者出现异常后，消息会不断requeue（重入队）到队列，再重新发送给消费者。如果消费者再次执行依然出错，消息会再次requeue到队列，再次投递，直到消息处理成功为止。
极端情况就是消费者一直无法执行成功，那么消息requeue就会无限循环，导致mq的消息处理飙升，带来不必要的压力

应对上述情况Spring又提供了消费者失败重试机制：在消费者出现异常时利用**本地重试**，而不是无限制的requeue到mq队列。例如
```
spring:
  rabbitmq:
    listener:
      simple:
        retry:
          enabled: true # 开启消费者失败重试
          initial-interval: 1000ms # 初识的失败等待时长为1秒
          multiplier: 1 # 失败的等待时长倍数，下次等待时长 = multiplier * last-interval
          max-attempts: 3 # 最大重试次数
          stateless: true # true无状态；false有状态。如果业务中包含事务，这里改为false
```
- 开启本地重试时，消息处理过程中抛出异常，不会requeue到队列，而是在消费者本地重试
- 重试达到最大次数后，Spring会返回reject，消息会被丢弃
对于失败的处理一共有三种策略：
-  RejectAndDontRequeueRecoverer：重试耗尽后，直接reject，丢弃消息。默认就是这种方式 
-  ImmediateRequeueMessageRecoverer：重试耗尽后，返回nack，消息重新入队 
-  RepublishMessageRecoverer：重试耗尽后，将失败消息投递到指定的交换机 

业务幂等性：
是指同一个业务，执行一次或多次对业务状态的影响是一致的。
数据的更新往往不是幂等的，如果重复执行可能造成不一样的后果。比如：
- 取消订单，恢复库存的业务。如果多次恢复就会出现库存重复增加的情况
- 退款业务。重复退款对商家而言会有经济损失。
保证消息处理的幂等性，这里给出两种方案：
- 唯一消息ID
    1. 每一条消息都生成一个唯一的id，与消息一起投递给消费者。
    2. 消费者接收到消息后处理自己的业务，业务处理成功后将消息ID保存到数据库
    3. 如果下次又收到相同消息，去数据库查询判断是否存在，存在则为重复消息放弃处理。
    4. 实现方式很简单，在Jackson的消息转换器里即可实现，见上面消息转换器部分
- 业务状态判断，基于业务本身的逻辑或状态来判断是否是重复的请求或消息

### 延迟消息
发送者发送消息后，消费者在指定时间后才收到消息。

假设现在有以下极端情况，用户下单后，进行支付服务，如果支付服务出现问题，导致支付服务无法通知交易服务修改订单状态，那么支付服务流水为已支付，交易服务的订单为未支付，状态不一致；如果用户没有支付，就会一直占有库存资源，导致其他客户无法正常交易

解决方法是，设置延迟消息，一段时间后通知交易服务查询支付状态即可。

实现延迟消息有死信交换机和延迟消息插件两种。

死信交换机：
当一个队列中的消息满足下列情况之一时，可以成为死信（dead letter）：
- 消费者使用basic.reject或 basic.nack声明消费失败，并且消息的requeue参数设置为false
- 消息是一个过期消息，超时无人消费
- 要投递的队列消息满了，无法投递
如果一个队列中的消息已经成为死信，并且这个队列通过dead-letter-exchange属性指定了一个交换机，那么队列中的死信就会投递到这个交换机中，而这个交换机就称为死信交换机（Dead Letter Exchange）。

延迟消息插件：
死信交换机的实现较繁琐，DelayExchange插件则更为便捷，步骤如下：
- 安装配置插件
- 声明延迟交换机，基于注解
```
@RabbitListener(bindings = @QueueBinding(
            value = @Queue(name = MQConstants.DELAY_EXCHANGE_NAME),
            exchange = @Exchange(name = MQConstants.DELAY_EXCHANGE_NAME, delayed = "true"),
            key = {MQConstants.DELAY_ORDER_KEY}
    ))
```
- 发送延迟消息
```
rabbitTemplate.convertAndSend(
                MQConstants.DELAY_EXCHANGE_NAME,
                MQConstants.DELAY_ORDER_KEY,
                order.getId(),
                message -> {
                    message.getMessageProperties().setDelay(1000 * 10 * 1);
                    return message;
                }
        );
```

综上，支付服务与交易服务之间的订单状态一致性是如何保证的？
- 首先，支付服务会正在用户支付成功以后利用MQ消息通知交易服务，完成订单状态同步。
- 其次，为了保证MQ消息的可靠性，我们采用了生产者确认机制、消费者确认、消费者失败重试等策略，确保消息投递的可靠性
- 最后，我们还在交易服务设置了定时任务，定期查询订单支付状态。这样即便MQ通知失败，还可以利用延迟任务作为兜底方案，确保订单支付状态的最终一致性。

## ElasticSearch

### 基础知识
数据库的模糊搜索存在以下问题：
- 数据库模糊查询不走索引，在数据量较大的时候，查询性能很差。
- 数据库的模糊搜索功能单一，必须恰好包含用户搜索的关键字。而在搜索引擎中，用户输入出现个别错字，或者用拼音搜索、同义词搜索都能正确匹配到数据。

Elasticsearch是由elastic公司开发的一套搜索引擎技术，它是elastic技术栈中的一部分。完整的技术栈包括：
- Elasticsearch：用于数据存储、计算和搜索
- Logstash/Beats：用于数据收集
- Kibana：用于数据可视化
整套技术栈被称为ELK，经常用来做日志收集、系统监控和状态分析等。

一般数据库搜索中，当搜索条件为模糊匹配时，由于索引无法生效，导致从索引查询退化为全表扫描，效率很差。
因此，正向索引适合于根据索引字段的精确搜索，不适合基于部分词条的模糊匹配。
elasticsearch之所以有如此高性能的搜索表现，正是得益于底层的倒排索引技术。

什么是***倒排索引***？
- 正向索引是最传统的，根据id索引的方式。但根据词条查询时，必须先逐条获取每个文档，然后判断文档中是否包含所需要的词条，是根据文档找词条的过程。 
- 而倒排索引则相反，是先找到用户要搜索的词条，根据词条得到保护词条的文档的id，然后根据id获取文档。是根据词条找文档的过程。
这里的文档就是数据库中的一行数据，词条就是分词后的词语。

创建倒排索引的过程：
- 分词：将文档中的词语进行分词，形成词条
- 建立倒排索引：将词条与文档id进行映射，形成倒排索引表

有了倒排索引，搜索流程就变成了：
- 用户输入搜索词条
- 对输入进行分词
- 根据分词结果在倒排索引表中查找文档id
- 根据文档id获取文档

一些**基础概念**对比：
| MySQL     | Elasticsearch | 说明                                                                                                                                                                     |
| --------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Table     | Index         | 索引(index)，就是文档的集合，类似数据库的表(table)                                                                                                                          |
| Row       | Document      | 文档（Document），就是一条一条的数据，类似数据库中的行（Row），文档都是JSON格式                                                                                                 |
| Column    | Field         | 字段（Field），就是JSON文档中的字段，类似数据库中的列（Column）                                                                                                               |
| Schema    | Mapping       | Mapping（映射）是索引中文档的约束，例如字段类型约束。类似数据库的表结构（Schema）                                                                                                |
| SQL       | DSL           | DSL是elasticsearch提供的JSON风格的请求语句，用来操作elasticsearch，实现CRUD                                                                                                    |

Mapping是对索引库中文档的约束，常见的Mapping属性包括：
- type：字段数据类型，常见的简单类型有： 
  - 字符串：text（可分词的文本）、keyword（精确值，例如：品牌、国家、ip地址）
  - 数值：long、integer、short、byte、double、float、
  - 布尔：boolean
  - 日期：date
  - 对象：object
- index：是否创建索引，默认为true
- analyzer：使用哪种分词器
- properties：该字段的子字段
### Kibana中的增删改查
Kibana是一个开源的数据分析和可视化平台，可以让你在Elasticsearch上进行数据分析和搜索。

**索引库的增删改查**
- 创建索引库
```
PUT /索引库名称
{
  "mappings": {
    "properties": {
      "字段名":{
        "type": "text",
        "analyzer": "ik_smart"
      },
      "字段名2":{
        "type": "keyword",
        "index": "false"
      },
      "字段名3":{
        "properties": {
          "子字段": {
            "type": "keyword"
          }
        }
      },
      // ...略
    }
  }
}
```
- 查询索引库
```
GET /索引库名
```
- 删除索引库
```
DELETE /索引库名
```
- 修改索引库，索引库一旦创建，无法修改mapping，但可以添加新的字段
```
PUT /索引库名/_mapping
{
  "properties": {
    "新字段名":{
      "type": "integer"
    }
  }
}
```

**文档的增删改查**
- 新增文档
```
POST /索引库名/_doc/文档id
{
    "字段1": "值1",
    "字段2": "值2",
    "字段3": {
        "子属性1": "值3",
        "子属性2": "值4"
    },
}
```
- 查询文档
```
GET /{索引库名称}/_doc/{id}
```
- 删除文档
```
DELETE /{索引库名}/_doc/id值
```
- 修改文档有两种方式，全量修改直接覆盖原来的文档；局部修改，修改文档中的部分字段，
```
全量修改：与新增文档语法一样
局部修改：
POST /{索引库名}/_update/文档id
{
    "doc": {
         "字段名": "新的值",
    }
}
```

### DSL查询
Elasticsearch提供了基于JSON的DSL（Domain Specific Language）语句来定义**复杂的查询条件**
Elasticsearch的查询可以分为两大类：
- 叶子查询（Leaf query clauses）：一般是在特定的字段里查询特定值，属于简单查询，很少单独使用。
- 复合查询（Compound query clauses）：以逻辑方式组合多个叶子查询或者更改叶子查询的行为方式。

**叶子查询**
- 全文检索查询（Full Text Queries）：利用分词器对用户输入搜索条件先分词，得到词条，然后再利用倒排索引搜索词条。例如：
  - match：
  - multi_match
- 精确查询（Term-level queries）：不对用户输入搜索条件分词，根据字段内容精确值匹配。但只能查找keyword、数值、日期、boolean类型的字段。例如：
  - ids：根据文档id查询
  - term：精确匹配某个字段的值
  - range：范围查询
- 地理坐标查询：用于搜索地理位置，搜索方式很多，例如：
  - geo_bounding_box：按矩形搜索
  - geo_distance：按点和半径搜索

**全文检索查询**
- match查询，例如模糊搜索商品名称为苹果手机的商品
```
GET /{索引库名}/_search
{
  "query": {
    "match": {
      "name": "苹果手机"
    }
  }
}
```
- multi_match查询：可以指定多个字段进行搜索。例如模糊搜索商品名称和品牌中包含苹果手机的商品
```
GET /{索引库名}/_search
{
  "query": {
    "multi_match": {
      "query": "苹果手机",
      "fields": ["name", "brand"]
    }
  }
}
```

**精确查询**
- term查询：精确匹配某个字段的值，例如查询品牌为苹果的商品
```
GET /{索引库名}/_search
{
  "query": {
    "term": {
      "brand": {
        "value": "苹果"
      }
    }
  }
}
```
- range查询：范围查询，例如查询价格在100-500之间的商品
```
GET /{索引库名}/_search
{
  "query": {
    "range": {
      "price": {
        "gte": 100,
        "lte": 500
      }
    }
  }
}
```

**复合查询**
复合查询大致可以分为两类：
- 第一类：基于逻辑运算组合叶子查询，实现组合条件，例如
  - bool
- 第二类：基于某种算法修改查询时的文档相关性算分，从而改变文档排名。例如：
  - function_score
  - dis_max

**bool查询**
bool查询是最常用的复合查询，利用逻辑运算来组合一个或多个查询子句的组合。bool查询支持的逻辑运算有：
- must：必须匹配每个子查询，类似“与”
- should：选择性匹配子查询，类似“或”，满足相关项即可提升匹配分数
- must_not：必须不匹配，不参与算分，类似“非”
- filter：必须匹配，不参与算分

例如查询商品名称中包含手机，品牌最好为vivo或小米，价格大于等于1000且小于2500的商品
```
GET /items/_search
{
  "query": {
    "bool": {
      "must": [
        {"match": {"name": "手机"}}
      ],
      "should": [
        {"term": {"brand": { "value": "vivo" }}},
        {"term": {"brand": { "value": "小米" }}}
      ],
      "must_not": [
        {"range": {"price": {"gte": 2500}}}
      ],
      "filter": [
        {"range": {"price": {"gte": 1000}}}
      ]
    }
  }
}
```

**排序**
elasticsearch默认是根据相关度算分（_score）来排序，但是也支持自定义方式对搜索结果排序。不过分词字段无法排序，能参与排序字段类型有：keyword类型、数值类型、地理坐标类型、日期类型等。
例如按商品价格降序排序
```
GET /items/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "price": {
        "order": "desc"
      }
    }
  ]
}
```

**分页**
elasticsearch 默认情况下只返回top10的数据。而如果要查询更多数据就需要修改分页参数了。
基础分页通过修改from和size参数来实现，from表示从第几条开始，size表示返回多少条数据。
```
GET /items/_search
{
  "query": {
    "match_all": {}
  },
  "from": 0, // 分页开始的位置，默认为0
  "size": 10,  // 每页文档数量，默认10
  "sort": [
    {
      "price": {
        "order": "desc"
      }
    }
  ]
}
```
**深度分页问题**
elasticsearch的数据一般会采用分片存储，也就是把一个索引中的数据分成N份，存储到不同节点上。这种存储方式比较有利于数据扩展，但给分页带来了一些麻烦。
比如要查找所有数据中最小的前1000条数据，需要在每个分片上查找前1000条数据，然后再在所有分片上合并排序，最后返回前1000条数据。这个过程会消耗大量的时间和资源。

当查询分页深度较大时，汇总数据过多，对内存和CPU会产生非常大的压力。
在ES中，`from+ size` 超过10000的请求会被拒绝。

针对深度分页，elasticsearch提供了两种解决方案：
- search after：分页时需要排序，原理是从上一次的排序值开始，查询下一页数据。官方推荐使用的方式。
- scroll：原理将排序后的文档id形成快照，保存下来，基于快照做分页。官方已经不推荐使用。

search after的优点是支持深度分页，但是必须依赖排序字段，而且排序字段的值必须是全局唯一的，不支持跳页。

**高亮**
我们在百度搜索时，关键字会变成红色，比较醒目，这叫高亮显示，实现如下：
```
GET /{索引库名}/_search
{
  "query": {
    "match": {
      "搜索字段": "搜索关键字"
    }
  },
  "highlight": {
    "fields": {
      "高亮字段名称": {
        "pre_tags": "<em>",
        "post_tags": "</em>"
      }
    }
  }
}
```
**聚合**
聚合（aggregations）可以让我们极其方便的实现对数据的统计、分析、运算。
聚合常见的有三类：
-  桶（Bucket）聚合：用来对文档做分组 
  - TermAggregation：按照文档字段值分组，例如按照品牌值分组、按照国家分组
  - Date Histogram：按照日期阶梯分组，例如一周为一组，或者一月为一组
-  度量（Metric）聚合：用以计算一些值，比如：最大值、最小值、平均值等 
  - Avg：求平均值
  - Max：求最大值
  - Min：求最小值
  - Stats：同时求max、min、avg、sum等
-  管道（pipeline）聚合：其它聚合的结果为基础做进一步运算 

>注意：参加聚合的字段必须是keyword、日期、数值、布尔类型

Bucket聚合语法，查询商品按照品牌分组
```
GET /items/_search
{
  "size": 0, //size为0表示不返回文档，只返回聚合结果
  "aggs": {
    "brand_agg": { //聚合名称
      "terms": { //桶聚合用terms
        "field": "brand",
        "size": 20 //返回几个桶
      }
    }
  }
}
```
Metric聚合语法，比如查询商品按品牌分组后，查看价格的统计数据
```
GET /items/_search
{
  "size": 0,
  "aggs": {
    "brand_agg": {
      "terms": {
        "field": "brand",
        "size": 20
      },
      "aggs": { //子聚合
        "stats_agg": { //聚合名称
          "stats": { //Metric聚合用stats
            "field": "price" 
          }
        }
      }
    }
  }
}
```
### RestClient基础操作
在elasticsearch提供的API中，与elasticsearch一切交互都封装在一个名为RestHighLevelClient的类中，必须先完成这个对象的初始化，建立与elasticsearch的连接。
一些基本操作如下：

- 初始化RestHighLevelClient：
```
@BeforeEach
void setUp() {
    client = new RestHighLevelClient(RestClient.builder(
            HttpHost.create("http://192.168.56.101:9200")
//                HttpHost.create("http://192.168.56.101:9200"),
//                HttpHost.create("http://192.168.56.101:9200")
    ));
}

@AfterEach
void tearDown() throws IOException {
    if (client != null) {
        client.close();
    }
}
```

- 创建索引库
```
void createIndex() throws IOException {
        // 创建索引
        CreateIndexRequest request = new CreateIndexRequest("items");
        request.source(MAPPING_TEMPLATE, XContentType.JSON);
        client.indices().create(request, RequestOptions.DEFAULT);
    }

    private static final String MAPPING_TEMPLATE = "{\n" +
            "  \"mappings\": {\n" +
            "    \"properties\": {\n" +
            "      \"id\": {\n" +
            "        \"type\": \"keyword\"\n" +
            "      },\n" +
            "      \"name\":{\n" +
            "        \"type\": \"text\",\n" +
            "        \"analyzer\": \"ik_max_word\"\n" +
            "      },\n" +
            "      \"price\":{\n" +
            "        \"type\": \"integer\"\n" +
            "      },\n" +
            "      \"image\":{\n" +
            "        \"type\": \"keyword\",\n" +
            "        \"index\": false\n" +
            "      },\n" +
            "      \"category\":{\n" +
            "        \"type\": \"keyword\"\n" +
            "      },\n" +
            "      \"brand\":{\n" +
            "        \"type\": \"keyword\"\n" +
            "      },\n" +
            "      \"sold\":{\n" +
            "        \"type\": \"integer\"\n" +
            "      },\n" +
            "      \"commentCount\":{\n" +
            "        \"type\": \"integer\",\n" +
            "        \"index\": false\n" +
            "      },\n" +
            "      \"isAD\":{\n" +
            "        \"type\": \"boolean\"\n" +
            "      },\n" +
            "      \"updateTime\":{\n" +
            "        \"type\": \"date\"\n" +
            "      }\n" +
            "    }\n" +
            "  }\n" +
            "}";
```
- 删除索引库
```
void deleteIndex() throws IOException {
        DeleteIndexRequest request = new DeleteIndexRequest("items");
        client.indices().delete(request, RequestOptions.DEFAULT);
    }
```
- 新增文档
```
void createDoc() throws IOException {
        //准备数据
        Item item = itemService.getById(317578L);
        ItemDoc itemDoc = BeanUtils.copyProperties(item, ItemDoc.class);
        //准备request,IndexRequest即能创建文档，也能全量更新文档
        IndexRequest request = new IndexRequest("items").id(item.getId().toString());
        //准备请求参数
        request.source(JSONUtil.toJsonStr(itemDoc), XContentType.JSON);
        //发送请求
        client.index(request, RequestOptions.DEFAULT);
    }
```
- 查询文档
```
void getDoc() throws IOException {
        //准备request
        GetRequest request = new GetRequest("items").id("317578");
        //发送请求
        GetResponse response = client.get(request, RequestOptions.DEFAULT);
        //解析结果
        String source = response.getSourceAsString();
        ItemDoc doc = JSONUtil.toBean(source, ItemDoc.class);
        System.out.println("doc = " + doc);
    }
```
- 删除文档
```
void deleteDoc() throws IOException {
        DeleteRequest request = new DeleteRequest("items").id("317578");
        client.delete(request, RequestOptions.DEFAULT);
    }
```
- 修改文档
```
void updateDoc() throws IOException {
        //局部更新
        UpdateRequest request = new UpdateRequest("items", "317578");
        request.doc("price", 9999);
        client.update(request, RequestOptions.DEFAULT);
    }
```
- 批量新增文档
```
void testBulkDoc() throws IOException {
        int pageNo = 1, pageSize = 500;
        while (true) {
            //准备数据
            Page<Item> page = itemService.lambdaQuery()
                    .eq(Item::getStatus, 1)
                    .page(new Page<>(pageNo, pageSize));
            List<Item> records = page.getRecords();
            if (records == null || records.isEmpty()) {
                return;
            }
            BulkRequest request = new BulkRequest();
            //批量插入
            for (Item item : records) {
                request.add(new IndexRequest("items")
                        .id(item.getId().toString())
                        .source(JSONUtil.toJsonStr(BeanUtils.copyProperties(item, ItemDoc.class)), XContentType.JSON));
            }
            //发送请求
            client.bulk(request, RequestOptions.DEFAULT);
            pageNo++;
        }
    }
```
- DSL match查询
```
@Test
void testMatch() throws IOException {
    // 1.创建Request
    SearchRequest request = new SearchRequest("items");
    // 2.组织请求参数
    request.source().query(QueryBuilders.matchQuery("name", "脱脂牛奶"));
    // 3.发送请求
    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    // 4.解析响应
    handleResponse(response);
}
```
- DSL multi_match查询
```
@Test
void testMultiMatch() throws IOException {
    // 1.创建Request
    SearchRequest request = new SearchRequest("items");
    // 2.组织请求参数
    request.source().query(QueryBuilders.multiMatchQuery("脱脂牛奶", "name", "category"));
    // 3.发送请求
    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    // 4.解析响应
    handleResponse(response);
}
```
- DSL bool查询，查询商品名称包含脱脂牛奶，品牌为德亚，价格在5000-30000之间的商品
```
void testSearch() throws IOException {
        // 1.创建Request
        SearchRequest request = new SearchRequest("items");
        // 2.组织请求参数
        request.source().query(QueryBuilders.boolQuery()
                .must(QueryBuilders.matchQuery("name", "脱脂牛奶"))
                .must(QueryBuilders.termQuery("brand", "德亚"))
                .filter(QueryBuilders.rangeQuery("price").gte(5000).lte(30000))
        );
        // 3.发送请求
        SearchResponse response = client.search(request, RequestOptions.DEFAULT);
        // 4.解析响应
        parseDocResponse(response);
    }
```
- DSL 排序和分页查询
```
void testPageAndSort() throws IOException {
        int pageNo = 1, pageSize = 5;
        // 1.创建Request
        SearchRequest request = new SearchRequest("items");
        // 2.组织请求参数
        request.source().query(QueryBuilders.matchAllQuery())
                .from((pageNo - 1) * pageSize)
                .size(pageSize)
                .sort("sold", SortOrder.DESC)
                .sort("price", SortOrder.ASC);
        // 3.发送请求
        SearchResponse response = client.search(request, RequestOptions.DEFAULT);
        // 4.解析响应
        parseDocResponse(response);
    }
```
- DSL 高亮查询
```
void testHighlight() throws IOException {
        // 1.创建Request
        SearchRequest request = new SearchRequest("items");
        // 2.组织请求参数
        request.source().query(QueryBuilders.matchQuery("name", "脱脂牛奶"))
                .highlighter(SearchSourceBuilder.highlight()
                        .field("name")
                        .preTags("<em>")
                        .postTags("</em>")
                );
        // 3.发送请求
        SearchResponse response = client.search(request, RequestOptions.DEFAULT);
        // 4.解析响应
        parseDocResponse(response);
    }
```
- DSL 聚合查询
```
void testAggs() throws IOException {
        //查询价格大于等于3000的手机，按品牌聚合，然后按每个品牌的数量升序
        // 1.创建Request
        SearchRequest request = new SearchRequest("items");
        // 2.组织请求参数
        BoolQueryBuilder boolQuery = QueryBuilders.boolQuery()
                .filter(QueryBuilders.termQuery("category", "手机"))
                .filter(QueryBuilders.rangeQuery("price").gte(300000));
        request.source().query(boolQuery).size(0);//size=0表示不查询doc

        String brandAggName = "brands_agg";
        request.source().aggregation(AggregationBuilders.terms(brandAggName)
                .field("brand")
                .size(10)
                .order(BucketOrder.count(true))
        );
        // 3.发送请求
        SearchResponse response = client.search(request, RequestOptions.DEFAULT);
        // 4.解析响应
        parseAggsResponse(response, brandAggName);
    }
```

- 解析响应
```
private static void parseDocResponse(SearchResponse response) {
        SearchHits searchHits = response.getHits();
        // 1.获取总条数
        long total = searchHits.getTotalHits().value;
        System.out.println("共搜索到" + total + "条数据");
        // 2.遍历结果数组
        SearchHit[] hits = searchHits.getHits();
        for (SearchHit hit : hits) {
            // 3.得到_source，也就是原始json文档
            String source = hit.getSourceAsString();
            // 4.反序列化并打印
            ItemDoc item = JSONUtil.toBean(source, ItemDoc.class);
            // 5.获取高亮字段
            Map<String, HighlightField> hfs = hit.getHighlightFields();
            if (CollUtils.isNotEmpty(hfs)) {
                HighlightField hf = hfs.get("name");
                if (hf != null) {
                    Text[] fragments = hf.fragments();
                    //拼接高亮字段
                    StringBuilder sb = new StringBuilder();
                    for (Text fragment : fragments)
                        sb.append(fragment);
                    item.setName(sb.toString());
                }
            }
            System.out.println(item);
        }
    }
```
- 解析聚合查询响应
```
private static void parseAggsResponse(SearchResponse response, String aggName) {
        Aggregations aggregations = response.getAggregations();
        //根据聚合名称获取聚合
        Terms terms = aggregations.get(aggName);
        //获取桶
        List<? extends Terms.Bucket> buckets = terms.getBuckets();
        for (Terms.Bucket bucket : buckets) {
            System.out.println("key:" + bucket.getKeyAsString() + "," + "cnt:" + bucket.getDocCount());
        }
    }
```

## Redis主从
单节点Redis的并发能力是有上限的，要进一步提高Redis的并发能力，就需要搭建主从集群，实现读写分离。

假如集群中有一个master节点、两个slave节点（现在叫replica）。当我们通过Redis的Java客户端访问主从集群时，应该做好路由：
- 如果是写操作，应该访问master节点，master会自动将数据同步给两个slave节点
- 如果是读操作，建议访问各个slave节点，从而分担并发压力
- 启动不同redis实例后，配置主从关系如下
```
# Redis5.0以前
slaveof <masterip> <masterport>
# Redis5.0以后
replicaof <masterip> <masterport>
```

两个重要的概念：
- Replication Id：简称replid，是数据集的标记，replid一致则是同一数据集。*每个master都有唯一的replid，slave则会继承master节点的replid*
- offset：偏移量，随着记录在repl_baklog中的数据增多而逐渐增大。slave完成同步时也会记录当前同步的offset。如果slave的offset小于master的offset，说明slave数据落后于master，需要更新。
- RDB: Redis Database Backup，Redis数据库备份文件，是一个二进制文件，保存了某个时间点的Redis数据库的数据。

全量同步：主从第一次建立连接时，执行全量同步，将master节点的所有数据都拷贝给slave节点
增量同步：全量同步需要先做RDB，然后将RDB文件通过网络传输个slave，成本太高。因此除了第一次做全量同步，其它大多数时候slave与master都是做增量同步。

主从同步步骤：
- slave节点请求增量同步
- master节点判断replid，发现不一致，拒绝增量同步，返回master的replid和offset
- master将完整内存数据生成RDB，发送RDB到slave
- slave清空本地数据，加载master的RDB
- master将slave加载RDB期间的命令记录在repl_baklog，并将命令发送给slave
- slave执行接收到的命令，保持与master之间的同步

repl_baklog原理：
该文件是一个固定大小的数组，只不过数组是环形，也就是说角标到达数组末尾后，会再次从0开始读写，这样数组头部的数据就会被覆盖。该文件记录了redis处理过的命令，以及master和slave的offset，他们之间offset的差异，就是slave要增量拷贝的数据
随着不断有数据写入，master的offset逐渐变大，slave也不断的拷贝，追赶master的offset
如果slave出现网络阻塞，导致还没有同步的数据被覆盖，repl_baklog找不到slave的offset，那么只能再次做全量同步

什么时候执行增量同步？
- slave节点断开又恢复，并且在repl_baklog中能找到offset时

可以从以下几个方面来优化Redis主从就集群：
- 在master中配置`repl-diskless-sync  yes`启用无磁盘复制，避免全量同步时的磁盘IO。
- Redis单节点上的内存占用不要太大，减少RDB导致的过多磁盘IO
- 适当提高repl_baklog的大小，发现slave宕机时尽快实现故障恢复，尽可能避免全量同步
- 限制一个master上的slave节点数量，如果实在是太多slave，则可以采用主-从-从链式结构，减少master压力

## Redis哨兵
Redis提供了哨兵（Sentinel）机制来监控主从集群监控状态，确保集群的高可用性。
哨兵的作用如下：
- 状态监控：Sentinel 会不断检查您的master和slave是否按预期工作
- 故障恢复（failover）：如果master故障，Sentinel会将一个slave提升为master。当故障实例恢复后会成为slave
- 状态通知：Sentinel充当Redis客户端的服务发现来源，当集群发生failover时，会将最新集群信息推送给Redis的客户端

sentinel如何监控？
Sentinel基于心跳机制监测服务状态，每隔1秒向集群的每个节点发送ping命令，并通过实例的响应结果来做出判断：
- 主观下线（sdown）：如果某sentinel节点发现某Redis节点未在规定时间响应，则认为该节点主观下线。
- 客观下线(odown)：若超过指定数量（通过quorum设置）的sentinel都认为该节点主观下线，则该节点客观下线。quorum值最好超过Sentinel节点数量的一半，Sentinel节点数量至少3台。

master出现故障如何处理？
- 首先要在sentinel中选出一个leader，由leader执行failover(下面3步)
- leader选定一个slave作为新的master，执行slaveof noone，切换到master模式
- 然后让所有节点都执行slaveof 新master
- 修改故障节点配置，添加slaveof 新master

sentinel选举leader的依据是什么？
- 票数超过sentinel节点数量1半
- 票数超过quorum数量
- 一般情况下最先发起failover的节点会当选

leader从slave中选取master的依据是什么？
- 首先会判断slave节点与master节点断开时间长短，如果超过down-after-milliseconds * 10则会排除该slave节点
- 然后判断slave节点的slave-priority值，越小优先级越高，如果是0则永不参与选举（默认都是1）。
- 如果slave-prority一样，则判断slave节点的offset值，越大说明数据越新，优先级越高
- 最后是判断slave节点的run_id大小，越小优先级越高（通过info server可以查看run_id）。

## Redis分片集群
主从模式可以解决高可用、高并发读的问题。但依然有两个问题没有解决：
- 海量数据存储
- 高并发写

分片集群特征：
-  集群中有多个master，每个master保存不同分片数据 ，解决海量数据存储问题
-  每个master都可以有多个slave节点 ，确保高可用
-  master之间通过ping监测彼此健康状态 ，类似哨兵作用
-  客户端请求可以访问集群任意节点，最终都会被转发到数据所在节点 

### 散列插槽
数据要分片存储到不同的Redis节点，肯定需要有分片的依据，这样下次查询的时候才能知道去哪个节点查询。
很多数据分片都会采用一致性hash算法。而Redis则是利用散列插槽（hash slot）的方式实现数据分片。

在Redis集群中，共有16384个hash slots，集群中的每一个master节点都会分配一定数量的hash slots。
当我们读写数据时，Redis基于`CRC16` 算法对key做hash运算，得到的结果与16384取余，就计算出了这个key的slot值。然后到slot所在的Redis节点执行读写操作。
不过hash slot的计算也分两种情况：
- 当key中包含{}时，根据{}之间的字符串计算hash slot (key是user，则根据user来计算hash slot)
- 当key中不包含{}时，则根据整个key字符串计算hash slot (key是user:{age}，则根据age来计算hash slot)

分片集群进行故障恢复时，互相通过ping的方式做心跳检测，超时未回应的节点会被标记为下线状态。当发现master下线时，会将这个master的某个slave提升为master。

Redis分片集群如何判断某个key应该在哪个实例？
- 将16384个插槽分配到不同的实例
- 根据key计算哈希值，对16384取余
- 余数作为插槽，寻找插槽所在实例即可

如何将同一类数据固定的保存在同一个Redis实例？
- Redis计算key的插槽值时会判断key中是否包含{}，如果有则基于{}内的字符计算插槽
- 数据的key中可以加入{类型}，例如key都以{typeId}为前缀，这样同类型数据计算的插槽一定相同

## Redis数据结构
常用的Redis数据类型有5种，分别是：String、List、Set、SortedSet、Hash
不管是任何一种数据类型，最终都会封装为RedisObject格式，它是一种结构体，包含了对象类型、编码方式、指向真实数据的指针等信息。

### SkipList
跳表的特点：
- 跳表是一个有序的双向链表
- 每个节点可以包含多层指针,层数是1到32之间的随机数
- 不同层指针到下一个节点的跨度不同,层级越高,跨度越大
- 增删改查效率与红黑树基本一致,实现却更简单。但空间复杂度更高

传统链表只有指向前后元素的指针，因此只能顺序依次访问。如果查找的元素在链表中间，查询的效率会比较低。而SkipList则不同，它内部包含跨度不同的多级指针，可以让我们跳跃查找链表中间的元素，效率非常高。
比如，某些元素含3个指针，每个指针跨度分别为4,5,6
多级指针的查询方式就避免了传统链表的逐个遍历导致的查询效率下降问题。在对有序数据做随机查询和排序时效率非常高。

跳表的结构：
```
typedef struct zskiplist {
    // 头尾节点指针
    struct zskiplistNode *header, *tail;
    // 节点数量
    unsigned long length;
    // 最大的索引层级
    int level;
} zskiplist;
```
跳表节点的数据机构:
```
typedef struct zskiplistNode {
    sds ele; // 节点存储的字符串
    double score;// 节点分数，排序、查找用
    struct zskiplistNode *backward; // 前一个节点指针
    struct zskiplistLevel {
        struct zskiplistNode *forward; // 下一个节点指针
        unsigned long span; // 索引跨度
    } level[]; // 多级索引数组
} zskiplistNode;
```

### SortedSet
SortedSet 数据结构的特点是：
- 每组数据都包含 score 和 member
- member 唯一
- 可根据 score 排序

SortedSet的底层数据结构是怎样的？
- 首先SortedSet需要能存储score和member值，而且要快捷的根据member查询score，因此底层有一个哈希表，以member为键，以score为value
- 其次SortedSet还需要能根据score排序，因此底层还维护了一个跳表。
- 当需要根据member查询score时，就去哈希表中查询；
- 当需要根据score排序查询时，则基于跳表查询

因为SortedSet底层需要用到两种数据结构，对内存占用比较高。因此Redis底层会对SortedSet中的元素大小做判断，如果满足以下条件，则会采用ZipList(压缩列表)来代替：
- SortSet 保存的键值对数量少于 128 个；
- 每个元素的长度小于 64 字节。

### Bitmap
Bitmap 看作是一个存储二进制数字（0 和 1）的数组，数组中每个元素的下标叫做 offset（偏移量）。

应用场景：
需要保存状态信息（0/1 即可表示）的场景，如用户签到情况、活跃用户情况、用户行为统计（比如是否点赞过某个视频）。

使用 Bitmap 统计活跃用户怎么做？
- 想要使用 Bitmap 统计活跃用户的话，可以使用日期（精确到天）作为 key，然后用户 ID 为 offset，如果当日活跃过就设置为 1。

## Redis内存回收
Redis之所以性能强，最主要的原因就是基于内存存储。然而单节点的Redis其内存大小不宜过大，会影响持久化或主从同步性能。
Redis内部会有两套内存回收的策略：
- 内存过期策略
- 内存淘汰策略

### 内存过期策略
Redis通过expire命令给Key设置TTL（Time To Live），当过期时间到了以后，再去查询数据，会发现数据已经不存在了。

Redis如何判断KEY是否过期呢？
Redis的本身是键值型数据库，其所有数据都存在一个redisDB的结构体中，其中包含两个哈希表：
- dict：保存Redis中所有的键值对
- expires：保存Redis中所有的设置了过期时间的KEY及其到期时间（写入时间+TTL）
所以可以去expires查询过期时间即可

TTL到期就会立即删除Key吗？
Redis并不会实时监测key的过期时间，在key过期后立刻删除。而是采用两种延迟删除的策略：
惰性删除：当有命令需要操作一个key的时候，检查该key的存活时间，如果已经过期才执行删除。
周期删除：通过一个定时任务，周期性的抽样部分有TTL的key，如果过期则执行删除。

周期删除的定时任务执行周期有两种：
SLOW模式：默认执行频率为每秒10次，但每次执行时长不能超过25ms，受server.hz参数影响。
FAST模式：频率不固定，跟随Redis内部IO事件循环执行。两次任务之间间隔不低于2ms，执行时长不超过1ms。

### 内存淘汰策略
仅仅依靠过期KEY清理是不够的，内存可能很快就达到上限。因此Redis允许设置内存告警阈值，当内存使用达到阈值时就会主动挑选部分KEY删除以释放更多内存。这叫做内存淘汰机制。
内存淘汰的策略有：
Redis支持8种不同的内存淘汰策略：
- noeviction： 不淘汰任何key，但是内存满时不允许写入新数据，默认就是这种策略。
- volatile-ttl： 对设置了TTL的key，比较key的剩余TTL值，TTL越小越先被淘汰
- allkeys-random：对全体key ，随机进行淘汰。也就是直接从db->dict中随机挑选
- volatile-random：对设置了TTL的key ，随机进行淘汰。也就是从db->expires中随机挑选。
- allkeys-lru： 对全体key，基于LRU算法进行淘汰
- volatile-lru： 对设置了TTL的key，基于LRU算法进行淘汰
- allkeys-lfu： 对全体key，基于LFU算法进行淘汰
- volatile-lfu： 对设置了TTL的key，基于LFI算法进行淘汰

- LRU（Least Recently Used），最近最久未使用。用当前时间减去最后一次访问时间，这个值越大则淘汰优先级越高。
- LFU（Least Frequently Used），最少频率使用。会统计每个key的访问频率，值越小淘汰优先级越高。

RedisObject中有lru属性，用于记录最后一次访问时间或者访问频率，选择LRU和LFU时这个属性的意义不同
- LRU：以秒为单位记录最近一次访问时间，长度24bit
- LFU：高16位以分钟为单位记录最近一次访问时间，低8位记录逻辑访问次数

之所以叫做逻辑访问次数，是因为并不是每次key被访问都计数，而是通过运算：
- ① 生成`[0,1)`之间的随机数R
- ② 计算 `1/(旧次数 * lfu_log_factor + 1)`，记录为P， lfu_log_factor默认为10
- ③ 如果 `R < P` ，则计数器 +1，且最大不超过255
- ④ 访问次数会随时间衰减，距离上一次访问时间每隔 lfu_decay_time 分钟(默认1) ，计数器-1
逻辑访问次数越大，说明单位时间内访问次数越多，所以更能**反应Key的重要性**

## Redis缓存

### 缓存一致性
缓存的通用模型有三种：
- Cache Aside（用的最多）：有缓存调用者自己维护数据库与缓存的一致性。即：
  - 查询时：命中则直接返回，未命中则查询数据库并写入缓存
  - 更新时：更新数据库并直接删除缓存，有查询时再更新缓存
- Read/Write Through：数据库自己维护一份缓存，底层实现对调用者透明。底层实现：
  - 查询时：命中则直接返回，未命中则查询数据库并写入缓存
  - 更新时：判断缓存是否存在，不存在直接更新数据库。存在则更新缓存，同步更新数据库
- Write Behind Cahing：读写操作都直接操作缓存，由线程异步的将缓存数据同步到数据库

更新时先更新数据库再删除缓存还是先删除缓存再更新数据库？
- 如果先删除缓存再更新数据库，假如刚删除缓存，来了一个查询请求，那么查询请求会发现缓存不存在，就会去数据库查询，这时数据库还没有更新，查询到的数据是旧数据，然后将旧数据写入缓存，然后此时数据库更新了，缓存还是旧数据。就会导致不一致。
- 如果先更新数据库再删除缓存，也有异常情况，比如线程1查询缓存未命中，于是去查询数据库，查询到旧数据，线程1将数据写入缓存之前，线程2来了，更新数据库，删除缓存，线程1执行写入缓存的操作，写入旧数据。这样也会不一致。

但是先更新数据库再删除缓存异常情况发生的条件极为苛刻，需要线程1写入缓存之前，线程2完成更新数据库和删除缓存两个操作，所以很难触发不一致的情况。

缓存一致性策略的最佳实践方案：
1. 低一致性需求：使用Redis的key过期清理方案
2. 高一致性需求：主动更新，并以超时剔除作为兜底方案
    - 读操作：
      - 缓存命中则直接返回
      - 缓存未命中则查询数据库，并写入缓存，设定超时时间
    - 写操作：
      - 先写数据库，然后再删除缓存
      - 要确保数据库与缓存操作的原子性
    
### 缓存穿透
缓存穿透是指客户端请求的数据在数据库中也不存在，从而导致请求穿透缓存，无法建立缓存，永远直接请求数据库的问题。
假如有不怀好意的人，开启很多线程频繁的访问一个数据库中也不存在的数据。由于缓存不可能生效，那么所有的请求都访问数据库，可能就会导致数据库因过高的压力而宕机。

解决方案有两种：

**缓存空对象**
当数据库也不存在数据时，将空值缓存到Redis，避免频繁查询数据库。优点是实现简单，维护方便，缺点是额外的内存消耗

**布隆过滤器**
布隆过滤是一种数据统计的算法，用于检索一个元素是否存在一个集合中。但是布隆过滤无需存储元素到集合，而是把元素映射到一个很长的二进制数位上。

- 首先需要一个很长很长的二进制数，默认每一位都是0
- 然后需要N个不同算法的哈希函数
- 将集合中的元素根据N个哈希函数做运算，得到N个数字，然后将每个数字对应的bit位标记为1
- 要判断某个元素是否存在，只需要把元素按照上述方式运算，判断对应的bit位是否是1即可

布隆过滤器的特点
- 当布隆过滤器认为元素不存在时，它肯定不存在
- 当布隆过滤器认为元素存在时，它可能存在，也可能不存在（存在哈希碰撞）


### 缓存雪崩
缓存雪崩是指在同一时段大量的缓存key同时失效或者Redis服务宕机，导致大量请求到达数据库，带来巨大压力。

常见的解决方案有：
- 给不同的Key的TTL添加随机值，这样KEY的过期时间不同，不会大量KEY同时过期
- 利用Redis集群提高服务的可用性，避免缓存服务宕机
- 给缓存业务添加降级限流策略
- 给业务添加多级缓存（如浏览器级别缓存、nginx级别缓存、jvm本地缓存等），比如先查询本地缓存，本地缓存未命中再查询Redis，Redis未命中再查询数据库。即便Redis宕机，也还有本地缓存可以抗压力

### 缓存击穿
缓存击穿问题也叫热点Key问题，就是一个被高并发访问并且缓存重建业务较复杂的key突然失效了，无数的请求访问会在瞬间给数据库带来巨大的冲击。

常见的解决方案有两种：
- 互斥锁：给重建缓存逻辑加锁，避免多线程同时指向
- 逻辑过期：热点key不要设置过期时间，在活动结束后手动删除。

如何保证缓存的双写一致性？
答：缓存的双写一致性很难保证强一致，只能尽可能降低不一致的概率，确保最终一致。我们项目中采用的是Cache Aside模式。简单来说，就是在更新数据库之后删除缓存；在查询时先查询缓存，如果未命中则查询数据库并写入缓存。同时我们会给缓存设置过期时间作为兜底方案，如果真的出现了不一致的情况，也可以通过缓存过期来保证最终一致。

追问：为什么不采用延迟双删机制？
答：延迟双删的第一次删除并没有实际意义，第二次采用延迟删除主要是解决数据库主从同步的延迟问题，我认为这是数据库主从的一致性问题，与缓存同步无关。既然主节点数据已经更新，Redis的缓存理应更新。而且延迟双删会增加缓存业务复杂度，也没能完全避免缓存一致性问题，投入回报比太低。