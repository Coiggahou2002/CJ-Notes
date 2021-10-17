# Concepts

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/infrastruture.png)

### MVC

Model - View - Controller

### ORM Framwork

ORM 全程 Object Relational Mapping (对象关系映射)，简单来说就是持久化类对象和数据库表项之间的映射规范，Hibernate 和 Mybatis 都属于 ORM 框架

### SSH

Struts2 + Spring + Mybatis

### SSM

SpringMVC + Spring  + Mybatis

### Controller

控制层，通过调用业务层，实现对具体的业务模块流程的控制. 负责在页面和程序之间传输数据，将用户填写的表单数据传入 Service 层，还有控制页面跳转的作用.

### Service

业务层，负责业务模块的逻辑设计，调用 DAO 层，负责对数据的处理，然后传递数据到 DAO 层.

### DAO

Data Access Object，数据持久层，负责跟数据库打交道，通常在 DAO 层写接口，在 `Mapper.xml` 中写 SQL 语句.

### POJO

Plain Ordinary Java Object，数据对象原型，也叫数据库实体类，一般对应数据库里的行.

### VO

Value Object，数据对象

### BO

Business Object