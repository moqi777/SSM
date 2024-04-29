## 一、SpringMVC概论

> Spring MVC是由Spring官方提供的基于MVC设计理念的web框架
>
> SpringMVC是基于Servlet封装的用于实现MVC控制的框架，实现前端和服务端的交互。

### 1.1 SpringMVC优势

- 严格遵守了MVC分层思想
- 采用了松耦合、插件式结构，相比较于我们封装的BaseServlet以及其他的一些MVC框架来说更灵活、更具有扩展性
- SpringMVC是基于Spring的扩展、提供了一套完善的MVC注解
- SpringMVC在数据绑定、视图解析都提供了多种处理方式，可灵活配置
- SpringMVC对RESTful URL设计方法提供了良好的支持

### 1.2 SpringMVC本质工作

- 接收并解析请求
- 处理请求
- 数据渲染，响应请求

## 二、SpringMVC框架部署

### 2.1 基于Maven创建一个web工程

创建web项目（基于Maven）

- 在IDEA创建Maven项目
- 在pom.xml中指定web项目的打包方式

````xml
<packaging>war</packaging>
````

- 在main包下面创建一个名为`webapp`的文件夹，webapp包下再创建一个WEB-INF的文件夹，在WEB-INF下再创建一个`web.xml`，模板如下

````xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
</web-app>
````

- 添加web依赖（javax.servlet-api、jsp-api），有两种方式

  - 第一种手动通过tamcat添加，方便但是换了环境需要重新添加

  ````java
  file-->Project Structure...-->左侧Modules-->右侧+号-->Library...-->Tomcat 8.5.41-->Add Selected-->Apply-->OK
  ````

  - 第二种通过pom.xml配置

  ````xml
  <!-- 添加web依赖 -->
  <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
  </dependency>
  <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.0</version>
      <scope>provided</scope>
  </dependency>
  ````

- 点击IDEA右上角添加tamcat

### 2.2 添加SpringMVC依赖

- spring-context
- spring-aspects
- spring-jdbc
- spring-test
- spring-web
- spring-webmvc

```xml
<properties>
    <spring.version>5.2.13.RELEASE</spring.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring.version}</version>
    </dependency>
</dependencies>
```

### 2.3 创建SpringMVC配置文件

- 添加MVC命名空间
- 创建名为spring-mvc的模板，后缀为xml，将下面代码复制至框内，记得勾选上Enable Live Templates
- 在resources目录下创建`spring-servlet.xml`的文件（文件名可以自定义）

````xml
<?xml version='1.0' encoding='UTF-8' ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 声明使用注解配置 -->
    <context:annotation-config/>
    <!-- 声明Spring工厂注解的扫描范围 -->
    <context:component-scan base-package=""/>
    <!-- 基于注解配置的aop代理 -->
    <aop:aspectj-autoproxy/>
    <!-- 声明MVC使用注解驱动 -->
    <mvc:annotation-driven/>

</beans>
````

### 2.4 在web.xml中配置SpringMVC的前端控制器

> SpringMVC提供了一个名为DispatcherServlet的类（SpringMVC前端控制器），用于拦截用户请求交由SpringMVC处理

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 配置服务器启动时的初始化参数 -->
        <!-- 服务器启动的时候就会实例化SpringMVC前端控制器，同时加载Spring配置文件 -->
        <!-- Spring配置文件声明了注解配置，所以会扫描注解 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-servlet.xml</param-value>
        </init-param>
        <!-- 服务器创建时首先来创建该实例，而不是用户请求第一次到达，可选项 -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/*</url-pattern>
    </servlet-mapping>
    
</web-app>
```

## 三、SpringMVC框架使用

> 在SpringMVC中，我们把接收用户请求，处理用户请求的类称之为Controller（控制器）

### 3.1 创建控制器

#### 3.1.1 创建控制器类

- 创建一个名为`com.qfedu.controllers`的包（包需要在被Spring扫描注解范围内）
- 创建一个类（无需做任何的继承和实现）
- 在类上添加`@Controller`注解声明此类为SpringMVC的控制器
- 在类上添加`@RequestMapping("/url")`声明此控制器请求的url

```xml
@Controller
@RequestMapping("/book")
public class BookController {
}
```

#### 3.1.2 在控制器类中定义处理请求的方法

- 在一个控制器类中可以定义多个方法处理不同的请求
- 在每个方法上添加`@RequestMapping("/url")`用于声明当前方法的url

````java
@Controller
@RequestMapping("/book")//类上注解可不写
public class BookController {

    @RequestMapping("/add")
    public void add(){
        System.out.println("-------book add");
    }

    @RequestMapping("/list")
    public void list(){
        System.out.println("--------book list");
    }

}
````

#### 3.1.3 访问

- http://localhost:8080/book/add
- http://localhost:8080/book/list

### 3.2 静态资源配置

> 静态资源：就是项目中的HTML、css、js、图片、字体等

#### 3.2.1 /*和/的区别

> web.xml中配置拦截路径

-  /*	拦截所有的HTTP请求，包括.jsp的请求，都作为控制器类的请求路径来处理
-  /      拦截所有的HTTP请求，但不包括.jsp的请求，但不会放行静态资源（html/css/js/图片）

#### 3.2.2 静态资源配置

- 在webapp文件夹添加css、pages、js、imgs等文件夹存放静态文件

- 在SpringMVC的配置文件`spring-servlet.xml`中，添加如下静态资源放行的配置

````xml
<!-- 配置静态资源放行 -->
<mvc:resources mapping="/css/**" location="/css/"/>
<mvc:resources mapping="/pages/**" location="/pages/"/>
<mvc:resources mapping="/js/**" location="/js/"/>
<mvc:resources mapping="/imgs/**" location="/imgs/"/>
````

### 3.3 前端提交数据到控制器

#### 3.3.1 表单提交

- 表单提交：输入框需要提供name属性，SpringMVC控制器通过name属性取值的

````html
<body>

    <h3>添加图书</h3>
    <form action="book/add" method="get">
        <p>图书名称：<input type="text"></p>
        <p>图书作者：<input type="text"></p>
        <p>图书价格：<input type="text"></p>
        <p><input type="submit" value="提交"></p>
    </form>

</body>
````

#### 3.3.2 URL提交

- URL提交：

````java
<a href="book/add?bookName=java">URL提交</a>
````

#### 3.3.3 AJAX提交

- AJAX提交：请求行、请求头、请求体都可以用来传值

````html
<h3>AJAX提交</h3>
<input type="button" value="ajax提交" id="btn1">
<script type="text/javascript" src="js/jquery-3.3.1.min.js"></script>
<script type="text/javascript">
    $("#btn1").click(function () {
        var obj = {};
        obj.bookName = "java";
        obj.bookAuthor = "张三";
        obj.bookPrice = 3.33;
        $.ajax({
            url:"book/add",
            type:"post",
            headers:{

            },
            contentType:"application/json",
            data:obj,
            success:function (res) {
                console.log(res);
            }
        });
    })
</script>
````

### 3.4 控制器接收前端提交的数据

#### 3.4.1 请求行传值

- 表单提交
- URL提交
- $.ajax()请求的url传值
- $.post/$.get()中的[]传值

**`@RequestParam`**注解用于接收请求行传递的数据

- 前端提交数据

````html
<form action="book/add" method="get">
    <p>图书名称：<input type="text" name="name"></p>
    <p>图书作者：<input type="text" name="author"></p>
    <p>图书价格：<input type="text" name="price"></p>
    <p><input type="submit" value="提交"></p>
</form>
````

- 控制器接收数据

````java
//接受请求行数据
@RequestMapping("/add")
public void add(@RequestParam("name") String a,
                @RequestParam("author") String b,
                @RequestParam("price") double c){
    System.out.println("-------book add");
    System.out.println(a);
    System.out.println(b);
    System.out.println(c);
}
````

`注意：`**如果控制器方法中接收数据的参数名与请求行传值的key一致，则@RequestParam注解可省略**

````java
//接受请求行数据
@RequestMapping("/add")
public void add(String name, String author,double price){
    System.out.println("-------book add");
    System.out.println(name);
    System.out.println(author);
    System.out.println(price);
}
````

#### 3.4.2 请求头传值

- ajax封装请求头数据

````js
$.ajax({
    ...
    headers:{
        
    },
    ...
})
````

**`@RequestHeader`**注解用于接收请求头传递的数据

- 前端

````js
<input type="button" value="ajax提交" id="btn1">
<script type="text/javascript" src="js/jquery-3.3.1.min.js"></script>
<script type="text/javascript">
    $("#btn1").click(function () {
    $.ajax({
        url:"book/list",
        type:"post",
        headers:{
            token:"wahahahahahaha"
        },
        contentType:"application/json",
        success:function (res) {
            console.log(res);
        }
    });
})
</script>
````

- 控制器

````java
@RequestMapping("/list")
public void list(@RequestHeader("token") String token){
    System.out.println(token);
    System.out.println("--------book list");
}
````

#### 3.4.3 请求体传值

- ajax封装请求体数据

````js
$.ajax({
    ...
    contentType:"application/json",
    data:{
        bookName:"Java",
        bookAuthor:"张三",
    },
    ...
})
````

**`@RequestBody`**注解用于接收请求体传递的数据

> @RequestHeader将前端请求体提交的JSON格式数据转换成Java对象，依赖jackson包

- 前端

````html
<input type="button" value="ajax提交" id="btn1">
<script type="text/javascript" src="js/jquery-3.3.1.min.js"></script>
<script type="text/javascript">
    $("#btn1").click(function () {
        var obj = {};
        obj.bookName = "java";
        obj.bookAuthor = "张三";
        obj.bookPrice = 3.33;
        
        var s = JSON.stringify(obj);	//将对象转换成JSON格式
        
        $.ajax({
            url:"book/update",
            type:"post",
            dataType:"json", //设置预期服务器返回的数据类型为JSON
            contentType:"application/json",
            //data的值为json格式字符串，contentType必须设置为"application/json"
            data:s,		
            success:function (res) {
                console.log(res);
            }
        });
    })
</script>
````

- 导入jackson依赖

````xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>
````

- 创建实体类（需要get set ToString方法和有参无参构造器，在此演示代码不写，但实际需要写）

```java
public class Book {
    private int bookId;
    private String bookName;
    private String bookAuthor;
    private double bookPrice;
}
```

- 控制器

````java
@RequestMapping("/update")
public void update(@RequestBody Book book){
    System.out.println("--------book update");
    System.out.println(book);
}
````

### 3.5 控制器响应前端请求

#### 3.5.1 控制器响应同步请求

> 同步请求：from、超链接
>
> 处理同步请求的方法的返回类型定义为String或者ModelAndView，以实现页面的跳转

- 返回类型为String

**请求跳转（转发）**

````java
@RequestMapping("/add")
public String add(String name, String author,double price){
    System.out.println("-------book add");
    //如何跳转到指定的页面
    return "/tips.jsp";
}
````

**重定向**

````java
@RequestMapping("/add")
public String add(String name, String author,double price){
    System.out.println("-------book add");
    //如何跳转到指定的页面
    return "redirect:/tips.jsp";
}
````



- 返回类型为ModelAndView

**请求跳转（转发）**

````java
@RequestMapping("/add")
public ModelAndView add(String name, String author,double price){
    System.out.println("-------book add");
    //如何跳转到指定的页面
    ModelAndView modelAndView = new ModelAndView("/tips.jsp");
    return modelAndView;
}
````

**重定向**

````java
@RequestMapping("/add")
public ModelAndView add(String name, String author,double price){
    System.out.println("-------book add");
    //如何跳转到指定的页面
    ModelAndView modelAndView = new ModelAndView("redirect:/tips.jsp");
    return modelAndView;
}
````

#### 3.5.2 控制器响应异步请求

> 异步请求：ajax请求

**使用response中的输出流进行响应**

- 控制器方法的返回类型为`void`
- 控制器方法添加`HttpServletResponse`参数
- 在方法中通过response获取输出流，使用流响应ajax请求

````java
@RequestMapping("/update")
public void update(@RequestBody Book book, HttpServletResponse response) throws IOException {
    System.out.println("--------book update");
    System.out.println(book);

    //使用ObjectMapper将对象转换为JSON格式字符串（注意在此需要用到上面导的jackson包）
    String s = new ObjectMapper().writeValueAsString(book);
    response.setCharacterEncoding("UTF-8");
    response.setContentType("application/json");
    PrintWriter out = response.getWriter();
    out.println(s);
    out.flush();
    out.close();
}
````

**直接在控制器方法返回响应的对象**

- 控制器方法的返回类型设置为响应给ajax请求的对象类型
- 在控制器方法前添加`@ResponseBody`注解，将返回的对象转换成JSON响应给ajax
- 如果一个控制器类中的所有方法都是响应ajax请求，则可以直接在控制器类前添加`@ResponseBody`注解

````java
@RequestMapping("/update")
@ResponseBody
public List<Book> update(){
    System.out.println("--------book update");
    List<Book> books = new ArrayList<Book>();
    books.add(new Book(1,"Java","老王",30.5));
    books.add(new Book(2,"C++","老李",30.5));
    return books;
}
````

#### 3.5.3 控制器响应同步请求的数据传递

> 对于同步请求的转发响应，我们可以传递参数到转发的页面

- 返回类型为String

````java
//1.在控制器方法中定义一个Model类型的参数
//2.在return页面之前，向Model中添加键值对，添加的键值对就会被传递到转发的页面
@RequestMapping("/add1")
public String add1(String name, String author, double price, Model model){
    model.addAttribute("key1","value1");
    model.addAttribute("book",new Book(1,"Java","老王",30.5));
    return "/tips.jsp";
}

//除了使用Model对象之外，还可以直接使用HttpServletRequest对象
@RequestMapping("/add1")
public String add1(String name, String author, double price, HttpServletRequest request){
    request.setAttribute("key1","value1");
    request.setAttribute("book",new Book(1,"Javas","老王",30.5));
    return "/tips.jsp";
}
````

`注意`转发到的页面想要用值，使用EL表达式${book.bookName}、${key1}

- 返回类型为ModelAndView

```java
@RequestMapping("/add2")
public ModelAndView add2(String name, String author,double price){
    ModelAndView modelAndView = new ModelAndView("/tips.jsp");
    //直接使用modelAndView的addObject方法
    modelAndView.addObject("key1","value1");
    modelAndView.addObject("book",new Book(1,"Java","老王",30.5));
    return modelAndView;
}
```

### 3.6 解决中文乱码问题

#### 3.6.1 前端编码

- jsp页面

```jsp
<!-- 头部在已有的基础上添加pageEncoding属性为UTF-8 -->
<%@ page contentType="text/html;charset=UTF-8" language="java" pageEncoding="UTF-8" %>
```

- html页面

```html
<!-- head标签内添加meta标签设置charset属性为UTF-8，一般都有无需设置 -->
<meta charset="UTF-8">
```

#### 3.6.2 设置服务器编码

- tomcat/conf/server.xml

````xml
<!-- 69行左右，在Connector标签中添加URIEncoding属性设置为UTF-8 -->
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" URIEncoding="UTF-8"/>
````

#### 3.6.3 设置SpringMVC的编码方式

- 在web.xml配置SpringMVC编码过滤器的编码方式

```xml
<filter>
    <filter-name>EncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>EncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

## 四、SpringMVC的请求处理流程

### 4.1 请求处理流程

> SpringMVC 通过前端控制器（DispatcherServlet）拦截并处理用户请求的

![1652087130554](.\img\1652087130554.png)

````txt
①前端发送请求被前端控制器DispatcherServlet拦截
②前端控制器调用处理器映射器HandlerMapping对请求URL进行解析，解析之后返回调用前端控制器
③前端控制器调用处理器适配器处理调用链
④处理器适配器基于反射通过适配器设计模式完成处理器（控制器）的调用处理用户请求
⑤处理器适配器将控制器返回的视图和数据信息封装成ModelAndView响应给前端控制器
⑥前端控制器调用视图解析器ViewResolver对ModelAndView进行解析，将解析结果（视图资源和数据）响应给前端控制器
⑦前端控制器调用视图view组件将数据进行渲染，将渲染结果（视图资源）响应给前端控制器
⑧前端控制器响应用户请求
````

### 4.2 SpringMVC的核心组件

- `DispatcherServlet`前端控制器，总控制器
  - 作用：接受请求，协同各组件工作、响应请求
- `HandlerMapping`处理器映射器
  - 作用：负责根据用户请求的URL找到对应的Handler，返回调用链
- `HandlerAdapter`处理器适配器
  - 作用：按照处理器映射器解析的用户请求的调用链，通过适配器模式完成Handler的调用
  - **可配置**springMVC提供了多个处理器映射的实现，可以根据需要进行配置
- `Handler`处理器/控制器
  - 由工程师根据业务的需求进行开发
  - 作用：处理请求
- `ModelAndView`视图模型
  - 作用：用于封装处理器返回的数据以及相应的视图
  - ModelAndView = Model + View
- `ViewResolver`视图解析器
  - 作用：对ModelAndView进行解析（即找到对应的资源）
  - **可配置**springMVC提供了多个视图解析器的实现，可以根据需要进行配置
- `View`视图
  - 作用：完成数据渲染

### 4.3 处理器映射器

> 不同的处理器映射器对URL处理的方法也不相同，使用对应的处理器之后我们的前端请求规则也需要发生响应的变化
>
> SpringMVC提供的处理器映射器：
>
> - BeanNameUrlHandlerMapping	根据控制器的ID访问控制器
> - SimpleUrlHandlerMapping    根据控制器配置的URL访问（默认）

配置处理器映射器

- 在SpringMVC的配置文件中通过bean标签声明处理器映射器
- 配置BeanNameUrlHandlerMapping	

```xml
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>
```

- 配置SimpleUrlHandlerMapping （一般使用@RequestMapping注解配置）

```xml
<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
    <property name="mappings">
        <props>
            <prop key="/aaa">bookController</prop>
        </props>
    </property>
</bean>
```

### 4.4 视图解析器

> SpringMVC提供了多个视图解析器：
>
> - UrlBasedViewResolver（给请求添加前后缀，需要依赖）
> - InternalResourceViewResolver（给请求添加前后缀，无需依赖）

- UrlBasedViewResolver（需要依赖jstl）

  - 添加jstl依赖

  ````xml
  <!-- https://mvnrepository.com/artifact/javax.servlet/jstl -->
  <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
  </dependency>
  ````

  - 配置视图解析器

  ````xml
  <bean id="viewResolver" class="org.springframework.web.servlet.view.UrlBasedViewResolver">
          <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
          <!-- 在所有请求前面自动添加一个/前缀 -->
          <property name="prefix" value="/"/>
          <!-- 在所有请求后面自动加上一个.jsp后缀 -->
          <property name="suffix" value=".jsp"/>
  </bean>
  ````

- InternalResourceViewResolver（与第一种唯一的区别就是不需要添加依赖，一般用这种）

  ````xml
  <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
      <property name="prefix" value="/"/>
      <property name="suffix" value=".jsp"/>
  </bean>
  ````

  

## 五、日期格式处理

### 5.1 在控制器中使用对象接收数据

- 前端：

````html
<form action="/test/add" method="get">
    <p>图书名称：<input type="text" name="bookName"></p>
    <p>图书作者：<input type="text" name="bookAuthor"></p>
    <p>图书价格：<input type="text" name="bookPrice"></p>
    <p><input type="submit" value="提交"></p>
</form>
````

- 后端：

````java
@Controller
@RequestMapping("/test")
public class TestController {

    @RequestMapping("/add")
    //表单提交的多个数据，在控制器方法中可以使用对象接收
    // 但是提交的数据的key必须要与对象的属性名一致，而且对象必须提供set方法
    public String addBook(Book book){
        System.out.println(book);
        return "tips";
    }

}
````

```java
public class Book {
    private int bookId;
    private String bookName;
    private String bookAuthor;
    private double bookPrice;
    ...set方法...
}
```

### 5.2 日期格式处理

> 如果前端需要输入日期格式，在控制器中转换成Date对象，SpringMVC要求前端输入的日期格式必须为yyyy/MM/dd
>
> 如果甲方要求日期格式必须为指定格式而这个指定格式SpringMVC不接受，该如何处理呢？
>
> - 自定义日期转换器

#### 5.2.1 创建自定义日期转换器

```java
/**
 * 1、创建一个类实现Converter接口，泛型指定从什么类型转换成什么类型
 * 2、实现convert转换方法
 */
public class MyDateConverter implements Converter<String, Date> {
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日");
    public Date convert(String s) {
        try {
            Date date = sdf.parse(s);
            return date;
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

#### 5.2.2 配置自定义转换器

> 将自定义转换器转成一个工厂，然后配置给MVC

```xml
<!-- 声明MVC使用注解驱动 -->
<mvc:annotation-driven conversion-service="converterFactory"/>

<bean id="converterFactory" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <bean class="com.qfedu.utils.MyDateConverter"/>
        </set>
    </property>
</bean>
```

## 六、文件上传下载

### 6.1 SpringMVC框架部署

- 基于Maven创建web工程（略，详情看2.1小节）
- 导入SpringMVC所需的依赖
  - Spring：context aspects jdbc web webmvc jsckson
- 创建SpringMVC配置文件
- 在web.xml中配置SpringMVC前端控制器
- 在web.xml中配置SpringMVC的编码过滤器
  - 配置SpringMVC静态资源处理策略

**index.jsp**

```html
<table width="100%" height="700">
    <tr>
        <td width="200" style="border-right: deepskyblue 2px solid;background: rgba(255,0,0,0.1)">
            <ul>
                <li><a href="book-add.jsp" target="mainFrame">上传图片</a></li>
                <li><a href="list.jsp" target="mainFrame">文件列表</a></li>
            </ul>
        </td>
        <td>
            <iframe name="mainFrame" width="100%" height="700" frameborder="0"></iframe>
        </td>
    </tr>
</table>
```

### 6.2 文件上传

> 案例：添加图书，同时提交图书的封面图片

#### 6.2.1 前端提交文件

- 表单提交方式必须为post
- 表单enctype属性设置为`multipart/from-date`

**book-add.jsp**

````html
<h4>添加图书信息</h4>
<form action="book/add" method="post" enctype="multipart/form-data">
    <p>图书名称：<input type="text" name="bookName"></p>
    <p>图书作者：<input type="text" name="bookAuthor"></p>
    <p>图书价格：<input type="text" name="bookPrice"></p>
    <p>图书封面：<input type="file" name="imgFile"></p>
    <p><input type="submit" value="提交"></p>
</form>
````

#### 6.2.2 控制器接收数据和文件

> SpringMVC处理上传文件需要借助于CommonsMultipartResolver文件解析器

- 添加依赖：commons-io    commons-fileupload

````xml
<!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.4</version>
</dependency>
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.4</version>
</dependency>
````

- 在spring-servlet.xml配置文件解析器

```xml
<!-- 配置文件解析器 -->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="maxUploadSize" value="10240000"/>
    <property name="maxInMemorySize" value="102400"/>
    <property name="defaultEncoding" value="utf-8"/>
</bean>
```

- 创建实体类

```java
public class Book {

    private int bookId;
    private String bookName;
    private String bookAuthor;
    private double bookPrice;
    private String bookImg;
	...get set方法，有参无参构造器，ToString方法...
	
}
```

- 控制器接收文件
  - 在处理文件上传的方法中定义一个MultipartFile类型的对象，就可以接收图片了

```java
@Controller
@RequestMapping("/book")
public class BookController {

    @RequestMapping("/add")
    public String addBook(Book book, MultipartFile imgFile, HttpServletRequest request) throws IOException {
        System.out.println("---------------add");
        System.out.println(book);
        //imgFile就表示上传的图片
        //1、截取上传文件的后缀名，生成新的文件名
        String originalFilename = imgFile.getOriginalFilename();
        //.lastIndexOf 获取到字符的下标    .substring 截取从参数位为往后字符串
        String ext = originalFilename.substring(originalFilename.lastIndexOf("."));//.jpg
        String fileName = System.currentTimeMillis()+ext;

        //2、获取imgs目录在服务器的路径
        String dir = request.getServletContext().getRealPath("imgs");
        String savePath = dir+"/"+fileName;
        //或者也可以保存在自定义目录下
        String path = "D:\\MyMaven\\springmvc-demo2\\src\\main\\webapp\\imgs\\"+fileName;

        //3、保存文件
        imgFile.transferTo(new File(path));

        //4、将图片的访问路径设置到book对象
        book.setBookImg("imgs/"+fileName);

        //5、调用service保存book保存数据库
        //略

        return "/tips.jsp";
    }

}
```

### 6.3 文件下载

#### 6.3.1 显示文件列表

> js文件夹下导入jQuery包jquery-3.3.1.min.js
>
> 使用了bootstrap前端框架展示图片，使用CDN无需导入依赖，联网即可。bootcss.com

**list.jsp**

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
    <!-- 最新版本的 Bootstrap 核心 CSS 文件 -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css" integrity="sha384-HSMxcRTRxnN+Bdg0JdbxYKrThecOKuH5zCYotlSAcp1+c8xmyTe9GYg1l9a69psu" crossorigin="anonymous">
    <!-- 可选的 Bootstrap 主题文件（一般不用引入） -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap-theme.min.css" integrity="sha384-6pzBo3FDv/PJ8r2KRkGHifhEocL+1X2rVCTTkUfGk7/0pbek5mMa1upzvWbrUbOZ" crossorigin="anonymous">
    <!-- 最新的 Bootstrap 核心 JavaScript 文件 -->
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js" integrity="sha384-aJ21OjlMXNL5UyIl/XNwTMqvzeRMZH2w8c5cRVpzpU8Y5bApTppSuUkhZXN0VxHd" crossorigin="anonymous"></script>
<body>

    <h4>图片列表</h4>

    <div class="row" id="container">

    </div>

    <script type="text/javascript" src="js/jquery-3.3.1.min.js"></script>
    <script type="text/javascript">
        $.get("book/list",function (res) {
            console.log(res)
            for (var i = 0; i < res.length; i++) {
                var fn = res[i];
                var htmlStr = "<div class='col-lg-2 col-md-3 col-sm-4 col-xs-6'> <div class='thumbnail'><img src='imgs/"+fn+"' alt='...'><div class='caption'><p><a href='#' class='btn btn-primary' role='button'>下载</a></p></div> </div></div>"
                $("#container").append(htmlStr)
            }
        },"json");
    </script>

</body>
</html>
```

- BookController

````java
@RequestMapping("/list")
@ResponseBody
public String[] listImgs(HttpServletRequest request){
    //1、从img目录下获取所有的图片信息
    String dir = request.getServletContext().getRealPath("imgs");
    File imgDir = new File(dir);
    String[] fileNames = imgDir.list();
    return fileNames;
}
````

#### 6.3.2 实现文件下载

- list.jsp（修改了a标签的href属性）

```jsp
var htmlStr = "<div class='col-lg-2 col-md-3 col-sm-4 col-xs-6'> <div class='thumbnail'><img src='imgs/"+fn+"' alt='...'><div class='caption'><p><a href='book/download?fname="+fn+"' class='btn btn-primary' role='button'>下载</a></p></div> </div></div>"
```

- BookController

```java
@RequestMapping("/download")
public void downloadImg(String fname,HttpServletRequest request, HttpServletResponse response) throws Exception {
    //从imgs目录找到当前文件
    String dir = request.getServletContext().getRealPath("imgs");
    String filePath = dir + "/" + fname;
    FileInputStream fileInputStream = new FileInputStream(filePath);

    //设置一个浏览器无法识别的类型，不然浏览器接收到图片会直接打开而不是下载
    response.setContentType("application/exe");
    //设置响应头，在响应头中将文件名写入，使得浏览器能正确下载
    response.addHeader("Content-Disposition","attachment;filename="+fname);

    IOUtils.copy(fileInputStream,response.getOutputStream());
}
```

## 七、统一异常处理

> 在我们的应用系统运行的过程中，可能由于运行环境，用户操作、资源不足等各方面的原因导致系统出现异常（HTTP状态异常、Exception）；如果系统出现了异常，这些异常将会通过浏览器呈现给用户，这种异常的显示是没有必要的，因此我们可以在服务器进行特定的处理——当系统出现异常之后，呈现给用户一个统一的、可读的异常提示页面。

### 7.1 HTTP异常状态统一处理

> HTTP Status 404

- 创建一个用于进行异常提示的页面：404.jsp
- 在web.xml中进行配置：

```xml
<error-page>
    <error-code>404</error-code>
    <location>/404.jsp</location>
</error-page>
```

### 7.2 Java代码异常的统一处理

#### 7..2.1 基于Servlet-api的处理

- 创建异常提示页面：err.jsp
- 在web.xml中进行配置

```xml
<error-page>
    <exception-type>java.lang.Exception</exception-type>
    <location>/err.jsp</location>
</error-page>
```

#### 7.2.2 SpringMVC处理

- 使用异常处理类进行统一处理

```java
@ControllerAdvice
public class MyExceptionHandler {

    @ExceptionHandler(NullPointerException.class)
    public String nullHandler(){
        return("/err1.jsp");
    }

    @ExceptionHandler(NumberFormatException.class)
    public String formatHandler(){
        return("/err2.jsp");
    }

}
```

## 八、拦截器

### 8.1 拦截器介绍

> SpringMVC提供的拦截器就类似于Servlet-apl中的过滤器，可以对控制器的请求进行拦截实现相关的预处理和后处理。

- 过滤器
  - 是Servlet规范的一部分，所有的web项目都可以使用
  - 过滤器在web.xml配置（可以使用注解），能够拦截所有web请求
- 拦截器
  - 是SpringMVC框架的实现，只有在SpringMVC框架中才能使用
  - 拦截器在SpringMVC配置文件进行配置，不会拦截SpringMVC放行的资源（jsp/html/css..(3.2节配置了放行)）

### 8.2 自定义拦截器

#### 8.2.1 创建拦截器

- 创建一个类实现HandlerInterceptor接口
- 实现两个方法preHandle、postHandle

```java
public class MyInterceptor1 implements HandlerInterceptor {

    //预处理方法，返回值是否放行
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("------------------预处理");
        Enumeration<String> keys = request.getParameterNames();
        while (keys.hasMoreElements()){
            String key = keys.nextElement();
            if ("bookId".equals(key)){
                return true;
            }
        }
        response.setStatus(415);
        return false;
    }
    //后处理方法
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        modelAndView.addObject("tips","这是通过拦截器的后处理添加的数据");
        System.out.println("------------------后处理");
    }
}
```

#### 8.2.2 配置拦截器

**spring-servlet.xml**

```xml
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/book/query"/>
        <!-- 可以配置多个拦截路径 -->
        <mvc:mapping path="/book/add"/>
        <!-- 会拦截Student中的所有，除了add -->
        <mvc:mapping path="/student/**"/>
        <mvc:exclude-mapping path="/studnet/add"/>
        <bean class="com.qfedu.utils.MyInterceptor1"/>
    </mvc:interceptor>
</mvc:interceptors>
```

#### 8.3 拦截器链

> 将多个拦截器按照一定的顺序构成一个执行链（按照在配置文件中上下位置的顺序来）

```xml
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/book/query"/>
        <bean class="com.qfedu.utils.MyInterceptor1"/>
    </mvc:interceptor>
    <mvc:interceptor>
        <mvc:mapping path="/book/query"/>
        <bean class="com.qfedu.utils.MyInterceptor2"/>
    </mvc:interceptor>
</mvc:interceptors>
```

![1652347993989](.\img\1652347993989.png)

## 九、SSM整合

### 9.1 创建web项目

略，详情看2.1小节

### 9.2 部署MyBatis

- 添加MyBatis依赖

````xml
<!-- mysql驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
<!-- MyBatis依赖 -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.6</version>
</dependency>
<!-- lombok依赖 -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.16</version>
    <scope>provided</scope>
</dependency>
````

- 创建MyBatis配置文件`mybatis-config.xml`

**使用mybatis-config模板**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
```

### 9.3 部署Spring、SpringMVC

#### 9.3.1 添加依赖

````xml
<properties>
    <spring.version>5.2.13.RELEASE</spring.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring.version}</version>
    </dependency>
</dependencies>
````

````xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>
````

#### 9.3.2 创建Spring配置

> 多配置文件分开配置，解耦

- spring-context.xml		只配置注解声明、以及类的管理

**使用spring-ioc-annotation模板**

```xml
<?xml version='1.0' encoding='UTF-8' ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 声明使用注解配置 -->
    <context:annotation-config/>

    <!-- 声明Spring工厂注解的扫描范围 -->
    <context:component-scan base-package="com.qfedu"/>

</beans>
```

- spring-mvc.xml               进行mvc相关配置、拦截器配置等

**使用spring-mvc模板，删除了context和aop命名空间与对应标签**

```xml
<?xml version='1.0' encoding='UTF-8' ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    
    <!-- 声明MVC使用注解驱动 -->
    <mvc:annotation-driven/>

</beans>
```

- spring-mybatis.xml        进行Spring与MyBatis整合的相关配置

**使用spring-mvc模板，删除了mvc命名空间与所有标签**

```xml
<?xml version='1.0' encoding='UTF-8' ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

</beans>
```

#### 9.3.3 配置SpringMVC前端控制器

- 在web.xml进行配置（注意与之前不同之处"*"）

````xml
<servlet>
    <servlet-name>SpringMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring-*.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>SpringMVC</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
````

### 9.4 整合配置（IoC）

#### 9.4.1 导入mybatis-spring依赖

````xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.2</version>
</dependency>
````

#### 9.4.2 配置druid连接池

- 添加druid依赖

````xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.10</version>
</dependency>
````

- 创建druid.properties并配置

````properties
druid.driver=com.mysql.jdbc.Driver
druid.url=jdbc:mysql://localhost:3306/db_2010_mybatis?characterEncoding=utf-8
druid.username=root
druid.password=kaipule452b.

**   连接池参数
druid.pool.init=1
druid.pool.minIdle=3
druid.pool.maxActive=20
druid.pool.timeout=30000
````

- 在spring-mybatis.xml配置数据源

```xml
<context:property-placeholder location="classpath:durid.properties"/>

<bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="${druid.driver}"/>
    <property name="url" value="${druid.url}"/>
    <property name="username" value="${druid.username}"/>
    <property name="password" value="${druid.password}"/>

    <property name="initialSize" value="${druid.pool.init}"/>
    <property name="minIdle" value="${druid.pool.minIdle}"/>
    <property name="maxActive" value="${druid.pool.maxActive}"/>
    <property name="maxWait" value="${druid.pool.timeout}"/>
</bean>
```

#### 9.4.3 配置SqlSessionFactory

- 在spring-mybatis.xml配置

````xml
<bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="druidDataSource"/>
    <property name="mapperLocations" value="classpath:mappers/*.xml"/>
    <property name="typeAliasesPackage" value="com.qfedu.bean"/>
    <property name="configLocation" value="classpath:mybatis-config.xml"/>
</bean>
````

#### 9.4.4 配置MapperScannerConfigurer

- 在spring-mybatis.xml配置

````xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactoryBean"/>
    <property name="basePackage" value="com.qfedu.dao"/>
</bean>
````

### 9.5 整合配置（AOP）

> 使用Spring提供的事务管理类完成Dao操作的事务管理
>
> 基于注解的事务管理配置：
>
> 在此小节操作需要导入tx命名空间（因用不到aop的命名空间，故直接通过将五个aop单词改成tx即可，方便一点）
>
> ````xml
> xmlns:tx="http://www.springframework.org/schema/tx"
> 
> http://www.springframework.org/schema/tx
> http://www.springframework.org/schema/tx/spring-tx.xsd">
> ````

- 将Spring提供的事务管理切面类配置到Spring容器
- 在spring-mybatis.xml配置

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="druidDataSource"/>
</bean>

<tx:annotation-driven transaction-manager="transactionManager"/>
```

## 9.6 整合测试

#### 9.6.1 完成User的查询操作

* 创建实体类

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@ToString
public class User {

    private int user_Id;
    private String userName;
    private String userPwd;
    private String userRealname;
    private String userImg;

}
```

* 在dao包中创建接口

```java
public interface UserDao {

    public User QueryUserByName(String name);

}
```

* 在mappers目录下创建映射文件

**UserMapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.qfedu.dao.UserDao">

    <resultMap id="userMap" type="User">
        <id column="user_id" property="userId"/>
        <result column="user_name" property="userName"/>
        <result column="user_pwd" property="userPwd"/>
        <result column="user_realname" property="userRealname"/>
        <result column="user_img" property="userImg"/>
    </resultMap>

    <select id="QueryUserByName" resultMap="userMap">
        select user_id,user_name,user_pwd,user_realname,user_img
        from users
        where user_name = #{userName}
    </select>

</mapper>
```

#### 9.6.2 对Dao单元测试

* 添加junit、spring-test依赖（spring-test上面已导过，故不再导）

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <!-- 该便签表示此依赖包仅做为测试，不参与打包 -->
    <scope>test</scope>
</dependency>
```

* 创建测试类

@RunWith(SpringJUnit4ClassRunner.class)     让测试运行于Spring测试环境

@ContextConfiguration Spring整合JUnit4测试时，使用注解引入多个配置文件

````xml
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration({"classpath:spring-context.xml","classpath:spring-mvc.xml","classpath:spring-mybatis.xml"})
public class UserDaoTest {

    @Resource
    private UserDao userDao;

    @Test
    public void queryUserByName() {
        User user = userDao.QueryUserByName("韩梅梅");
        System.out.println(user );
    }
}
````

#### 9.6.3 对Service单元测试

* 在service包下创建接口

```java
public interface UserService {

    public User checkLogin(String userName,String userPwd);

}
```

* 在service包下创建impl包，在impl包下创建类实现接口

```java
@Service
public class UserServiceImpl implements UserService {

    @Resource
    private UserDao userDao;

    public User checkLogin(String userName, String userPwd) {
        User user = userDao.QueryUserByName(userName);
        //加密省略
        if (user == null){
            return null;
        }
        if (user.getUserPwd().equals(userPwd)){
            return user;
        }else {
            return null;
        }
    }
}
```

* 创建测试类

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration({"classpath:spring-context.xml","classpath:spring-mvc.xml","classpath:spring-mybatis.xml"})
public class UserServiceImplTest {

    @Resource
    private UserService userService;

    @Test
    public void testcheckLogin() {
        User user = userService.checkLogin("韩梅梅", "11111");
        assertNotNull(user);
    }
}
```

#### 9.6.4 对controller单元测试

- 创建在controller包下创建controller类

```java
@Controller
@RequestMapping("/user")
public class UserController {

    @Resource
    private UserService userService;

    @RequestMapping("/login")
    public String login(String userName, String userPwd, HttpServletRequest request){
        User user = userService.checkLogin(userName, userPwd);
        if (user == null){
            request.setAttribute("tips","用户名或密码错误！");
            return "/login.jsp";
        }else {
            request.getSession().setAttribute("user",user);
            return "redirect:/index.jsp";
        }
    }

}
```

* 创建两个个jsp页面

**index.jsp**

```html
<body>
<h3>这是主页面</h3>
欢迎您，${user.userName}
</body>
```

**login.jsp**

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <base href="${pageContext.request.contextPath}/">
    <title>Title</title>
</head>
<body>
<h3>登录页面</h3>
${tips}
<form action="user/login" method="post">
    <p>账号：<input type="text" name="userName"></p>
    <p>密码：<input type="password" name="userPwd"></p>
    <p>密码：<input type="submit" value="登录"></p>
</form>
</body>
</html>
```

注意：因为在controller中登录失败使用的是转发，路径不会改变，所以在此页面发起请求的时候需要改变路径（在项目根目录下）。登录用的时候中文名时会出错，解决方式详情看3.6.3小节

最后，在浏览器中输入http://localhost:8080/user/login测试即可