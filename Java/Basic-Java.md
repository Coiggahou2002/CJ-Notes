# Basic-Java

## 按值调用

Java 中总是采用按值调用的方式. 

由于方法修改的永远是传入参数的副本，所以基本数据类型无法被方法修改.
但为什么当参数是对象时可以修改呢？因为当对象作为参数传入时，即便会复制一份副本，但这份副本和原来传入的参数所引用的是同一个对象，所以可以修改，而且这并没有违背“按值调用”的原则.

## 静态的含义

`static` 修饰的字段和方法，可以直接通过类来调用，不需要创建类的实例.
换句话说，这些字段和方法都属于类而不是属于类的实例对象.

如 `Math.random()`

## 权限修饰

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/image-20210702204228758.png)

## 包机制

```java
package com.baidu.wenku.xxx
```

本质上就是文件夹，作用是用于区别类名的命名空间.

一般用域名倒置作为包名，如 `com.baidu.www.xxx`

如果创建包时没有自动分级，应在项目设置齿轮图标中取消勾选 `Compact Middle Packages`

类的完全限定名是指“所在包名.类名”，如 `com.mysql.cj.jdbc.Driver`

```java
//使用某个包的成员，需要用import关键字导入该包
import java.util.Date;
import java.util.*; //*是通配符，导入util下的所有类
```
## 多态

定义一个父类类型引用，指向子类对象

+ 该引用只可以调用父类含有的方法（如果该方法被子类重写，则执行子类重写的内容）
+ 该引用不可以调用子类独有的方法

下面是具体例子：

```java
public class Person {
    public void walk() {
        System.out.println("person walking");
    }
}

public class Student extends Person {
    @Override
    public void walk() {
        System.out.println("student walking");
    }
    
    public void study() {
        ...
    }
}
Person coiggahou = new Student();
coiggahou.walk(); // 输出student walking
coiggahou.study()； // 报错

```

总之，对象能调用什么方法，主要看引用类型（也就是左边）.

`static`，`final`，`private` 修饰的方法不能被重写



`instanceof` 关键字用于测试左边的对象是否为右边的实例，如

我自己的理解：对于 `X instanceof Y` ，如果 Y 在 X 所在的继承树上，而且 Y 是 X 的祖宗节点，那么就会返回 `true`

```java
// 继承树：Object -> Person -> Student

Person x = new Student();
sout(x instanceof Object); // true
sout(x instanceof Person); // true
sout(x instanceof Student); // true
sout(x instanceof String); // false
```


## 抽象类

只要某个类中包含抽象方法， 这个类必须声明为抽象类.

抽象类不可以生成实例.

抽象方法充当着占位方法的角色，不含有具体实现（具体实现由他的子类来完成）

## 接口

接口是对行为的抽象，描述了“实现我的类应该做些什么”，但不描述“它们应该如何做”.

如果一个类去实现某个接口，那么这个类必须实现接口中写的所有方法.

## 注解

### 使用的地方

package, class, method, field 上面都可以加注解

### 元注解

元注解的作用时负责对普通注解进行注解

`@Target`：描述注解的使用对象

`@Retention`：表示在什么级别保存该注释信息，描述注解的声明周期 (SOURCE < CLASS < RUNTIME)

`@Documented`：说明该注解将被包含在 javadoc 中

`@Inherited`：说明子类可以继承父类中的该注解

### 自定义注解

```java
@MyAnnotation2(args = "heihe", id = 20)
public class Test01 {

    @MyAnnotation2(args = "heihei", id = 18)
    String getStr() {

    }

}

@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation2 {
    String args() default "";
    int id();
}
```

### 常见注解

`@Override`：只适用于修辞方法，表示一个方法声明打算重写父类中的另一个方法声明

`@FunctionalInterface`：函数式接口

`@Deprecated`：修辞方法、属性、类，表示不鼓励使用，已经废弃

`@SuppressWarnings`：用于抑制编译时的警告信息

### 注解的作用

+ 解释说明作用
+ 可以被编译器读取

## 反射

反射机制允许程序在执行期间取得任何类的内部信息，并能直接操作任意对象的内部属性和方法.

### 具体功能

+ 在运行时判断任意一个对象所属的类
+ 在运行时构造任意一个类的对象
+ 在运行时判断任意一个类具有的字段和方法
+ 在运行时获取泛型信息
+ 在运行时处理注解
+ 生成动态代理

### 主要 API

`java.lang.Class`

`java.lang.reflect.Method`

`java.lang.reflect.Field`

`java.lang.reflect.Constructor`

### 关于 Class

一个类在内存中只有一个 Class 对象

相同类型的实例对象对应的 Class 对象只有一个

一个类被加载后，整个类结构都被封装在 Class 对象中.

### 获取 Class 对象实例

1.`Class.forName(“类的全限定名”)`

多用于配置文件，通过读取配置文件获取类名字符串，加载类

2.`类名.class`

多用于参数的传递

3.`目标类的一个实例对象.getClass`

多用于对象的获取字节码的方式

```
* Class对象功能：
		* 获取功能：
			1. 获取成员变量们
				* Field[] getFields() ：获取所有public修饰的成员变量
				* Field getField(String name)   获取指定名称的 public修饰的成员变量
	
				* Field[] getDeclaredFields()  获取所有的成员变量，不考虑修饰符
				* Field getDeclaredField(String name)  
			2. 获取构造方法们
				* Constructor<?>[] getConstructors()  
				* Constructor<T> getConstructor(类<?>... parameterTypes)  
	
				* Constructor<T> getDeclaredConstructor(类<?>... parameterTypes)  
				* Constructor<?>[] getDeclaredConstructors()  
			3. 获取成员方法们：
				* Method[] getMethods()  
				* Method getMethod(String name, 类<?>... parameterTypes)  
	
				* Method[] getDeclaredMethods()  
				* Method getDeclaredMethod(String name, 类<?>... parameterTypes)  
	
			4. 获取全类名	
				* String getName()  


	* Field：成员变量
		* 操作：
			1. 设置值
				* void set(Object obj, Object value)  
			2. 获取值
				* get(Object obj) 
	
			3. 忽略访问权限修饰符的安全检查
				* setAccessible(true):暴力反射



	* Constructor:构造方法
		* 创建对象：
			* T newInstance(Object... initargs)  
	
			* 如果使用空参数构造方法创建对象，操作可以简化：Class对象的newInstance方法


	* Method：方法对象
		* 执行方法：
			* Object invoke(Object obj, Object... args)  
	
		* 获取方法名称：
			* String getName:获取方法名


	* 案例：
		* 需求：写一个"框架"，不能改变该类的任何代码的前提下，可以帮我们创建任意类的对象，并且执行其中任意方法
			* 实现：
				1. 配置文件
				2. 反射
			* 步骤：
				1. 将需要创建的对象的全类名和需要执行的方法定义在配置文件中
				2. 在程序中加载读取配置文件
				3. 使用反射技术来加载类文件进内存
				4. 创建对象
				5. 执行方法
