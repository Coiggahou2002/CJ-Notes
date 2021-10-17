# Spring Security

## 基础

### 概念

JWT：JSON Web Token

UUID：Universally Unique Identifier，通用唯一识别码

### 整合

`pom.xml` 加入 `spring-boot-starter-security` 即可

访问 `localhost:{端口号}` 就会自动跳转到登录页，同时会在后台打印随机生成的密码，默认用户名 user

```shell
Using generated security password: 56b9b3fe-ecdc-4e08-b9c3-86f6fdb7bed1
```

## 关于加密

基本逻辑：

加密前的明文密码叫 rawPassword，加密后的叫 encodedPassword.

加密算法保证不可逆，并且使 rawPassword 和 encodedPassword 保持一一对应的关系，即不存在一个 rawPassword 经加密后对应多个 encodedPassword 的情况，也不存在同个 encodedPassword 的原文可以是多个 rawPassword 的情况.

用户在注册时，明文密码经加密后，变成密文，存入数据库，下次登录时，通过判断输入密码加密后的密文是否与数据库中的密文相匹配，来验证密码是否正确.

## 配置

### 配置类

要加 `@Configuration` 注解

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private UserDetailsService userDetailsService;

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).passwordEncoder(new BCryptPasswordEncoder());
        
    }
}
```



**取消所有请求对登录的验证**

重写 WebSecutiryConfigurerAdapter 中的 `void configure(HttpSecurity http)` 方法，对所有请求放行即可.

```java
@Configuration
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests().anyRequest().permitAll();
    }

}
```

## 架构

![Spring Security Architecture](https://bs-uploads.toptal.io/blackfish-uploads/uploaded_file/file/412345/image-1602672495860.085-952930c83f53503d7e84d1371bec3775.png)

## 过滤器

FilterSecutiryInterceptor 方法级权限过滤器，位于最底部

ExceptionTranslationFilter 异常过滤器

UsernamePasswordAuthenticationFilter

### 过滤器如何进行加载的？

DelegatingFilterProxy

## 加密

PasswordEncoder 加密接口 

## 用户名密码认证

我们要自定义，怎么办？

1. 自己写过滤器，继承 `UsernamePasswordAuthenticationFilter`
2. 重写它的 3 个方法
   1. `attemptAuthentication()` 获取用户名密码
   2. `successfulAuthentication()` 认证成功后做什么
   3. `unsuccessfulAuthentication()` 认证失败后做什么
3. 查数据库部分要写到 `UserDetailsServiceImpl` 中，实现 `UserDetailsService`
   1. 实现 `UserDetails loadUserByUsername(String username)` 方法
   2. 返回 User 对象，该对象要实现 UserDetails 接口？



Spring Security 给我们提供的 User 类已经实现 UserDetails 接口，由三部分组成

+ `String username` 用户名
+ `String password` 密码
+ `Collection<? extends GrantedAuthority> authorities` 权限集

