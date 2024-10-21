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

```