# SpringBoot简介

## 前景介绍

1. 为什么要使用SpringBoot

   因为Spring，SpringMVC需要使用大量的配置文件（xml文件）

   还需要配置各种对象，把使用的对象放到Spring容器中才能使用对象

   需要了解其他框架配置规则

2. SpringBoot就相当于不需要配置文件的Spring+SpringMVC。常用的框架和第三方库都已经配置好了，拿来就可以用

3. SpringBoot 开发效率高，使用方便

# 第一章	回顾 Spring

## 1.1 javaConfig

定义：使用java类作为xml配置文件的代替，是配置Spring容器的纯java方式，在这个java类中可以创建java对象，把对象放入spring容器中（注入到容器）

使用两个注解：

​	1）@Configuration ： 放在一个类的上面，表示这个类是作为配置文件使用的。

​	2）@Bean：声明对象，把对象注入到容器中。

## 1.2 回顾Spring使用Xml文件配置

1. 定义Studen 实体类

   ~~~java
   package com.fengshun.pojo;
   
   public class Student {
       private String name;
       private String age;
       private String sex;
   
       public Student() {
       }
   
       public Student(String name, String age, String sex) {
           this.name = name;
           this.age = age;
           this.sex = sex;
       }
   
       //getter and setter
       //toString
      
   }
   
   ~~~

2. 定义配置文件

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       <bean id="student" class="com.fengshun.pojo.Student">
           <property name="name" value="张三"/>
           <property name="age" value="18"/>
           <property name="sex" value="男"/>
       </bean>
   </beans>
   
   ~~~

3. 使用

   ~~~java
   @Test
       public void studentBeanTest(){
           ApplicationContext applicationContext = new ClassPathXmlApplicationContext("beans.xml");
           Student myStudent = applicationContext.getBean("myStudent", Student.class);
           System.out.println("容器中的对象："+myStudent);
       }
   ~~~

   

## 1.3 回顾Spring 使用全注解方式

1. 定义注解类

   ~~~java
   @Configuration
   public class SpringConfig {
   
       @Bean(name = "myStudent")
       public Student getStudent(){
           Student student = new Student();
           student.setName("李四");
           student.setAge("20");
           student.setSex("男");
           return student;
       }
   }
   
   ~~~

2. 测试

   ~~~java
       @Test
       public void configTest(){
           ApplicationContext applicationContext = new AnnotationConfigApplicationContext(SpringConfig.class);
           Student myStudent = applicationContext.getBean("myStudent", Student.class);
           System.out.println("容器中的对象："+myStudent);
       }
   ~~~

   

注意：@Bean 如果不指定name ，那么默认是方法的名称。

​			name 属性就类似于 <bean> 标签中的 id属性

## 1.4 @ImportResource

@ImportResource 是导入其他 xml 配置，等同于 xml 文件的 resources

~~~xml
<import resource=""/>
~~~

1. 导入单个配置文件

   ```java
   @Configuration
   @ImportResource(value = "classpath:MyBatis.xml")
   public class SpringConfig {
   }
   ```

2. 导入多个配置文件

   ~~~java
   
   @Configuration
   @ImportResource(value = {"classpath:MyBatis.xml","classpath:beans.xml"})
   public class SpringConfig {
   
   }
   ~~~

   

## 1.5 @PropertyResource

@PropertyResource 注解是读取 properties 属性配置文件。使用属性配置文件可以实现外部化配置

类似于：

~~~xml
<context:property-placeholder location="classpath:jdbc.properties"/>
~~~

使用：

~~~java
package com.fengshun.resources;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.*;
import com.alibaba.druid.pool.DruidDataSource;


import javax.sql.DataSource;

@Configuration
@ImportResource(value = {"classpath:MyBatis.xml","classpath:beans.xml"})
@PropertySource(value = "classpath:jdbc.properties")
public class SpringConfig {
    @Value("${jdbc.driver}")
    private String driverClassName;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;

   
    @Bean(name = "dataSource")
    public DataSource getDataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(driverClassName);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }
}

~~~

# 第二章	SpringBoot 入门

##  2.1 介绍

SpringBoot是Spring中的一个成员， 可以简化Spring，SpringMVC的使用。 他的核心还是IOC容器。

特点：

- Create stand-alone Spring applications

  创建spring应用

- Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)

  内嵌的tomcat， jetty ， Undertow 

- Provide opinionated 'starter' dependencies to simplify your build configuration

  提供了starter起步依赖，简化应用的配置。   

  比如使用MyBatis框架 ， 需要在Spring项目中，配置MyBatis的对象 SqlSessionFactory ， Dao的代理对象

  在SpringBoot项目中，在pom.xml里面, 加入一个 mybatis-spring-boot-starter依赖

- Automatically configure Spring and 3rd party libraries whenever possible

  尽可能去配置spring和第三方库。叫做自动配置（就是把spring中的，第三方库中的对象都创建好，放到容器中， 开发人员可以直接使用）

- Provide production-ready features such as metrics, health checks, and externalized configuration

  提供了健康检查， 统计，外部化配置

- Absolutely no code generation and no requirement for XML configuration

  不用生成代码， 不用使用xml，做配置

## 2.2 创建Spring Boot项目

### 2.2.1 第一种方式， 使用Spring提供的初始化器， 就是向导创建SpringBoot应用

使用的地址： https://start.spring.io

<img src="C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230926204142978.png" alt="image-20230926204142978" style="zoom:67%;" />

<img src="C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230926204204345.png" alt="image-20230926204204345" style="zoom:67%;" />

SpringBoot 目录结构：

![image-20230926204025315](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230926204025315.png)

pox.xml 文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!--SpringBoot 的父项目-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.1.4</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <!--当前项目的坐标-->
    <groupId>com.fengshun</groupId>
    <artifactId>boot002-firstProject</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>boot002-firstProject</name>
    <description>boot002-firstProject</description>
    <properties>
        <java.version>17</java.version>
    </properties>
    <dependencies>
        <!--web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--测试-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!--Maven的插件-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

自动导入的依赖包：![image-20230926204615078](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230926204615078.png)

### 2.2.1  使用国内的地址

地址：https://start.springboot.io

步骤和第一种方式完全一样，只是地址不同。

项目结构：![image-20230926205204133](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230926205204133.png)

## 2.3 第一个 SpringBoot 程序 

在Controller 包下新建一个类：UserController

```java
package com.fengshun.controller;

import com.fengshun.pojo.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class UserController {
    @RequestMapping("toUser")
    @ResponseBody
    public User toUser(){
        return new User("张三","0515");
    }

}
```

启动SpringBoot创建时自动生成的类：

```java
package com.fengshun;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class,args);
    }
}
```

日志信息：

![image-20230926211038955](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230926211038955.png)

直接访问 http://localhost:8080/toUSer 即可：

![image-20230926211218769](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230926211218769.png)

## 2.4 注解的使用

@SpringBootApplication 注解是复合注解，又以下注解组成：

<img src="C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230926211434949.png" alt="image-20230926211434949" style="zoom:80%;" />

其中，最主要的是以下三个注解：

```
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan
```



1. @SpringBootConfiguration 注解：

   > <img src="C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230926211717048.png" alt="image-20230926211717048" style="zoom:67%;" />
   >
   > 它被 @Configuration 注解给标注，说明，使用了 @SpringBootConfiguration 注解标注的类，可以作为配置文件使用，可以使用Bean 声明对象，注入到容器中。

2. @EnableAutoConfiguration 注解：

   > 启用自动配置，把java对象配置好，注入到Spring容器中。例如可以把MyBatis的对象创建好，放入到容器中

3. @ComponentScan 注解

   > 组件扫描器，找到注解，根据注解的功能创建对象
   >
   > 组件扫描器扫描的包：默认扫描主程序所在的包下的所在的包以及所有子包

## 2.5 SpringBoot的配置文件

配置文件名称：application

这个配置文件主要有两种格式：

```
1.properties 格式的文件（k:v）
2.yml 格式的文件（k:v）
```

配置SpringBoot：

```
#设置端口号
server.port=8080
#设置访问应用上下文路径， contextpath
server.servlet.context-path=/myboot
```

如果在项目中有两个配置文件（一个properties一个yml），那么SpringBoot 默认是使用 properties 的配置文件

## 2.6 多环境配置

有开发环境， 测试环境， 上线的环境。

每个环境有不同的配置信息， 例如端口， 上下文件， 数据库url，用户名，密码等等

使用多环境配置文件，可以方便的切换不同的配置。

**使用方式**： 创建多个配置文件， 名称规则： application-环境名称.properties(yml)

1. 新建一个开发环境的配置文件

   ```properties
   #开发环境
   server.port=8081
   server.servlet.context-path=/dev
   ```

2. 新建一个测试环境的配置文件

   ```properties
   #测试环境
   server.port=8082
   server.servlet.context-path=/test
   ```

3. 在默认的配置文件中进行配置

   ```properties
   #激活使用哪个配置文件
   #启动开发环境的配置文件
   spring.profiles.active=dev  
   ```

## 2.7 自定义配置

   ### 2.7.1 @Value 注解

1. 使用 @Value 注解，读取application.properties 配置文件中自定义的 key=value

   > 自定义配置
   >
   > ```properties
   > #自定义配置
   > school.name=张三
   > school.address=rs
   > ```
   >
   > 读取：
   >
   > ```java
   > package com.fengshun.controller;
   > 
   > import com.fengshun.pojo.School;
   > import org.springframework.beans.factory.annotation.Value;
   > import org.springframework.stereotype.Controller;
   > import org.springframework.web.bind.annotation.RequestMapping;
   > import org.springframework.web.bind.annotation.ResponseBody;
   > 
   > @Controller
   > public class SchoolController {
   >     @Value("${school.name}")
   >     public String name;
   > 
   > 
   >     @Value("${school.address}")
   >     public String address;
   > 
   >     @RequestMapping("toSchool")
   >     public @ResponseBody String toSchool(){
   >         return "name="+name+";address="+address;
   >     }
   > }
   > ```

2. 在自定义类中读取配置文件：

> 定义实体类：
>
> ```java
> package com.fengshun.pojo;
> 
> import org.springframework.beans.factory.annotation.Value;
> import org.springframework.boot.context.properties.ConfigurationProperties;
> import org.springframework.context.annotation.PropertySource;
> import org.springframework.stereotype.Component;
> 
> @Component
> @ConfigurationProperties(prefix="school")
> public class School {
>  @Value("${school.name}")
>  public String name;
> 
> 
>  @Value("${school.address}")
>  public String address;
> 
>  public School() {
>  }
> 
>  public School(String name, String address) {
>      this.name = name;
>      this.address = address;
>  }
> 
>  @Override
>  public String toString() {
>      return "school{" +
>              "name='" + name + '\'' +
>              ", address='" + address + '\'' +
>              '}';
>  }
> }
> ```
>
> 测试代码：
>
> ```java
> package com.fengshun.controller;
> 
> import com.fengshun.pojo.School;
> import jakarta.annotation.Resource;
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.beans.factory.annotation.Qualifier;
> import org.springframework.beans.factory.annotation.Value;
> import org.springframework.stereotype.Controller;
> import org.springframework.web.bind.annotation.RequestMapping;
> import org.springframework.web.bind.annotation.ResponseBody;
> 
> @Controller
> public class SchoolController {
>     @Resource(name = "school")
>     private School school;
> 
>     @RequestMapping("tos")
>     @ResponseBody
>     public School tos() {
>         return school;
>     }
> }
> ```

### 2.7.2 @ConfigurationProperties()

需要导入依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

@ConfigurationProperties: 把配置文件的数据映射为java对象。

属性：prefix 配置文件中的某些key的开头的内容。

## 2.8 在SpringBoot中使用jsp

SpringBoot不推荐使用jsp ，而是使用模板技术代替jsp

使用jsp需要配置：

1. 加入一个处理jsp的依赖。 负责编译jsp文件

~~~xml
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
</dependency>
~~~

如果需要使用servlet， jsp，jstl的功能需要添加以下依赖

```xml
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>jstl</artifactId>
</dependency>

<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>javax.servlet-api</artifactId>
</dependency>

<dependency>
<groupId>javax.servlet.jsp</groupId>
	<artifactId>javax.servlet.jsp-api</artifactId>
	<version>2.3.1</version>
</dependency>

```

2. 创建一个存放jsp的目录，一般叫做webapp

   <img src="C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230927133718617.png" alt="image-20230927133718617" style="zoom: 50%;" />

​    		新建index.jsp

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>使用jsp，输出数据===${name}</h1>
</body>
</html>
~~~



3. 需要在pom.xml指定jsp文件编译后的存放目录。

  		META-INF/resources

```xml
<resources>
    <resource>
        <!--jsp原来的目录-->
        <directory>src/main/webapp</directory>
        <!--指定编译后存放的目录-->
        <targetPath>META-INF/resources</targetPath>
        <!--指定处理的目录和文件-->
        <includes>
            <!--编译 webapp 目录下的所有子目录中的所有文件-->
            <!--将编译后的文件存放在 META-INF/resources 目录中-->
            <include>**/*.*</include>
        </includes>
    </resource>
</resources>
```

4. 创建Controller， 访问jsp

```java
package com.fengshun.controller;

import jakarta.servlet.http.HttpServletRequest;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class JspController {

    @RequestMapping("doJSP")
    public String doJSP(HttpServletRequest request){
        request.setAttribute("data","张三");
        return "index";
    }
    @RequestMapping("doJSP2")
    public String doJSP2(Model model){
        model.addAttribute("data","张三");
        return "index";
    }
}
```

5. 在application.propertis文件中配置视图解析器

```properties
#配置视图解析器
# / 相当于是 src/main/webapp
spring.mvc.view.prefix=/
spring.mvc.view.suffix=.jsp
```

## 2.9 手动获取容器中的对象

想要通过代码，从容器中获取对象。

通过 SpringApplication.run(Application.class,args); 返回值获取容器。

run方法：

```
public static ConfigurableApplicationContext run(Class<?> primarySource, String... args) {
    return run(new Class[]{primarySource}, args);
}
```

> run方法的返回值是： ConfigurableApplicationContext
>
> ConfigurableApplicationContext 是一个接口，它是 ApplicationContext 的子接口
>
> ```
> public interface ConfigurableApplicationContext extends ApplicationContext,
> ```
>
> 所以 ConfigurableApplicationContext  是一个容器。

1. 定义一个Service

   ```java
   package com.fengshun.service;
   
   import org.springframework.stereotype.Service;
   
   @Service("userServiceImpl")
   public class UserServiceImpl {
   
       public void sayHello(){
           System.out.println("=================================");
       }
   }
   ```

2. 在Application 类中获取

   ```java
   package com.fengshun;
   
   import com.fengshun.service.UserServiceImpl;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.context.ConfigurableApplicationContext;
   
   @SpringBootApplication
   public class Application {
       public static void main(String[] args) {
           // 获取容器对象
           ConfigurableApplicationContext run = SpringApplication.run(Application.class, args);
           // 从容器中获取对象
           UserServiceImpl userServiceImpl = run.getBean("userServiceImpl", UserServiceImpl.class);
           userServiceImpl.sayHello();
   
       }
   }
   ```

## 2.9 ComnandLineRunner 接口 ，  ApplcationRunner接口

这两个接口都 有一个run方法。 执行时间在容器对象创建好后， 自动执行run（）方法。

可以完成自定义的在容器对象创建好的一些操作。例如访问数据库等。

接口源码：

~~~java
@FunctionalInterface
public interface CommandLineRunner {
    void run(String... args) throws Exception;
}

@FunctionalInterface
public interface ApplicationRunner {
    void run(ApplicationArguments args) throws Exception;
}

~~~

运用：

```java
package com.fengshun;

import com.fengshun.service.UserServiceImpl;
import jakarta.annotation.Resource;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;

@SpringBootApplication
public class Application implements CommandLineRunner {

    @Resource(name = "userServiceImpl")
    private UserServiceImpl userService;

    public static void main(String[] args) {
        System.out.println("容器创建之前");
        // 获取容器对象
		SpringApplication.run(Application.class, args);
        System.out.println("创建容器之后");

    }

    @Override
    public void run(String... args) throws Exception {
        // 使用容器中的对象
        userService.sayHello();
        System.out.println("在容器对象创建好，执行的方法");
    }
}
```

运行结果：

<img src="C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230927170858148.png" alt="image-20230927170858148" style="zoom:50%;" />

# 第三章 Web组件

## 3.1 拦截器

### 3.1.1 回顾 SpringMVC 中的拦截器

实现步骤：

> 1. 创建类实现SpringMVC框架的HandlerInterceptor接口
>
>    ```java
>    package com.fengshun.Interceptor;
>    
>    import jakarta.servlet.http.HttpServletRequest;
>    import jakarta.servlet.http.HttpServletResponse;
>    import org.springframework.web.servlet.HandlerInterceptor;
>    
>    public class UserInterceptor implements HandlerInterceptor {
>        @Override
>        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
>            System.out.println("这是一个拦截器");
>            return true;
>        }
>    }
>    ```
>
> 2. 在SpringMVC 的配置文件中，声明拦截器
>
>    ~~~xml
>    <!--注册拦截器-->
>    <mvc:interceptors>
>        <mvc:interceptor>
>            <!--配置拦截的路径(哪些请求被拦截)-->
>            <mvc:mapping path="/**"/>
>            <!--设置放行的请求-->
>            <mvc:exclude-mapping path="/login"></mvc:exclude-mapping>
>            <mvc:exclude-mapping path="/showLogin"></mvc:exclude-mapping>
>            <!--设置进行功能处理的拦截器类-->
>            <bean class="com.fengshun.interceptor.UserInterceptor"></bean>
>        </mvc:interceptor>
>    </mvc:interceptors>
>    ~~~
>
>    

### 3.1.2 SpringBoot 中的拦截器

在SpringBoot中，依然需要自定义一个类，去实现 HandlerInterceptor 接口，只不过跟SpringMVC 不同的是，不再在xml配置文件中进行配置拦截器，而是 定义一个类，去实现 WebMvcConfigurer

在 WebMvcConfigurer 接口中，有很多针对SpringMVC处理的方法：

> ```java
> 
> public interface WebMvcConfigurer {
>    /*...*/
>     
>    //向容器中添加拦截器
>     default void addInterceptors(InterceptorRegistry registry) {
>     }
>     //处理静态资源
>     default void addResourceHandlers(ResourceHandlerRegistry registry) {
>     }
>     //处理视图管理
>     default void addViewControllers(ViewControllerRegistry registry) {
>     }
>     /*...*/
> 
>     @Nullable
>     default Validator getValidator() {
>         return null;
>     }
> 
>     @Nullable
>     default MessageCodesResolver getMessageCodesResolver() {
>         return null;
>     }
> }
> ```
>
> 

addInterceptors 方法的参数为 InterceptorRegistry ，由它指定拦截器

```java
//运用 InterceptorRegistry 的addInterceptor 方法，指定拦截器
public InterceptorRegistration addInterceptor(HandlerInterceptor interceptor) {}
```

addInterceptor() 方法的返回值类型为 ： InterceptorRegistration 类·，由它指定拦截的url以及放行的url

<img src="C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230927202701257.png" alt="image-20230927202701257" style="zoom: 67%;" />

定义 LoginInterceptor 拦截器类，需要实现 HandlerInterceptor 接口：

```java
public class UserInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("这是一个拦截器");
        return true;
    }
}
```

定义 Controller 类：

```java
@Controller
public class LoginController {

    @RequestMapping("toLogin")
    @ResponseBody
    public String toLogin(){
        return "拦截器";
    }
}
```

定义 MyAppConfig 类，实现WebMvcConfigurer接口，用来配置SpringMVC

```java
@Configuration
public class MyAppConfig implements WebMvcConfigurer {
    // 添加拦截器对象，注入到容器中
    @Override
    public void addInterceptors(InterceptorRegistry registry) {

        registry.addInterceptor(new LoginInterceptor()).addPathPatterns("/**").excludePathPatterns("/user");
    }
}
```

注意：配置类，需要加上 @Configuration 注解

## 3.2 Servlet

在SpringBoot框架中使用 Servlet 对象：

使用步骤：

> 1. 创建Servlet类，创建类继承 HttpServlet
>
>    ```java
>    public class MyServlet extends HttpServlet {
>        @Override
>        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
>            doPost(req, resp);
>        }
>    
>        @Override
>        protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
>           // 使用HttpServletResponse 输出数据，应答结果
>            response.setContentType("text/html;charset=utf-8");
>            PrintWriter writer = response.getWriter();
>            writer.println("执行的是Servlet");
>            writer.flush();
>            writer.close();
>        }
>    }
>    ```
>
> 2. 注册Servlet，让框架能找到Servlet
>
>    定义配置类：
>
>    ```java
>    @Configuration
>    public class WebApplicationConfig {
>        // 定义方法，注册Servlet对象
>        @Bean
>        public ServletRegistrationBean servletRegistrationBean(){
>            // 创建 ServletRegistrationBean 对象
>            /**  public ServletRegistrationBean(T servlet, String... urlMappings)
>             *  第一个参数是：Servlet 对象
>             *  第二个参数是url地址
>             *  */
>            ServletRegistrationBean bean = new ServletRegistrationBean(new MyServlet(),"/MyServlet");
>            return bean;
>        }
>    }
>    ```
>
>    注意：
>
>    1. 配置类需要 @Configuration 注解标注，方法需要使用 @Bean 注解标注，方法的返回值类型ServletRegistrationBean。
>    2. ServletRegistrationBean 类的构造方法：
>
>    运用 ServletRegistrationBean 的其他构造方法
>
>    ```java
>        @Bean
>        public ServletRegistrationBean servletRegistrationBean1(){
>            ServletRegistrationBean bean = new ServletRegistrationBean();
>            bean.setServlet(new MyServlet());
>            bean.addUrlMappings("/MyServlet","/User");
>            return bean;
>        }
>    ```

## 3.3 过滤器 Filter

SpringBoot中的过滤器和Servlet一样，都需要定义配置类，用来注册Filter对象。

Filter 是Servlet 规范中的过滤器，可以处理请求，对请求的参数，属性进行调整。常常在过滤器中处理字符编码。

实现步骤：

1. 创建Filter对象

   定义 AppFilter 类，实现  jakarta.Servlet.Filter 接口，并重写

   ```java
   package com.fengshun.filter;
   
   import jakarta.servlet.*;
   
   import java.io.IOException;
   
   public class AppFilter implements Filter {
       @Override
       public void init(FilterConfig filterConfig) throws ServletException {
           Filter.super.init(filterConfig);
       }
   
       @Override
       public void destroy() {
           Filter.super.destroy();
       }
   
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           System.out.println("这是过滤器");
           filterChain.doFilter(servletRequest,servletResponse);
       }
   }
   ```

2. 注册Filter

   定义配置类，并定义方法，方法的返回值类型为 FilterRegistrationBean。方法需要 @Bean 注解标注，类需要 @Configuration 注解标注

   ```java
   package com.fengshun.config;
   
   import com.fengshun.filter.AppFilter;
   import org.springframework.boot.web.servlet.FilterRegistrationBean;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   @Configuration
   public class AppFilterConfig {
       @Bean
       public FilterRegistrationBean filterRegistrationBean(){
           FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean();
           // 指定过滤器类
           filterRegistrationBean.setFilter(new AppFilter());
           // 指定过滤的url
           filterRegistrationBean.addUrlPatterns("/*");
           return filterRegistrationBean;
       }
   }
   ```

注意：拦截器的url 是 /**   过滤器是 /*

## 3.4 字符集过滤器的应用

CharacterEncodingFilter : 解决post请求中乱码的问题

在SpringMVC框架， 在web.xml 注册过滤器。 配置他的属性。 

<img src="C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230927221416545.png" alt="image-20230927221416545" style="zoom:67%;" />

注册字符集过滤器：

跟过滤器的类似，需要在配置类中定义方法，方法的返回值类型依旧是 FilterRegistrationBean，

不再需要我们自定义过滤器类，而是直接在方法中直接实例化 CharacterEncodingFilter  类。

实现步骤：

1. 配置字符集过滤器

   ~~~java
    // 字符集过滤器
       @Bean
       public FilterRegistrationBean characterEncodingFilter(){
           // 使用框架中的过滤器类
           FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean();
           //使用 CharacterEncodingFilter 类指定字符集
           CharacterEncodingFilter characterEncodingFilter = new CharacterEncodingFilter();
           // 指定使用字符集编码
           characterEncodingFilter.setEncoding("UTF-8");
           // 指定 request，response 都使用 encoding 的值
           characterEncodingFilter.setForceEncoding(true);
           /*
           // 分别指定 request，response 使用 encoding 的值
           characterEncodingFilter.setForceRequestEncoding(true);
           characterEncodingFilter.setForceResponseEncoding(true);
           */
           // 指定字符集过滤器
           filterRegistrationBean.setFilter(characterEncodingFilter);
           // 指定过滤的url
           filterRegistrationBean.addUrlPatterns("/*");
           return filterRegistrationBean;
       }
   ~~~

2. 修改 application.properties 文件，让自定义的过滤器起作用

   在SpringBoot 中默认已经配置了 CharacterEncodingFilter 。 编码默认为 ： ISO-8859-1

   ```properties
   # enable=false 作用是关闭系统中配置好的过滤器，使用自定义的CharacterEncodingFilter
   # 默认值为true 表示默认使用系统中配置好的过滤器
   server.servlet.encoding.enabled=false
   ```

## 3.5 通过配置文件指定字符集

```properties
#设置使用默认字符集过滤器
server.servlet.encoding.enabled=ture
#指定使用的编码方式
server.servlet.encoding.charset=UTF-8
#指定 request，response 都使用 encoding 的值
server.servlet.encoding.force=true
#分别指定 request，response 使用 encoding 的值
#server.servlet.encoding.force-request=true
#server.servlet.encoding.force-response=true
```

# 第四章 ORM 操作 MySQL

使用MyBatis框架操作数据，  在SpringBoot框架集成MyBatis

使用步骤：

1. mybatis起步依赖：完成mybatis对象自动配置，对象放在容器中

   > 注意：在新建模块时需要添加驱动：
   >
   > <img src="C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot2\笔记\images\image-20230927233227598.png" alt="image-20230927233227598" style="zoom:67%;" />
   >
   > 

2. pom.xml 指定吧 src/main/java 目录中的xml文件包含到classpath中

   > ```xml
   > <resources>
   >     <resource>
   >         <directory>src/main/java/</directory>
   >         <includes>
   >             <include>**/*.xml</include>
   >         </includes>
   >     </resource>
   > </resources>
   > ```

3. 创建实体类 Student

4. 创建Mapper接口StudentMapper，创建一个查询学生的方法

   > ```java
   >     // 告诉Mybatis 这是mapper接口，创建次接口的代理对象，使用位置在类的上面
   >     @Mapper
   >     public interface StudentMapper {
   >         Student selectById(Long id);
   >     }
   > ```
   >
   > 注意：需要在Mapper接口上添加 @Mapper 注解

5. 创建Mapper接口对应的Mapper文件，xml文件，写Sql语句

   > ```xml
   > <select id="selectById" resultType="com.example.mybatis.pojo.Student">
   >     select <include refid="all"></include> from t_student where id=#{id}
   > </select>
   > ```

6. 创建Service层对象，创建StudentService接口和它的实现类，去dao对象的方法。完成数据库的操作

7. 创建Controller对象，访问Service。

   > ```java
   > @RequestMapping("selectStu")
   > @ResponseBody
   > public Student selectStu(Long id){
   >     return studentService.queryStudnetByID(id);
   > }
   > ```

8. 写application.properties文件

   配置数据库的连接信息。

   ```properties
   server.servlet.context-path=/orm
   server.port=10000
   #配置连接数据库信息
   spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
   spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
   spring.datasource.username=root
   spring.datasource.password=051727
   ```

## 第一种方式：@Mapper

   @Mapper：放在dao接口的上面， 每个接口都需要使用这个注解。

   作用：告诉MyBatis这是dao接口，创建此接口的代理对象。

   ~~~java
   // 告诉Mybatis 这是mapper接口，创建次接口的代理对象，使用位置在类的上面
   @Mapper
   public interface StudentMapper {
       Student selectById(Long id);
   }
   ~~~

## 第二种方式  @MapperScan

   在项目中通常由多个Mapper接口，为了方便，可以使用 @MapperScan 注解，指定 MyBatis 扫描哪个包下的mapper

   @MapperScan 注解 是使用在 application 类上的

   ```java
   @SpringBootApplication
   @MapperScan(basePackages = {"com.example.mybatis.Mapper"})
   public class Application {
   
       public static void main(String[] args) {
           SpringApplication.run(Application.class, args);
       }
   
   }
   ```

## 第三种方式： Mapper文件和Dao接口分开管理

   通常mapper.xml 文件放在resource目录中。

   当 sql 映射文件存放在 resource 目录中的时候，有两种情况：

   > 1. 存放的目录和 mapper 接口的包名相同，例如，mapper接口的包名为：com.fengshun.mapper，resourc目录中应该新建目录为："com/fengshun/mapper"
   >
   > 2. sql 映射文件在resource 目录中，但是文件位置和接口包名不同时，需要在 application.properties 文件中指定mapper文件的目录
   >
   >    ~~~properties
   >    #mapper文件存放在resource目录下的mapper目录中
   >    #指定mapper文件的位置
   >    mybatis.mapper-locations=classpath:mapper/*.xml
   >    #指定mybatis的日志
   >    mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
   >    ~~~
   >
   >    注意：在pom.xml中指定 把resources目录中的文件 ， 编译到目标目录中
   >
   >    ~~~xml
   >    <!--resources插件-->
   >    <resources>
   >       <resource>
   >          <directory>src/main/resources</directory>
   >          <includes>
   >             <include>**/*.*</include>
   >          </includes>
   >       </resource>
   >    </resources>
   >    ~~~
   >
   >    

## SpringBoot中的事物

Spring框架中的事务：

1） 管理事务的对象： 事务管理器（接口， 接口有很多的实现类）

​      例如：使用Jdbc或mybatis访问数据库，使用的事务管理器：DataSourceTransactionManager

2 ) 声明式事务：  在xml配置文件或者使用注解说明事务控制的内容

​     控制事务： 隔离级别，传播行为， 超时时间

3）事务处理方式：

​      1） Spring框架中的@Transactional

​      2)    aspectj框架可以在xml配置文件中，声明事务控制的内容



SpringBoot中使用事务： 

1. 可以使用上面的方式。

2. 使用SpringBoot自带的方式：

   > 1）在业务方法的上面加入@Transactional ,  加入注解后，方法有事务功能了。
   >
   > @Transactional 标注在方法上，表示这个方法支持事物，标注在类上，表示类中所有的方法都支持事物。
   >
   > ```java
   > @Override
   > @Transactional
   > public int save(Student insertStu, Student updateStu) {
   >     int i = studentMapper.insertStudent(insertStu);
   >     int m = 10 / 0;
   >     i += studentMapper.updateStu(updateStu);
   >     return i;
   > }
   > ```
   >
   > 2）明确的在 **主启动类**的上面 ，加入@EnableTransactionManager,这个注解可加可不加，但最好还是加上
   >
   > @EnableTransactionManager 注解 表示启用事物管理器
   >
   > ```java
   > @SpringBootApplication
   > @MapperScan(basePackages = {"com.example.mybatis.Mapper"})
   > @EnableTransactionManagement
   > public class Application {
   > ```

## 起别名

在MyBatis中通过配置文件起别名：

~~~xml

<!-- 起别名 -->
    <typeAliases>
        <package name="org.entity"/>
    </typeAliases>
~~~

SSM中通过在配置文件中配置：

~~~xml
 <!--SqlSeesionFactoryBean-->
 <bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
     <!--配置数据源-->
     <property name="dataSource" ref="dataSource"/>
     <!--关联MyBatis核心配置文件-->
     <property name="configLocation" value="classpath:mybatis-config.xml"/>
     <!--指定别名包，类似于在MyBatis配置文件中使用 typeAliases 标签-->
     <property name="typeAliasesPackage" value="com.fengshun.pojo"/>
 </bean>
~~~

在SpringBoot中 只需要在aapplication 配置文件中配置：

~~~properties
#起别名
mybatis.type-aliases-package=com.fengshun.pojo
~~~





# 第五章 接口架构风格 —RESTful

接口： API（Application Programming Interface，应用程序接口）是一些预先定义的接口（如函数、HTTP接口），或指[软件系统](https://baike.baidu.com/item/软件系统/224122)不同组成部分衔接的约定。 用来提供[应用程序](https://baike.baidu.com/item/应用程序)与开发人员基于某[软件](https://baike.baidu.com/item/软件)或硬件得以访问的一组[例程](https://baike.baidu.com/item/例程/2390628)，而又无需访问源码，或理解内部[工作机制](https://baike.baidu.com/item/工作机制/9905789)的细节。

在Vue的学习中，使用的天气API就是接口，只需要访问url就能获取数据



接口架构风格：就是API组织方式（样子）

  就是一个传统的：    http://localhost:9002/mytrans/addStudent?name=lisi&age=26 

​                                      在地址上提供了 访问的资源名称addStudent, 在其后使用了get方式传递参数。

## 5.1 认识 REST 

**REST（英文：Representational State Transfer，简称 REST）** 

一种互联网软件架构设计的风格，但它并不是标准，它只是提出了一组客户端和服务器交互时的架构理念和设计原则，基于这种理念和原则设计的接口可以更简洁，更有层次，REST这个词，是 Roy Thomas Fielding 在他 2000 年的博士论文中提出的。任何的技术都可以实现这种理念，如果一个架构符合 REST 原则，就称它为 RESTFul 架构

比如我们要访问一个 http 接口：http://localhost:8080/boot/order?id=1021&status=1

采用 RESTFul 风格则 http 地址为：http://localhost:8080/boot/order/1021/1



RESTful架构风格

1)REST :  （英文： Representational State Transfer , 中文： 表现层状态转移)。

   REST：是一种接口的架构风格和设计的理念，不是标准。

   优点： 更简洁，更有层次



  表现层状态转移: 

​         表现层就是视图层， 显示资源的， 通过视图页面，jsp等等显示操作资源的结果。

​          状态： 资源变化

​         转移： 资源可以变化的。 资源能创建，new状态，  资源创建后可以查询资源， 能看到资源的内容，

这个资源内容 ，可以被修改， 修改后资源 和之前的不一样。  



2）REST中的要素：

   用REST表示资源和对资源的操作。  在互联网中，表示一个资源或者一个操作。 

   资源使用url表示的， 在互联网， 使用的图片，视频， 文本，网页等等都是资源。

   资源是用名词表示。



  对资源进行操作： 

​        查询资源： 看，通过url找到资源。 

​        创建资源： 添加资源

​        更新资源：更新资源 ，编辑

​        删除资源： 去除

资源使用url表示，通过名词表示资源。

​     在url中，使用名词表示资源， 以及访问资源的信息,  在url中，使用“ / " 分隔对资源的信息

​     http://localhost:8080/myboot/student/1001

 使用http中的动作（请求方式）， 表示对资源的操作（CURD）

   GET:  查询资源  --  sql select

​                 处理单个资源： 用他的单数方式

​                  http://localhost:8080/myboot/student/1001

​                 http://localhost:8080/myboot/student/1001/1

​                处理多个资源：使用复数形式

​                  http://localhost:8080/myboot/students/1001/1002

​                

   POST: 创建资源  -- sql insert

​                http://localhost:8080/myboot/student

​                在post请求中传递数据

```html
<form action="http://localhost:8080/myboot/student" method="post">
	姓名：<input type="text" name="name" />
    年龄：<input type="text" name="age" />
  </form>
```


   PUT： 更新资源  --  sql  update

   ```xml
<form action="http://localhost:8080/myboot/student/1" method="post">
	姓名：<input type="text" name="name" />
    年龄：<input type="text" name="age" />
         <input type="hidden" name="_method" value="PUT" />
  </form>
   ```



   DELETE: 删除资源  -- sql delete

~~~html
<a href="http://localhost:8080/myboot/student/1">删除1的数据</a>
~~~

 需要的分页，  排序等参数，依然放在  url的后面， 例如 

 http://localhost:8080/myboot/students?page=1&pageSize=20



3） 一句话说明REST： 

​    使用url表示资源 ，使用http动作操作资源。

## 5.2 RESTful 的注解 

Spring Boot 开发 RESTful 主要是几个注解实现



  @PathVariable :  从url中获取数据

  @GetMapping:    支持的get请求方式，  等同于 @RequestMapping( method=RequestMethod.GET)

  @PostMapping:  支持post请求方式 ，等同于 @RequestMapping( method=RequestMethod.POST)

  @PutMapping:  支持put请求方式，  等同于 @RequestMapping( method=RequestMethod.PUT)

   @DeleteMapping: 支持delete请求方式，  等同于 @RequestMapping( method=RequestMethod.DELETE)

  

  @RestController:  符合注解， 是@Controller 和@ResponseBody组合。

​               在类的上面使用@RestController ， 表示当前类者的所有方法都加入了 @ResponseBody



```java
package com.fengshun.restful.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyRestController {

    @GetMapping("/student/{id}")
    public String queryStudent( @PathVariable(value = "id") Integer id){
        return "学生id"+id;
    }
}
```

## 5.3 RESTful 优点 

➢ 轻量，直接基于 http，不再需要任何别的诸如消息协议

get/post/put/delete 为 CRUD 操作

➢ 面向资源，一目了然，具有自解释性。

➢ 数据描述简单，一般以 xml，json 做数据交换。

➢ 无状态，在调用一个接口（访问、操作资源）的时候，可以不用考虑上下文，不用考虑当前状态，

极大的降低了复杂度。

➢ 简单、低耦合

## 5.4  在页面中或者ajax中，支持put，delete请求

在SpringMVC中 有一个过滤器， 支持post请求转为put ,delete



过滤器： org.springframework.web.filter.HiddenHttpMethodFilter

作用： 把请求中的post请求转为 put ， delete



实现步骤：

1. application.properties(yml) : 开启使用 HiddenHttpMethodFilter 过滤器

   ```properties
   #启用 hiddenmethodFilter 过滤器
   spring.mvc.hiddenmethod.filter.enabled=true
   ```

2. 在请求页面中，包含 _method参数， 他的值是 put， delete  ，  发起这个请求使用的post方式

   ```html
   <form action="student/test" method="post">
       <input type="hidden" name="_method" value="put">
       <input type="submit" value="测试请求方式">
   </form>
   ```

注意：url加上请求方式必须是唯一的：

> 针对一下程序：
>
> ```java
> @GetMapping("/student/{id}")
> public String queryStudent1( @PathVariable(value = "id") Integer id){
>     return "学生id"+id;
> }
> @GetMapping("/student/{age}")
> public String queryStudent2( @PathVariable(value = "age") Integer age){
>     return "学生age"+age;
> }
> ```
>
> 两个方法的请求的url都相似的。都是使用的GET请求，如果在页面中 访问的url是：http://localhost:9000/rest/student/1000110 那么程序并不知道需要执行哪个方法。所以，url和请求方式结合起来需要唯一。

# 第十章 总结

## 10.1 注解

Spring + SpringMVC + SpringBoot 

```java
创建对象的：
@Controller: 放在类的上面，创建控制器对象，注入到容器中
@RestController: 放在类的上面，创建控制器对象，注入到容器中。
             作用：复合注解是@Controller , @ResponseBody, 使用这个注解类的，里面的控制器方法的返回值                   都是数据

@Service ： 放在业务层的实现类上面，创建service对象，注入到容器
@Repository : 放在dao层的实现类上面，创建dao对象，放入到容器。 没有使用这个注解，是因为现在使用MyBatis框               架，  dao对象是MyBatis通过代理生成的。 不需要使用@Repository、 所以没有使用。
@Component:  放在类的上面，创建此类的对象，放入到容器中。 

赋值的：
@Value ： 简单类型的赋值， 例如 在属性的上面使用@Value("李四") private String name
          还可以使用@Value,获取配置文件者的数据（properties或yml）。 
          @Value("${server.port}") private Integer port

@Autowired: 引用类型赋值自动注入的，支持byName, byType. 默认是byType 。 放在属性的上面，也可以放在构造             方法的上面。 推荐是放在构造方法的上面
@Qualifer:  给引用类型赋值，使用byName方式。   
            @Autowird, @Qualifer都是Spring框架提供的。

@Resource ： 来自jdk中的定义， javax.annotation。 实现引用类型的自动注入， 支持byName, byType.
             默认是byName, 如果byName失败， 再使用byType注入。 在属性上面使用


其他：
@Configuration ： 放在类的上面，表示这是个配置类，相当于xml配置文件

@Bean：放在方法的上面， 把方法的返回值对象，注入到spring容器中。

@ImportResource ： 加载其他的xml配置文件， 把文件中的对象注入到spring容器中

@PropertySource ： 读取其他的properties属性配置文件

@ComponentScan： 扫描器 ，指定包名，扫描注解的

@ResponseBody: 放在方法的上面，表示方法的返回值是数据， 不是视图
@RequestBody : 把请求体中的数据，读取出来， 转为java对象使用。

@ControllerAdvice:  控制器增强， 放在类的上面， 表示此类提供了方法，可以对controller增强功能。

@ExceptionHandler : 处理异常的，放在方法的上面

@Transcational :  处理事务的， 放在service实现类的public方法上面， 表示此方法有事务


SpringBoot中使用的注解
    
@SpringBootApplication ： 放在启动类上面， 包含了@SpringBootConfiguration
                          @EnableAutoConfiguration， @ComponentScan


    
MyBatis相关的注解

@Mapper ： 放在类的上面 ， 让MyBatis找到接口， 创建他的代理对象    
@MapperScan :放在主类的上面 ， 指定扫描的包， 把这个包中的所有接口都创建代理对象。 对象注入到容器中
@Param ： 放在dao接口的方法的形参前面， 作为命名参数使用的。
    
Dubbo注解
@DubboService: 在提供者端使用的，暴露服务的， 放在接口的实现类上面
@DubboReference:  在消费者端使用的， 引用远程服务， 放在属性上面使用。
@EnableDubbo : 放在主类上面， 表示当前引用启用Dubbo功能。
    
    
    
    
    


    


```

