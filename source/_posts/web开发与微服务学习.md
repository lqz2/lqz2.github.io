---
title: web开发与微服务学习
date: 2024-10-13 15:03:53
categories: 学习
math:
tags:
---
# web开发与微服务学习
<!-- TOC -->

- [web开发与微服务学习](#web开发与微服务学习)
    - [springboot的依赖注入](#springboot的依赖注入)
        - [构造函数注入](#构造函数注入)
        - [setter方法注入](#setter方法注入)
        - [字段注入](#字段注入)
    - [springboot3自动配置原理](#springboot3自动配置原理)
        - [主要步骤](#主要步骤)
        - [使用示例](#使用示例)
    - [自定义starter](#自定义starter)
    - [JWT令牌](#jwt令牌)
    - [ThreadLocal](#threadlocal)
        - [特点](#特点)
        - [使用](#使用)
    - [拦截器](#拦截器)
        - [作用](#作用)
        - [拦截器与过滤器的区别](#拦截器与过滤器的区别)
        - [实现](#实现)
        - [拦截器的执行顺序](#拦截器的执行顺序)

<!-- /TOC -->


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

熟悉自动配置之后，自定义starter的步骤其实很简单，只有两步：
- 首先，创建一个模块，提供自动配置功能，即该模块包含自动配置类(定义了自动配置类并在`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`中声明)
- 创建另一个模块作为starter，在pom文件中引入上面的自定义自动配置类模块的坐标

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
