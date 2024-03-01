# 第一章 JDK 新特性

JDK8-19 新增了不少新特性，这里吧实际常用的新特性，介绍一下：

1. Java Record
2. Switch 开关表达式
3. Text Block 文本快
4. var 声明局部变量
5. Sealed 密封类

## 1.1 Java Record

Java14中预览的新特性叫做Record，在java中 ，Record是一个特殊类型的java类，可以用来创建不可变类，语法简短。参考 https://openjdk.java.net/jeps/395 。 Jsckson 2.12 支持 Record 类

任何时候创建java类，都会创建大量的样板代码，我们可能做如下：

1. 每一个字段的set，get方法
2. 公共的构造方法
3. 重写hashCode，toString()，equals() 方法



Java Record 避免上述的样板代码，如下特点：

1. 带有全部参数的构造方法
2. public 访问器
3. toString(),hashCode(),equals()
4. 无 set get方法。没有遵循Bean的命名规范
5. final类，不能继承 Record ，Record  为隐式订单final 类。除此以外与普通类一样
6. 不可变类，通过构造创建 Record 
7. final 属性，不可修改
8. 不能声明实例属性，能声明 static 成员

### 1.1.1 第一个 Record

定义一个 Record ：

```java
package com.fengshun.pk1;

public record Student(Integer id,String name ,Integer age) {
}
```

测试：

```java
@Test
void tset01() {
    // 创建 Record 对象
    Student zhangsan = new Student(1001,"张三",18);
    // 获取对象中的参数
    Integer id = zhangsan.id();
    String name = zhangsan.name();
    Integer age = zhangsan.age();
    Student lisi = new Student(1002, "李四", 23);
    // 运用 equals()
    System.out.println(zhangsan.equals(lisi));
    // 运用tostring方法
    System.out.println("zhangsan="+zhangsan);
    System.out.println("lisi="+lisi);
}
```

### 1.1.2 实例方法与静态方法

```java
package com.fengshun.pk1;

public record Student(Integer id,String name ,Integer age) {
    // 实例方法 .concat 连接字符串
    public String concat(){
        return String.format("姓名是%s,年龄是%d",this.name,this.age);
    }
    // 静态方法
    public static String nameReverse(String name){

        StringBuilder stringBuilder = new StringBuilder();
        stringBuilder.append(name);
        return stringBuilder.reverse().toString();
    }
}
```

测试：

```java
    @Test
    public void test02(){
        Student zhangsan = new Student(1001,"张三",18);
        System.out.println(zhangsan.concat());
    }
    @Test
    public void test03(){
        System.out.println(Student.nameReverse("zhangsna"));
    }
}
```

### 1.1.3 Record的构造方法

我们可以在Record中添加构造方法，有三种类型的构造方法，分别是：紧凑的、规范的和定制构造方法

- 紧凑型构造方法没有任何参数，甚至没有括号
- 规范构造方法是一所有成员作为参数
- 定制构造方法是自定义参数个数

1. 紧凑型构造方法

   ```java
   public record Student(Integer id,String name ,Integer age) {
       // 紧凑型构造方法
       public Student{
           if (id<1) {
               throw new RuntimeException("id<1");
           }
           System.out.println("id="+id);
       }
   }
   ```

2. 定制型构造方法

   ```java
   public record Student(Integer id,String name ,Integer age) {
       // 定制构造方法
       public Student(Integer id,String name){
           this(id,name,null);
       }
   }
   ```

测试代码：

```java
@Test
public void test04(){
    Student lisi = new Student(2001,"lisi");
    System.out.println(lisi);
}
```

结果截图：

![image-20230928215132397](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20230928215132397.png)

通过以上测试代码发现，程序会先执行紧凑型的构造方法，随后才会执行定制型构造方法，最后执行 toString方法

编译后的 Record ：

![image-20230928215634983](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20230928215634983.png)

### 1.1.4 Record 与 Lombok

Java Record 是创建不可变类且减少样板代码的好方法。Lombok是一种减少样板代码的工具。两者有表面上的重叠部
分。可能有人会说Java Record 会代替Lombok.两者是有不同用途的工具。

Lombok提供语法的便利性，通常预装一些代码模板，根据您加入到类中的注解自动执行代码模板。这样的库纯粹是
为了方便实现POJO类。通过预编译代码。将代码的模板加入到class中。

Java Record 是语言级别的，一种语义特性，为了建模而用，数据聚合。简单说就是提供了通用的数据类，充当“数据
载体”，用于在类和应用程序之间进行数据传输。

### 1.1.5 Record 实现接口

定义接口：

```java
package com.fengshun.pk2;

public interface PrintInterface {
    void print();
}
```

定义Record 实现接口：

```java
package com.fengshun.pk2;

public record User(String name,Integer age,String sex) implements PrintInterface{
    @Override
    public void print() {
        System.out.println("姓名="+name+",年龄="+age+",性别="+sex);
    }
}
```

编辑测试类：

```java
package com.fengshun.pk2;

import org.junit.jupiter.api.Test;


class UserTest {
    @Test
    void test1() {
        User user = new User("张三", 24, "男");
        user.print();
    }
}
```

### 1.1.6 Local Record

Record 可以作为局部对象使用。在代码块中定义并使用Record：

```java
@Test
public void test2(){
    // 定义java Record
    record SaleRecord(String saleId,String productName,Double moneny){};
    // 创建 Local Record
    SaleRecord saleRecord = new SaleRecord("s001", "电脑", 1500.0);
    // 使用 SaleRecord
    System.out.println("销售记录="+saleRecord);
}
```

### 1.1.7 嵌套 Record

多个 Record 可以组合定义，一个 Record  能够包含其他的Record 。

我们定义 Record  为 Customer ，存储客户信息，包含了 Addres 和 PhoneNumber 两个 Record 

1. 定义 Record

   ```java
   //定义 Address
   public record Address(String city,String address,String zipcode) {
   }
   //定义PhoneNumber
   public record PhoneNumber(String areaCode,String number) {
   }
   //定义Customer
   public record Customer(String id,String name ,PhoneNumber phoneNumber,Address address) {
   }
   ```

2. 创建 Customer 对象

   ```java
   @Test
   void test1() {
       Address address = new Address("四川省", "仁寿县", "120202");
       PhoneNumber phoneNumber = new PhoneNumber("010","400-4512-101");
       Customer customer = new Customer("C001","张三",phoneNumber,address);
       System.out.println("客户="+customer);
   }
   ```

3. 运行结果：

   ![image-20230928223000092](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20230928223000092.png)

### 1.1.8 instanceof 判断 Record 类型

instanceof 能够与 Java Record 一起使用，编译器知道记录组件的确切数量和类型。

1. 声明 PersonRecord ，拥有两个属性 name 和 age

   ```java
   public record Person(String name,Integer age) {
   }
   ```

2. 在一个业务方法判断当时Record类型时，继续判断age年龄是否满足18岁

   ```java
   public class SomeService {
       public boolean isEligible(Object obj){
           // 判断 obj 为Person记录类型
           if (obj instanceof Person){
               return ((Person)obj).age()>=18;
           }
           return false;
       }
   }
   ```

3. 测试：

   ```java
   @Test
   void test1() {
       Person lisi = new Person("李四",10);
       SomeService someService = new SomeService();
       if (someService.isEligible(lisi)) {
           System.out.println("成功");
       }else {
           System.out.println("失败");
       }
   }
   ```

instanceof  还可以有下面的方式：

可以添加一个命名空间：

```java
public boolean isEligible2(Object obj){
    // 判断 obj 为Person记录类型
    if (obj instanceof Person p){
        return p.age()>=18;
    }
    return false;
}
```

### 1.1.9 总结

- abstract 类 java.lang.Record 是所有 Record 的父类

- 有对于 equals() hashCode(),toString() 方法的定义和说明

- Record 类能实现 java.io.Serializable 序列化或反序列化

- Record 支持泛型，例如 record Gif<T> (T t){}

- java.lang.Class 类与Record类有两个方法：

  > boolean isRecord() : 判断一个类是否是Record 类型
  >
  > RecordComponent[] getRecordComponents() : Record的数组，表示此记录类的所有记录组件
  >
  > ```java
  > @Test
  > public void test2(){
  >     Address address = new Address("四川省", "仁寿县", "120202");
  >     PhoneNumber phoneNumber = new PhoneNumber("010","400-4512-101");
  >     Customer customer = new Customer("C001","张三",phoneNumber,address);
  > 
  >     // 判断Customer 是否为 Java Record类型
  >     boolean record = customer.getClass().isRecord();
  >     System.out.println("record="+record);
  > 
  >     RecordComponent[] recordComponents = customer.getClass().getRecordComponents();
  >     for (RecordComponent recordComponent : recordComponents) {
  >         System.out.println("recordComponent="+recordComponent);
  >     }
  > 
  > }
  > ```

## 1.2 Switch

​	Switch 的三个方面：

  > - 支持箭头表达式
  > - 支持yied返回值
  > - 支持 Java Record

### 1.2.1 箭头表达式，新的case标签

Switch 新的语法，case label -> 表达式|throw 语句 |block

week 表示周几，用switch判断：

```java
@Test
public void test1()  {
    int week=7;
    String memo="";
    switch (week){
        case 1 -> memo="星期日，休息";
        case 2,3,4,5,6 -> memo="工作日";
        case 7 -> memo="星期六，休息";
        default -> throw new RuntimeException("无效日期");
    }
    System.out.println("memo="+memo);
}
```

箭头表达式中使用大括号：

```java
@Test
public void test2() {
    int week = 7;
    String memo = "";
    switch (week) {
        case 1 -> {
            memo = "星期日，休息";
        }
        case 2, 3, 4, 5, 6 -> {
            memo = "工作日";
        }
        case 7 -> {
            memo = "星期六，休息";
        }
        default -> {
            memo="无效日期";
        }
    }
    System.out.println("memo=" + memo);
}
```

注意：

> case 标签 ->   与  case 标签： 两者不能混用，一个Switch语句块中只能使用一种语法格式。
>
> Switch 作为表达式，赋值给变量，需要yield或者case ->  表达式 。 ->  的右侧表达式为case的返回值

### 1.2.2 yeild 返回值

yeild 让 Switch 作为表达式，能够返回值：

**语法**：

​			变量 = switch(value) { case v1 : yield 结果值； case v2 : yield 结果值； case v3 , v4 , v5 : yield 结果值；}

​	说明： yield 是 Switch 的返回值，yield 表示跳出当前 switch 语句块

示例： yield 返回值，跳出Switch块

```java
@Test
    public void test5() {
        int week = 7;
        String memo = "";
        memo=switch (week) {
            case 1 -> {
                yield "星期日，休息";
            }
            case 2, 3, 4, 5, 6 -> {
                yield "工作日";
            }
            case 7 -> {
                yield "星期六，休息";
            }
            default -> {
                yield "无效日期";
            }
        };
        System.out.println("memo=" + memo);
    }
```

### 1.2.3 switch 结合 Java Record 

switch 表达式中使用 record ，结合 case标签 -> 表达式，yield 实现复杂的计算

```java
@Test
public void test3() {
    Line line = new Line(10, 100);
    Rectangle rectangle = new Rectangle(100, 200);
    Shape shape = new Shape(200, 200);

    Object obj = line;
    int result = switch (obj) {
        case Line(int x, int y) ->{
            System.out.println("图形是Line，x=" + x + ",y=" + y);
            yield x + y;
        }
        case Rectangle(int width,int height) ->{
            System.out.println("图形是Rectangle,width="+width+",height="+height);
            yield 2*(width+height);
        }
        case Shape(int width ,int height)->{
            yield width*height;
        }
        default -> {
            throw new RuntimeException("异常");
        }
    }
}
```

## 1.3 Text Block

text Block 处理多行文本十分方便，省时省力，无需连接 “ + ” ，单引号，换行符等。

### 1.3.1 认识文本块

语法：使用三个双引号字符括起来

~~~
"""
内容
"""
~~~

示例：

```java
@Test
public void t(){
    //String name1= """lsi""";  //Error 不能将文本块放在单行上
    String name2 = """
            lisi
            20""";      //不推荐：文本块的内容不能再没有中间结束符的情况下跟随三个开头双引号
    String  name3= """
            lisi
            20
            """;    //推荐使用
    System.out.println(name2);
}
```

### 1.3.2 空白

JEP 378 中包含空格处理的详细算法说明。

Text Block 中的缩进会自动去除，左侧和右侧的空格会被删除

要保留左侧的缩进或者空格。需要将文本块的内容向左移动（tab 键）

```java
public void test7(){
    String str = """
            <html>
                <body><body/>
            <html/>
            """;
    System.out.println(str);
}
```

![image-20230930101834647](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20230930101834647.png)

### 1.3.3 文本块的方法

formatted（） 方法

```java
@Test
void test8(){
    String info= """
            name:%s
            Phone:%s
            age:%d
            """.formatted("张三","1654315432413",24);
    System.out.println(info);
}
```

String stripIndent() : 删除每行开头和结尾的空白。

String translateEscaoes()： 转义序列转换为字符串字面量

### 1.3.4 转义字符

新的转义字符 "\" ，表示隐士换行符，这个转义字符被 Text Block 转义为空格。通常用于是拆分非常长的字符串文本，串联多个较小字符串，包装为多行文本字符串。

```java
@Test
void test9(){
    String str= """
            Spring Boot 是一个快速开发框架 \
            基于\" Spring \" 框架，创建Spring 应用
            """;
    System.out.println(str);
}
```

## 1.4 var

在 JDK 10以及更高版本中，可以使用 var 标识符声明具有非空初始化式的局部变量，这可以帮助您编写简洁的代码，消除冗余信息使代码更具有可读性，需要谨慎使用。

### 1.4.1 var 声明局部变量

var 特点：

- var 是一个保留字，不是关键字（可以声明var为变量名）
- 方法内声明的局部变量，必修有初值
- 每次声明一个变量，不可复合声明多个变量。var  s1=“hellol”，age=12  // Error
- var 动态类型是编译器根据变量所赋的值来推断类型
- var代替显示类型，代码简介，减少不必要的排版

```java
@Test
public void test10(){
    for (var i=0;i<10;i++){
        System.out.println(i);
    }
}
```

### 1.4.2 什么时候使用 var

- 简单的临时变量
- 复杂，多步骤逻辑，嵌套的表达式等，简短的变量有助理解代码
- 能够确定变量初始值
- 变量类型比较长时

## 1.5 密闭类 sealed

sealed 翻译为密封，密封类（Sealed Classes）的首次提出在java15

**Sealed Class 主要特点是限制继承**

**Sealed Classes 限制无限的扩张**



在java中已有sealed 的设计

> 1. final 关键字，修饰类不能被继承
> 2. private 限制私有类

sealed 作为关键字，可以在class和interface 上使用，结合permits 关键字。定义限制继承的密封类

### 1.5.1 Sealed Classes

sealed class 类名 permits 子类1，子类N列表{}

使用：

1. 定义父类：

   ```java
   package com.fengshun.pk6;
   
   public sealed class Shape permits Circle,Square,Rectangle{
       private Integer width;
       private Integer height;
       public void  draw(){
           System.out.println("画一个图形");
       }
   }
   //表示 Shape 的子类有： Circle,Square,Rectangle
   ```

   permits 表示允许的子类，一个或者多个

2. 声明子类

   子类声明有三种:

   1. final 	终结，依然是密封的

   2. sealed 	子类是密封类，需要子类实现

   3. non-sealed 	非密封类，扩展使用，不受限

   ```java
   //第一种方式： final
   public final class Circle extends Shape {  //表示 Circle 不会再有子类
   }
   //第二种方式 sealed
   public sealed class Square extends Shape permits RoundSquare{ //表示Square有个子类RoundSquare
       @Override
       public void draw() {
           System.out.println("================Square图形=============S");
       }
   }
   public final class RoundSquare extends Square {
   }
   //第三种方式 non-sealed
   //非密闭类
    public non-sealed class Rectangle extends Shape{  //表示Rectangle类可以被任意类继承
   }
   
   ```

### 1.5.2 Sealed Interface

密闭接口和密闭类是一样的道理

1. 声明密闭接口

   ~~~java
   public sealed interface SomeService permits SomServiceImpl{
       void dpThing();
   }
   ~~~

   

2. 实现接口

   ~~~java
   public final class SomeServiceImpl implements SomeService{
       @Ovrride
       public void doThing(){
           
       }
   }
   ~~~

   以上类和接口要在同一包可访问范围内

# 第二章 SpringBoot 基础篇

## 2.1 SpringBoot

Spring Boot 是目前流行的微服务框架 倡导 约定优先于配置”其设 目的是 用来简化新Spring 应用的初始化搭建以及开发过程。Spring Boot 提供了很多核心的功 能，比如自动化配置 starter（启动器)简化 Maven配置、内嵌Servlet 容器、应用监控等功能，让我们可以快速构建企业级应用程序。

特性:

1. 创建独立的Spring 应用程序。
2. 嵌入式 Tomcat、Jetty、Undertow 容器（jar)
3. 提供的starters简化构建配置（简化依赖管理和版本控制）
4. 尽可能自动配置 spring应用和第三方库
5. 提供生产指标，例如指标、健壮检查和外部化配置
6. 没有代码生成，无需XML配置

SpringBoot 同时提供 “开箱即用”，“约定优于配置”的特性。

**开箱即用**：Spring Boot应用无需从0开始，使用脚手架创建项目。基础配置已经完成。 集成大部分第三方库对象，无需配置就能使用。例如在Spring Boot 项目中使用MyBatis。可以直接使用XXXMapper对象，调用方法执行sql语句。

**约定优于配置**：Spring Boot定义了常用类，包的位置和结构，默认的设置。代码不需要做调整，项目能够按照预期运行。比如启动类在根包的路径下，使用了@SpringBooApplication注解。创建了默认的测试类。controller,service，dao 应该放在根包的子包中。application为默认的配置文件。

### 2.1.1 与Spring的关系

Spring 框架:

Spring Boot 创建的是Spring应用，对于这点非常重要。也就是使用Spring 框架创建的应用程序。这里的Spring是指Spring Framework。我们常说的Spring，一般指Spring 家族，包括 Spring Boot、Spring Framework、Spring Data , Spring Security,Spring Batch, Spring Shell, Spring for Apache Kafka…...

Spring 的核心功能：IoC，AOP，事务管理，JDBC，SpringMVC，Spring WebFlux,集成第三方框架，MyBatis,Hibernate,Kafka，消息队列.….

Spring 包含 SpringMVC，SpringMVC作为web开发的强有力框架，是Spring中的一个模块。

### 2.1.2 与 SpringCloud 关系

**微服务**：微服务(Microservices Architecture)是一种架构和组织方法，微服务是指单个小型的但有业务功能的服务，
每个服务都有自己的处理和轻量通讯机制，可以部署在单个或多个服务器上。

将一个大型应用的功能，依据业务功能类型，抽象出相对独立的功能，称为服务。每个服务就上一个应用程序,有自己的业务功能，通过轻量级的通信机制与其他服务通信（通常是基于HTTP的RESTfulAPI)，协调其他服务完成业务请求的处理。 这样的服务是独立的，与其他服务是隔离的，可以独立部署，运行。与其他服务解耦合。

微服务看做是模块化的应用，将一个大型应用，分成多个独立的服务，通过http或rpc将多个部分联系起
来。请求沿着一定的请求路径，完成服务处理。

Spring Cloud 包含的这些框架和工具各负其职，例如Spring CloudConfig 提供配置中心的能力，给分布式多个服务提供动态的数据配置，像数据库的url，用户名和密码等，第三方接口数据等。Spring Cloud Gateway网关，提供服务统一入口，鉴权，路由等功能。
学习Spring Colud难度比较大，里面框架，工具比较多。有多个框架需要学习，在把框架组合起来又是一个难度。

### 2.1.3 最新的SpringBoot3 新特性

2022年11月24日。Spring Boot3发布，里程碑的重大发布。这个版本应该是未来5年的使用主力。Spring
官网支持Spring Boot3.0.X版本到2025年。

SpringBoot3中的重大变化：

1.  JDK最小Java 17,能够支持17-20.
2. Spring Boot 3.0已将所有底层依赖项从 Java EE 迁移到了 Jakarta EE API。原来javax开头的包名，修改为jakarta。例如jakarta.servlet.http.HttpServlet 原来javax.servlet.http.HttpServlet 
3. 支持GraalvM 原生镜像。将Java应用编译为本机代码，提供显著的内存和启动性能改进。
4. 对第三方库，更新了版本支持。
5. 自动配置文件的修改。
6. 提供新的声明式Htp服务，在接口方法上声明@HttpExchange 获取http 远程访问的能力。代替 OpenFeign
7. Spring HTTP 客户端提供基于 Micrometer 的可观察性.跟踪服务，记录服务运行状态等
8. AOT（预先编译) 支持Ahead Of Time，指运行前编译
9. Servlet6.0规范
10. 支持Jackson 2.14。
11. Spring MVC：默认情况下使用的PathPatternParser。删除过时的文件和 FreeMarker、JSP支持。



伴随着Spring Boot3的发布，还有 Spring Framework6.0的发布(2022-11-16),先于Spring Boot发布。

## 2.2 脚手架

脚手架是一种用在建筑领域的辅助工具，是为了保证建筑施工过程顺利进行而搭设的工作平台。软件工程中的脚手架是用来快速搭建一个小的可用的应用程序的骨架，将开发过程中要用到的工具、环境都配置好，同时生成必要的模板代码。

脚手架辅助创建程序的工具，Spring Initializr 是创建Spring Boot 项目的脚手架。快速建立 Spring Boot项目的最好方式。他是一个web应用，能够在浏览器中使用。IDEA中继承了此工具，用来快速创建Spring Boot项目以及Spring Cloud项目。

Spring Initializr 脚手架的 web地址： https://start.spring.io/

阿里云脚手架：https://start.aliyun.com/

### 2.2.1 使用脚手架创建项目

使用网站创建项目

<img src="C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20230930142934844.png" alt="image-20230930142934844" style="zoom:50%;" />

使用idea创建项目

<img src="C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20230930142955247.png" alt="image-20230930142955247" style="zoom:67%;" />

## 2.3 代码结构

### 2.3.1 单一模块

一个工程代表一个模块。

模块结构如下：

<img src="C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20230930143757531.png" alt="image-20230930143757531" style="zoom:67%;" />

### 2.3.2 多模块

一个SpringBoot中多个模块。在根包下创建每个模块的子包，子包中可以按 "单一模块" 包结构定义。

创建包含多个功能的单体SpringBoot。

### 2.3.3 包和主类

我们通常建议您将主应用程序类定位在其他类之上的根包中。@SpringBootApplication 注释通常放在主类上，它隐式地为某些项定义了一个基本的“搜索包”。例如，如果您正在编写一个JPA应用程序，则使用@SpringBootApplication 注释类的包来搜索@Entity 项。使用根包还允许组件扫描只应用于您的项目。

Spring Boot 支持基于java的配置。尽管可以将SpringApplication与XML源一起使用，但我们通常建议您的主
源是单个@Configuration类。通常，定义主方法的类可以作为主@Configuration类。

### 2.3.4 spring-boot-starter-parent

pom.xml中的< parent>指定spring-boot-starter-parent作为坐标，表示继承Spring Boot提供的父项目。从spring-boot-starter-parent 继承以获得合理的默认值和完整的依赖树，以便快速建立一个Spring Boot项目。

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.1.4</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

父项目提供以下功能：

1.  JDK的基准版本，比如<java.version>17</java.version>
2. 源码使用UTF-8格式编码
3. 公共依赖的版本
4. 自动化的资源过滤：默认把src/main/resources目录下的文件进行资源打包
5. maven的占位符为 ‘ @ ’
6. 对多个 Maven 插件做了默认配置，如maven-compile-plugin, maven-jar-plugin

## 2.4 starter

‘starter 是一组依赖描述，应用中包含starter，可以获取spring相关技术的一站式的依赖和版本。不必复制、粘粘代码。通过starter 能够快速启动并运行项目。
starter包含：依赖坐标、版本传递依赖的坐标、版本>配置类，配置项

```xml
<!--依赖列表-->
<dependencies>
    <!--Spring Web依赖
        带有starter 单词叫做启动器（启动依赖）
        spring-boot-starter-xxx : 是Spring官方推出的启动器。
        xxx-starter：非Spring推出的，由其他组织提供的
    -->
    <!--starter ：启动器，是一组依赖的描述（说明那些maven，gav以及其他需要的gav）
           包含:
                 依赖和版本
                 传递依赖和版本
                 配置类，配置项

     -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

## 2.5 外部化配置

应用程序=代码+数据（数据库，文件，url)

应用程序的配置文件：Spring Boot 允许在代码之外，提供应用程序运行的数据，以便在不同的环境中使用相同的应用程序代码。避免硬编码，提供系统的灵活性。可使用各种外部配置源，包括Java属性文件、YAML文件、环境变童和命令行参数。

项目中经常使用properties与yaml文件，其次是命令行参数

### 2.5.1 配置文件基础

#### 2.5.1.1 配置文件格式

配置文件有两种格式分别：properies 和yaml（yml)。properties是Java中的常用的一种配置文件格式，
key=value。key 是唯一的，文件扩展名为properties。

yaml(YAML Ain't Markup Language）也看做是yml，是一种做配置文件的数据格式，基本的语法  key : [空格]
值。yml 文件文件扩展名是yaml或yml（常用）。

yml 格式特点：

**YAML基本语法规则**：

- 大小写敏感
- 使用缩进表示层级关系
- 缩进只可以使用空格，不允许使用Tab键
- 缩进的空格数目不重要，相同层级的元素左侧对齐即可
- \#字符表示注释，只支持单行注释。#放在注释行的第一个字符
- YAML缩进必须使用空格，而且区分大小写，建议编写YAML文件只用小写和空格。

YAML支持三种数据结构

- 对象：键值对的集合，又称为映射（mapping)/哈希（hashes)/字典（dictionary)
- 数组：一组按次序排列的值，又称为序列（sequence)/列表（list）
- 标量（scalars）：单个的、不可再分的值，例如数字、字符串、true|false等
- 



需要掌握数据结构完整内容，可从https://yaml.org/type/index.html获取详细介绍。

#### 2.5.1.2 application文件

Spring Boot 同时支持properties和yml格式的配置文件。配置文件名称默认是application。我们可以使用
application.properties , application.yml

读取配置文件的key 值，注入到Bean的属性可用@Value，@Value一次注入一个key 的值，将多个key值绑定到Bean的多个属性要用到@ConfigurationProperties注解。在代码中访问属性除了注解，Spring提供了外部化配置的抽象对象Environment。Environment包含了几乎所有外部配置文件，环境变量，命令行参数等的所有key和value。需要使用Environment的注入此对象，调用它的getProperty(String key）方法即可。

#### 2.5.1.3 Environment

Environment 是外部化的抽象，是多种数据来源的集合。从中可以读取 application 配置文件，环境变量，系统属性。使用方式在Bean 中注入 Environment。调用他的getProperty（key）方法。

```java
@Service("readConfig")
public class ReadConfig {
    @Autowired
    private Environment environment;

    public void print(){
        // 获取某个key的值
        String name = environment.getProperty("app.name");

        // 判断key是否存在
        boolean isAge = environment.containsProperty("app.age");

        // 读取key的值转为期望的类型，同时提供默认值
        Integer port = environment.getProperty("app.port", Integer.class, 9000);

        System.out.println("app.age 是否存在："+isAge);
        System.out.println(String.format("app.name=%s,port=%d", name, port));

    }
}
```

测试：

```java
@SpringBootTest
class ReadConfigTest {

    @Autowired()
    @Qualifier("readConfig")
    private ReadConfig readConfig;

    @Test
    void test(){
        readConfig.print();
    }
}
```

![image-20230930221409873](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20230930221409873.png)

#### 2.5.1.4 组织多文件

大型集成的第三方框架，中间件比较多。每个框架的配置细节相对复杂。如果都将配置集中到一个 application文件，导致文件内容多，不易于阅读。我们将每个框架独立一个配置文件，最后将多个文件集中到application。我们使用导入文件的功能。



导入多个配置：

​	语法：spring.config.import : 文件的url



需求：项目集成redis，数据库mysql。将redis，数据库单独放到独立的配置文件。

1. 在resource目录中创建两个配置文件

   db.yml

   ```yaml
   spring:
     datasource:
       url: jdbc:mysql://localhost:3306/mybatis
       username: root
       password: 051727
   ```

   redis.yml

   ```yaml
   spring:
     redis:
       host: 192.168.0.176
       hort: 6379
       password: 0517247
   ```

2. 在application.properties 导入多个配置

   ```properties
   spring.config.import=config/db.yml,config/redis.yml
   ```

#### 2.5.1.5 多环境配置

   

   软件开发中经常提到环境这个概念，与日常生活中的环境概念一样。环境影响居住体验。影响软件运行的也叫做环境，例如应用中访问数据库的ip，用户名和密码，Redis的端口，配置文件的路径，windows，linux系统,tomcat 服务器等等。围绕着程序周围的都是环境。环境影响软件的运行。

Spring Profiles 表示环境，Profiles有助于隔离应用程序配置，并使它们仅在某些环境中可用。常说开发环境.测色环境，生产环境等等。一个环境就是一组相关的配置数据，支撑我们的应用在这些配置下运行。应用启动时指定适合的环境。开发环境下每个开发人员使用自己的数据库ip，用户，redis端口。同一个程序现在要测试了。需要把数据库ip，redis的改变为测试服务器上的信息。此时使用多环境能够方便解决这个问题。

Spring Boot 规定环境文件的名称 application-{profile}.properties(yml)。其中profile为自定义的环境名称，推荐使用dev 表示开发，test 表示测试。prod表示生产，feature 表示特性。总是profile名称是自定义的。Spring Boot会加载application 以及application-{profile}两类文件，不是只单独加载application-{profile}。

**使用方式**： 创建多个配置文件， 名称规则： application-环境名称.properties(yml)

1. 新建一个开发环境的配置文件  application-dev.properties

   ```properties
   #开发环境
   server.port=8081
   server.servlet.context-path=/dev
   ```

2. 新建一个测试环境的配置文件  application-test.properties

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

## 2.6 绑定Bean

### 2.6.1 简单的属性绑定

从配置文件中读取数据并且赋值到Bean中：

1. 在 properties配置文件中添加值：

   ```properties
   app.name=张三
   app.port=56546
   app.owner=时候
   ```

2. 定义Bean对象（需要无参构造方法以及set方法）

   @ConfigurationProperties 注解，通过set方法注入‘

   注意：属性必须是非静态的（static）

   @ConfigurationProperties 注解 只负责给属性赋值，还是需要依靠@Component类注解，创建对象。或者加上 @Configuration 注解

   ```java
   package com.fengshun.project.repository;
   
   import org.springframework.boot.context.properties.ConfigurationProperties;
   import org.springframework.stereotype.Component;
   
   @Component
   @ConfigurationProperties(prefix = "app")
   public class AppBean {
       private String name;
       private String owner;
       private Integer port;
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public String getOwner() {
           return owner;
       }
   
       public void setOwner(String owner) {
           this.owner = owner;
       }
   
       public Integer getPort() {
           return port;
       }
   
       public void setPort(Integer port) {
           this.port = port;
       }
   
       public AppBean(String name, String owner, Integer port) {
           this.name = name;
           this.owner = owner;
           this.port = port;
       }
   
       public AppBean() {
       }
   
       @Override
       public String toString() {
           return "AppBean{" +
                   "name='" + name + '\'' +
                   ", owner='" + owner + '\'' +
                   ", port=" + port +
                   '}';
       }
   }
   ```

3. 测试：

   ```java
   @SpringBootTest
   class AppBeanTest {
   
       @Autowired
       private AppBean appBean;
   
       @Test
       public void test(){
           System.out.println(appBean);
       }
   }
   ```

注意：

> 使用 @Configuration 注解标注后的Bean，是一个由SpringBoot 创建的代理Bean![image-20231001110723101](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231001110723101.png)
>
> 想要Bean为普通Bean，需要给 @Configuration 注解添加属性：
>
> ```java
> @Configuration(proxyBeanMethods = false)
> //添加这个属性后，可以提高程序的效率
> ```
>
> 添加后的运行结果：![image-20231001110654079](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231001110654079.png)

### 2.6.2 嵌套Bean

Bean中包含其他Bean作为属性，将配置文件中的配置项绑定到Bean以及引用类型的成员。Bean的定义无特殊要求

1. 定义两个Bean

   ```java
   
   public class Security {
       private String username;
       private String password;
       //setter and getter
       //toString
       
   }
   
   
   @Component
   @ConfigurationProperties(prefix = "app")
   public class NestAppBean {
       private String name;
       private String owner;
       private Integer port;
       private Security security;
       //setter and getter
       //toString
   }
   ```

2. 添加配置：

   ```properties
   app.name=张三
   app.port=56546
   app.owner=时候
   app.security.username=李四
   app.security.password=6546
   ```

3. 测试：

   ```java
   @SpringBootTest
   class NestAppBeanTest {
       @Autowired
       private NestAppBean nestAppBean;
       @Test
       void test(){
           System.out.println(nestAppBean);
       }
   }
   ```

   ![image-20231001111835935](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231001111835935.png)

### 2.6.3 扫描注解

想要 @ConfigurationProperties 注解起作用，除了在Bean上添加 @Configuration 注解以外，还还可以使用 @EnableConfigurationProperties 或者 @ConfigurationPropertiesScan 。这个注解是专门寻找 @ConfigurationProperties 注解的，将他的对象注入到Spring容器中，在启动类上使用扫描注解。

1. 使用 @EnableConfigurationProperties  注解

   这个注解需要value属性，value属性的值是一个数组，它指的是Bean

   ```java
   @SpringBootApplication
   @EnableConfigurationProperties(value = {NestAppBean.class})
   public class Application {
   
       public static void main(String[] args) {
           SpringApplication.run(Application.class, args);
       }
   
   }
   ```

2. 使用 @ConfigurationPropertiesScan 注解：

   ![image-20231001113506195](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231001113506195.png)

```java
@SpringBootApplication
@ConfigurationPropertiesScan(basePackages = {"com.fengshun.project.repository"})
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

### 2.6.4 处理第三方库对象

上面的例子都是在源代码上使用 @ConfigurationProperties 注解，如果某个类需要在配置文件中提供数据，但是没有源代码。此时 @ConfigurationProperties 结合 @Bean 一起在方法上面使用。

比如现在有一个 Security 类是第三方库中的类，现在要提供它的 username，password 属性的值。

1. 添加配置

   ```properties
   security.username=root
   security.password=0581727
   ```

2. 创建配置类

   ```java
   @Configuration
   public class ApplocationConfig {
       @ConfigurationProperties(prefix = "security")
       @Bean
       public Security ceateSecurity(){
           return new Security();
       }
   
   }
   ```

3. 测试：

4. 

5. ```java
   @SpringBootTest
   class ApplicationTests {
       @Autowired
       private ApplocationConfig applocationConfig;
   
   
       @Test
       void test(){
           System.out.println(applocationConfig.ceateSecurity());
       }
   
   }
   ```

![image-20231001114558996](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231001114558996.png)

### 2.6.5 集合Map，List 以及 Array

1. 创建保存数据的Bean

   ```java
   public class User {
       private String name;
       private String sex;
       private Integer age;
       //setter and getter
       //toString
   }
   public class MyServer {
       private String title;
       private String ip;
       //getter and setter 
       //toString
       
   }
   
   @ConfigurationProperties(prefix = "bean")
   public class ColletionConfig {
       private List<MyServer> myServers;
       private Map<String,User> userMap;
       private String[] names;
       //setter and getter
       //toString
   }
   ```

2. 编写配置文件：

   yml格式文件：

   ```yaml
   # "-" 后面也需要跟上空格
   bean:
     #配置map集合
     userMap:
       user1:
         name: 张三
         sex: 男
         age: 21
       user2:
         name: 李四
         sex: 男
         age: 32
     #配置数组
     names:
       - zhangsan
       - lisi
       - wangwu
   #配置list集合
     myServers:
       - title: 华为服务器
         ip: 192.168.1.1
       - title: 小米服务器
         ip: 192.154.15.152
   ```

 	properties 格式文件注入：

```properties
#创建map集合 Map<String,User> userMap
#user1
bean.userMap.user1.name=张三
bean.userMap.user1.sex=男
bean.userMap.user1.age=18
#user2
bean.userMap.user2.name=李四
bean.userMap.user2.sex=男
bean.userMap.user2.age=32

#创建List List<MyServer> myServers;
#第一个对象
bean.myServers[0].title=华为服务器
bean.myServers[0].ip=156.154.133.245
#第二个对象
bean.myServers[1].title=小米服务器
bean.myServers[1].ip=156.231.015.132

#创建数组 String[] names;
bean.names=zhangsan,lisi,wangwu
```

### 2.6.6 指定数据源文件

application 做配置是经常使用的，除以以外我们能够指定某个文件作为数据来源。@PropertySource是主力,用以加载指定的properties文件。也可以是XML文件（无需了解）。@PropertySource 与@Configuration一同使用，其他注解还有@Value，@ConfigurationProperties。
需求：一个组织信息，在单独的properties文件提供组织的名称，管理者和成员数量

1. 新建配置文件 group.properties

   ```properties
   group.name=xueix
   group.leader=张三
   group.members=561
   ```

2. 新建类接受参数：

   ```java
   @Configuration
   @PropertySource(value = {"classpath:group.properties"})
   @ConfigurationProperties(prefix = "group")
   public class Group {
       private String name;
       private String leader;
       private Integer members;
       //getter and setter
       //toString
   }
   ```

3. 测试：

   ```java
   @SpringBootTest
   class GroupTest {
       @Autowired
       private Group group;
   
       @Test
       public void test(){
           System.out.println(group);
       }
   
   }
   ```

## 2.7 创建对象的三种方式

将对象注入到spring容器，可以通过以下方式：

- 传统的XML配置文件
- Java Config 技术，@Configuration 与 @Bean
- 创建对象的注解，@Controller，@Service，@Repository，@Component

Spring Boot 不建议使用Xml文件的方式，自动配置已经解决了大部分xml中的工作了。如果需要xml提供bean的声明，@ImportResource 加载Xml注册Bean

1. 创建Person类，对象由容器管理

   ```java
   public class Person {
       private String name;
       private Integer age;
       //getter and setter
       
       //toStirng
   }
   ```

2. 编写applicationContext.xml文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   <bean id="person" class="com.fengshun.project.pk4.Person">
       <property name="name" value="张三"/>
       <property name="age" value="32"/>
   </bean>
   </beans>
   ```

3. 在启动类上添加 @ImportResource注解，并测试

   ```java
   @SpringBootApplication
   @ImportResource(locations = "classpath:config/applicationContext.xml")
   public class Application {
       @Autowired
       private Person person;
   
       public static void main(String[] args) {
   
           ConfigurableApplicationContext run = SpringApplication.run(Application.class, args);
           System.out.println(run.getBean("person", Person.class));
           
       }
   
   }
   ```



## 2.8 Aop

在springBoot中想要使用切面编程，需要添加如下依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

这个依赖包含了aop依赖和aspectj依赖

使用方式和Spring中的切面编程完全一致。

# 第三章 自动配置

启用autoconfigure（自动配置），框架尝试猜测和Bean 要使用的Bean，从类路径中查找xxx.jar，创建这个jar中某些需要的Bean。例如我们使用MyBatis访问数据，从我们项目的类路径中寻找mybatis.jar，进一步创建SqlSessionFactory,还需要DataSource 数据源对象，尝试连接数据。这些工作交给XXXAutoConfiguration类，这些就是自动配置类。在spring-boot-autoconfigure-3.0.2.jar 定义了很多的XXXAutoConfiguration类。第三方框架的starter 里面包含了自己 XXXAutoConfiguration。

**自动配置**:从类路径中，搜索相关的jar，根据jar的内容，尝试创建所需的对象。如果有mybatis.jar，尝试创建DataSource(根据配置文件中的url，username,password)连接数据库。还需要创建SqlSessionFactory,Dao接口的代理对象。 这些开发人员不需要写一行代码，就能使用Mybatis框架了

第三方框架MyBatis,mybatis-spring-boot-starter 的MyBatisAutoConfiguration自动配置类 

![image-20231001175536348](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231001175536348.png)

自动配置的注解@EnableAutoConfiguration（通常由@SpringBootApplication注解带入）所在的包，具有特殊的含义，是Spring Boot 中的默认包，默认包是扫描包的起点(根包)。@Controller,@Service,@Repository,@Component,@Configuration放在根包以及子包中就会被扫描到。



# 第四章 访问数据库

Spring Boot 框架为SQL数据库提供了广泛的支持，既有用JdbcTemplate 直接访问JDBC，同时支持“object relational mapping”技术（如Hibernate，MyBatis)。Spring Data 独立的项目提供对多种关系型和非关系型数据库的访问支持。比如 MySQL, Oracle,MongoDB,Redis,R2DBC，Apache Solr，Elasticsearch...

Spring Boot 也支持嵌入式数据库比如H2，HSQL,and Derby。这些数据库只需要提供jar包就能在内存中维护
数据。我们这章访问关系型数据库。

## 4.1 DataSource

通常项目中使用MySQL,Oracle,PostgreSQL等大型关系数据库。Java中的jdbc技术支持了多种关系型数据库的访问。在代码中访问数据库，我们需要知道数据库程序所在的ip，端口，访问数据库的用户名和密码以及数据库的类型信息。以上信息用来初始化数据源，数据源也就是DataSource。数据源表示数据的来源，从某个ip上的数据库能够获取数据。javax.sql.DataSource 接口表示数据源，提供了标准的方法获取与数据库绑定的连接对象(Connection）。

javax.sql.Connection 是连接对象，在Connection上能够从程序代码发送查询命令，更新数据的语句给数据库;同时从Connection获取命令的执行结果。Connection很重要，像一个电话线把应用程序和数据库连接起来。

DataSource 在 application 配置文件中以 spring.datasource.* 作为配置项。类似下面代码：

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
spring.datasource.username=root
spring.datasource.password=051727
```

DataSourceProperties.java 是数据源的配置类，更多配置参考这个类的属性：

![image-20231001215443562](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231001215443562.png)

Spring Boot 支持多种数据库连接池，优先使用 HikariCP，其次是Tomcat pooling，再次是Cgmmons DBCP2,如果以上都没有，最后会使用Oracle UCP连接池。当项目中starter依赖了spring-boot-starter-jabc或者spring-boot-starter-data-jpa 默认添加HikariCP 连接池依赖，也就是默认使用HikariCP连接池。

## 4.2 轻量的JdbcTemplate

使用JdbcTemplate 我们提供自定义SQL,Spring 执行这些SQL得到记录结果集。JdbcTemplate和NamedParameterJdbcTemplate 类是自动配置的，您可以@Autowire注入到自己的Bean中。开箱即用。

JdbcTemplate 执行完整的SQL语句，我们将SQL语句拼接好，交给JdbcTemplate 执行，JdbcTemplate底层
就是使用JDBC执行SQL语句。是JDBC的封装类而已。

NamedParameterJdbcTemplate 可以在SQL语句部分使用“：命名参数”作为占位符，对参数命名，可读性更好。NamedParameterJdbcTemplate包装了JdbcTemplate对象，“：命名参数”解析后，交给JdbcTemplate 执行SQL语句。

JdbcTemplateAutoConfiguration自动配置了JdbcTemplate 对象，交给 JdbcTemplateConfiguration创建了JdbcTemplate 对象。并对JdbcTemplate 做了简单的初始设置（QueryTimeout，maxRows等）。

### 4.2.1 准备环境	

访问数据库先准备数据库的script。SpringBoot 能够自动执行DDL，DML脚本。两个脚本文件名称默认是schema.sql和data.sql。脚本文件在类路径中自动加载。

自动执行脚本还涉及到spring.sql.init.mode 配置项：

- always：总是执行数据库初始化脚本
- never：禁用数据库初始化

当配置好数据源，创建好sql脚本文件后，在 application 配置文件中添加：spring.sql.init.mode=always，启动程序后，sql脚本文件将会被执行，并生成表。

### 4.2.2 Jdbc Template 访问 MySQL

项目中依赖了spring-jdbc 6.0.3，JdbcTemplate 对象会自动创建好。把JdbcTemplate 对象注入给你的Bean,
再调用JdbcTemplate的方法执行查询，更新，删除的SQL。

JdbcTemplate上手快，功能非常强大。提供了丰富、实用的方法，归纳起来主要有以下几种类型的方法：

​	（1) execute方法：可以用于执行任何SQL语句，常用来执行DDL语句。

​	（2) update、batchUpdate方法：用于执行新增、修改与删除等语句。

​	（3) query 和queryForXXX方法：用于执行查询相关的语句。

​	（4）call方法：用于执行数据库存储过程和函数相关的语句。



测试：

```java
@SpringBootTest
class Lession04JdbcApplicationTests {
@Autowired
private JdbcTemplate jdbcTemplate;
    @Test
    void contextLoads() {
        String sql = "select count(*) from article";
        Long count=jdbcTemplate.queryForObject(sql,Long.class);
        System.out.println("文章总数="+count);
    }

}
```

1. 查询单行记录

   ```java
   @Test
   void testQuery() {
       String sql = "select * from article where id=?";
       //BeanPropertyRowMapper 将查询结果集，列名与属性名匹配，
       Acticle acticle = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<>(Acticle.class), 1);
       System.out.println("查询到的文章="+acticle);
       
   }
   ```

2. 查询多行记录

   ```java
   @Test
   void testList(){
       String sql = "select * from articleorder by id";
       List<Map<String,Acticle>> mapList = jdbcTemplate.queryForList(sql);
       mapList.forEach(e1->{
           e1.forEach((field,value)->{
               System.out.println("字段名称："+field+",列名："+value);
           });
           System.out.println("+++++++++++++++++++++++++++");
       });
   }
   ```

3. 更新操作

   ```java
   @Test
   void testUpdate(){
       String sql ="update article set title=? where id=?";
       int update = jdbcTemplate.update(sql,"Java 核心技术",2);
       System.out.println("更新的记录"+update);
   }
   ```

### 4.2.3 NamedParameterJdbcTemplate 

NamedParameterJdbcTemplate 能够接受命名的参数，通过具名的参数提供代码的可读性，JdbcTemplate 使用
的是参数索引的方式。

在使用模板的位置注入NamedParameterJdbcTemplate对象，编写SQL语句，在SQL中WHERE部分“：命名参数”。调用NamedParameterJdbcTemplate的诸如query，queryForObject,execute,update等时，将参数封装到Map中。

1. 注入模板对象

   ```java
       @Resource
       private NamedParameterJdbcTemplate namedParameterJdbcTemplate;
   ```

2. 使用命名参数

   ```java
   @Test
   void testNameQuery(){
       // : 参数名
       String sql ="select count(*) as ct from article where user_id=:uid and read_count > :num";
       //key是命名参数
       HashMap<String, Object> param = new HashMap<>();
       param.put("uid",2123);
       param.put("num",0);
       Long aLong = namedParameterJdbcTemplate.queryForObject(sql, param, Long.class);
       System.out.println("aLong="+aLong);
   }
   ```

### 4.2.4 多表查询

   多表查询关注是查询结果如何映射为Java Object。常用两种方案：一种是将查询结果转为Map。列名是key,列值是value，这种方式比较通用，适合查询任何表。第二种是根据查询结果中包含的列，创建相对的实体类。属性和查询结果的列对应。将查询结果自定义RowMapper、ResultSetExtractor 映射为实体类对象。

## 4.3 MyBatis

数据库访问MyBatis,MyBatis-Plus 国内很常用，掌握了MyBatis, MyBatis-Plus 就会了大部分了。MyBatis-Plus附加的功能需要单独学习。我们以MyBatis 来自介绍Spring Boot集成ORM框架。

MyBatis 使用最多的是mapper xml文件编写SQL语句。本章使用MyBatis的注解，JDK新特性文本块，以及Record完成java对象和表数据的处理。

### 4.3.1 单表CRUD

1. 新建实体类

   ```java
   public class Student {
       private Long id;
       private Integer age;
       private String name;
       private Double height;
       private Data brith;
       private Character sex;
       //getter and setter 
       //toString
   }
   ```

2. 新建mapper接口

   ```java
   package com.fengshun.jdbc.mapper;
   
   import com.fengshun.jdbc.pojo.Student;
   import org.apache.ibatis.annotations.*;
   
   @Mapper
   public interface StudentMapper {
       // 根据id查询
       @Select("""
               select id,name,height,brith,sex from t_student where id=#{id}
               """)
       @Results(id = "BaseStudentMap", value = {
               @Result(id = true, column = "id", property = "id"),
               @Result(column = "age", property = "age"),
               @Result(column = "name", property = "name"),
               @Result(column = "height", property = "height"),
               @Result(column = "brith", property = "brith"),
               @Result(column = "sex", property = "sex")
       })
       Student selectById(Long id);
       //添加
       @Insert("""
               insert into t_student(age,name,height,brith,sex) values(#{age},#{name},#{height},#{brith},#{sex})
               """)
       int insertStudent(Student student);
   
       //修改
       @Update("""
               update t_student set age=#{age},name=#{name},height=#{height},brith=#{brith},sex=#{sex} where id=#{id}
               """)
       int updateStudent(Student student);
       //删除
       @Delete("""
              
              delete from t_student where id=#{id}
               """)
       int deleteStudent(Long id);
   }
   
   ```

3. 测试

   ```java
   package com.fengshun.jdbc.mapper;
   
   import com.fengshun.jdbc.pojo.Student;
   import jakarta.annotation.Resource;
   import org.junit.jupiter.api.Test;
   import org.springframework.boot.test.context.SpringBootTest;
   
   import java.text.SimpleDateFormat;
   import java.util.Date;
   
   import static org.junit.jupiter.api.Assertions.*;
   
   @SpringBootTest
   class StudentMapperTest {
   
       private SimpleDateFormat simpleDateFormat= new SimpleDateFormat("yyyy-MM-dd");
       @Resource
       private StudentMapper studentMapper;
   
       @Test
       void testSelectById() {
           Student student = studentMapper.selectById(1L);
           System.out.println(student);
       }
   
       @Test
       void TestInsert() {
           Student student = new Student(41, "哈市", 1.85, new Date(), '男');
           int i = studentMapper.insertStudent(student);
           System.out.println(i);
       }
   
       @Test
       void testUpdate() throws Exception{
           Student student = new Student(23, "应于", 1.14,simpleDateFormat.parse("1993-11-22"), '男');
           student.setId(10L);
           int i = studentMapper.updateStudent(student);
           System.out.println(1);
       }
   
       @Test
       void testDelete() {
           int i = studentMapper.deleteStudent(12L);
           System.out.println(i);
       }
   }
   
   ```

### 4.3.2 mybatis常用设置

1. **开启驼峰命名**：

   ```properties
   mybatis.configuration.map-underscore-to-camel-case=true
   ```

2. **指定别名包**：

   ~~~properties
   mybatis.type-aliases-package=com.fengshun.pojo
   ~~~

3. **结果集映射**

   在xml配置文件中使用 < ResultMap> 处理 响应结果

   使用全注解开发形式：

   ```java
   @Select("""
           select id,name,height,brith,sex from t_student where id=#{id}
           """)
   @Results(id = "BaseStudentMap", value = {
           @Result(id = true, column = "id", property = "id"),
           @Result(column = "age", property = "age"),
           @Result(column = "nam e", property = "name"),
           @Result(column = "height", property = "height"),
           @Result(column = "brith", property = "brith"),
           @Result(column = "sex", property = "sex")
   })
   Student selectById(Long id);
   ```

   使用 @ResultMap 注解 复用之前写好的结果集映射

   ~~~java
   @Select("""
           select id,name,height,brith,sex from t_student
          """)
   @ResultMap(value="BaseStudentMap")
   List<Student> selectAll();
   ~~~

   半注解的开发形式：

   ​	在xml 文件中指定 ResultMap，在代码中使用：

    1. 添加mapper配置文件

       ```xml
       <?xml version="1.0" encoding="UTF-8" ?>
       <!DOCTYPE mapper
               PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
               "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
       <mapper namespace="com.fengshun.jdbc.mapper.StudentMapper">
           <resultMap id="BaseStudentMapper" type="com.fengshun.jdbc.mapper.StudentMapper">
               <id column="id" property="id"/>
               <result column="name" property="name"/>
               <result column="age" property="age"/>
               <result column="height" property="height"/>
               <result column="brith" property="brith"/>
               <result column="sex" property="sex"/>
           </resultMap>
       </mapper>
       ```

    2. 在 application 配置文件中指定mapper文件的位置

       ```properties
       # 类路径下的mapper文件下的所有包下的所有xml结尾的文件
       mybatis.mapper-locations=classpath:mapper/**/*.xml 
       ```

    3. 使用 @ResultMapper 注解

       ```java
       @Select("""
           select id,name,height,brith,sex from t_student
          """)
       @ResultMap(value = "baseStudentMapper")
       List<Student> selectAll();
       ```

    4. 测试：

       ```java
       @Test
       void testSelectAll(){
           List<Student> students = studentMapper.selectAll();
           students.forEach(student -> System.out.println(student));
       }
       ```

4. **配置MyBatis日志**

   ```properties
   mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
   ```

5. **获取自动生成的主键**

   - 在原生MyBatis中，通过在 Insert 标签中添加属性，useGeneratedKeys="true"  表示：使用自动生成的主键值。

   keyProperty="id"  表示：指定主键值赋值给对象的哪个属性，这个就表示将主键值赋值给id属性。

   ```xml
   <insert id="insertCarUsegeneratedKey" useGeneratedKeys="true" keyProperty="id">
       insert into t_car(id,car_num,brand,guide_price,produce_time,car_type)
       value(null,#{car_num},#{brand},#{guide_price},#{produce_time},#{car_type})
   </insert>
   ```

   在SpringBoot中使用注解：
   ~~~java
   @Options(useGeneratedKeys=true,keyColumn="id",keyProperty="id")
   //表示将keyColumn 的值传给keyProperty，查询出来的id值会自动赋值给实体类中
   ~~~

   

### 4.3.3 SQL 提供者

我们能在方法上面直接编写SQL语句。使用Text Block编写长的语句。方法上编写SQL显的不够简洁。MyBatis 提供了SQL提供者的功能。将SQL以方法的形式定义在单独的类中。Mapper 接口通过引用SQL提供者中的方法名称，表示要执行的SQL。

SQL 提供者有四类@SelectProvider,@InsertProvider,@UpdateProvider,@DeleteProvider。

SOL提供者首先创建提供者类，自定义的。类中声明静态方法，方法体是SOL语句并返回SOL。例如：

~~~java
public static String selectByid(){
    return "select id,name,height,brith,sex from t_student";
}
~~~

其次Mapper接口的方法上面，应用  @SelectProvider( type= 提供类.class , method=" 方法名称 ")

1. 定义提供者类：

   ```java
   package com.fengshun.jdbc.provider;
   
   public class SqlProvider {
       // 查询
       public static String selectStudent(){
           return "select name,height,brith,sex from t_student where id=#{id}";
       }
       // 更新
       public static String updateStudent(){
           return " update t_student set age=#{age},name=#{name},height=#{height},brith=#{brith},sex=#{sex} where id=#{id}";
       }
   }
   ```

2. 使用提供者

   ```java
   @Results(id = "BaseStudentMap", value = {
           @Result(id = true, column = "id", property = "id"),
           @Result(column = "age", property = "age"),
           @Result(column = "name", property = "name"),
           @Result(column = "height", property = "height"),
           @Result(column = "brith", property = "brith"),
           @Result(column = "sex", property = "sex")
   })
   @SelectProvider(type=SqlProvider.class,method="selectStudent")
   Student selectById(Long id);
   ```

### 4.3.4 @One 一对一查询

MyBatis支持一对一，一对多，多对多查询。XML文件和注解都能实现关系的操作。我们使用注解表示article和article_detail的一对一关系。MyBatis维护这个关系，开发人员自己也可以维护这种关系。

@One：一对一

@Many：一对多

1. 新建实体类：Student ，Clazz

   ```java
   public class Student1 {
       private Long sid;
       private String sname;
       private Clazz clazz;
       //getter and setter
       //toString
   }
   public class Clazz {
       private Long cid;
       private String cname;
       private List<Student1> studentList;
       //getter and stter
       //toString
   }
   ```

2. 新建mapper接口

   studentMapper：

   ```java
   package com.fengshun.jdbc.mapper;
   
   import com.fengshun.jdbc.pojo.Student1;
   import org.apache.ibatis.annotations.*;
   import org.apache.ibatis.mapping.FetchType;
   
   @Mapper()
   public interface Student1Mapper {
   
       @Select("""
               select s.sid,s.sname,s.cid
               from t_stu s 
               where s.sid=#{id}
               """)
       @Results(id = "BaseStudent",value = {
               @Result(id = true,property = "sid", column = "sid"),
               @Result(property = "sname",column = "sname"),
               @Result(property = "clazz",column = "cid",one = @One(select = "com.fengshun.jdbc.mapper.ClazzMapper.selectClazzByid",fetchType = FetchType.LAZY))
       })
       Student1 selectById(Long id);
   }
   ```

   ClazzMapper：

   ```java
   package com.fengshun.jdbc.mapper;
   
   import com.fengshun.jdbc.pojo.Clazz;
   import org.apache.ibatis.annotations.Mapper;
   import org.apache.ibatis.annotations.Result;
   import org.apache.ibatis.annotations.Results;
   import org.apache.ibatis.annotations.Select;
   
   @Mapper
   public interface ClazzMapper {
   
       @Select("""
               select cid,cname from t_clazz where cid=#{cid}
               """)
       @Results(id = "BaseClazz",value = {
               @Result(id = true,property = "cid",column = "cid"),
               @Result(property = "cname",column = "cname")
       })
       Clazz selectClazzByid(Long cid);
   }
   ```

3. 编写测试类：

   ```java
   @SpringBootTest
   class Student1MapperTest {
   @Resource
   private Student1Mapper student1Mapper;
   
       @Test
       void testSelectById() {
           Student1 student1 = student1Mapper.selectById(2L);
           System.out.println(student1.getSname());
           System.out.println(student1.getSid());
           System.out.println(student1.getClazz());
       }
   }
   ```

**原理**：

> 一对一查询，先分步查询，即查询出Student信息，后查询Clazz信息
>
> 在StudentMapper类中的 selectById 上添加 @Results 注解。
>
> ~~~java
>  @Result(property = "clazz",column = "cid",one = @One(select = "com.fengshun.jdbc.mapper.ClazzMapper.selectClazzByid",fetchType = FetchType.LAZY))
> property = "clazz" 表示 Student 实体类中的 clazz属性。
> column = "cid" 表示从Student 表中查询出 cid 并传递给 com.fengshun.jdbc.mapper.ClazzMapper.selectClazzByid 
> @One(select） select 属性的值，必须是全限定方法名，即：包名+类名+方法名
> fetchType = FetchType.LAZY 表示启用懒加载，需要用到clazz时才查询。
> 
> ~~~
>
> 

### 4.3.5 @Many 一对多查询

一对多查询使用@Many注解，步骤与一对一基本相同

1. 新建实体类：Student ，Clazz

   ```java
   public class Student1 {
       private Long sid;
       private String sname;
   
       //getter and setter
       //toString
   }
   public class Clazz {
       private Long cid;
       private String cname;
       private List<Student1> studentList;
       //getter and stter
       //toString
   }
   ```

2. 编写mapper接口

   Student：

   ```java
   //查询某个班级的所有学生信息
   @Select("""
           select sid,sname
           from t_stu  
           where cid=#{cid}
           """)
   @Results(id = "BaseStudentByCid",value = {
           @Result(id = true,property = "sid", column = "sid"),
           @Result(property = "sname",column = "sname")
   })
   List<Student1> selectStudentByCid(Long cid);
   ```

   Clazz：

   ```java
   @Select("""
           select cid,cname from t_clazz where cid=#{cid}
           """)
   @Results(value = {
           @Result(id = true, property = "cid", column = "cid"),
           @Result(property = "cname", column = "cname"),
           @Result(property = "studentList", column = "cid",
                   many = @Many(select = "com.fengshun.jdbc.mapper.Student1Mapper.selectStudentByCid",
                           fetchType = FetchType.LAZY))
   
   })
   Clazz selectClazztoStudent(Long cid);
   ```

3. 测试

   ```java
   @SpringBootTest
   class ClazzMapperTest {
       
   @Resource
   private ClazzMapper clazzMapper;
       
       @Test
       void selectClazztoStudent() {
           Clazz clazz = clazzMapper.selectClazztoStudent(1001L);
           System.out.println(clazz.getCid());
           System.out.println(clazz.getStudentList());
       }
   }
   ```

## 4.4 声明式事物

事务分为全局事务与本地事务，本地事务是特定于资源的，例如与JDBC连接关联的事务。本地事务可能更容易使用，但有一个显著的缺点：它们不能跨多个事务资源工作。比如在方法中处理连接多个数据库的事务，本地事务是无效的。

Spring 解决了全局和本地事务的缺点。它允许应用程序开发人员在任何环境中使用一致的编程模型。只需编写一次代码，就可以从不同环境中的不同事务管理策略中获益。Spring框架同时提供声明式和编程式事务管理。推荐声明式事务管理。

Spring 事务抽象的关键是事务策略的概念，org.springframework.transaction.PlatformTransactionManager 接口定义了事务的策略。

**事务控制的属性**：

- Propagation：传播行为。代码可以继续在现有事务中运行（常见情况），也可以暂停现有事务并创建新事务
- Isolation：隔离级别。此事务与其他事务的工作隔离的程度。例如，这个事务能看到其他事务未提交的写吗？
- Timeout 超时时间：该事务在超时和被底层事务基础结构自动回滚之前运行的时间。
- Read-only 只读状态：当代码读取但不修改数据时，可以使用只读事务。在某些情况下，例如使用Hibernate时，只读事务可能是一种有用的优化。

**AOP**:

Spring Framework的声明式事务管理是通过Spring 面向方面编程(AOP)实现的。事务方面的代码以样板的方式使用，及时不了解AOP概念，仍然可以有效地使用这些代码。事务使用AOP的环绕通知(TransactionInterceptor）。

**声明式事务的两种方式**：

- XML配置文件：全局配置
- @Transactional注解驱动：和代码一起提供，比较直观。和代码的耦合比较高。【Spring 团队建议您只使用@Transactional注释具体类（以及具体类的方法)，而不是注释接口。当然，可以将@Transactional注解放在接口（或接口方法）上，但这只有在使用基于接口的代理时才能正常工作】

**方法的可见性**：

​	公共(public)方法应用@Transactional主机。如果使用@Transactional注释了受保护的、私有的或包可见的方法，则不会引发错误，但注释的方法不会显示配置的事务设置，事务不生效。如果需要受保护的、私有的方法具有事务考虑使用 AspectJ。而不是基于代理的机制。

### 4.4.1 使用事物注解

```java
@Service("clazzService")
@Transactional
public class ClazzService {
    @Resource
    private Student1Mapper student1Mapper;
    @Resource
    private ClazzMapper clazzMapper;

    int save(Student1 student, Clazz clazz){
        int i = clazzMapper.insertClazz(clazz);
        student.setCid(clazz.getCid());
        i+=student1Mapper.insertStudent(student);
        return i;
    }
}
```

@Transactional 注解可以作用在类上也可以作用在方法上

```java
@MapperScan(basePackages = {"com.fengshun.jdbc.mapper"}) //mapper包 扫描
@ComponentScan(basePackages = {"com.fengshun.jdbc.service"})
@EnableTransactionManagement
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

@EnableTransactionManagement 注解作用在启动类上，是可选的

### 4.4.2 无效事务

无效事务1：Spring事物处理是AOP的环绕通知,只要通过代理对象调用具有事物的方法才能生效。类中有A方法，调用带有事物的B方法。调用A方法事物无效。当然 protected，private 方法默认是没有事物功能的。

无效事务2：方法在线程中运行的，在同一线程中方法具有事物功能，新的线程中的代码事物无效。简单来讲，在一个线程中开启了事物，而在该线程中开启了新的线程，新的线程中的事物是无效的。

### 4.4.3 事物回滚规则

- RuntimeException的实例或子类时回滚事务
- Error 会导致回滚
- 已检查异常不会回滚。默认提交事务



DTransactional 注解的属性控制回滚

1. rollbackFor

2. rollbackForClassName

   ```java
   //表示出现Io异常就回滚事物
   @Transactional(rollbackFor = IOException.class,rollbackForClassName = "java.io.IOException")
   ```

3. noRollbackFor

4. noRollbackForClassName

   ```
   //表示出现IO异常时，不回滚事务	
   @Transactional(noRollbackFor = IOException.class,noRollbackForClassName = "java.io.IOException")
   ```

# 第五章 Web 服务

基于浏览器的B/S结构应用十分流行。Spring Boot 非常适合Web应用开发。可以使用嵌入式Tomcat、Jetty、Undertow 或Netty 创建一个自包含的HTTP服务器。一个Spring Boot的Web应用能够自己独立运行，不依赖需要安装的Tomcat，Jetty等。

Spring Boot 可以创建两种类型的Web应用

- 基于Servlet 体系的Spring WebMVC应用
- 使用spring-boot-starter-webflux 模块来构建响应式，非阻塞的Web应用程序

Spring WebFlux 是单独一个体系的内容，其他课程来说。当前文档讲解Spring Web MVC。又被称为“Spring MVC”。Spring MVC 是“model view controller”的框架。专注web应用开发。我们快速的创建控制器(Controller),接受来自浏览器或其他客户端的请求。并将业务代码的处理结果返回给请求方。

SpringMVC 处理请求：

![image-20231003144012728](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231003144012728.png)

## 5.1  高效构建Web应用

创建web应用，lession05-we。依赖选择 spring-web 包含了SpringMVC，Restful，Tomcat 这些功能。在选择 Thymeleaf（视图技术，代替jsp），Lombok 依赖。

![image-20231003144625296](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231003144625296.png)

编译Controller：

```java
@Controller
public class QuickController {
    @RequestMapping("/quick")
    public String quick(Model model){
        // 业务处理结果数据，放到model模型中
        //model 表示模型，存储数据，这个数据最后是放在request作用域中
        model.addAttribute("title","web开发");
        model.addAttribute("time", LocalDateTime.now());
        return "quick";
    }
}
```

添加相应页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div style="margin-left: 200px">
    <h3>视图技术</h3>
    <div th:text="${title}"></div>
    <div th:text="${time}"></div>
</div>
</body>
</html>
```

Thymeleaf 能代替jsp

### 5.1.2 JSON 视图

上面的例子以Html文件作为视图，可以编写复杂的交互的页面，CSS美化数据。除了带有页面的数据，还有一种只需要数据的视图。比如手机应用app，app的数据来自服务器应用处理结果。app内的数据显示和服务器无关，只需要数据就可以了。主流方式是服务器返回json格式数据给手机app应用。我们可以通过原始的HttpServletResponse 应该数据给请求方。借助Spring MVC能够无感知的处理json。

```java
@RequestMapping("/toJSON")
@ResponseBody
public String toJSON(){
    return "返回JSON数据";
}
```

### 5.1.3 给项目添加 favicon

什么是 favicon.ico 

​	 favicon.ico 是网站的缩略标志，可以显示在浏览器标签、地址栏左边和收藏夹，是展示网站个性的logo标志。

![image-20231003175250608](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231003175250608.png)

我们自己的网站定制logo.首先找一个在线工具创建favicon.ico.比如https://quanxin.org/favicon，用文字,图片生成我们需要的内容。生成的logo 文件名称是favicon.ico

step1：将生成的 favicon.ico拷贝项目的resources/或resources/static/目录。

step2：在你的视图文件，加入对favicon.ico的引用。

在视图的<head>部分加入 < link rel="icon" href="../favicon.ico" type="image/x-icon"/>

## 5.2 SPringMVC

Spring MVC是非常著名的Web应用框架，现在的大多数Web项目都采用SpringMVC。它与Spring有着紧密的关系。是Spring 框架中的模块，专注Web应用，能够使用Spring 提供的强大功能，IoC，Aop等等。

Spring MVC框架是底层是基于Servlet 技术。遵循Servlet规范，Web组件Servlet，Filter,Listener 在 SpringMVC中都能使用。同时Spring MVC也是基于MVC架构模式的，职责分离，每个组件只负责自己的功能，组件解耦。学习Spring MVC首先具备 Servlet的知识，关注MVC架构的M、V、C在 Spring MVC框架中的实现。掌握了这些就能熟练的开发Web应用。

Spring Boot的自动配置、按约定编程极大简化，提高了Web应用的开发效率

### 5.2.1 控制器Controller

控制器一种有Spring管理的Bean对象，赋予角色是“控制器”。作用是处理请求，接收浏览器发送过来的
参数，将数据和视图应答给浏览器或者客户端app等。

控制器是一个普通的Bean，使用@Controller 或者@RestController 注释。@Controller 被声明为@Component。
所以他就是一个Bean 对象。源代码如下：

![image-20231003211955139](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231003211955139.png)

#### 5.2.1.1 匹配请求路径带控制器方法

URL=协议+域名+端口号+URi

SpringMVC 支持多种策略，匹配请求路径到控制器方法。 AntPathMatcher 、PathPatternParser

![image-20231003212953237](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231003212953237.png)

从 SpringBoot3 推荐使用 PathPatternParser 策略。比之前 AntPathMatcher 提示6-8倍吞吐量。
我们看一下PathPatternParser中有关 uri 的定义

通配符：

- ？：一个字符
- \* ：0或多个字符。在一个请求路径段中匹配字符
- \*\*：匹配0个或多个路径段，相当于是所有
- 正则表达式：支持正则表达式

RESTFul 风格的支持路径变量 

- {变量名}
- {myname:[a-z]+}：正则表达式 a-z的多个字面，“myname” 是自定义的路径变量名称。通过使用 @PathVariable(“myname”) 注解获取myname的值，“+” 表示一个或者多个字符
- {*myname}：匹配多个路径一直到uri的结尾

例子：

~~~java
@GetMapping("/file/t?st.html")
//匹配只有一个字符
//例如：http://localhost:8080/file/test.html
// 非：http://localhost:8080/file/teest.html（×）

~~~

~~~java
@GetMapping("/file/*.gif")
// *:表示匹配一个字段中0或多个字符
/* 例如：
		1.http://localhost:8080/file/user.gif
		2.http://localhost:8080/file/cat.gif
		3.http://localhost:8080/file/.gif
	http://localhost:8080/file/use/cat.gif	（×）
*/
~~~

~~~java
@GetMapping("/pic/**")
// **: 表示0或多段路径，匹配 /pic 开始的所有请求
/* 例如：
		1.http://localhost:8080/pic/p1.gif
		2.http://localhost:8080/pic/user
		3.http://localhost:8080/pic
*/
~~~

RESTFul

1. {*myname}：匹配多个路径一直到uri的结尾

   ~~~java
   @GetMapping("/order/{*id}")
   /*
   	{*id} 匹配 /order 开始的所有请求，id表示order 后面直到路径末尾的所有内容。
   	id自定义路径变量名称。与 @PathVariable 一样使用
   	
   	例如：
   		1.http://localhost:8080/order/1001    id=/1001
   		2.http://localhost:8080/order/1001/2022-05-44   id=/1001/2022-05-44
   */
   @GetMapping("/order/{*id}")
   public String path4(@PathVariable("id") String orderId,HttpServletRequest request){
       return "/order/{*id} 请求"+request.getRequestURI()+",id="+orderId;
   }
   //注意：@GetMapping("/order/{*id}/{*date}") 无效。 {*..} 后面不能再有匹配规则
   ~~~

2. {myname:[a-z]+}：正则表达式

   ~~~java
   @GetMapping("/pages/{fname:\\w+}.log")
   //  :\\w+ 正则匹配，xxx.log  w表示的某个普通字符  +表示一个或者多个字符
   	uri需要以.log结尾
   
   /*
   例如：
   		1.http://localhost:8080/pages/req.log   fname=req
   		2.http://localhost:8080/pages/111.log	faname=111
   	http://localhost:8080/pages/req.txt （×）
   	http://localhost:8080/pages/###.log （×）
   */
   @GetMapping("/pages/{fname:\\w+}.log")
   public String path4(@PathVariable(“fname”) String fname,HttpServletRequest request){
           return "/pages/{fname:\\w+}.log 请求="+request.getRequestURI()+",fname="+fname;
   }
   ~~~

#### 5.2.1.2 唯一路径

   推荐使用固定的uri

   ```java
   @RequestMapping("/test/quick")
   public String quick(Model model){}
   ```

这种方式的请求路径与方法一一对应，不至于请求路径重复或相似时执行多个方法。

#### 5.2.1.3 @RequestMapping

@RequestMapping：用于将 web请求映射到控制器类的方法。此方法处理请求。可用在类上或方法上。在类和方法同时组合使用。

​	重要的属性

- value：别名path 表示请求的uri，在类和方法方法同时使用value，方法上的继承类上的value值。
- method：请求方式，支持 GET,POST,HEAD,OPTIONS,PUT,PATCH,DELETE,TRACE。值为：RequestMethod[]methodO)，RequestMethod是enum类型。

​	快捷注解

- @GetMapping:表示get 请求方式的@RequestMapping
- @PostMapping:表示 post请求方式的@RequestMapping
- @PutMapping：表示put 请求方式的@RequestMapping
- @DeleteMapping：表示delete 请求方式的@RequestMapping

对于请求方式 get，post，put, delete 等能够HttpMethod表示，Spring Boot3 之前他是enum,Spring Boot3
他是class

public enum HttpMethod								Spring Boot3之前他是enu

public final class HttpMethod 						Spring Boot3 他是class

#### 5.2.1.4 控制器方法参数类型与可用返回值类型

![image-20231003225918595](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231003225918595.png)

![image-20231003225927026](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231003225927026.png)

#### 5.2.1.5 接受请求参数

用户在浏览器中点击按钮时，会发送一个请求给服务器，其中包含让服务器程序需要做什么的参数。这些参数发送给控制器方法。控制器方法的形参列表接受请求参数。

接受参数方式：

- 请求参数与形参 一 一对应，适用简单类型。形参可以有合适的数据类型，比如String，Integer，int等。
- 对象类型，控制器方法形参是对象，请求的多个参数名与属性名相对应。
- @RequestParam注解，将查询参数，form表单数据解析到方法参数，解析multipart文件上传。
- @RequestBody，接受前端传递的json格式参数，一般使用post请求
- HttpServletRequest 使用request对象接受参数，request.getParameter(“...")
- @RequestHeader,从请求header中获取某项值

解析参数需要的值，SpringMVC 中专门有个接口来干这个事情，这个接口就是：HandlerMethodArgumentResolver,
中文称呼：处理器方法参数解析器，说白了就是解析请求得到Controller 方法的参数的值。

##### 使用 @RequestHeader 接收参数

获取请求头中 Accept 的参数值

```java
@GetMapping("testRequestHeader")
@ResponseBody
public String testRequestHeader(@RequestHeader(value = "Accept") String accept){
    return "Accept="+accept;
}
```

![image-20231003231914475](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231003231914475.png)

##### 使用@RequestBody接收JSON

前端数据：{" username ":" lisi " , "age" : 20}

@RequestBody ：从请求体中读取json数据，将数据转为形参对象的属性值	

​								框架创建User对象，将username ，age 的值赋值给两个同名的属性。

前端请求中需要指定：请求头中 Content-type ： application/json

1. 创建实体类

   ```java
   public class User {
       private String name;
       private Integer age;
   }
   ```

2. 创建控制器方法：

   ```java
   @PostMapping("testRequestBody")
   public String testRequestBody(@RequestBody User user){
       System.out.println("接收的json："+user.toString());
       return "json转为User对象"+user.toString();
   }
   ```

##### Read-InputStream

Reader 或者 InputStream 读取请求体的数据，适合post请求体的各种数据。具有广泛性。

创建新的控制器方法：

~~~java
    @PostMapping("testReeder")
    public String testReeder(Reader in) {
        StringBuffer content = new StringBuffer("");
        try(BufferedReader bufferedReader = new BufferedReader(in)) {
            String line = null;
            while ((line=bufferedReader.readLine())!=null){
                content.append(line);
            }
        }catch (IOException e){
            e.printStackTrace();
        }
        return "reader="+content.toString();
    }
~~~

##### 数组参数接收多个值

```java
@GetMapping("testArray")
public String testArray(Integer[] id){
    List<Integer> integers = Arrays.stream(id).toList();
    return integers.toString();
}

/*
	测试请求
			1.http://localhost:8080/testArray?id=1&id=2&id=3
			2.http://localhost:8080/testArray?id=1,2,3,4

*/
```

#### 5.2.1.6 验证参数

服务器端程序，Controller 在方法接受了参数，这些参数是由用户提供的，使用之前必须校验参数是我们需要的吗，值是否在允许的范围内，是否符合业务的要求。比如年龄不能是负数，姓名不能是空字符串，email必须有@符号，phone国内的11位才可以。

验证参数

- 编写代码，手工验证，主要是i语句，switch等等。
- Java Bean Validation： JSR-303是JAVAEE6中的一项子规范，叫做 Bean Validation，是一个运行时的数据
- 验证规范，为JavaBean 验证定义了相应的元数据模型和API。

##### Java Bean Validation

Spring Boot 使用Java Bean Validation验证域模型属性值是否符合预期，如果验证失败，立即返回错误信息。Java Bean Validation 将验证规则从controller,service 集中到Bean对象。一个地方控制所有的验证。

Bean的属性上，加入JSR-303的注解，实现验证规则的定义。JSR-343是规范，hibernate-validator 是实现。

JSR-303: https://beanvalidation.org/,最新 3.0版本，2020年10.

hibernate-validator: https://hibernate.org/validator/

​									https://docs.jboss.org/hibernate/validator/4.2/reference/en-US/html/

##### JSR-303 注解

![image-20231004174525825](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231004174525825.png)

![image-20231004174537185](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231004174537185.png)

##### 快速上手

验证Blog中的文章信息。用户提交文章给Controller，控制器使用Java Object 接收参数，给Bean添加约束注解，验证文章数据。

1. 添加Bean Validator Starter 依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-validation</artifactId>
   </dependency>
   ```

2. 创建文章数据类，添加约束注解

   ```java
   package com.fengshun.web.pojo;
   
   import jakarta.validation.constraints.*;
   
   public class Article {
       // 文章主键
       private Integer id;
       @NotNull(message = "必须要有作者")
       private Integer userId;
   
       // 同一个属性可以指定多个注解
       
       //标题
       @NotBlank(message = "文章必须有标题")
       // @Size中null认为是有效值，所以需要@NotBlank
       @Size(min = 3, max = 30, message = "标题必须3个字以上")
       private String title;
       //副标题
       @NotBlank(message = "文章必须有副标题")
       @Size(min = 8,max=60,message = "副标题必须30字以上")
       private String summary;
       //访问数量
       @DecimalMin(value = "0",message = "已读最小是0")
       private Integer readCount;
       //邮箱
       @Email(message = "邮箱格式不正确")
       private String email;A
           
           
           //getter and setter
           //toString
   }
   ```

3. Controller使用Bean

   ```java
   package com.fengshun.web.controller;
   
   import com.fengshun.web.pojo.Article;
   import org.springframework.validation.BindingResult;
   import org.springframework.validation.FieldError;
   import org.springframework.validation.annotation.Validated;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;
   
   import java.util.HashMap;
   import java.util.List;
   import java.util.Map;
   
   @RestController
   public class ArticleController {
   
       // 发布新文章
       @PostMapping("/addArticle")
       public Map<String ,Object> addArticle(@Validated @RequestBody Article article, BindingResult br){
           // service 方法处理文章的业务
   
           // 返回结果数据
           Map<String ,Object> map = new HashMap<>();
           if (br.hasErrors()) { //如果有错误则执行以下代码
               List<FieldError> fieldErrors = br.getFieldErrors(); //获取错误列表
               //一个字段可以有多个错误
               for (int i = 0; i < fieldErrors.size(); i++) {
                   FieldError field = fieldErrors.get(i);
                   map.put(i+"-"+field.getField(),field.getDefaultMessage());
               }
           }
           return map;
       }
   }
   ```

   注意：BindingResult 用于接受错误

测试：

```http
POST localhost:8080/addArticle
Content-Type:application/json

{
  "id": 0,
  "userId": 10,
  "title": "",
  "summary": "云原生SpringCloud，Liux",
  "readCount": 1,
  "email": "abc@163.com"
}

```

结果：![](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231004212207697.png)

##### 分组校验

上面的Ariticle用来新增文章，新的文章主键id是系统生成的。现在要修改文章，比如修改某个文章的title,summary， readCount，email 等。此时id必须有值，修改这个id的文章。 新增和修改操作对id有不同的要求约束要求。通过 group来区分是否验证。

把验证规则分成某一个组，当启用某一个组的时候，使用对应的验证规则，例如增加文章的组中不验证id值，在编辑修改文章的组中验证ID。

group 是Class作为表示，java中包加类一定是唯一的，这个标识没有其他实际意义

1. 添加group标志

   ```java
   public class Article {
       // 新增组
       public static interface AddArticleGroup { }
       
       // 编辑修改组
       public static interface EditArticleGroup { }
       
       // 文章主键
       @NotNull(message = "文章ID不能为空", groups = {EditArticleGroup.class})
       @Min(value = 1, message = "文章ID从1开始", groups = {EditArticleGroup.class})
       private Integer id;
       @NotNull(message = "必须要有作者", groups = {AddArticleGroup.class, EditArticleGroup.class})
       private Integer userId;
   
       // 同一个属性可以指定多个注解
   
       // 标题
       @NotBlank(message = "文章必须有标题", groups = {AddArticleGroup.class, EditArticleGroup.class})
       // @Size中null认为是有效值，所以需要@NotBlank
       @Size(min = 3, max = 30, message = "标题必须3个字以上", groups = {AddArticleGroup.class, EditArticleGroup.class})
       private String title;
       // 副标题
       @NotBlank(message = "文章必须有副标题", groups = {AddArticleGroup.class, EditArticleGroup.class})
       @Size(min = 8, max = 60, message = "副标题必须30字以上", groups = {AddArticleGroup.class, EditArticleGroup.class})
       private String summary;
       // 访问数量
       @DecimalMin(value = "0", message = "已读最小是0", groups = {AddArticleGroup.class, EditArticleGroup.class})
       private Integer readCount;
       // 邮箱
       @Email(message = "邮箱格式不正确", groups = {AddArticleGroup.class, EditArticleGroup.class})
       private String email;
       
       //getter and setter
        //toString
   }
   ```

2. 在Controller方法中的 @Validated 注解中指定组的名字

   ![image-20231004213649537](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231004213649537.png)

### 5.2.2 模型 Model

![image-20231004214051147](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231004214051147.png)

### 5.2.3 视图View

Spring MVC中的View（视图）用于展示数据的，视图技术的使用是可插拔的。无论您决定使用thymleaf、jsp 还是其他技术，classpath有jar 就能使用视图了。开发者主要就是配置更改。Spring Boot3不推荐FreeMarker、jsp 这些了。页面的视图技术Thymeleaf，Groovy Templates。

org.springframework.web.servlet.View 视图的接口，实现此接口的都是视图类，视图类作为Bean 被Spring管理。当然这些不需要开发者编写代码。

ThymeleafView：使用thymeleaf视图技术时的，视图类。

InternalResourceView：表示jsp的视图类。

控制器方法返回值和视图有是关系的。

String：如果项目中有thymeleaf，这个String 表示xxx.html视图文件（/resources目录）

ModelAndView： View 中就是表示视图。

@ResponeBody,@RestController 适合前后端分离项目

String：表示一个字符串数据

Object：如果有Jackson库，将Objet转为json字符串

#### ResponseEntity

 ResponseEntity 包含 HttpStatus code（http状态码） 和 应答数据。因为有Http Code 能表达标准的语义，200表示成功，404表示没有发现等。

1. ResponseEntity 做控制器方法的返回值

   ```java
   @RequestMapping("testResponseEntity")
   @ResponseBody
   public ResponseEntity testResponseEntity(){
       User user = new User("张三",26);
       ResponseEntity responseEntity = new ResponseEntity<>(user, HttpStatus.OK);
       return responseEntity;
   
   }
   ```

2. 测试：

   ~~~http
   POST localhost:8080/testResponseEntity
   ~~~

   ![image-20231004220148193](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231004220148193.png)

响应的状态码为200，如果指定返回的响应状态为 HttpStatus.NOT_FOUND 则浏览器显示404

![image-20231004220430233](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231004220430233.png)

## 5.3 SpringMVC 请求流程

Spring MVC 框架是基于Servlet 技术的。以请求为驱动，围绕Servlet 设计的。 Spring MVC处理用户请求与访问一个Servlet 是类似的，请求发送给Servlet，执行doService方法，最后响应结果给浏览器完成一次请求处理。

### 5.3.1 DispatcherServlet 是一个Servlet

DispatcherServlet 是核心对象，称为中央调度器（前端控制器 Front Controller）。负责接收所有对 Controller的请求，调用开发者的Controller 处理业务逻辑，将Controller方法的返回值经过视图处理响应给浏览器。

DispatcherServlet 作为SpringMVC中的C，职责：

1. 是一个门面，接收请求，控制请求的处理过程。所有请求都必须有DispatcherServlet 控制。SpringMVC对外的入口。可以看做门面设计模式。

2. 访问其他的控制器。这些控制器处理业务逻辑
3. 创建合适的视图，将2中得到业务结果放到视图，响应给用户。
4. 解耦了其他组件，所有组件只与DispatcherServlet交互。彼此之间没有关联
5. 实现 ApplictionContextAware，每个DispatcherServlet 都拥自己的WebApplicationContext，它继承了ApplicationContext。WebApplicationContext包含了Web相关的Bean对象，比如开发人员注释@Controller的类，视图解析器，视图对象等等。DispatcherServlet 访问容器中Bean对象。

## 5.4 SpringMVC 常用配置

1. 设置访问路径

   ```properties
   spring.mvc.servlet.path=/course
   ```

2. 设置Servlet的加载顺序，越小创建时间越早

   ```properties
   spring.mvc.servlet.load-on-startup=0
   ```

3. 设置时间格式，可以在接受请求参数中使用：

   ```properties
   spring.mvc.format.date=yyyy-MM-dd HH:mm:ss
   ```

   如果发出的请求的日期格式是指定的格式，那么可以接收到参数，如果日期格式不匹配，则无法接收到参数，会显示400错误

4. 访问templates目录下的静态网页：

   第一种方式：添加Thymeleaf 依赖

   第二种方式：添加配置：

   ```properties
   spring.web.resources.static-locations=classpath:/templates/
   ```

## 5.5 Servlets, Filters, and Listeners

Web 应用还会用到Servlet、Filter 或Listener。这些对象能够作为Spring Bean注册到嵌入式的Tomcat中。ServletRegistrationBean、FilterRegistrationBean 和 ServletListenerRegistrationBean 控制 Servlet，Filter, Listener。@Order 或Ordered 接口控制对象的先后顺序。

### 5.5.1  Servlets

#### 5.5.1.2 使用注解方式创建Servlet

​	Servlet 现在完全支持注解的使用方式，@WebServlet

1. 定义一个Servlet

   ```java
   package com.fengshun.servletandfilt.web;
   
   import jakarta.servlet.ServletException;
   import jakarta.servlet.annotation.WebServlet;
   import jakarta.servlet.http.HttpServlet;
   import jakarta.servlet.http.HttpServletRequest;
   import jakarta.servlet.http.HttpServletResponse;
   
   import java.io.IOException;
   import java.io.PrintWriter;
   
   @WebServlet(name = "HelloServlet",urlPatterns = "/HelloServlet")
   public class HelloServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          resp.setContentType("text/html;charset=utf-8");
           PrintWriter writer = resp.getWriter();
           writer.println("这是一个SpringBoot中的Servlet");
           writer.flush();
           writer.close();
       }
   }
   ```

2. 在启动类中添加 @ServletComponentScan 注解，指定Servlet所在的包：

   ```java
   @SpringBootApplication
   @ServletComponentScan(basePackages = {"com.fengshun.servletandfilt.web"})
   public class Application {
   }
   ```

#### 5.5.1.2 通过编码方式控制Servlet

在以上代码中，是在主启动类上添加 @ServletComponentScan 注解，而使用编码方式控制Servlet，就不需要添加注解。

需要使用 ServletRegistrationBean 类，

在主启动类中添加方法，方法的返回值类型为 ServletRegistrationBean

1. 重新定义一个Servlet

   ```java
   package com.fengshun.servletandfilt.web;
   
   import jakarta.servlet.ServletException;
   import jakarta.servlet.http.HttpServlet;
   import jakarta.servlet.http.HttpServletRequest;
   import jakarta.servlet.http.HttpServletResponse;
   
   import java.io.IOException;
   import java.io.PrintWriter;
   
   public class LoginServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           resp.setContentType("text/html;charset=utf-8");
           PrintWriter writer = resp.getWriter();
           writer.println("这是一个SpringBoot中的Servlet");
           writer.flush();
           writer.close();
       }
   }
   ```

2. 在主启动类中添加方法

   ```java
       @Bean
       public ServletRegistrationBean addServlet(){
           ServletRegistrationBean bean = new ServletRegistrationBean();
           bean.setServlet(new LoginServlet());
           bean.addUrlMappings("/user/login");
           bean.setLoadOnStartup(1);
           return bean;
       }
   }
   ```

   或者新建配置类，添加 @Configuration注解，并添加配置方法。

### 5.5.2 Filter

Filter 对象使用评率较高，比如记录日志，权限验证，敏感字符过滤等。Web框架中包含内置的Filter，SpringMVC中也包含较多的内置Filter，比如 CommonsRequestLoggingFilter，CorsFilter，FormContentFilter...

#### 5.5.2.1 注解方式创建Filter

 @WebFilter创建 Filter 对象，使用方式和 @WebServlet 一样

1. 创建过滤器

   ```java
   package com.fengshun.servletandfilt.filter;
   
   import jakarta.servlet.*;
   import jakarta.servlet.annotation.WebFilter;
   import jakarta.servlet.http.HttpServletRequest;
   
   import java.io.IOException;
   
   @WebFilter(urlPatterns = "/*")
   public class LoginFilter implements Filter {
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
           String uri = ((HttpServletRequest) servletRequest).getRequestURI().toString();
           System.out.println("过滤器执行了，uri="+uri);
           filterChain.doFilter(servletRequest,servletResponse);
       }
   }
   ```

2. 需要在启动类上添加 @ServletComponentScan 注解

   ```java
   @ServletComponentScan(basePackages = {"com.fengshun.servletandfilt.filter"})
   ```

#### 5.5.2.2 编码方式创建Filter

使用编码方式创建Filter ，定义的Filter 类就不需要使用 @WebFilter 注解以及 @ServletComponentScan 注解。

需要使用 FilterRegistrationBean对象

1. 注册Filter

   ```java
   @Bean
   public FilterRegistrationBean addFilter(){
       FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean();
       filterRegistrationBean.setFilter(new LoginFilter());
       filterRegistrationBean.addUrlPatterns("/*");
       return filterRegistrationBean;
   }
   ```

#### 5.5.2.3 Filter 排序

在项目中有多个Filter，如果多个Filter需要排序，有两种方式：

- 过滤器类名称，按字典顺序排序，AuthFilter -> LogFilter
- FilterRegistrationBean 登记 Filter，设置order顺序，数值越小，先执行。

**使用过滤器名称进行排序**：

应用程序会自动使用过滤器的类名进行排序

1. 创建两个Filter，使用之前定义好的AuthFilter，LogFilter

   ```java
   // AuthFilter
   @WebFilter("/*")
   public class AuthFilter implements Filter {
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           String url = ((HttpServletRequest) servletRequest).getRequestURL().toString();
           System.out.println("AuthFilter 过滤器执行了，url="+url);
           filterChain.doFilter(servletRequest,servletResponse);
       }
   }
   
   //LoginFilter
   @WebFilter("/*")
   public class LoginFilter implements Filter {
   
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           String uri = ((HttpServletRequest) servletRequest).getRequestURI().toString();
           System.out.println("LoginFilter 过滤器执行了，uri="+uri);
           filterChain.doFilter(servletRequest,servletResponse);
       }
   }
   ```

2. 在启动类中添加 @ServletComponentScan 注解

3. 运行结果

   ![image-20231005153035360](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231005153035360.png)

**通过编码方式改变过滤器执行的顺序**

1. 去掉两个过滤器上的 @WebFilter 注解

2. 在配置类中定义方法，注册Filter

   ```java
   @Bean
   public FilterRegistrationBean addFilter1(){
       FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean();
       filterRegistrationBean.setFilter(new LoginFilter());
       filterRegistrationBean.addUrlPatterns("/*");
       filterRegistrationBean.setOrder(2); //设置拦截顺序
       return filterRegistrationBean;
   }
   @Bean
   public FilterRegistrationBean addFilter2(){
       FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean();
       filterRegistrationBean.setFilter(new AuthFilter());
       filterRegistrationBean.addUrlPatterns("/*");
       filterRegistrationBean.setOrder(1); //设置拦截顺序
       return filterRegistrationBean;
   }
   ```

#### 5.5.2.4 使用内置Filter

SpringBoot1中有许多已经定义好的Filter，这些Filter实现了一些功能，如果我们需要使用他们。可以像自己的Filter一样，通过FilterRegistrationBean 注册 Filter 对象。

例如使用CommonsRequestLoggingFilter记录每个请求的日志

1. 注册CommonsRequestLoggingFilter

   ```java
   @Bean
   public FilterRegistrationBean addCommonsRequestLoggingFilter() {
       // 创建 Filter 对象
       CommonsRequestLoggingFilter filter = new CommonsRequestLoggingFilter();
       // 记录请求的uri
       filter.setIncludeQueryString(true);
       // 注册Filter
       FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean();
       filterRegistrationBean.setFilter(filter);
       filterRegistrationBean.addUrlPatterns("/*");
       return filterRegistrationBean;
   }
   ```

2. CommonsRequestLoggingFilter类记录日志时，需要是debug级别，所以设置SpringBoot的日志为debug

   ~~~properties
   logging.level.web=debug
   ~~~

3. 访问url：[localhost:8080/user/login?id=1001&sex=0](http://localhost:8080/user/login?id=1001&sex=0)

   运行结果：

   ![image-20231005154757977](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231005154757977.png)



注意：字符集过滤器（CharacterEncodingFilter）默认是启用的。

### 5.5.3 Listener

@WebListener 注解用于监听器，监听器必须要实现以下接口之一：

- jakarta.servlet.http.HttpSessionAttributeListener
- jakarta.servlet.http.HttpSessionListener
- jakarta.servlet.ServletContextAttributeListener
- jakarta.servlet.ServletContextListener
- jakarta.servlet.ServletRequestAttributeListener
- jakarta.servlet.ServletRequestListener
- jakarta.servlet.http.HttpSessionIdListener

另一种方式用 ServletListenerRegistrationBean 注册Listener对象

1. 创建监听器：

   ```java
   package com.fengshun.servletandfilt.listener;
   
   import jakarta.servlet.annotation.WebListener;
   import jakarta.servlet.http.HttpSessionEvent;
   import jakarta.servlet.http.HttpSessionListener;
   
   @WebListener("Listener的描述说明")
   public class MySessionListener implements HttpSessionListener {
       @Override
       public void sessionCreated(HttpSessionEvent se) {
           HttpSessionListener.super.sessionCreated(se);
       }
   }
   ```

## 5.6 WebMvcConfiguration

WebMvcConfigurer 作为配置类是，采用JavaBean的形式来代替传统的xml配置文件形式进行针对框架个性化定制，就是Spring MVC XML配置文件的JavaConfig(编码）实现方式。自定义Interceptor, ViewResolver,MessageConverter。WebMvcConfigurer 就是JavaConfig形式的Spring MVC的配置文件

WebMvcConfigurer 是一个接口，需要自定义某个对象，实现接口并覆盖某个方法。主要方法功能介绍一下：

```java
public interface WebMvcConfigurer {
    //帮助配置HandlerMapping
    default void configurePathMatch(PathMatchConfigurer configurer) {
    }
    //处理内容协商
    default void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
    }
    //异步请求
    default void configureAsyncSupport(AsyncSupportConfigurer configurer) {
    }
    //配置默认 Servlet
    default void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
    }
    //配置内容转换器
    default void addFormatters(FormatterRegistry registry) {
    }
    //配置拦截器
    default void addInterceptors(InterceptorRegistry registry) {
    }
    //处理静态资源
    default void addResourceHandlers(ResourceHandlerRegistry registry) {
    }

    default void addCorsMappings(CorsRegistry registry) {
    }

    default void addViewControllers(ViewControllerRegistry registry) {
    }

    default void configureViewResolvers(ViewResolverRegistry registry) {
    }

    default void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
    }

    default void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> handlers) {
    }

    default void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
    }

    default void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
    }

    default void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
    }

    default void extendHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
    }

    @Nullable
    default Validator getValidator() {
        return null;
    }

    @Nullable
    default MessageCodesResolver getMessageCodesResolver() {
        return null;
    }
}
```

### 5.6.1 页面跳转控制器

Spring Boot 中使用页面视图，比如Thymeleaf。要跳转显示某个页面，必须通过Controller对象。也就是我们需要创建一个Controller，转发到一个视图才行。如果我们现在需要显示页面，可以无需这个Controller。addViewControllersO完成从请求到视图跳转。

需求：访问/welcome 无需经过Controller，直接访问到项目首页index.html(Thyemeleaf创建的对象)

1. 新建index.html 文件

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <h1>===========================</h1>
   </body>
   </html>
   ```

2. 新建配置类，实现 WebMvcConfigurer 接口

   ```java
   package com.fengshun.webmvcconfig.settings;
   
   import org.springframework.context.annotation.Configuration;
   import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
   import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
   
   @Configuration
   public class MvcSetting implements WebMvcConfigurer {
   
       //页面跳转控制器，从请求直达视图页面，无需Controller
       @Override
       public void addViewControllers(ViewControllerRegistry registry) {
           // 配置页面控制：.addViewController("请求uri"). 指定他的视图 setViewName(目标视图)
           registry.addViewController("/welCome").setViewName("index");
   
       }
   }
   ```

### 5.6.2 数据格式化

Formatter<T>是数据转换接口，将一种数据类型转换为另一种数据类型。与Formatter<T>功能类型的还有Converter<S，T>。本节研究 Formatter<T>接口。Formatter<T>只能将 String 类型转为其他数据数据类型。这点在Web应用适用更广。因为Web请求的所有参数都是String，我们需要把String转为Integer，Long，Date等等。

Spring 中内置了一下 Formatter<T>:

- DateFormatter：String 和Date 之间的解析与格式化
- InetAddressFormatter：String 和InetAddress之间的解析与格式化
- PercentStyleFormatter ：String 和Number 之间的解析与格式化，带货币符合
- NumberFormat：String 和Number 之间的解析与格式化

我在使用@DateTimeFormat，@NumberFormat 注解时，就是通过Formatter<T>解析String类型到我们期望的Date 或Number 类型

Formatter<T>也是Spring的扩展点，我们处理特殊格式的请求数据时，能够自定义合适的Formatter<T>，将
请求的String 数据转为我们的某个对象，使用这个对象更方便我们的后续编码。

接口原型：

```java
public interface Formatter<T> extends Printer<T>, Parser<T> {
}
```

Formatter<T> 是一个组合接口，没有自己的方法。内容来自 Printer<T> 和 Parser<T>

Printer<T>:将 T 类型转为 String，格式化输出

Parser<T>：将String类型转为期望的T对象。

我们项目开发，可能面对多种类型的项目，复杂程度有简单，有难一些。特别是与硬件打交道的项目，数据的格式与一般的name： lisi,age:20不同。数据可能是一串“1111；2222；333,NF；4;561”。

需求：将“1111；2222；333,NF；4；561”接受，代码中用Devicelnfo存储参数值。

1. 创建 DeviceInfo 数据类：

   ```java
   package com.fengshun.webmvcconfig.pojo;
   
   public class DeviceInfo {
       private  String item1;
       private  String item2;
       private  String item3;
       private  String item4;
       private  String item5;
       //getter and setter
       //toString
   }
   ```

2. 自定义 Formatter 实现 Formatter<DeviceInfo> 接口

   ```java
   package com.fengshun.webmvcconfig.settings;
   
   import com.fengshun.webmvcconfig.pojo.DeviceInfo;
   import org.springframework.format.Formatter;
   import org.springframework.util.StringUtils;
   
   import java.text.ParseException;
   import java.util.Locale;
   import java.util.StringJoiner;
   
   public class DeviceFormatter implements Formatter<DeviceInfo> {
       // 将String数据，转为 DeviceInfo
       @Override
       public DeviceInfo parse(String text, Locale locale) throws ParseException {
           // text表示请求参数的值
           DeviceInfo deviceInfo = null;
           // 判断text是否有值
           if (StringUtils.hasLength(text)) {
               String[] items = text.split(";");
               deviceInfo = new DeviceInfo();
               deviceInfo.setItem1(items[0]);
               deviceInfo.setItem2(items[1]);
               deviceInfo.setItem3(items[2]);
               deviceInfo.setItem4(items[3]);
               deviceInfo.setItem5(items[4]);
           }
           return deviceInfo;
       }
   
       // 将DeviceInfo 转为 String
       @Override
       public String print(DeviceInfo object, Locale locale) {
           StringJoiner stringJoiner = new StringJoiner("#");
           stringJoiner.add(object.getItem1()).add(object.getItem2())
                       .add(object.getItem3()).add(object.getItem4())
                       .add(object.getItem5());
           return stringJoiner.toString();
       }
   }
   ```

3. 新建配置类，实现 WebMvcConfigurer 接口

   ```java
   package com.fengshun.webmvcconfig.settings;
   
   import org.springframework.context.annotation.Configuration;
   import org.springframework.format.FormatterRegistry;
   import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
   import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
   
   @Configuration
   public class MvcSetting implements WebMvcConfigurer {
   
       //页面跳转控制器，从请求直达视图页面，无需Controller
       @Override
       public void addViewControllers(ViewControllerRegistry registry) {
           // 配置页面控制：.addViewController("请求uri"). 指定他的视图 setViewName(目标视图)
           registry.addViewController("/welCome").setViewName("index");
   
       }
       //转换器
       @Override
       public void addFormatters(FormatterRegistry registry) {
           registry.addFormatter(new DeviceFormatter());
       }
   }
   ```

4. 测试：

   ![image-20231005210857115](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231005210857115.png)

说明：controller 接收到请求参数后，会交给 DeviceFormatter 类进行解析，DeviceFormatter 通过parse 方法对请求参数进行解析。

###  5.6.3 拦截器

Handlerinterceptor 接口和它的实现类称为拦截器，是SpringMVC的一种对象。拦截器是SpringMVC框架的对象与Servlet无关。拦截器能够预先处理发给Controller的请求。可以决定请求是否被Controller处理。用户请求是先由 DispatcherServlet 接收后，在Controller之前执行的拦截器对象。

一个项目中有众多的拦截器：框架中预定义的拦截器，自定义拦截器。下面我说说自定义拦截器的应用。
根据拦截器的特点，类似权限验证，记录日志，过滤字符，登录token处理都可以使用拦截器。

拦截器定义步骤：

1. 声明类实现HandlerInterceptor 接口，重写三个方法（需要那个重写那个）
2. 登记拦截器

#### 5.6.3.1 一个拦截器

需求：zhangsan操作员用户，只能查看文章，不能修改，删除

1. 创建文章的Controller

   ```java
   package com.fengshun.webmvcconfig.comtroller;
   
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.bind.annotation.RestController;
   
   @RestController
   @RequestMapping("/article")
   public class ArticleController {
   
       @RequestMapping("/add")
       public String addArticle() {
           return "发布新的文章";
       }
   
       @RequestMapping("/edit")
       public String editArticle() {
           return "修改文章";
       }
   
       @RequestMapping("/query")
       public String queryArticle() {
           return "查询文章";
       }
   
   }
   ```

2. 创建有关权限的拦截器（需要实现HandlerInterceptor接口 ）

   ```java
   package com.fengshun.webmvcconfig.interceptor;
   
   import jakarta.servlet.http.HttpServletRequest;
   import jakarta.servlet.http.HttpServletResponse;
   import org.springframework.web.servlet.HandlerInterceptor;
   import org.springframework.web.servlet.ModelAndView;
   
   public class AuthInterceptor implements HandlerInterceptor {
       private final String COMMON_USER="zhangsan";
   
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("=========AuthInterceptor 权限拦截器==============");
           //获取登录的用户
           String loginUser = request.getParameter("loginUser");
           //获取操作的url
           String requestURI = request.getRequestURI();
           if (COMMON_USER.equals(loginUser) && (requestURI.startsWith("/article/add")
                                                 || requestURI.startsWith("/article/edit")
                                                 || requestURI.startsWith("/article/remove"))){
               return false;
           }
           return true;
       }
   }
   
   ```

3. 登记拦截器

   ```java
   package com.fengshun.webmvcconfig.settings;
   
   import com.fengshun.webmvcconfig.interceptor.AuthInterceptor;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.format.FormatterRegistry;
   import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
   import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
   import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
   
   @Configuration
   public class MvcSetting implements WebMvcConfigurer {
   
       @Override
       public void addInterceptors(InterceptorRegistry registry) {
           AuthInterceptor authInterceptor = new AuthInterceptor();
           registry.addInterceptor(authInterceptor)    //注册拦截器
                   .addPathPatterns("/article/**")     //拦截article开始的所有请求
                   .excludePathPatterns("/article/query");     //排除/article/query 请求
   
       }
   }
   ```

4. 测试：

   访问地址：[localhost:8080/article/add?loginUser=zhangsan](http://localhost:8080/article/add?loginUser=zhangsan)  页面不显示

   访问地址：[localhost:8080/article/query?loginUser=zhangsan](http://localhost:8080/article/query?loginUser=zhangsan)

   ![image-20231005213511790](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231005213511790.png)

### 5.6.2 多个拦截器

增加一个验证登录用户的拦截器，只有zhangsan，lisi，admin能够登录系统。其他用户不可以。两个拦截器登录的拦截器先执行，权限拦截器后执行，orderO方法设置顺序，整数值越小，先执行。

step1：创建登录拦截器

```java
package com.fengshun.webmvcconfig.interceptor;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.util.StringUtils;
import org.springframework.web.servlet.HandlerInterceptor;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class LoginInterceptor implements HandlerInterceptor {
    private List<String > permitUser = new ArrayList<>();

    public LoginInterceptor() {
        permitUser= Arrays.asList("zhangsan","lisi","admin");
    }

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("=========LoginInterceptor 权限拦截器==============");
        //获取登录的用户
        String loginUser = request.getParameter("loginUser");
        if (StringUtils.hasText(loginUser) && permitUser.contains(loginUser)) {
            return true;
        }
        return false;
    }
}
```

step2：登记拦截器

```java
package com.fengshun.webmvcconfig.settings;

import com.fengshun.webmvcconfig.interceptor.AuthInterceptor;
import com.fengshun.webmvcconfig.interceptor.LoginInterceptor;
import org.springframework.context.annotation.Configuration;
import org.springframework.format.FormatterRegistry;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class MvcSetting implements WebMvcConfigurer {
    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //权限拦截器
        AuthInterceptor authInterceptor = new AuthInterceptor();
        registry.addInterceptor(authInterceptor)    //注册拦截器
                .order(2)
                .addPathPatterns("/article/**")     //拦截article开始的所有请求
                .excludePathPatterns("/article/query");     //排除/article/query 请求

        //登录拦截器
        LoginInterceptor loginInterceptor = new LoginInterceptor();
        registry.addInterceptor(loginInterceptor)        //注册拦截器
                .order(1)			//值越小，越先执行
                .addPathPatterns("/article/**")         //拦截article开始的所有请求
                .excludePathPatterns("/article/query");      //排除/article/query 请求
    }
}

```

测试：
访问：[localhost:8080/article/add?loginUser=zhangsan](http://localhost:8080/article/add?loginUser=zhangsan)

![image-20231005215652412](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231005215652412.png)

## 5.7 文件上传

上传文件大家首先想到的就是Apache Commons FileUpload，这个库使用非常广泛。Spring Boot3版本中已经
不能使用了。代替他的是Spring Boot中自己的文件上传实现。

Spring Boot 上传文件现在变得非常简单。提供了封装好的处理上传文件的接口MultipartResolver，用于解析上传文件的请求，他的内部实现类StandardServletMultipartResolver。之前常用的CommonsMultipartResolver不可用了。CommonsMultipartResolver 是使用 Apache Commons FileUpload库时的处理类。

StandardServletMultipartResolver 内部封装了读取POST其中体的请求数据，也就是文件内容。我们现在只需要在Controller 的方法加入形参 @RequestParam MultipartFile。MultipartFile表示上传的文件，提供了方便的方法保存文件到磁盘。

MultipartFile 常用方法：

1. getName() 		参数有名称（upfile）
2. getOriginalFilename()             上传文件原始名称
3. isEmpty()                上传文件是否为空
4. getSize()              上传的文件字节大小
5. getInputStream()         文件的InputStream，可以用于读取部件的内容
6. transferTo(File dest)       保存上传文件到目标磁盘（dest）

### 5.7.1 MultipartFile 

代码实现：

1. 添加文件上传页面：

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <div style="margin: 3em auto">
   <h3>上传文件</h3>
       <form action="uploadFile" enctype="multipart/form-data" method="post">
           <input type="file" name="upfile"><br>
           <button type="submit">上传</button>
       </form>
   </div>
   
   </body>
   </html>
   ```

   想要使用文件上传  enctype="multipart/form-data" 属性是必须的

2. 添加上传成功的页面：

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <h1>文件上传成功！！！</h1>
   </body>
   </html>
   ```

3. 添加Controller

   ```java
   package com.fengshun.uploadfiles.controller;
   
   import org.springframework.context.annotation.Conditional;
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.web.multipart.MultipartFile;
   
   import java.io.File;
   import java.util.HashMap;
   import java.util.Map;
   import java.util.UUID;
   
   @Controller
   
   public class UploadFleController {
   
       @RequestMapping("/uploadFile")
       public String uploadfile(@RequestParam("upfile") MultipartFile multipartFile) {
           System.out.println("开始处理上传的文文件");
           Map<String, Object> map = new HashMap<>();
           try {
               // 判断上传了文件
               if (!multipartFile.isEmpty()) {
                   map.put("上传文件的参数名称", multipartFile.getName());
                   map.put("内容类型", multipartFile.getContentType());
                   String filename = multipartFile.getOriginalFilename();
                   map.put("文件原始名称", filename);
                   String ext = "unknown";
                   if (filename.indexOf(".") > 0) {
                       // 获取文件的后缀
                       ext = filename.substring(filename.indexOf(".") + 1);
                       map.put("文件类型", ext);
                   }
                   // 生成服务器使用的文件名称
                   String newFilename = UUID.randomUUID().toString() + ext;
                   // 定义上传文件的路径
                   String path = "C:\\Users\\fengshun\\Desktop\\自学\\微服务框架\\SpringBoot\\SpringBootIdeaProject\\SpringBoot3\\lession08-UploadFiles" + newFilename;
   
   
                   // 把文件保存到path目录
                   multipartFile.transferTo(new File(path));
               }
           } catch (Exception e) {
               e.printStackTrace();
           }
           System.out.println(map.toString());
           return "redirect:/uploadsuccee.html";
       }
   }
   ```

4. 测试：

   ![image-20231006104314939](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231006104314939.png)



注意：

> 1. SpringBoot 默认单个文件最大支持1M，一次请求最大10M。可以改变默认值，需要application 修改配置项
>
>    ~~~properties
>    spring.servlet.multipart.max-file-size=800B  #单个文件最大支持大小
>    spring.servlet.multipart.max-request-size=5MB  #一次请求最大内存大小
>    spring.servlet.multipart.file-size-threshold=0KB
>    ~~~
>
>    multipart.file-size-threshold 表示超过指定大小，直接写文件到磁盘，不再内存处理。
>
> 2. 错误页面
>
>    在static目录中定义错误页面：
>
>    ![image-20231006110635536](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231006110635536.png)
>
>    如果出现400+ 或者500+错误，SpringBoot会默认访问该页面。
>
>    依然需要添加配置：
>
>    ```properties
>    spring.web.resources.static-locations=classpath:/templates/,classpath:/static/
>    ```
>
>    

### 5.7.2 Servlet 规范

Servlet3.0规范中，定义了jakarta.servlet.http.Part 接口处理multipart/form-data POST请求中接收到表单数据有了Part对象，其writeO方法将上传文件保存到服务器本地磁盘目录。

在HttpServletRequest 接口中引入的新方法：

- getPartsO：返回Part对象的集合
- getPart(字符串名称)：检索具有给定名称的单个Part对象。

Spring Boot 3使用的Servlet规范是基于5的，所以上传文件使用的就是Part接口。
StandardServletMultipartResolver 对 Part 接口进行的封装，实现基于Servlet 规范的文件上传。
原生的Serlvet 规范的文件上传

```java
@PostMapping("files")
public String upload(HttpServletRequest request){
    try {
        for (Part part:request.getParts()){
            String fileName = extractFileName(part);
            //将文件写入到磁盘
            part.write(fileName);
        }
    }catch (IOException e){
        throw new RuntimeException(e);
    }catch (ServletException e){
        throw new RuntimeException(e);
    }
    return "redirect:/uploadsuccee.html";
}

private String  extractFileName(Part part){
    String contentDisp = part.getHeader("content-disposition");
    String[] items = contentDisp.split(";");
    for (String s : items) {
        if (s.trim().startsWith("filename")) {
            return s.substring(s.indexOf("=")+2,s.length()-1);
        }
    }
    return "";
}
```

上传文件包含 header 头 content-diposition ，类似下面的内容，可获取文件原始名称。

​		form-data;name="dataFile";filename="header.png"

application 文件，可以配置服务器存储文件位置，例如：

```properties
spring.servlet.multipart.location=C://files/
```

### 5.7.3 多文件上传

控制器方法接收的参数改为数组即可：

```java
@RequestMapping("/uploadFile")
public String uploadfile(@RequestParam("upfile") MultipartFile[] multipartFiles) {
    System.out.println("开始处理上传的文文件");
    Map<String, Object> map = new HashMap<>();
    try {
        //遍历 multipartFiles
        for (MultipartFile multipartFile : multipartFiles) {
            // 判断上传了文件
            if (!multipartFile.isEmpty()) {
                map.put("上传文件的参数名称", multipartFile.getName());
                map.put("内容类型", multipartFile.getContentType());
                String filename = multipartFile.getOriginalFilename();
                map.put("文件原始名称", filename);
                String ext = "unknown";
                if (filename.indexOf(".") > 0) {
                    // 获取文件的后缀
                    ext = filename.substring(filename.indexOf(".") + 1);
                    map.put("文件类型", ext);
                }
                // 生成服务器使用的文件名称
                String newFilename = UUID.randomUUID().toString() + ext;
                // 定义上传文件的路径
                String path = "C:\\Users\\fengshun\\Desktop\\自学\\微服务框架\\SpringBoot\\SpringBootIdeaProject\\SpringBoot3\\lession08-UploadFiles" + newFilename;


                // 把文件保存到path目录
                multipartFile.transferTo(new File(path));
            }
        }
        
    } catch (Exception e) {
        e.printStackTrace();
    }
    System.out.println(map.toString());
    return "redirect:/uploadsuccee.html";
}
```

## 5.8 全局异常处理

在Controller 处理请求过程中发生了异常，DispatcherServlet将异常处理委托给异常处理器（处理异常的类）。实现 HandlerExceptionResolver 接口的都是异常处理类

项目的异常一般集中处理，定义全局异常处理器。在结合框架提供的注解，诸如：@ExceptionHandler,
@ControllerAdvice或者@RestControllerAdvice一起完成异常的处理。
@ControllerAdvice与@RestControllerAdvice 区别在于：@RestControllerAdvice加了@RepsonseBody.

创建项目 Lession16-ExceptionHandler，Maven 构建工具，JDK19。依赖选择Spring Web,Lombok,Thymeleaf。
包名称com.bjpowernode.ch。

### 5.8.1 全局异常处理器

需求：应用计算两个数字相除，当用户被除数为0，发生异常。使用自定义异常处理器代替默认的异常处理程序。

1. 新建发送请求的页面：

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <form action="/divide" method="post">
       除数：<input type="text" name="n1"><br/>
       被除数：<input type="text" name="n2"><br/>
       <button type="submit">提交</button>
   </form>
   </body>
   </html>
   ```

2. 新建出现异常后需要显示的页面

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <h3 th:text="${message}"></h3>
   </body>
   </html>
   ```

3. 定义处理异常的控制器方法

   ```java
   package com.example.lessionexceptionhandler.handler;
   
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.ControllerAdvice;
   import org.springframework.web.bind.annotation.ExceptionHandler;
   
   @ControllerAdvice
   public class GlobalExceptionHandler {
       // 定义方法，处理数学异常
       @ExceptionHandler({ArithmeticException.class})
       public String handlerArtithmericException(ArithmeticException e, Model model){
           String message = e.getMessage();
           model.addAttribute("message",message);
           return "exp";
       }
   }
   ```

   处理异常的控制器类和普通的控制器类似，处理异常的控制器类需要加上 @ControllerAdvice 或者 @RestControllerAdvice 注解

   方法上需要添加 @ExceptionHandler 注解，它的参数为数组，当页面发生指定的异常后，定义好的控制器方法就能接收到参数，**建议参数为具体的异常类型，以减少异常类型和原因异常之间不匹配的问题，考虑创建多个 @ExceptionHandler 方法，每个方法通过其签名匹配单个特定的异常类型。最后增加一个根异常，考虑没有匹配的其他异常。**

#### 异常处理返回数据

   异常处理控制器方法的返回值为map

```java
@ExceptionHandler({ArithmeticException.class})
@ResponseBody
public Map<String,Object> handlerArtithmericException(ArithmeticException e){
    Map<String, Object> map = new HashMap<>();
    map.put("msg",e.getMessage());
    map.put("tips","被除数不能为0");
    return map;
}
```

![image-20231006153046908](C:\Users\fengshun\AppData\Roaming\Typora\typora-user-images\image-20231006153046908.png)

### 5.8.2 BeanValidator

使用JSR-303 验证参数时，我们是在Controller方法，声明 BindingResult 对象获取校验结果。Controller的方法很多，每个方法都加入BindingResult 处理检验参数比较繁琐。校验参数失败抛出异常给框架，异常处理器能捕获到 MethodArgumentNotValidException，她是BindException 的子类。

<img src="C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231006195425040.png" alt="image-20231006195425040" style="zoom: 33%;" />

BindException 异常实现了 BindResult 接口，异常类能够得到 BindingResult 对象，进一步获取 JSR303 校验的异常信息。

