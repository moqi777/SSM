## 一、MyBatis简介

### 1.1 框架概念

框架，就是软件的半成品，完成了软件开发过程中的通用操作，程序员只需很少或者不用进行加工就能够实现特定的功能，从而简化了开发人员在软件开发中的步骤，提高效率。

### 1.2 常用框架

- MVC框架：简化了Servlet的开发步骤
  - Struts
  - Struts2
  - **SpringMVC**
- 持久层框架：完成数据库操作的框架
  - apache DBUtils
  - Hibernate
  - Spring JPA
  - **MyBatis**
- 胶水框架：**Spring**

SSM   Spring Spring MyBatis

SSH   Spring Struts2 Hiberbate

### 1.3 MyBatis介绍

MyBatis是一个**半自动**的**ORM**框架（全自动：Hibernate）

ORM（Object Relational Mapping）对象关系映射，将java中的一个对象与数据表中的一行记录一一对应。

ORM框架提供了实体类与数据表的映射关系，通过映射文件的配置，实现对象的持久化。

- MyBatis的前身是iBatis，iBatis是Apache软件基金会提供的一个开源项目。
- 2010年iBatis迁移到Google code，正式更名为MyBatis
- 2013年迁移到Github托管
- MyBatis特点
  - 支持自定义SQL、存储过程
  - 对原有的JDBC进行了封装，几乎消除了所有JDBC代码，让开发者只需关注SQL本身
  - 支持XML和注解配置的方式自动完成ORM操作，实现结果映射

## 二、MyBatis框架部署

> 框架部署，就是将框架引入到我们的项目中

### 2.1 创建Maven项目

- Java工程
- Web工程

### 2.2 在项目中添加Mybatis依赖

- 在pom.xml中添加依赖（写在dependencies标签中）

  - mybatis
- mysql driver
  
| pom.xml，Maven库网址：https://mvnrepository.com/ |
| ------------------------------------------------ |
| ![1650463518193](.\img\1650463518193.png)        |

  

### 2.3 创建MyBatis配置文件

- 创建自动义模板：src-->resources，右键New-->Edit File Templates...，点左上角+号，Name填mybatis-config，Extension填xml，框内复制粘贴MyBatis模板，勾选上Enable Live Templates，最后Apply。

- mybatis配置文件模板代码：

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
```

- 在resources中创建mybatis-config.xml文件
- 在mybatis-config.xml文件配置数据库连接信息

| mybatis-config.xml                        |
| ----------------------------------------- |
| ![1650526294599](.\img\1650526294599.png) |



## 三、MyBatis框架使用

> 案例：学生信息的数据库库操作

### 3.1 创建数据表

| tb_students                               |
| ----------------------------------------- |
| ![1650522191078](.\img\1650522191078.png) |

### 3.2 创建实体类

使用Lombok，详解在 "lombok使用.md" 中，需导包

| Student.java                 |
| ---------------------------- |
| ![](.\img\1650454708349.png) |

### 3.3 创建Dao接口，定义操作方法

StudentDao.java

| StudentDao.java                           |
| ----------------------------------------- |
| ![1650457219707](.\img\1650457219707.png) |

### 3.4 创建Dao接口的映射文件

- 在resources目录下，新建名为mappers文件夹
- 在mappers中新建名为StudentMapper.xml的映射文件（根据模板创建，没有模板创建模板，模板名为mapper，按上面方式创建）

mapper模板：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="">

</mapper>
```

- 在映射文件中对Dao中定义的文件进行实现：

| StudentMapper.xml                         |
| ----------------------------------------- |
| ![1650522343138](.\img\1650522343138.png) |

### 3.5 将映射文件添加至主配置文件

| mybatis-config.xml                        |
| ----------------------------------------- |
| ![1650456807429](.\img\1650456807429.png) |

## 四、单元测试

### 4.1 添加单元测试依赖

```
<dependency>
   <groupId>junit</groupId>
   <artifactId>junit</artifactId>
   <version>4.12</version>
</dependency>
```

### 4.2 创建单元测试类

> 手动创建：在test包下手动创建class继承接口(也可不继承)，实现方法，在方法头上写@Test

> 使用快捷键创建（如果想要可以直接勾选方法需要先将interface改成class，创建好测试类后再改会interface）：

| 在被测试类名后alt+insert --- 选择Test。                      |
| ------------------------------------------------------------ |
| ![1650457391360](.\img\1650457391360.png)                    |
| 进入后Testing library选择JUnit4，勾选上下面需要的测试方法    |
| ![1650528209713](C:\Users\86134\AppData\Roaming\Typora\typora-user-images\1650528209713.png) |

### 4.3 测试代码

```
package com.qfedu.dao;

import com.qfedu.pojo.Student;
import com.sun.javaws.security.Resource;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

import static org.junit.Assert.*;

public class StudentDaoTest {

        @org.junit.Test
    public void insertStudent(){

        try {
            //加载mybatis配置文件
            InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
            SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
            //会话工厂
            SqlSessionFactory factory = builder.build(is);
            //会话（连接）
            SqlSession sqlSession = factory.openSession();
            StudentDao studentDao = sqlSession.getMapper(StudentDao.class);
            //通过会话获取Dao对象
            System.out.println(studentDao);
            //测试StudentDao中的方法
            int i = studentDao.insertStudent(new Student(0,"10001","张三","男",21));
            //需要手动提交
            sqlSession.commit();
            System.out.println(i);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    @org.junit.Test
    public void deleteStudent(){
    }

}
```

## 五、MyBatis的CRUD操作

> 案例：学生信息的增删查改

### 5.1 添加操作

略

### 5.2 删除操作

>根据学号删除一条学生信息

- 在StudentDao中定义删除方法

| StudentDao                                |
| ----------------------------------------- |
| ![1650529039838](.\img\1650529039838.png) |

- 在StudentMapper.xml中对接口方法进行“实现”

| StudentMapper.xml                         |
| ----------------------------------------- |
| ![1650529169276](.\img\1650529169276.png) |

- 测试：在StudentDao的测试类中添加测试方法

| StudentDaoTest                            |
| ----------------------------------------- |
| ![1650531023877](.\img\1650531023877.png) |

### 5.3 修改操作

> 根据学生学号，修改其他字段

- 在StudentDao中定义修改方法

| StudentDao                                |
| ----------------------------------------- |
| ![1650541900855](.\img\1650541900855.png) |

- 在StudentMapper.xml中对接口方法进行“实现”

| StudentMapper.xml                         |
| ----------------------------------------- |
| ![1650541978564](.\img\1650541978564.png) |

- 单元测试

注：单元测试规范使用assert断言查看结果，例如assertEquals（参数1，参数2）。参数1为预期值，2为实际值

| StudentDaoTest                            |
| ----------------------------------------- |
| ![1650542094477](.\img\1650542094477.png) |

### 5.4 查询操作-查询所有

> 查询学生所有信息

- 在StudentDao接口中定义查询方法

| StudentDao                                |
| ----------------------------------------- |
| ![1650544608243](.\img\1650544608243.png) |

- 在StudentMapper.xml中“实现”Dao中定义的方法

| StudentMapper.xml，第一种使用取别名的方式  |
| ------------------------------------------ |
| ![1650544759299](.\img\1650544759299.png)  |
| StudentMapper.xml，第二种使用resultMap映射 |
| ![1650544810058](.\img\1650544810058.png)  |

- 单元测试

| StudentDaoTest                            |
| ----------------------------------------- |
| ![1650545085328](.\img\1650545085328.png) |

### 5.5 查询操作-查询一条记录

> 根据学号查询一个学生信息

- 在StudentDao接口中定义方法

| StudentDao                                |
| ----------------------------------------- |
| ![1650546155513](.\img\1650546155513.png) |

- 在StddentDaoMapper.xml中配置StudentDao接口的方法实现——SQL

| StddentDaoMapper.xm                       |
| ----------------------------------------- |
| ![1650546212768](.\img\1650546212768.png) |

- 单元测试

| StudentDaoTest                            |
| ----------------------------------------- |
| ![1650546249926](.\img\1650546249926.png) |

### 5.6 查询操作-多参数查询

> 分页查询
>
> - 分页查询（参数	start , pageSize）

- 在StudentDao定义操作方法，如果方法有多个参数，使用@Param注解声明参数的别名

| StudentDao                                |
| ----------------------------------------- |
| ![1650628720148](.\img\1650628720148.png) |

- 在StudentMapper.xml中配置sql时，使用#{别名}获取到指定的参数

| StudentMapper.xml                         |
| ----------------------------------------- |
| ![1650629039827](.\img\1650629039827.png) |

'**'注意'**' 如果Dao操作方法没有通过@Param指定参数别名，在SQL中也可以通过arg0,arg1,...或者param1,param2,...获取参数

- 单元测试

略

### 5.7 查询操作-查询总记录数

- 在StudentDao接口中定义方法

| StudentDao                                |
| ----------------------------------------- |
| ![1650672633178](.\img\1650672633178.png) |

- 在StudentMapper.xml配置sql，通过resultType指定当前操作的返回类型为int

| StudentMapper.xml                         |
| ----------------------------------------- |
| ![1650672680773](.\img\1650672680773.png) |

### 5.8 添加操作回填生成的主键

- StudentMapper.xml的添加操作标签——insert

```
<!-- useGeneratedKeys 设置添加操作是否需要回填生成的主键 -->
<!-- keyProperty设置 设置回填的属性值赋值到参数对象的哪个属性 -->
<insert id="insertStudent" useGeneratedKeys="true" keyProperty="stuId">
	insert into tb_students(stu_num,stu_name,stu_gender,stu_age)
	values(#{stuNum},#{stuName},#{stuGender},#{stuAge})
</insert>
```

## 六、MyBatis工具类封装

- MyBatisUtil（初步）

```java
public class MyBatisUtil {

    private static  SqlSessionFactory factory;
    //使用线程锁实现只需打开一次Session，防止资源浪费
    private static final ThreadLocal<SqlSession> Local = new ThreadLocal<SqlSession>();

    //使用静态方法使得只需加载一次配置文件，防止资源浪费
    static {
        try {
            //加载mybatis配置文件
            InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
            SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
            //会话工厂
            factory = builder.build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static SqlSessionFactory getFactory(){
        return factory;
    }

    public static SqlSession getSqlSession(){
        SqlSession sqlSession = Local.get();
        if (sqlSession == null){
            sqlSession = factory.openSession();
            Local.set(sqlSession);
        }
        return sqlSession;
    }

    public static <E extends Object>E getMapper(Class<E> c){
        SqlSession sqlSession = getSqlSession();
        return sqlSession.getMapper(c);
    }

}
```

## 七、 事务管理

> SqlSession对象
>
> - getMapper（Dao.class）获取Mapper（Dao接口的实例）
> - 事务管理

### 7.1 手动提交事务

- `sqlSession.commit();`提交事务
- `sqlSession.rollback();事务回滚`

```
    @Test
    public void insertStudent(){
            SqlSession sqlSession = MyBatisUtil.getSqlSession();

            //1、当我们获取SqlSession对象时，就默认开启了事物
            try {
                //通过会话获取Dao对象
                StudentDao studentDao = sqlSession.getMapper(StudentDao.class);
                //测试StudentDao中的方法
                Student student = new Student(0, "10005", "Lily", "女", 21);
                int i = studentDao.insertStudent(student);
                System.out.println(student);
                //2、操作完成并成功之后，需要手动提交需要手动提交
                sqlSession.commit();
                System.out.println(i);
            }catch (Exception e){
                //3、当操作出现异常，调用rollback进行回滚
                sqlSession.rollback();
            }
    }
```

### 7.2 自动提交事务

> 通过SqlSessionFactory调用openDession方法获取SqlSession对象时，可以通过参数设置事物是否自动提交
>
> - 如果参数设置为true，表示自动提交事务：sqlSession = factory.openSession(true);
>
> - 如果参数设置为false，或者不设置参数，表示手动提交:sqlSession = factory.openSession();/sqlSession = factory.openSession(false);

- MyBatisUtil优化（最终版本）

```
public class MyBatisUtil {

    private static  SqlSessionFactory factory;
    //使用线程锁实现只需打开一次Session，防止资源浪费
    private static final ThreadLocal<SqlSession> Local = new ThreadLocal<SqlSession>();

    //使用静态方法使得只需加载一次配置文件，防止资源浪费
    static {
        try {
            //加载mybatis配置文件
            InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
            SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
            //会话工厂
            factory = builder.build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static SqlSessionFactory getFactory(){
        return factory;
    }

    private static SqlSession getSqlSession(boolean isAutoCommit){
        SqlSession sqlSession = Local.get();
        if (sqlSession == null){
            sqlSession = factory.openSession(isAutoCommit);
            Local.set(sqlSession);
        }
        return sqlSession;
    }

    //手动事务管理
    public static SqlSession getSqlSession(){
        return getSqlSession(false);
    }

    //自动事务提交
    public static <E extends Object>E getMapper(Class<E> c){
        SqlSession sqlSession = getSqlSession(true);
        return sqlSession.getMapper(c);
    }

}
```

- 测试操作

```
@Test
    public void testUpdateStudent(){
        StudentDao studentDao = MyBatisUtil.getMapper(StudentDao.class);
        int i = studentDao.updateStudent(new Student(0, "10001", "韩梅梅", "女", 18));
        assertEquals(1,i);
    }
```

- 业务逻辑层自动事务管理

```
public class StudentServiceImpl implements StudentService {

    private StudentDao studentDao = MyBatisUtil.getMapper(StudentDao.class);

    public boolean addStudent(Student student) {
        int i = studentDao.insertStudent(student);
        return i>0;
    }
}
```

## 八、MyBatis主配置文件

> mybatis-config.xml是MyBatis框架的主配置文件，主要用于配置MyBatis数据源及属性信息
>
> `注意`environments标签是必有的，其他标签若想使用一定要按顺序

### 8.1 properties标签

> 用于设置键值对，或者加载属性文件

- 在resources目录下创建`jdbc.properties`文件，配置键值对如下

```
mysql_driver=com.mysql.jdbc.Driver
mysql_url=jdbc:mysql://localhost:3306/user?characterEncoding=utf-8
mysql_username=root
mysql_password=kaipule452b.
```

- 在mybatis-config.xml中通过`properties`标签引用`jdbc.properties`文件，引入之后，在配置environment时，可以直接使用jdbc.properties的key获取对应的value

| mybatis-config.xml                        |
| ----------------------------------------- |
| ![1650697404737](.\img\1650697404737.png) |

### 8.2 settings标签

```
<!-- 设置mybatis的属性 -->
<settings>
    <!-- 启动二级缓存 -->
    <setting name="cacheEnabled" value="true"/>
    <!-- 启动延迟加载 -->
    <setting name="lazyLoadingEnabled" value="true"/>
</settings>
```

### 8.3 typeAliases标签

```
    <!-- typeAliases标签用于给实体类取别名，在映射文件中可以直接使用别名来替代实体类的权限定名 -->
    <typeAliases>
        <typeAlias type="com.qfedu.pojo.Student" alias="Student"></typeAlias>
    </typeAliases>
```

### 8.4 plugins标签

```
    <!-- plugins标签：主要用于配置MyBatis插件（例如分页插件） -->
    <plugins>
        <plugin interceptor=""></plugin>
    </plugins>
```

### 8.5 environments标签

```
<!--    在environments中配置数据库连接信息-->
<!--    在environments标签中可以定义多个environment标签，每个environment标签中可以定义一套连接配置-->
<!--    default属性，用来指定使用哪个environment标签-->
    <environments default="mqsql">
        <!-- environment 标签用于配置数据库连接信息
        type="JDBC"     可以进行事务的提交和回滚操作
        type="MANAGED"  依赖容器完成事务管理，本身不进行事务的提交和回滚操作
        -->
        <environment id="mqsql">
<!--            transactionManager标签用于配置数据库管理方式-->
            <transactionManager type="JDBC"></transactionManager>
<!--            dataSource标签就是用来配置数据库连接信息 POOLED|UNPOOLE -->
            <dataSource type="POOLED">
                <property name="driver" value="${mysql_driver}"/>
                <property name="url" value="${mysql_url}"/>
                <property name="username" value="${mysql_username}"/>
                <property name="password" value="${mysql_password}"/>
            </dataSource>
        </environment>
    </environments>
```

### 8.6 mappers标签

> 加载映射配置（映射文件、Dao注解）

```
    <!-- mappers标签用于载入映射文件 -->
    <mappers>
        <mapper resource="mappers/StudentMapper.xml"></mapper>
    </mappers>
```

## 九、映射文件

### 9.1 MyBatis初始化

![1650703006014](.\img\1650703006014.png)

### 9.2 mapper根标签

> mapper文件相当于Dao接口的"实现类"，namespace属性要指定`实现`Dao接口的权限定名(包名+文件名)

### 9.3 insert标签

> 声明添加操作（sql:insert...）
>
> ``常用属性`` 
>
> id属性，绑定对应Dao接口中的方法
>
> parameterType属性，用于指定接口中对应方法的参数类型（可省略）
>
> useGeneratedKeys属性，设置添加操作是否需要回填生成的主键
>
> keyProperty属性，指定回填的id设置到参数对象中的哪个属性
>
> timeout属性，设置此操作的超时时间，如果不设置则一直等待

``主键回填``两种方式

````java
    <insert id="insertStudent" useGeneratedKeys="true" keyProperty="stuId">
        insert into tb_students(stu_num,stu_name,stu_gender,stu_age)
        values(#{stuNum},#{stuName},#{stuGender},#{stuAge})
    </insert>
````

````
    <insert id="insertStudent">
        <selectKey keyProperty="stuId" resultType="java.lang.Integer">
            select Last_insert_id()
        </selectKey>
        insert into tb_students(stu_num,stu_name,stu_gender,stu_age)
        values(#{stuNum},#{stuName},#{stuGender},#{stuAge})java
    </insert>
````

### 9.4 delete标签

> 声明删除操作（属性与insert标签类似，少了关于主键回填的属性）

### 9.5 update标签

> 声明修改操作（属性与insert标签类型，少了关于主键回填的属性）

### 9.6 select标签

> 声明查询操作
>
> - id属性，指定绑定方法的方法名
> - parameterType属性，设置参数类型（一般省略）
> - resultType属性，指定当前sql返回数据封装的对象类型（实体类）
> - resultMap属性，指定从数据表到实体类字段和属性的对应关系
> - useCache属性，指定此查询操作是否需要缓存
> - timeOut属性，设置超时时间

### 9.7 resultMap

````java
    <!-- resultMap标签用于定义实体类与数据表的映射关系（ORM） -->
    <resultMap id="studentMap" type="Student">
        <id column="sid" property="stuId"/>
        <result column="stu_num" property="stuNum"/>
        <result column="stu_name" property="stuName"/>
        <result column="stu_gender" property="stuGender"/>
        <result column="stu_age" property="stuAge"/>
    </resultMap>
````

### 9.8 cache标签

> 设置当前Dao进行数据库操作时的缓存属性

````
<cache type="" size="" readOnly="false"/>
````

### 9.9 sql和include

> SQL片段

````java
<sql id="wanglaoji">sid,stu_num,stu_name,stu_gender,stu_age</sql>

<select id="listStudents" resultMap="studentMap">
    select <include refid="wanglaoji"></include> from tb_students
</select>
````

## 十、分页插件

> 分页插件是一个独立于MyBatis框架之外的第三方插件

### 10.1 添加分页插件的依赖

>PageHelper

````
        <!-- pagegelper分页插件 -->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.1.10</version>
        </dependency>
````

### 10.2 配置插件

> 在mybatis的主配置文件中`mybatis-config.xml`通过plugins标签进行配置

````
    <!-- plugins标签：主要用于配置MyBatis插件（例如分页插件） -->
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
    </plugins>
````

### 10.3 分页实例

> 对学生信息进行分页查询

````java
	@Test
    public void testQueryStudentByPage(){
        StudentDao studentDao = MyBatisUtil.getMapper(StudentDao.class);
        PageHelper.startPage(1,4);
        List<Student> students = studentDao.listStudents();
        //pageInfo中包含了数据分页信息
        PageInfo<Student> pageInfo = new PageInfo<Student>(students);

        List<Student> list = pageInfo.getList();
        for (Student stu : list) {
            System.out.println(stu);
        }
    }
````

``带条件分页``

````java
    @Test
    public void testQueryStudentByPage(){
        StudentDao studentDao = MyBatisUtil.getMapper(StudentDao.class);//sqlSession
        PageHelper.startPage(1,4);
        List<Student> students = studentDao.listStudentsByGender("女");
        //pageInfo中包含了数据分页信息
        PageInfo<Student> pageInfo = new PageInfo<Student>(students);
    }
````

## 十一、关联映射

### 11.1 实体关系

> 实体——数据实体，实体关系指的就是数据与数据之间的关系
>
> 例如：用户和角色、房屋和楼栋、订单和商品

实体关系分为以下四种：

#### 一对一关联

实例：人和身份证、学生和学生证、用户基本信息和详情

数据表关系：

- 主键关联（用户表主键和详情主键相同时，表示是匹配的数据）

![1650765662328](.\img\1650765662328.png)

- 唯一外键关联

![1650765876404](.\img\1650765876404.png)

#### 一对多关联，多对一关联

实例：

- 一对多：班级和学生、类别和商品、楼栋和房屋
- 多对一：学生和班级、商品和类别

数据表关系：

- 在多的一段添加外键和一的一段进行关联

#### 多对多关联

实例：用户和角色、角色和权限、房屋和业主、学生和社团、订单和商品

数据表关系：建立第三章关系表添加两个外键分别与两张表主键进行关联

用户(user_id)			用户角色表(uid,rid)			角色(role_id)



### 11.2 创建项目，部署框架MyBatis框架

创建web项目（基于Maven）

- 在IDEA创建Maven项目
- 在pom.xml中指定web项目的打包方式

```xml
<packaging>war</packaging>
```

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

  ```java
  file-->Project Structure...-->左侧Modules-->右侧+号-->Library...-->Tomcat 8.5.41-->Add Selected-->Apply-->OK
  ```

  - 第二种通过pom.xml配置

  ```xml
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
  ```

- 点击IDEA右上角添加tamcat

- 部署MyBatis框架

  - 添加依赖

  ````
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.4.6</version>
  </dependency>
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.47</version>
  </dependency>
  ````

  - 配置文件
  - 帮助类

  ````
  public class MyBatisUtil {
  
      private static SqlSessionFactory factory;
      private static final ThreadLocal<SqlSession> Local = new ThreadLocal<SqlSession>();
  
      static {
          try {
              InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
              factory = new SqlSessionFactoryBuilder().build(is);
          } catch (IOException e) {
              e.printStackTrace();
          }
      }
  
      public static SqlSessionFactory getFactory() {
          return factory;
      }
  
      private static SqlSession getSqlSession(boolean isAutoCommit){
          SqlSession sqlSession = Local.get();
          if (sqlSession == null){
              sqlSession = factory.openSession(isAutoCommit);
              Local.set(sqlSession);
          }
          return sqlSession;
      }
  
      public static SqlSession getSqlSession(){
          return getSqlSession(false);
      }
  
      public static <T extends Object>T getMapper(Class<T> c){
          SqlSession sqlSession = getSqlSession(true);
          return sqlSession.getMapper(c);
      }
  }
  ````

### 11.3 一对一关联

> 实例：用户——详情

#### 11.3.1 创建数据表

````sql
-- 用户信息表
create table users(
	user_id int primary key auto_increment,
	user_name varchar(20) not null unique,
    user_pwd varchar(20) not null,
    user_realname varchar(20) not null,
    user_img varchar(100) not null
);

-- 用户详情表
create table details(
	detail_id int primary key auto_increment,
    user_addr varchar(50) not null,
    user_tel char(11) not null,
    user_desc varchar(200),
    uid int not null unique
);
````

#### 11.3.2 创建实体类	

- User

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

- Detail

````java
@Data
@AllArgsConstructor
@NoArgsConstructor
@ToString
public class Detail {
    private int detailId;
    private String userAddr;
    private String userTel;
    private String userDesc;
    private int userId;
}
````

#### 11.3.3 添加操作（事务）

| 测试代码                                  |
| ----------------------------------------- |
| ![1650790631123](.\img\1650790631123.png) |

#### 11.3.4 一对一关联查询

> 在查询用户的同时关联查询出与之对应的详情

**实体**

| User                                      | Detail                                    |
| ----------------------------------------- | ----------------------------------------- |
| ![1650787290184](.\img\1650787290184.png) | ![1650787233415](.\img\1650787233415.png) |

**映射文件**

- 连接查询

| 连接查询                                  |
| ----------------------------------------- |
| ![1650788947589](.\img\1650788947589.png) |

| 子查询                                    |
| ----------------------------------------- |
| DetailMapper.xml                          |
| ![1650790013202](.\img\1650790013202.png) |
| UserMapper.xml                            |
| ![1650790094655](.\img\1650790094655.png) |

### 11.4 一对多关联

> 案例：班级(1)-学生(n)

#### 11.4.1 创建数据表

````sql
-- 创建班级信息表
create table classes(
	cid int primary key auto_increment,
	cname varchar(38) not null unique,
	cdesc varchar(100)
);

-- 创建学生表
create table students(
	sid char primary key,
	sname varchar(20) not null,
	sage int not null,
	scid int not null 
);
````

#### 11.4.2 创建实体类

| Clazz                                     | Student                                   |
| ----------------------------------------- | ----------------------------------------- |
| ![1650877493048](.\img\1650877493048.png) | ![1650877232734](.\img\1650877232734.png) |

#### 11.4.3 关联查询

> 当查询一个班级的时候，要关联查询出这个班级下的所有学生

**连接查询**

| 连接查询的映射配置                        |
| ----------------------------------------- |
| ![1650880047239](.\img\1650880047239.png) |

**子查询**

| 子查询映射配置                            |
| ----------------------------------------- |
| ![1650884961481](.\img\1650884961481.png) |
| ![1650885002873](.\img\1650885002873.png) |

### 11.5 多对一关联

> 实例：学生(n)——班级(1)
>
> 当查询一个学生的时候，关联查询这个学生所在的班级信息

#### 11.5.1 创建实体类

| Student                                   | Clazz                                     |
| ----------------------------------------- | ----------------------------------------- |
| ![1650885454034](.\img\1650885454034.png) | ![1650885538920](.\img\1650885538920.png) |

#### 11.5.2 关联查询

**连接查询**

| 连接查询映射配置                          |
| ----------------------------------------- |
| ![1650886503759](.\img\1650886503759.png) |

**子查询**

| 子查询映射配置                            |
| ----------------------------------------- |
| ![1650887157543](.\img\1650887157543.png) |
| ![1650887223986](.\img\1650887223986.png) |

### 11.6 多对多关联 

> 案例：学生(n)——课程(n)

#### 11.6.1 创建数据表

````sql
-- 学生信息表如上
-- 课程信息表
create table courses(
	course_id int primary key auto_increment,
    course_name varchar(50) not null
);
-- 选课信息表/成绩表(学号，课程号，成绩)
create table grades(
	sid char(5) not null,
    cid int not null,
    score int not null
);
````

#### 11.6.2 关联查询

> 查询学生时，同时查询出学生选择的课程（不使用此案例）

| Student                                   | Course                                    |
| ----------------------------------------- | ----------------------------------------- |
| ![1650974620268](.\img\1650974620268.png) | ![1650974652047](.\img\1650974652047.png) |

> 根据课程编号，查询课程时，同时查询选择了这门课程的学生（以此为案例）

| Student                                   | Course                                    |
| ----------------------------------------- | ----------------------------------------- |
| ![1650974699541](.\img\1650974699541.png) | ![1650974741893](.\img\1650974741893.png) |

| 连接查询映射配置                          |
| ----------------------------------------- |
| ![1650976115442](.\img\1650976115442.png) |

| 子查询映射配置                            |
| ----------------------------------------- |
| ![1650977024960](.\img\1650977024960.png) |
| ![1650977119279](.\img\1650977119279.png) |

## 十二、动态SQL

> 交友网：珍爱网、百合网		筛选心仪对象	性别	年龄	城市
>
> 电商：淘宝、京东		筛选商品	羽毛球拍	品牌	价格

> 用户的筛选条件不同，我们完成筛选执行的SQL也不一样，我们可以通过穷举来一一的完成不同条件的筛选，但是这种实现思路过于繁琐和复杂，MyBatis就提供了动态SQL的配置方式来实现多条件查询。

### 12.1 什么是动态SQL？

> 根据查询条件动态完成SQL的拼接

### 12.2 动态SQL使用案例

> 案例：心仪对象搜索

创建新项目mybatis-demo3。将mybatis-demo2拷贝过来，方便一点。

方法：将mybatis-demo2复制粘贴一份，然后进入将除了src和pom.xml其他文件都删掉。进入ppom.xml，将artifactId标签内的名字改成mybatis-demo3。然后打开项目，将dao层，pojo层，mapper文件，test下的测试类都删掉，主配置文件内的别名和mapper也删掉，至此完毕。

#### 12.2.1 创建数据表

````sql
create table members(
	member_id int primary key auto_increment,
    member_nick varchar(20) not null unique,
    member_gender char(2) not null,
    member_age int not null,
    member_city varchar(30) not null
);
````

#### 12.2.2 创建实体类

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@ToString
public class Member {
    private int memberId;
    private String memberNick;
    private String memberGender;
    private int memberAge;
    private String memberCity;
}
```

````java
@Data
public class MemberSearchCondition {
    private String gender;
    private Integer minAge;
    private Integer maxAge;
    private String city;
}
````

#### 12.2.3 创建Dao接口

> 在Dao接口中定义一个多条件查询的方法

| MemberDao                                 |
| ----------------------------------------- |
| ![1651057955930](.\img\1651057955930.png) |

#### 12.2.4 if标签

| MemberMapper.xml                          |
| ----------------------------------------- |
| ![1651058118427](.\img\1651058118427.png) |

#### 12.2.5 where标签

| MemberMapper.xml                          |
| ----------------------------------------- |
| ![1651058196797](.\img\1651058196797.png) |

#### 12.2.6 trim标签

> 使用trim标签有四个参数
>
> prefix：添加前缀
>
> prefixOverrides：如果有该前缀则去除
>
> suffix：添加后缀
>
> suffixOverrides：如果有该后缀则去除

| MemberMapper.xml                          |
| ----------------------------------------- |
| ![1651058713613](.\img\1651058713613.png) |

#### 12.2.7 foreach标签

>使用foreach标签有五个参数
>
>collection：循环遍历的类型，可填两个值（collection  |  list）
>
>item：每次遍历到的值存放的变量
>
>separator：每次遍历到的值之间的分隔符
>
>open：最开始的符号
>
>close：结尾的符号

**Dao层添加一个方法**

````java
public List<Member> searchMemberByCity(List<String> cities);
````

| MemberMapper.xml                          |
| ----------------------------------------- |
| ![1651060279244](.\img\1651060279244.png) |

#### 12.2.8 测试案例

````java
public class MemberDaoTest {

    @Test
    public void searchMember() {
        HashMap<String ,Object> params = new HashMap<String, Object>();
        params.put("gender","女");
        params.put("minAge",18);
        params.put("maxAge",23);
        params.put("city","武汉");

        //=====================================================
        MemberSearchCondition params2 = new MemberSearchCondition();
        params2.setGender("女");
//        params2.setMinAge(21);
//        params2.setMaxAge(30);
        //params2.setCity("武汉");

        //=======================================================

        MemberDao memberDao = MyBatisUtil.getMapper(MemberDao.class);
        List<Member> member = memberDao.searchMember(params2);

        for (Member m : member) {
            System.out.println(m);
        }
    }

    @Test
    public void searchMemberByCity(){
        List<String> cities = new ArrayList<String>();
        cities.add("武汉");
        cities.add("宜昌");
        MemberDao memberDao = MyBatisUtil.getMapper(MemberDao.class);
        List<Member> members = memberDao.searchMemberByCity(cities);

        for (Member m : members) {
            System.out.println(m);
        }
    }
}
````

## 十三、模糊查询

> 案例：根据昵称查询会员信息（模糊匹配 like）

### 13.1 模糊查询实现

> 模糊查询需要使用${}取值，与我们的sql进行拼接

#### 13.1.1 Dao

````java
public interface MemberDao {
    //根据昵称查询用户信息——模糊查询
    //在使用${}符时，即使只有一个参数也需要使用@Param注解声明参数的key
    //非String对象参数不用声明
    public List<Member> searchMemberByNick(HashMap params);
    public List<Member> searchMemberByNick(@Param("keyWord") String keyWord);
}
````

#### 13.1.2 映射文件

````java
<!-- 如果参数是String类型，需要parameterType声明参数类型 -->
<select id="searchMemberByNick" resultMap="memberMap" parameterType="java.lang.String">
	select member_id,member_nick,member_gender,member_age,member_city
	from members
	where member_nick like '%${keyWord}%'
</select>
````

#### 13.1.3 测试

````java
    @Test
    public void testsearchMemberByNick(){
        MemberDao memberDao = MyBatisUtil.getMapper(MemberDao.class);

        //调用参数类型为Map的
        //HashMap<String, Object> parsms = new HashMap<String, Object>();
        //parsms.put("keyWord","小");
        //List<Member> members = memberDao.searchMemberByNick(parsms);
        
        //调用参数类型为String的
        List<Member> members = memberDao.searchMemberByNick("花");
        for (Member m : members) {
            System.out.println(m);
        }
    }
````

### 13.2 #{}和${}的区别

- ${key} 表示获取参数，先获取参数的值拼接到SQL语句中，再编译执行SQL语句，可能引起SQL注入问题
- #{key} 表示获取参数，先完成SQL语句的编译(预编译)，预编译之后再将获取的参数设置到SQL语句中，可以避免SQL注入问题
- SQL注入问题：参数的值导致sql语句的原意发生变化

## 十四、MyBatis日志配置

> MyBatis做为一个封装好的ORM框架，其运行过程我们没办法跟踪，为了让开发者了解MyBatis执行流程及每一个执行步骤所完成的工作，MyBatis框架本身支持log4j日志框架，对运行的过程进行跟踪记录。我们只需对MyBatis进行相关的日志配置，就可以看到MyBatis运行过程中的日志信息。

### 14.1 添加日志框架依赖

````java
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
````

### 14.2 添加日志配置文件

- 在resources目录下创建名为`log4j.properties`文件
- 在`log4j.properties`文件配置日志输出的方式
  - 添加模板，名为log4j，后缀为properties，复制以下模板至框内，记得勾选框下Enable Live Templates选项，最后Apply

````proper
# 声明日志的输出级别及输出方式
log4j.rootLogger=DEBUG,stdout
# Mybatis Logging configuration...
log4j.logger.org.mybatis.example.BlogMapper=TRACE
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
# 定义日志的打印格式  %t 表示线程名称  %5p 日志级别  %msg 日志信息  \:%m%n换行
log4j.appender.stdout.layout.ConversionPattern=[%t] %5p - %msg \:%m%n
````

### 14.3 日志信息的级别

> 在使用日志框架输出日志信息的时候，会根据输出的日志信息的重要程度分为5个级别

| 级别  | 说明         |
| ----- | ------------ |
| DEBUG | 输出调试信息 |
| INFO  | 输出提示信息 |
| WARN  | 输出警告信息 |
| ERROR | 一般性错误   |
| FATAL | 致命性错误   |

## 十五、配置数据库连接池-整合Druid

> MtBatis做为一个ORM框架，在进行数据库操作时是需要和数据库建立连接的，MyBatis支持基于数据库连接池的连接创建方式
>
> 当我们配置MyBatis数据源时，只要配置了dataSource标签的type属性值为POOLED时，就可以使用MyBatis内置的连接池管理连接。
>
> 如果我们想要使用第三方的数据库连接池，则需进行自定义配置

### 15.1 常见的连接池

- DBCP	麻烦
- C3P0     效率低
- Druid     性能也比较好，提供了比较便捷的监控系统
- Hikari     性能最好

| 功能            | dbcp                | druid              | c3p0                               | HikariCP                           |
| --------------- | ------------------- | ------------------ | ---------------------------------- | ---------------------------------- |
| 是否支持PSCache | 是                  | 是                 | 是                                 | 否                                 |
| 监控            | jmx                 | jmx/log/http       | jmx,log                            | jmx                                |
| 扩展性          | 弱                  | 好                 | 弱                                 | 弱                                 |
| sql拦截及解析   | 无                  | 支持               | 无                                 | 无                                 |
| 代码            | 简单                | 中等               | 复杂                               | 简单                               |
| 更新时间        | 2015.8.6            | 2015.10.10         | 2015.12.09                         | 2015.12.3                          |
| 特点            | 依赖于common-pool   | 阿里开源，功能全面 | 历史久远，代码逻辑复杂，且不易维护 | 优化力度大，功能简单，起源于boneCP |
| 连接池管理      | LinkedBlockingDeque | 数组               |                                    | threadlocal+CopyOnWriteArrayList   |

### 15.2 添加Druid依赖

````java
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.5</version>
</dependency>
````

### 15.3 创建Druid连接池工厂

- 在utils包下创建一个继承Pooled工厂的类

````java
public class DruidDataSourceFactory extends PooledDataSourceFactory {

    //将Mybatis的内置连接池Pooled的数据源修改成Druid的数据源
    public DruidDataSourceFactory() {
        this.dataSource = new DruidDataSource();
    }

}
````

### 15.4 将DruidDataSourceFactory配置给MyBatis数据源

> MyBatis需要的是一个PooledDataSourceFactory工厂，使用多态，创建一个Druid工厂继承Pooled工厂，并将数据源修改成Druid的，这样就将连接池修改成Druid的了。type里给的是自定义Druid工厂类的地址。
>
> Druid的驱动名和url名与Pooled的不同（driver	-->	driverClass）（url	-->	jdbcUrl）

````java
<environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"></transactionManager>

            <!-- POOLED 使用MyBatis内置的连接池实现 -->
    <!-- MyBatis需要一个连接池工厂，这个工厂可以产生数据库连接池 PooledDataSourceFactory -->
    <dataSource type="com.qfedu.utils.DruidDataSourceFactory">
                <property name="driverClass" value="${driver}"/>
                <property name="jdbcUrl" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
````

## 十六、Mybatis缓存

> MyBatis是基于JDBC的封装，使数据库操作更加便捷；MyBatis除了对JDBC操作步骤封装之外也对其性能进行了优化；
>
> - 在MyBatis引入缓存机制，用于提升MyBatis的检索效率
> - 在MyBatis引入延迟加载机制，用于减少对数据库不必要的访问

### 16.1 缓存的工作原理

> 缓存，就是存储数据的内存

![1651136746558](.\img\1651136746558.png)

### 16.2 MyBatis缓存

>MyBatis缓存分为一级缓存和二级缓存

#### 16.2.1 一级缓存

> 一级缓存也叫做SqlSession级缓存，为每个SqlSession单独分配的缓存内存，无需手动开启可直接使用，多个SqlSession的缓存是不共享的。
>
> 特性：
>
> 1. 如果多次查询使用的是同一个SqlSession对象，则第一次查询之后数据会存放到缓存，后续的查询则直接访问缓存中存储的数据。
> 2. 如果第一次查询完成之后，对查询出的对象进行修改（此修改会影响到缓存），第二次查询会直接访问缓存，造成第二次查询的结果与数据库不一致。
> 3. 当我们进行在查询时想要跳过缓存直接查询数据库，则可以通过sqlSession1.clearCache();来清除当前SqlSession的缓存。
> 4. 如果第一次查询之后第二次查询之前，使用当前的SqlSession执行了修改操作，此修改操作会使第一次查询并缓存的数据失效，因此第二次查询会再次访问数据库。

测试代码：

````java
@Test
public void testQueryMemberById(){
    SqlSession sqlSession1 = MyBatisUtil.getFactory().openSession();
    SqlSession sqlSession2 = MyBatisUtil.getFactory().openSession();

    //        SqlSession sqlSession1 = MyBatisUtil.getSqlSession();
    //        SqlSession sqlSession2 = MyBatisUtil.getSqlSession();

    MemberDao memberDao = sqlSession1.getMapper(MemberDao.class);
    Member member1 = memberDao.queryMemberById(1);
    System.out.println(member1);

    member1.setMemberAge(99);
    sqlSession1.clearCache();

    System.out.println("===================================================");

    MemberDao memberDao2 = sqlSession1.getMapper(MemberDao.class);
    Member member2 = memberDao2.queryMemberById(1);
    System.out.println(member2);
}
````

#### 16.2.2 两次查询与数据库数据不一致问题

![1651279859965](.\img\1651279859965.png)

#### 16.2.3 二级缓存

> 二级缓存也称为SqlSessionFactory级缓存，通过同一个factory对象获取的SqlSession可以共享二级缓存；在应用服务器中SqlSessionFactory是单例的，因此我们二级缓存可以实现全局共享。
>
> 特性：
>
> 1. 二级缓存默认没有开启，需要在mybatis-config.xml中的setting标签开启。
> 2. 二级缓存只能缓存实现序列化接口的对象。

- 在mybatis-config.xml开启使用二级缓存

````java
<settings>
    <!-- 启动二级缓存 -->
    <setting name="cacheEnabled" value="true"/>
</settings>
````

- 在需要使用二级缓存的Mapper文件中配置cache标签使用二级缓存

````java
<cache/>
````

- 被缓存的实体类实现序列化接口

````java
@Data
@NoArgsConstructor
@AllArgsConstructor
@ToString
public class Member implements Serializable {
    private int memberId;
    private String memberNick;
    private String memberGender;
    private int memberAge;
    private String memberCity;
}
````

- 测试（必须手动commit才会缓存到二级缓存）

````java
@Test
public void testQueryMemberById(){
    SqlSessionFactory factory = MyBatisUtil.getFactory();
    //多个SqlSession对象必须来自于同一个SqlSessionFactory
    SqlSession sqlSession1 = factory.openSession(true);
    SqlSession sqlSession2 = factory.openSession(true);

    MemberDao memberDao = sqlSession1.getMapper(MemberDao.class);
    Member member1 = memberDao.queryMemberById(1);
    System.out.println(member1);
    sqlSession1.commit();   //第一次查询之后执行commit，会将当前sqlSession1的查询结果缓存到二级缓存

    System.out.println("===================================================");

    MemberDao memberDao2 = sqlSession2.getMapper(MemberDao.class);
    Member member2 = memberDao2.queryMemberById(1);
    System.out.println(member2);
}
````

### 16.3 查询操作的缓存开关

>select标签中的useCache属性
>
>true：执行此查询先从二级缓存中查找
>
>false：执行此查询直接从数据库中查找

````java
<select id="queryMemberById" resultMap="memberMap" useCache="false">
        select member_id,member_nick,member_gender,member_age,member_city
        from members
        where member_id = #{mid}
</select>
````

## 十七、延迟加载

> 延迟加载——如果在MyBatis开启了延迟加载，在执行了子查询（至少查询两次及以上）时，默认只执行第一次查询，当用到子查询的结果时，才会触发子查询的执行，如果无需使用子查询结果，则子查询不会执行。

- 延迟加载默认关闭
- 延迟加载只对子查询有效

### 17.1 在mybatis-config.xml中设置延迟加载

````java
    <settings>
        <setting name="lazyLoadingEnabled" value="true"/>
    </settings>
````

### 17.2 在mappers中设置延迟加载

![1651286774117](.\img\1651286774117.png)