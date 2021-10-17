# JSP & Servlet

全称 Java Server Pages.

它使用 JSP 标签在 HTML 网页中插入 Java 代码。标签通常以 `<%` 开头以 `%>` 结束。

JSP通过网页表单获取用户输入数据、访问数据库及其他数据源，然后动态地创建网页。

JSP标签有多种功能，比如访问数据库、记录用户选择信息、访问JavaBeans组件等，还可以在不同的网页中传递控制信息和共享信息。

## Tomcat 服务器

### 下载配置

官网下载 Tomcat 的 tar.gz 包，解压到某个位置，为了方便可设置一个环境变量指向它.

例如我设置了环境变量 `TOMCAT=/home/coiggahou/Developer_Tools/apache-tomcat-9.0.50`

通过以下命令即可启动 Tomcat 服务器

```shell
$TOMCAT/bin/startup.sh
```

访问 `localhost:8080` 看到 Apache 页面即成功.

关闭方法：

```shell
sh $TOMCAT/bin/shutdown.sh
```

### JSP 引擎

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/jsp-processing.jpg)

将 JSP 文件转化为 Servlet，然后将 Servlet 编译成可执行类，将原始请求传递给 Servlet 引擎.

一般情况下，JSP 引擎会检查 JSP 文件对应的 Servlet 是否已经存在，并且检查 JSP 文件的修改日期是否早于 Servlet。如果 JSP 文件的修改日期早于对应的 Servlet，那么容器就可以确定 JSP 文件没有被修改过并且 Servlet 有效。这使得整个流程与其他脚本语言（比如 PHP）相比要高效快捷一些。

总的来说，JSP 网页就是用另一种方式来编写 Servlet 而不用成为 Java 编程高手。除了解释阶段外，JSP 网页几乎可以被当成一个普通的 Servlet 来对待。

## Servlet

它是 Server 和 Applet 的缩写，是服务端小程序的意思.

Servlet 主要运行在服务端，由服务器调用执行.

Servlet 本质上就是 Java 类，但是要遵循一定的规范来写，它没有 main 方法，其创建、使用、销毁都由 Servlet 容器进行管理（**提供了 Servlet 功能的服务器就叫 Servlet 容器**，如 Tomcat）

Servlet 可以处理 HTTP 协议相关的所有内容.

### 实现 Servlet

1.创建一个普通 Java 类，继承 HttpServlet

2.重写 Service 方法，用于处理请求

3.在 Servlet 类上方设置注解 `@WebServlet("希望设置的访问路径")` ，制定访问路径

```java
// name没什么用，value可以填单个字符串或者字符串数组，表示外部访问路径（用数组）
@WebServlet(name = "Servlet01", value = {"/ser01", "/ser02"})

public class Servlet01 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //super.service(req, resp);
        System.out.println("---HelloServlet---");
        resp.getWriter().write("Hello Servlet!");
    }
}
```

