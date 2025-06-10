---
title: mcp学习
date: 2025-06-10 10:25:58
categories: 机试
math:
tags:
---

<!-- TOC -->

- [mcp学习](#mcp学习)
    - [介绍](#介绍)
    - [使用Spring AI创建mcp server](#使用spring-ai创建mcp-server)
        - [基本概念](#基本概念)
        - [基于stdio的mcp服务端实现](#基于stdio的mcp服务端实现)
        - [基于SSE的mcp服务端实现](#基于sse的mcp服务端实现)
    - [使用Spring AI创建mcp client](#使用spring-ai创建mcp-client)
        - [基于stdio的mcp client实现](#基于stdio的mcp-client实现)
        - [基于SSE的mcp client实现](#基于sse的mcp-client实现)

<!-- /TOC -->
# mcp学习

## 介绍
模型上下文协议（model context protocol）是由Anthropic提出的一种协议，它给LLM和各类工具提供了一种标准化的桥梁，让LLM可以使用各种工具和API。

## 使用Spring AI创建mcp server

### 基本概念
Spring AI MCP包括以下组件：
- Spring AI服务端：使用Spring AI框架构建想要通过MCP访问数据的AI应用程序。
- Spring MCP客户端：MCP协议的spring ai实现，一个客户端可以配置多个mcp server

此外，Spring提供了两种机制快速搭建mcp server：
- 基于stdio的进程间通信传输，以独立的进程运行在本地，比较适合轻量级的工具
- 基于SSE（server sent events）进行远程服务访问，适合mcp server部署在远程服务器上，客户端通过http请求等方式访问。

### 基于stdio的mcp服务端实现

1. 添加依赖
```
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-mcp-server-spring-boot-starter</artifactId>
</dependency>
```
2. 在`application.yml`中配置mcp服务端
```
spring:
    main:
        web-application-type: none # 禁用web应用程序类型
        banner-mode: off # 禁用banner
    ai:
        mcp:
            server:
                stdio: true
                name: my-weather-server
                version: 0.0.1
```
3. 实现mcp工具，通过`@Tool`注解标记工具方法，其中`decription`可以添加对工具的描述，`@ToolParameter`可以标明工具具体的接收参数。
```
@Service

public class WeatherTool {

    private final WebClient webClient;

    public WeatherTool(WebClient.Builder webClientBuilder) {
        this.webClient = webClientBuilder
            .baseUrl("https://api.open-meteo.com/v1")
            .build();
    }

    @Tool(description = "根据经纬度获取天气预报")
    public String getWeatherForecast(
            @ToolParameter(description = "纬度，如39.9042") String latitude,
            @ToolParameter(description = "经度，如116.4074") String longitude) {
        try {
            String response = webClient.get()
                .uri(uriBuilder -> uriBuilder
                    .path("/forecast")
                    .queryParam("latitude", latitude)
                    .queryParam("longitude", longitude)
                    .queryParam("current", "temperature_2m,wind_speed_10m")
                    .queryParam("timezone", "auto")
                    .build())
                .retrieve()
                .bodyToMono(String.class)
                .block();
            return "天气信息"+ response;
        } catch (Exception e) {
            return "Error fetching weather data: " + e.getMessage();
        }
    }

    @Tool(description = "根据经纬度获取空气质量信息")
    public String getAirQuality(
            @ToolParameter(description = "纬度，如39.9042") String latitude,
            @ToolParameter(description = "经度，如116.4074") String longitude) {
        try {
            String response = webClient.get()
                .uri(uriBuilder -> uriBuilder
                    .path("/air-quality")
                    .queryParam("latitude", latitude)
                    .queryParam("longitude", longitude)
                    .queryParam("timezone", "auto")
                    .build())
                .retrieve()
                .bodyToMono(String.class)
                .block();
            return "空气质量信息" + response;
        } catch (Exception e) {
            return "Error fetching air quality data: " + e.getMessage();
        }
    }
}
```
4. 注册mcp工具，主要是返回`ToolCallbackProvider`的bean
```
@Bean
public ToolCallbackProvider weatherTools(WeatherTool weatherTool) {
    return MethodToolCallbackProvider.builder()
            .toolObjects(weatherTool)
            .build();
}
```

5. 启动应用程序，mcp服务端会监听标准输入输出流，等待客户端的请求。

### 基于SSE的mcp服务端实现
基于SSE的mcp服务端通过http协议进行通信，适合远程服务访问，具体流程与stdio非常类似

1. 添加依赖
```
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-mcp-server-webflux-spring-boot-starter</artifactId>
</dependency>

```
2. 在`application.yml`中配置mcp服务端
```
server:
    port: 8080 # 设置端口号
spring:
    ai:
        mcp:
            server:
                name: my-weather-server
                version: 0.0.1
```
3. 实现mcp工具，通过`@Tool`注解标记工具方法，其中`decription`可以添加对工具的描述，`@ToolParameter`可以标明工具具体的接收参数。
```
@Service

public class WeatherTool {

    private final WebClient webClient;

    public WeatherTool(WebClient.Builder webClientBuilder) {
        this.webClient = webClientBuilder
            .baseUrl("https://api.open-meteo.com/v1")
            .build();
    }

    @Tool(description = "根据经纬度获取天气预报")
    public String getWeatherForecast(
            @ToolParameter(description = "纬度，如39.9042") String latitude,
            @ToolParameter(description = "经度，如116.4074") String longitude) {
        try {
            String response = webClient.get()
                .uri(uriBuilder -> uriBuilder
                    .path("/forecast")
                    .queryParam("latitude", latitude)
                    .queryParam("longitude", longitude)
                    .queryParam("current", "temperature_2m,wind_speed_10m")
                    .queryParam("timezone", "auto")
                    .build())
                .retrieve()
                .bodyToMono(String.class)
                .block();
            return "天气信息"+ response;
        } catch (Exception e) {
            return "Error fetching weather data: " + e.getMessage();
        }
    }

    @Tool(description = "根据经纬度获取空气质量信息")
    public String getAirQuality(
            @ToolParameter(description = "纬度，如39.9042") String latitude,
            @ToolParameter(description = "经度，如116.4074") String longitude) {
        try {
            String response = webClient.get()
                .uri(uriBuilder -> uriBuilder
                    .path("/air-quality")
                    .queryParam("latitude", latitude)
                    .queryParam("longitude", longitude)
                    .queryParam("timezone", "auto")
                    .build())
                .retrieve()
                .bodyToMono(String.class)
                .block();
            return "空气质量信息" + response;
        } catch (Exception e) {
            return "Error fetching air quality data: " + e.getMessage();
        }
    }
}
```
4. 注册mcp工具，主要是返回`ToolCallbackProvider`的bean
```
@Bean
public ToolCallbackProvider weatherTools(WeatherTool weatherTool) {
    return MethodToolCallbackProvider.builder()
            .toolObjects(weatherTool)
            .build();
}
```

5. 启动应用程序，mcp服务端会监听HTTP请求，等待客户端的请求。

## 使用Spring AI创建mcp client
客户端同样通过stdio或SSE两种方式进行通信，具体流程与服务端类似。
### 基于stdio的mcp client实现
1. 添加依赖
```
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-mcp-client-spring-boot-starter</artifactId>
</dependency>
```
2. 在`application.yml`中配置mcp服务器
```
spring:
    ai:
        dashscope:
            api-key: YOUR_API_KEY # 替换为你的API密钥
        mcp:
            client:
                stdio:
                    servers-configuration: classpath:/mcp-servers-config.json # MCP服务器配置文件路径
```

3. 创建MCP服务器配置文件`mcp-servers-config.json`，内容如下：
```
{
  "mcpServers":{
    //定义名为"weather"的MCP服务器
    "weather":{
        //指定启动命令
        "command": "java",
        "args":[
            "-Dspring.ai.mcp.server.stdio=true",
            "-Dspring.main.web-application-type=none",
            "-jar",
            "path/to/your/weather-server.jar"  # 替换为你的MCP服务器JAR包路径
        ],
        //指定环境变量
        "env": {}
    }
    // 可以添加更多的MCP服务器配置
  }
}
```
4. 编写启动类进行测试
```
@SpringBootApplication
public class McpClientApplication {

    public static void main(String[] args) {
        SpringApplication.run(McpClientApplication.class, args);
    }

    @Bean
    public CommandLineRunner predefineQuestions(
        ChatClient.Builder chatClientBuilder,
        ToolCallbackProvider tools,
        ConfigurableApplicationContext context
    ) {
        return args -> {
            // 创建ChatClient实例
            var chatClient = chatClientBuilder
                .defaultTools(tools) //传入预定义的工具
                .build();

            // 定义预设问题
            String question1 = "请告诉我北京的天气预报";
            
            // 打印问题
            System.out.println("问题: " + question1);

            //调用LLM并打印响应
            System.out.println("响应: " + chatClient.prompt(question1)
                .call()
                .content());
            
            context.close();
        };    
                
    }
}
```

### 基于SSE的mcp client实现
1. 添加依赖
```
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-mcp-client-webflux-spring-boot-starter</artifactId>
</dependency>
```

2. 在`application.yml`中配置mcp服务器
```
spring:
    ai:
        dashscope:
            api-key: YOUR_API_KEY # 替换为你的API密钥
        mcp:
            client:
                sse:
                    connections:
                        server1:
                            url: http://localhost:8080 //替换为你的MCP服务器地址
                        server2: xxxx
```

3. 编写启动类进行测试
```
@SpringBootApplication
public class McpClientApplication {

    public static void main(String[] args) {
        SpringApplication.run(McpClientApplication.class, args);
    }

    @Bean
    public CommandLineRunner predefineQuestions(
        ChatClient.Builder chatClientBuilder,
        ToolCallbackProvider tools,
        ConfigurableApplicationContext context
    ) {
        return args -> {
            // 创建ChatClient实例
            var chatClient = chatClientBuilder
                .defaultTools(tools) //传入预定义的工具
                .build();

            // 定义预设问题
            String question1 = "请告诉我北京的天气预报";
            
            // 打印问题
            System.out.println("问题: " + question1);

            //调用LLM并打印响应
            System.out.println("响应: " + chatClient.prompt(question1)
                .call()
                .content());
            
            context.close();
        };    
                
    }
}
```



