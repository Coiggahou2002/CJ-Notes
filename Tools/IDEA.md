# IDEA 攻略

## 快捷键

`Ctrl` + `O`  弹出父类可重写方法列表

## Maven

## 数据库

## 服务器

### 集成 Tomcat

Settings -> Application Server -> Tomcat Server

告诉 IDEA 你的 Tomcat 服务器安装路径即可

### 细节配置

设置项目在服务器的访问路径：右上角 Tomcat -> Configuration -> Deployment -> ApplicationContext，修改即可

## 项目类型

**普通项目**：Java

**Web 项目**

1.Java Enterprise -> Web Application

2.配置下列选项

+ Application Server 选服务器
+ Build system
  + Maven
  + Gradle
+ Language
  + Java
  + Kotlin
  + Groovy
+ Test framework
  + JUnit
  + TestNG
+ JDK Version

## 常见问题

ubuntu20.04 idea 不能输入中文

**解决方案**

在idea打开页面

- 点击 help
- 点击Edit Custom VM options
- 在末行添加： `-Drecreate.x11.input.method=true`



Initial heap size set to a larger value than the maximum heap size

`home/{username}/.config/JetBrains/IntellJ.../idea64.vmoptions`

把 `-Xms` 调成比 `-Xmx` 小的值就可以了