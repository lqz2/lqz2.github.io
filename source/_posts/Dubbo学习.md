---
title: Dubbo学习
date: 2025-05-25 15:56:51
categories: 学习
math:
tags:
---

<!-- TOC -->

- [Dubbo学习](#dubbo学习)
    - [Dubbo架构的核心组件](#dubbo架构的核心组件)
    - [Dubbo的工作流程](#dubbo的工作流程)
    - [基本使用](#基本使用)
    - [SPI机制是什么？Dubbo如何扩展默认实现？](#spi机制是什么dubbo如何扩展默认实现)
        - [SPI机制](#spi机制)
        - [Dubbo扩展默认实现](#dubbo扩展默认实现)
    - [Dubbo的负载均衡策略](#dubbo的负载均衡策略)
    - [一次完整的RPC是怎样的？](#一次完整的rpc是怎样的)
    - [RPC和http的区别？](#rpc和http的区别)

<!-- /TOC -->

## Dubbo学习

Dubbo是一个高性能的Java RPC框架，提供了分布式服务治理的解决方案。它支持多种协议和序列化方式，能够实现服务的注册、发现、调用等功能。
### Dubbo架构的核心组件
- Container： 服务运行容器，负责加载、运行服务提供者。必须。
- Provider： 暴露服务的服务提供方，会向注册中心注册自己提供的服务。必须。
- Consumer： 调用远程服务的服务消费方，会向注册中心订阅自己所需的服务。必须。
- Registry： 服务注册与发现的注册中心。注册中心会返回服务提供者地址列表给消费者。非必须。
- Monitor： 统计服务的调用次数和调用时间的监控中心。服务消费者和提供者会定时发送统计数据到监控中心。 非必须

### Dubbo的工作流程
1. 服务容器负责启动，加载，运行服务提供者。
2. 服务提供者在启动时，向注册中心注册自己提供的服务。
3. 服务消费者在启动时，向注册中心订阅自己所需的服务。
4. 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
5. 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
6. 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

### 基本使用

- 引入dubbo和zookeeper的依赖

- 服务提供方定义接口和实现类
```
public interface HelloService {
    String sayHello(String name);
}

// @Service: 这是 Dubbo 框架的注解，用于标记一个类为服务提供者。使得 Dubbo 可以自动扫描并将服务发布到注册中心，供消费者发现和调用。
@Service(interfaceClass=HelloService.class , retries=-1, version="1.0.0", timeout=15000)
public class HelloServiceImpl implements HelloService {

    @Override
    public String sayHello(String name) {
        return "【producer】 Hello, " + name + "!";
    }

}
```

- 服务消费者调用服务提供方的接口
```
public class ProducerService {
	// @Reference 是 Dubbo 框架中的一个注解，用于在消费者端标记对 Dubbo 服务的引用。
    // Dubbo 框架会自动为该注解标记的字段或方法参数注入代理对象。这个代理对象封装了底层的远程调用逻辑，使得消费者可以像调用本地方法一样调用远程服务。
    @Reference(retries=-1, version="1.0.0", timeout = 15000)
    private HelloService helloService;

    public String consumerSayHello(String name){
        String hello = helloService.sayHello(name);
        System.out.println("[consumer] "+ hello);
        return hello;
    }
}
```

### SPI机制是什么？Dubbo如何扩展默认实现？
#### SPI机制
SPI 的具体原理是这样的：将接口的实现类放在配置文件中，在程序运行过程中读取配置文件，通过反射加载实现类。这样，我们可以在运行的时候，动态替换接口的实现类。和 IoC 的解耦思想是类似的。

实现步骤如下：
- 定义一个接口
```
public interface Search {
    public List<String> searchDoc(String keyword);   
}
```

- 定义两个不同的实现类
```
public class FileSearch implements Search{
    @Override
    public List<String> searchDoc(String keyword) {
        System.out.println("文件搜索 "+keyword);
        return null;
    }
}

public class DatabaseSearch implements Search{
    @Override
    public List<String> searchDoc(String keyword) {
        System.out.println("数据库搜索 "+keyword);
        return null;
    }
}
```
- 在 META-INF/services 目录下创建一个文件，文件名为接口的全限定名，内容为实现类的全限定名
```
# META-INF/services/com.example.spi.Search
com.example.spi.FileSearch
```
- 测试结果
```
public class TestCase {
    public static void main(String[] args) {
        ServiceLoader<Search> s = ServiceLoader.load(Search.class);
        Iterator<Search> iterator = s.iterator();
        while (iterator.hasNext()) {
           Search search =  iterator.next();
           search.searchDoc("hello world");
        }
    }
}
//输出文件搜索 hello world
```
- 如果在`com.example.spi.Search`文件里写上两个实现类，那最后的输出结果就是两行了。这就是因为`ServiceLoader.load(Search.class)`在加载某接口时，会去META-INF/services下找接口的全限定名文件，再根据里面的内容加载相应的实现类。

#### Dubbo扩展默认实现
比如使用自定义的负载均衡器，可以这么做：
- 定义一个实现类实现`LoadBalance`接口
- 将实现类的路径添加到`META-INF/dubbo/org.apache.dubbo.rpc.cluster.LoadBalance`文件中

### Dubbo的负载均衡策略
- 加权轮询
- 加权随机，默认的负载均衡策略
- 最小活跃数
- 一致性哈希等
具体解释可以到[常见问题](https://lqz2.github.io/2022/08/26/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98/)里查看

### 一次完整的RPC是怎样的？
1. 调用方持续把请求参数对象序列化成二进制数据，经过 TCP 传输到服务提供方；

2. 服务提供方从 TCP 通道里面接收到二进制数据；

3. 根据 RPC 协议，服务提供方将二进制数据分割出不同的请求数据，经过反序列化将二进制数据逆向还原出请求对象，找到对应的实现类，完成真正的方法调用；

4. 然后服务提供方再把执行结果序列化后，回写到对应的 TCP 通道里面；

5. 调用方获取到应答的数据包后，再反序列化成应答对象。

### RPC和http的区别？
RPC（远程过程调用）是一种面向“方法”的调用模型，让我们可以像调用本地函数一样调用远程服务；而HTTP是一种面向“资源”的应用层协议，我们通过URL来定位资源，用GET、POST等方法来操作它

1. 核心思想与抽象级别不同
RPC：是面向方法 (Action-oriented) 的。开发者关注的是“我需要调用哪个服务的哪个方法”。
HTTP：是面向资源 (Resource-oriented) 的。开发者关注的是“我要操作哪个资源”。
2. 传输效率与协议不同
RPC：通常追求高性能。它经常使用二进制协议（如Protobuf）进行序列化，报文体积小，解析速度快。通信方式上，它倾向于使用长连接，避免了频繁创建和销毁连接的开销。
HTTP：通常使用文本协议（如JSON/XML），可读性好但冗余信息多，报文体积大。它多为短连接，每次请求都可能需要建立新的TCP连接（虽然有Keep-Alive优化），效率相对较低。
3. 服务治理能力不同
RPC：成熟的RPC框架（如gRPC、Dubbo）通常内置了完善的服务治理能力。比如服务发现、负载均衡、熔断降级、限流等功能。
HTTP：本身不带服务治理能力。要实现类似的功能，通常需要依赖外部组件或基础设施，比如通过Nginx做负载均衡，通过Consul/Nacos做服务发现，通过API网关来实现熔断和限流。
4. 应用场景不同
RPC：因其高性能、高效率和完善的服务治理，主要用于公司内部的微服务之间调用。
HTTP：主要用于对外暴露的服务，它的普适性是最大的优势。