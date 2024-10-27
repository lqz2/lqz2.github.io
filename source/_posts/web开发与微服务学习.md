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
    - [vue常用指令](#vue常用指令)
        - [ref和reactive](#ref和reactive)
        - [v-for](#v-for)
        - [v-bind](#v-bind)
        - [v-if 和 v-show](#v-if-和-v-show)
        - [v-on](#v-on)
        - [v-model](#v-model)
    - [let和var的区别](#let和var的区别)

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