# Spring

## 概况

### 核心思想

+ 控制反转 IoC (Inversion of Control)
+ 面向切面编程 AOP

### 七大模块

![img](https://images2017.cnblogs.com/blog/1219227/201709/1219227-20170930225010356-45057485.gif)

**Spring Core 核心容器**

+ ​	Spring以bean的方式组织和管 理Java应用中的各个组件及其关系。Spring使用BeanFactory来产生和管理Bean，它是工厂模式的实现。BeanFactory使用控制反转(IoC)模式将应用的配置和依赖性规范与实际的应用程序代码分开。

**Spring AOP 面向切面**

**Spring Context 应用上下文**

+ Application context
+ UI support
+ Validation
+ JNDL EJB support and remodeling Mail

**Spring DAO**

+ Transaction infrastructure
+ JOBC support
+ DAO support

**Spring ORM 对象实体映射**

+ Spring框架插入了若干个ORM框架，从而提供了ORM对象的关系工具，其中包括了Hibernate、JDO和 IBatis SQL Map等

**Spring Web**

+ WebApplicationContext
+ Multipart resolver
+ Web utilities

**Spring Web MVC**

+ Web MVC Framework

### 拓展

Spring Boot

+ 快速开发脚手架
+ 约定大于配置

Spring Cloud

+ 基于 SpringBoot 实现

### 学习路线

Spring -> SpringMVC -> SpringBoot

## IoC

Inversion of Control，控制反转.

### 原理

#### 依赖倒置原则

Dependency Inversion Principle，依赖倒置.

**宏观解释**

在传统的开发方式中，依赖的架构是“自底向上”的，也就是说，上层建筑是依赖下层建筑的，一旦下层建筑需要改动，那么所有依赖该下层建筑的上层建筑都需要针对该改动进行维护，换句话说，依赖关系树上，下层建筑的所有祖先节点都需要维护，会造成维护成本非常高，牵一发动全身.

> 例如，先设计轮胎，再根据轮胎设计汽车底盘，再根据底盘设计车身架构，一旦轮胎尺寸修改，所有依赖轮胎的上层部件都需要修改.

如果是采用依赖倒置的模式，依赖的架构是“自顶向下”的，下层建筑是依赖上层建筑的，上层建筑在设计时已经为不同规格的下层建筑留好了位置，各种规格的下层建筑都可以直接填充进来，而且上层建筑不关心下层建筑的具体类型.

> 例如，先设计汽车车身，为可能出现的底盘留足空间，再设计底盘，设计底盘时又为可能出现的轮胎尺寸留足空间，最后再把轮胎填进来即可，当轮胎尺寸需要修改时，上层部件全部无需改动.

**微观解释**

上面所说的“下层建筑”具体到代码中就是一个底层的对象，在传统开发中，依赖该对象的上层对象在创建时，都需要 `new` 一个底层对象作为自己的属性，如此套娃一直到顶层. 那么加入某一天，我的底层对象需要加一个属性，同时需要在它的构造器中加一个参数，那么所有依赖该底层对象的上层对象的类定义中，有关这个底层对象的所有 `new constructor(args...)` 方法都需要修改 .

如果采用依赖倒置模式，所有上层对象在属性中使用下层对象作为自己的属性时，不使用 `new constructor(args...)` 的形式，而是使用一个 `setter` 也就是 `set()` 方法，通过接受外部传进来的下层对象作为自己的属性，就可以实现“上层对象不关心下层对象的具体细节”，避免了 `new` 一个下层对象时还要关心它的各种参数，那么对下层对象作改动之后，上层对象类的定义完全不需要修改.

### 应用方式

在 `ApplicationContext.xml` 中配置所有的 Bean，再通过 `ApplicationContext` 对象的 `getBean(String id)` 方法获取.

```xml
<!--id用于在getBean的时候定位对象-->
<!--class表明对象类型-->
<!--property项用于设置对象属性-->
<bean id="hello"  class="com.coiggahou.pojo.Hello">
    <property name="str" value="Spring222"/>
</bean>
```

测试代码中需要获取应用上下文，通过它来获取 Bean，并且强转成所需类型.

`ApplicationContext` 对象一旦创建好，意味着配置文件中预设的所有 Bean 都已经创建好.

```java
public static void main(String[] args) {
    ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");
    Hello hello1 = (Hello)context.getBean("hello");
    System.out.println(hello1.toString());
}
```

### 配置元数据

XML configuration

Annotation-based configuration

Java-based configuration

### 依赖注入

#### Set 方法注入

原对象类定义如下：

```java
public class Student {
    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbies;
    private Map<String,String> card;
    private Set<String> games;
    private String wife;
    private Properties info;
    // 此处省略 Getter 和 Setter 方法，Setter 必须有，否则无法直接注入
}
```

各种属性对应的注入方法如下：

普通属性注入（直接设置 name 和 value）

```xml
<property name="name" value="小蔡"/>
```

引用注入

```xml
<property name="name" value="小蔡"/>
```

数组类型属性注入

```xml
<property name="books">
    <array value-type="java.lang.String">
        <value>红楼梦</value>
        <value>三国演义</value>
        <value>西游记</value>
    </array>
</property>
```

List 容器属性注入

```xml
<property name="hobbies">
    <list value-type="java.lang.String">
        <value>Listening</value>
        <value>Coding</value>
        <value>Smile</value>
    </list>
</property>
```

Map 容器属性注入

```xml
<property name="card">
    <map>
        <entry key="ID" value="445221200201302212"/>
        <entry key="Credit Card" value="6228484"/>
    </map>
</property>
```

Set 容器属性注入

```xml
<property name="games">
    <set>
        <value>LOL</value>
        <value>DNF</value>
        <value>CF</value>
    </set>
</property>
```

空指针属性注入

```xml
<property name="girlfriend">
    <null/>
</property>
```

Properties 属性注入

```xml
<property name="info">
    <props>
        <prop key="Number">200110617</prop>
        <prop key="Email">1096119575@qq.com</prop>
    </props>
</property>
```

#### 构造器注入

与 Set 方法注入格式很像，只不过 Set 方法注入时，xml 中的 `property` 项对应的是每一个拥有 setter 的属性，而构造器注入时，使用 xml 中的 `constructor-arg` 项对应类的有参构造器的每一个参数.

```xml
<bean id="student2" class="com.coiggahou.pojo.Student">
    <constructor-arg name="address" ref="address"/>
    <constructor-arg name="books">
        <array>
            <value>三国演义</value>
            <value>红楼梦</value>
        </array>
    </constructor-arg>
    <constructor-arg name="info">
        <props>
            <prop key="ID">44523423434823482</prop>
        </props>
    </constructor-arg>
</bean>
```

#### 拓展注入

##### p 命名空间

其实就是 property-namespace

需要在 xml 头部加上一句

```xml
xmlns:p="http://www.springframework.org/schema/p"
```

然后可以在每个 Bean 标签属性中直接用 `p:类定义中属性名=xxx` 来填充属性值

##### c 命名空间

其实就是 constrctor-namespace

需要在 xml 头部加上

```xml
xmlns:c="http://www.springframework.org/schema/c"
```

然后可以在每个 Bean 标签属性中直接用 `c:有参构造器参数名=xxx` 来注入属性

### Bean 作用域

#### singleton

单例是 Spring 默认机制. 

单例模式下，多次用 `ApplicationContext` 取 Bean 的时候，取到的是同一份对象的不同引用.

```xml
<bean id="student" class="com.coiggahou.pojo.Student" scope="singleton"/>
```

#### prototype

原型模式下，多次用 `ApplicationContext` 取 Bean 的时候，取到的是不同的对象，可以输出 HashCode 验证.

```xml
<bean id="student" class="com.coiggahou.pojo.Student" scope="prototype"/>
```

#### request

#### session

#### application

#### websocket

### Bean 自动装配

#### 使用 XML

##### byName

在 Bean 标签属性中加一个 `autowire="byName"`

比如说，如果需要给 `<bean id="person" class="com.coiggahou.pojo.Person">` 自动根据 Name 装配 `Arm` 类的对象实例，那么首先需要在 XML 中存在 `<bean id="arm" class="com.coiggahou.pojo.Arm">` ，然后还要保证 `Person` 类中存在 一个名为 `set + id首字母大写` 的 set 方法，也就是 `setArm(xxx)` 的方法.

**简单来说，id 要对应 set 方法名的 "set" 后面的字符串（不区分首字母大小写）**

缺点：必须保证所有 bean 的 id 都是唯一的

##### byType

在 Bean 标签属性中加一个 `autowire="byType"`

比如说，如果需要给 `<bean id="person" class="com.coiggahou.pojo.Person"` 自动根据 Type 装配 `Arm` 类的对象实例，那么首先需要在 XML 中存在 `<bean id="arm" class="com.coiggahou.pojo.Arm">` ，然后还要保证 `Person` 类中存在一个 `class名` 类型的属性，也就是存在一个`Arm` 类型的属性. 

缺点：XML 中存在多个相同类型的 bean 时无法自动装配

#### 使用注解

XML 模板如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

`@Autowired` 默认通过 byType 进行注入，如果有多个相同类型，就加上 `@Qulifier(value="xxx")` 通过 byName 注入.

`@Resource(name=xxx)` 相当于通过 byName 注入

### 注解开发

使用注解之前要在 XML 中制定要扫描的包

```xml
<context:component-scan base-package="com.coiggahou.pojo"/>
```

**常见注解**

`@Component` ：写在类声明上方，表明该类被 Spring IoC 容器管理

`@Repository` ：一般用于标注 DAO 层

`@Service` ：一般用于标注 Service 层

`@Controller`  ：一般用于标注 Controller 层

`@Scope("xxx")` ：写明 Bean 的作用域，xxx 填 singleton 或者 prototype

实际业务一般用 XML 管理 Bean，而 Bean 的注入使用注解来做.

## AOP

### 动态代理

动态代理，包含两个要素：代理目标和代理类.

代理目标相当于房东，代理类相当于售楼中介.

动态代理做的事情就是——**我们提供代理目标，它为我们自动生成代理类.**

#### JDK 实现

JDK 动态代理的代理目标对象必须是接口

使用 `java.lang.reflect.Proxy` 中的静态方法 

`Proxy.newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)`

参数解释：

+ `ClassLoader loader` 目标对象的类加载器
+ `Class<?>[] interfaces` 目标对象的接口
+ `InvocationHandler h` 代理对象的执行处理器

