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