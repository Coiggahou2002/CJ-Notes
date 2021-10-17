# Spring Boot

## 入门

可在官网(https://start.spring.io/)快速配置，下载项目包，然后用 IDEA 导入项目.

等待 IDEA 把一大堆 Maven 依赖包导完，项目就绪.

所有的 controller, pojo, service, dao 包都需要放在 `Application.java` 的同级目录下，否则无法读取.

启动 `Application.java` ，然后就可以在 `localhost:8080` 看到页面.

IDEA 的 Maven 菜单中 `build` 可以将项目打包成可执行 jar 包，通过以下命令就可以运行程序.

```shell
java -jar /xxx/f
```

## application.yml

在这个文件里可以配置 SpringBoot 的相关选项，也可以在里面定义 Bean.

定义了某个实体类的 Bean，需要在对应实体类的 java 源文件中加上 `@Component` 和 `@ConfigurationProperties(prefix = "xxx")` 两个注解，才能绑定，要用到这个 Bean 的时候，加上 `@Autowired` 就OK

这个文件还有一个“统一配置”的作用，例如说我们的项目需要组合很多项技术，这些技术各自都有各自的配置文件，要修改配置的时候要东找西找，很麻烦，但现在我们可以把各项技术的配置都写成实体类，然后再在 `application.yml` 中写具体配置项，通过注解和实体类绑定，然后将配置 Bean 注入项目，这样一来，就可以做到所有配置全都在 `application.yml` 中了

## JSR303 数据校验

如果某个实体类的字段数据需要数据校验，需要在该实体类上方加 `@Validated` 注解，并在需要校验的字段上方写特定的校验要求，例如 `@Email` 就表明该类的实例的该字段值必须是合法邮件格式，否则会报错.

如果注解报错需要在 `pom.xml` 中导入 `spring-boot-starter-validation` 依赖

![img](img/3145530-8ae74d19e6c65b4c)

![img](img/3145530-10035c6af8e90a7c)

## 多环境配置切换

通过 `spring.profile` 指定生产环境别名

通过 `spring.profiles.actice` 指定当前生产环境

## 基本目录结构

+ src
  + main
    + java
      + com.coiggahou.projectName
        + controller
        + dao
        + pojo
        + service
    + resources
      + templates 模板引擎文件
      + static 静态资源
      + application.yml 配置文件
  + test
+ target
+ pom.xml 依赖管理

## Web开发

要解决的问题：

+ 导入静态资源
+ 首页
+ 模板引擎 Thymeleaf
+ 装配扩展 SpringMVC
+ 增删改查
+ 拦截器
+ 国际化

## Controller

在某类上方加 `@Controller` 表明该类充当一个控制器，类里面的方法控制页面跳转.

控制器类中的方法可以和 URL 绑定.

例如，我希望访问 `www.coiggahou.com/test` 时，服务器收到 `/test` 请求后自动执行 `findAll()` 方法，那我只需要在控制器类中的 `findAll()` 方法上方加上 `@RequestMapping("/test")` 注解即可.

### 路径映射

首先，一个 Controller 类可以和一个 URL 映射，然后，类中的方法可以和一个子 URL 映射.

接下来我针对以下代码进行讲解.

```java
@RestController
@CrossOrigin
@RequestMapping("/product")
public class ProductController {
    
    @Autowired
    private ProductService productService;
    
    @GetMapping("/selectNameAndDescByPriceLowerBound")
    @ResponseBody
    List<Product> selectNameAndDescByPriceLowerBound(@RequestParam String priceLowerBound) {
        return productService.selectNameAndDescByPriceLowerBound(priceLowerBound);
    }
    
}
```

父子 URL 合并成总 URL：`http://localhost:{端口号}/product/selectNameAndDescByPriceLowerBound`

这个总 URL 就与这个 `List<Product> selectNameAndDescByPriceLowerBound()` 方法进行了绑定，而且写的是 `@GetMapping`，表示当前端向这个 URL 发来 GET 请求时，执行这个方法.

此时前端对应的代码应该这样写：

```javascript
axios({
   url: 'http://{后端服务器地址}:{端口号}/product/selectNameAndDescByPriceLowerBound',
   method: 'get',
   // ...
}).then(resp => {
   // 对返回对象的处理
});
```

### 传参

传递参数的方式有以下几种

#### @PathVariable

在方法参数前直接加注解 `@PathVariable("user_id")`，同时在该方法的映射路径中附带参数，如 `@GetMapping("/user/selectByName/{userName}")`.

前端在发请求时将参数通过**字符串拼接的方式**写进来：

```javascript
let _this = this;
axios({
   url: 'http://{后端服务器地址}:{端口号}/user/selectByName/' + _this.userName + '',
   method: 'get',
}).then(resp => {
   // 处理返回数据
});
```

这种方法可能会造成冲突，比如前端有个页面在 `/user/share` 访问，而我数据库里面有个用户刚好叫 share，那就会出问题.

#### @RequestParam

在方法参数前直接加注解 `@RequestParam`，如

```java
@GetMapping("/selectNameAndDescByPriceLowerBound")
@ResponseBody
List<Product> selectNameAndDescByPriceLowerBound(@RequestParam String priceLowerBound) {
    return productService.selectNameAndDescByPriceLowerBound(priceLowerBound);
}
```

这样写的话，接受的参数位于 request 对象的 params 中（实际上还是会添加到请求 URL 中，但是不是直接添加，做了一些处理，所以不会冲突）.

前端针对这个接口发请求的代码是这样的：

（这里以 GET 请求为例，POST 请求也可以用这个）

```javascript
axios({
  url: 'http://{后端服务器地址}:{端口号}/product/selectNameAndDescByPriceLowerBound',
  method: 'get',
  params: {
    priceLowerBound: _this.priceLowerBound // 将前端用户输入的表单数据传递过来
  }
}).then(resp => {
  // 处理返回结果
})
```

#### @RequestBody

在方法参数前直接加注解 `@RequestBody`，参数放在请求的 Data 域中

```javascript
axios({
  url: 'http://{后端服务器地址}:{端口号}/product/....',
  method: 'post',
  data: {
    // 数据域
  }
}).then(resp => {
  // 处理返回数据
});
```

### 返回数据

直接在方法名上加 `@ResponseBody`，会将方法的返回值智能转换为 JSON 对象或者含 JSON 对象的集合 (如 Array)

