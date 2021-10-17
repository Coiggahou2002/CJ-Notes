# JDBC

## 一般步骤

1.导入具体数据库的驱动 jar 包

比如 MySQL 8.0 以上版本，到 MySQL 官网下载标注 `Platform Independent` 的 jar 包，放入当前项目所在的 `lib/` 目录下，右击 `lib` 文件夹点击 `Add As Library`，即成功导入 jar 包.

2.注册驱动

`Class.forName("com.mysql.cj.jdbc.Driver");`

3.获取连接对象

```java
Connection conn = DriverManager.getConnection("jdbc:mysql//localhost:3306/DB_NAME", "USERNAME", "PASSWORD");
// DB_NAME 数据库名
// USERNAME MySQL账户名
// PASSWORD MySQL账号密码
```

4.定义 SQL 语句

```java
String sql = "SELECT * FROM mytable";
```

5.获取 Statement 对象

```java
Statement stmt = conn.createStatement();
```

6.执行 SQL 语句

```java
ResultSet rs = conn.executeQuery(sql); // 执行查询语句并将结果生成结果集对象
```

7.处理结果

```java
while (rs.next()) {
    // 用结果集的迭代器提取数据并对数据进行处理
    int id = rs.getInt("id");
    String name = rs.getString("name");
    String url = rs.getString("url");
}
```

8.释放资源

```java
rs.close();
stmt.close();
conn.close();
```

## 事务

指一组包含多个步骤的业务，这些操作要么同时成功，要么同时失败。

## 各个对象详解

### DriverManager

功能：

+ 注册驱动
  + `Class.forName("com.mysql.cj.jdbc.Driver")`
  + 告诉程序该使用哪一个数据库系统的驱动程序
  + 其实只要导入 jar 包之后，程序会自动帮你注册，所以注册步骤可以省略
+ 获取数据库连接对象
  + `static Connection getConnection(String url, String user, String password)`
  + `url` 参数
    + `jdbc:mysql//ipAddress:Port/DatabaseName`
    + 例如本机是 `localhost`，本地 MySQL 端口号一般是 3306

### Connection

+ 用来获取 Statement 对象
  + `Statement createStatement()`
  + `PreparedStatement prepareStatement(String sql)`
+ 管理事务
  + 开启事务 `void setAutoCommit(boolean autoCommit)`
    + 设置为 `false`，意味着取消自动提交，也就是要手动提交，就是开启事务
  + 提交事务 `void commit()`
  + 回滚事务 `void rollback()`

### Statement

+ 用来执行静态 SQL 语句并返回 ResultSet
  + `boolean execute(String sql)` 
    + 可以执行任意 SQL 语句
  + `int executeUpdate(String sql)` 
    + 执行 **DML (insert, update, delete)** 语句和 **DDL (create,  alter, drop)** 语句
    + 返回值是：影响的行数 (rows affected)
  + `ResultSet executeQuery(String sql)`
    + 执行 DQL (select) 语句
    + 返回结果集对象

### ResultSet

```JAVA
String querySQL = "...";
ResultSet rs = stmt.executeQuery(sql);
while (rs.next()) {
    /* 取数据，操作数据 */
    String name = rs.getString("Name");
    ...
}
```



+ `boolean next()` 光标向前移动一行
+ `xxx getXxx()` 获取数据
  + `getString(String columnLabel)`
  + `getInt(String columnLabel)`

### PreparedStatement

使用 Statement 执行静态 SQL 语句会存在 SQL 注入的安全问题，这时候就需要预编译 SQL语句.

```java
String username = ...;
String password = ...;
String sql = "select * from usrinfo where username = ？ and password = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, username);
pstmt.setString(2, password);
ResultSet rs = pstmt.executeQuery();
while (rs.next()) {
    ...
}
```

总而言之，预编译 SQL 语句中，参数使用 `?` 占位符，创建好 `pstmt` 之后再给参数赋值，可以有效防止 SQL 注入的问题.

## 抽取 JDBC 工具类

目的是简化以下功能的书写：

+ 注册驱动
+ 创建连接
+ 释放资源

