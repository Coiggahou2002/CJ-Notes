# Mybatis

## 基本步骤

1.创建新的 Maven 项目



2.将依赖包放到 `pom.xml` 中，如下方加入了 MySQL, Mybatis, Junit 的依赖

```xml
<dependencies>
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.25</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.7</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/junit/junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```



3.在路径 `src/main/resources/` 下创建 `mybatis-config.xml`，填入官方的实例配置

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    
  <!--可以配置多套环境，并设置默认环境-->
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <!--配置数据源四大项：驱动、URL、用户名、密码，下面以MySQL为例-->
      <dataSource type="POOLED">
        <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/${数据库名}"/>
        <property name="username" value="${用户名}"/>
        <property name="password" value="${密码}"/>
      </dataSource>
    </environment>
  </environments>
    
  <!--所有的Mapper配置文件必须在这里先注册-->  
  <mappers>
    <!--此处的resource路径需要换成具体Mapper配置文件的位置，不能用点，要用斜杠-->
    <!--还要在pom.xml中设置静态资源过滤-->
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
    
</configuration>
```



4.在工厂模式下获得 SqlSession

第一步，要先用超级工厂 `SqlSessionFactoryBuilder` 获得工厂 `SqlSessionFactory`.

在你自己的 `MybatisUtils` 类下加入静态代码块如下：

```java
static {
    String resource = "mybatis-config.xml";
    InputStream inputStream = Resources.getResourceAsStream(resource);
    SqlSessionFactory sqlSessionFactory  = new SqlSessionFactoryBuilder().build(inputStream);
}
```

第二步，用工厂 `SqlSessionFactory` 获取 `SqlSession`

```java
public static SqlSession getSqlSession() {
    return sqlSessionFactory.openSession();
}
```

完整的 `MybatisUtils` 工具类如下

```java
package com.coiggahou.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MybatisUtils {

    private static SqlSessionFactory sqlSessionFactory;

    static {

        String resource = "mybatis-config.xml";

        try {
            
            InputStream inputStream = Resources.getResourceAsStream(resource);

            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession();
    }
}

```



5.在 `pom.xml` 中加入静态资源过滤设置，以便 Maven 不会自动忽略某些我们自己写的配置文件

```xml
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
        </resource>
    </resources>
</build>
```



6.创建要处理的数据库表中每一行的实现类

如数据库每一行都是一个用户，字段有 id, name, password.

需要做的事情有：

+ 设置好基本属性
+ 设置空参和全参构造器
+ 对所有属性设置 Getter 和 Setter 方法
+ 方便测试，可以再加重写的 toString 方法

```java
package com.coiggahou.pojo;
public class User {
    private int id;
    private String name;
    private String password;
}
```



7.给实现类创建对应的映射器 (Mapper)

如给上面的 User 实现类创建一个映射器类 UserMapper

```java
package com.coiggahou.dao;

import com.coiggahou.pojo.User;
import java.util.List;

public interface UserMapper {
     List<User> getUserList();
}
```



8.给映射器创建对应的配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace填写对应的映射器类的位置-->
<mapper namespace="com.coiggahou.dao.UserMapper">
    
    <!--将select查询语句与映射器类中的方法绑定-->
    <!--id填绑定的方法名-->
    <!--resultType填查询语句返回的每行数据对应的那个实现类-->
    <select id="getUserList" resultType="com.coiggahou.pojo.User">
        select * from mybatis.usr
    </select>
    
</mapper>
```

## 增删改查实现

### 基本框架

1. 在实现类对象的 Mapper 接口中添加方法
2. 在 Mapper 的 xml 配置文件中为方法绑定对应的 SQL 语句
3. 写单元测试，测试运行效果

### 具体例子

在接口中写好方法和参数

```java
public interface UserMapper {
     List<User> getUserList();

     // 查询
     User getUserById(int id);

     // 增加
     int addUser(User user);
     int addUser2(Map<String, Object> map);

     // 更新
     int updateUser(User user);
     int updateUser2(Map<String, Object> map);

     // 删除
     int deleteUserById(int id);

     // 模糊匹配
     List<User> obsecureMatchByName(String name);
}
```

在 `UserMapper.xml` 中绑定 SQL 语句

+ 语句类型 (select, insert, update, delete) 要在 xml 标签注明
+ `id` 项与接口类中对应方法名一致
+ `parameterType` 对应接口类对应方法中的参数类型
  + 普通类型直接写，如 `int`，`double`
  + 实体类要写全路径，如 `com.coiggahou.pojo.User`
  + 用 map 的话直接写 `map`
+ `resultType` 填返回值类型
  + select 语句一般返回实体类
  + insert, update, delete 语句一般不写
+ 标签内部填对应的 SQL 语句
  + 占位符格式 `#{名字}`
    + 如果传进来的是实体类，占位符中可以直接填实体类的属性名
    + 如果传进来的是 map，占位符可以直接填 map 中 key 项的名称

```xml
<mapper namespace="com.coiggahou.dao.UserMapper">

    <select id="getUserList" resultType="com.coiggahou.pojo.User">
        select * from mybatis.usr
    </select>

    <select id="getUserById" parameterType="int" resultType="com.coiggahou.pojo.User">
        select * from mybatis.usr where id = #{id}
    </select>

    <insert id="addUser" parameterType="com.coiggahou.pojo.User">
        insert into mybatis.usr values (#{id},#{name},#{password})
    </insert>

    <insert id="addUser2" parameterType="map">
        insert into mybatis.usr (id, name, password)
        values (#{id}, #{name}, #{password});
    </insert>

    <update id="updateUser" parameterType="com.coiggahou.pojo.User">
        update mybatis.usr
        set name = #{name}, password = #{password}
        where id = #{id};
    </update>

    <update id="updateUser2" parameterType="map">
        update mybatis.usr
        set name = #{name}, password = #{password}
        where id = #{id};
    </update>

    <delete id="deleteUserById" parameterType="int">
        delete
        from mybatis.usr
        where id = #{id};
    </delete>

    <select id="obsecureMatchByName" resultType="com.coiggahou.pojo.User">
        select * from mybatis.usr where name like "%"#{name}"%"
    </select>
</mapper>
```

