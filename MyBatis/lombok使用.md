## lombok使用

### 1. 介绍

- 定义：Lombok 是一种 Java 实用工具，可用来帮助开发人员消除 Java 的冗长，尤其是对于简单的 Java 对象（POJO）。它通过注解实现这一目的。



拿lombok官网的一个例子来说:

```
public class Mountain{
    private String name;
    private double longitude;
    private String country;
}
```



要使用这个对象,必须还要写一些getter和setter方法,可能还要写一个构造器、equals方法、或者hash方法.这些方法很冗长而且没有技术含量,我们叫它**样板式代码**.

lombok的主要作用是通过一些注解，消除样板式代码，像这样：

![](D:\A-学习资料\SSM\img\a2bed7a1f670361d93f1c51208d200fe_r.png)

如果觉得@Data这个注解有点简单粗暴的话,Lombok提供一些更精细的注解,比如@Getter,@Setter,(这两个是field注解),@ToString,@AllArgsConstructor(这两个是类注解).

具体使用在https://www.zhihu.com/question/42348457中

### 2.配置

首先在IDEA插入插件：settings-->Pluging-->右侧搜索框搜Lombok，图标为一个小辣椒，下载重启插入

然后将jar包导入项目lib中即可使用

jar包下载地址https://projectlombok.org/download

Manve项目下载地址https://mvnrepository.com/artifact/org.projectlombok/lombok

```
		<!-- Lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.22</version>
            <scope>provided</scope>
        </dependency>
```

### 3. 实例(常用)

````java
@Data
@NoArgsConstructor
@AllArgsConstructor
@ToString
public class User {
    private int userId;
    private String userName;
    private String userPwd;
    private String userRealname;
    private String userImg;
}
````

@Data包括了get和set

@NoArgsConstructor是无参构造器

@AllArgsConstructor是有参构造器

@ToString构造器