# SpringMVC

## 一，概念

### 1，**三层架构** 

1. 开发服务器端程序，一般都基于两种形式，一种C/S架构程序，一种B/S架构程序 

2. 使用Java语言基本上都是开发B/S架构的程序，B/S架构又分成了三层架构

3. 三层架构 
   1. 表现层：WEB层，用来和客户端进行数据交互的。表现层一般会采用MVC的设计模型 
   2. 业务层：处理公司具体的业务逻辑的 
   3. 持久层：用来操作数据库的 

   

### **2，MVC模型** ：Model View Controller 模型视图控制器

1. Model：数据模型，JavaBean的类，用来进行数据封装。

2. View：指JSP、HTML用来展示数据给用户 

3. Controller：用来接收用户的请求，整个流程的控制器



### **3，SpringMvc**



1.SpringMVC 是一种基于 Java 的实现 MVC 设计模型的请求驱动类型的轻量级 Web 框架

2.Spring 框架提供了构建 Web 应用程序的全功 能 MVC 模块。使用 Spring 可插入的 MVC 架构，从而在使用 Spring 进行 WEB 开发时，可以选择使用 Spring 的 Spring MVC 框架或集成其他 MVC 开发框架，如 Struts1(现在一般不用)，Struts2 等。

3.SpringMVC 已经成为目前最主流的 MVC 框架之一，它通过一套注解，让一个简单的 Java 类成为处理请求的控制器，而无须实现任何接口。同时它还支持 RESTful 编程风格的请求



**前端控制器（`DispatcherServlet`）**  

**请求到处理器映射（`HandlerMapping`）**

​	1.负责根据用户请求找到 Handler 即处理器，SpringMVC 提供了不同的映射器实现不同的映射方式

​	2.也就是，根据注解，调用对应请求需要的方法

**处理器适配器（`HandlerAdapter`）**

​	1.具体业务控制器 

​	2.对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理 器进行执行

**视图解析器（`ViewResolver`）**  

​	1.也就是，经过处理后，需要调用其他的页面	

**处理器或页面控制器（`Controller`）**

**验证器（` Validator`）**  

​	命令对象（Command  请求参数绑定到的对象就叫命令对象）

​	表单对象（Form Object 提供给表单展示和提交到的对象就叫表单对象） 



**在 SpringMVC 的各个组件中，处理器映射器、处理器适配器、视图解析器称为 SpringMVC 的三大组件** 



**SpringMvc 和 Struts2 的对比**

共同点：  

​	都是表现层框架，都是基于 MVC 模型编写的

​	底层都离不开原始 ServletAPI

​	处理请求的机制都是一个核心控制器

区别：

​	**Spring MVC 的入口是 Servlet, 而 Struts2 是 Filter**   

​	**Spring MVC 是基于方法设计的，而 Struts2 是基于类**，Struts2 每次执行都会创建一个动作类。所 以 Spring MVC 会稍微比 Struts2 快些。 

​	 **Spring MVC 使用更加简洁,同时还支持 JSR303, 处理 ajax 的请求更方便**  (JSR303 是一套 JavaBean 参数校验的标准，它定义了很多常用的校验注解，我们可以直接将这些注 解加在我们 JavaBean 的属性上面，就可以在需要校验的时候进行校验了。)  

​	**Struts2 的 OGNL 表达式使页面的开发效率相比 Spring MVC 更高些**，但执行效率并没有比 JSTL 提 升，尤其是 struts2 的表单标签，远没有 html 执行效率高



## 二，入门使用

1. 当启动Tomcat服务器的时候，因为配置了`load-on-startup标签`，所以会创建`DispatcherServlet对象`， 就会加载springmvc.xml配置文件 

2. 开启了注解扫描，那么`HelloController对象`就会被创建

3. 从`index.jsp`发送请求，请求会先到达`DispatcherServlet核心控制器`，根据配置`@RequestMapping注解` 找到执行的具体方法

4. 根据执行方法的返回值，再根据配置的视图解析器，去指定的目录下查找指定名称的JSP文件 

5. Tomcat服务器渲染页面，做出响应 



### 1，导入依赖

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>2.5</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.0</version>
    <scope>provided</scope>
</dependency>
```



### 2，配置web.xml文件（`DispatcherServlet`）

**1.配置`DispatcherServlet`接收用户的请求，并调用其他的对象，执行某个功能**

**2.<init-param>配置`springmvc`的xml配置文件，让项目会去读取配置文件中的信息**

**3. <load-on-startup>**

```xml
<!-- SpringMVC的核心控制器 -->  
<servlet>        
    <servlet-name>dispatcherServlet</servlet-name>    
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servletclass>      

<!-- 配置Servlet的初始化参数，读取springmvc的配置文件，创建spring容器 -->
<init-param>
    <param-name>contextConfigLocation</param-name>        
    <param-value>classpath:springmvcconfig.xml</param-value>  
</init-param> 
      
<!-- 配置servlet启动时加载对象 --> 
    <load-on-startup>1</load-on-startup>   
</servlet>
```



### 3，配置springmvcconfig.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" 
       xmlns:mvc="http://www.springframework.org/schema/mvc" 
       xmlns:context="http://www.springframework.org/schema/context" 
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"   
       xsi:schemaLocation="        
       http://www.springframework.org/schema/beans     
       http://www.springframework.org/schema/beans/spring-beans.xsd    
       http://www.springframework.org/schema/mvc     
      http://www.springframework.org/schema/mvc/spring-mvc.xsd    
      http://www.springframework.org/schema/context     
      http://www.springframework.org/schema/context/spring-context.xsd">  
      <!--可以在spring的官网的查到xml信息-->
</beans>      
```

具体配置

```xml
	<!-- 配置spring创建容器时要扫描的包 -->   
    <context:component-scan base-package=""></context:component-scan>      
    
	<!-- 配置视图解析器 -->  
    <bean 
    id="viewResolver" 
    class="org.springframework.web.servlet.view.InternalResourceViewResolver">  
        <property name="prefix" value="/WEB-INF/pages/"></property>      
        <property name="suffix" value=".jsp"></property>   
    </bean>     
    <!-- 配置spring开启注解mvc的支持   -->
    <mvc:annotation-driven></mvc:annotation-driven>
```



### 4，编写web页面和控制器类

```java
@Controller 
public class HelloController {  
       @RequestMapping(path="/hello")    
       public String sayHello() {          
        return "success";   
        }
}
```





![img](E:\有道云笔记\新建文件夹\qqA18D73086F0D435363C26D86B8CAD5E6\fbd9a02dec984fbfa2d8fc5c13a2f938\springmvc执行流程原理.jpg)



## 三，常用注解

### **@RequestMapping** 

1. RequestMapping注解的作用是建立请求URL和处理方法之间的对应关系 

2. RequestMapping注解可以作用在方法和类上
   1. 作用在类上：第一级的访问目录 
   2. 作用在方法上：第二级的访问目录

相关属性

```properties
1. path: 指定请求路径的url 
2. value: value属性和path属性是一样的
3. mthod: 指定该方法的请求方式，是一个对象数组
4. params: 指定限制请求参数的条件
# params="a" 说明是要求参数中要有a
5. headers: 发送的请求中必须包含的请求头
```



> **@PsotMapping**
>
> **@GetMapping**
>
> 这个注解是对 @RequestMapping（method）的衍生
>
> 即，接受post请求，get请求



### @RequestParam

**作用**：把请求中的指定名称的参数传递给控制器中的形参赋值 

**属性** ：

1. **value**：请求参数中的名称 

2. **required**：请求参数中是否必须提供此参数，默认值是true，**必须提供** 



**应用**：就是在参数名和控制器方法中的参数不一致时，进行转换

```java
 @RequestMapping(path="/hello")    
 public String sayHello(@RequestParam(value="username",required=false)String name) {}
```



### **@RequestBody**

**作用**：用于获取请求体的内容（注意：get方法不可以）

**属性 ：**

1. **required**：是否必须有请求体，默认值是true 

```java
@RequestMapping(path="/hello")    
public String sayHello(@RequestBody String body) {}
```



### @PathVariable

**作用**：拥有绑定url中的占位符的

> 例如：url中有/delete/{id}，{id}就是占位符 

**属性** 

1. value：指定url中的占位符名称 

**应用**：Restful风格的URL 

请求路径一样，可以根据不同的请求方式去执行后台的不同方法 ，也就是说所有控制器的方法都没有虚拟路径，只有类上有虚拟路径

而Restful风格就会根据每个方法中不同的请求方式去执行不同的方法



restful风格的URL优点
1. 结构清晰 2. 符合标准 3. 易于理解 4. 扩展方便 

```java
@RequestMapping(path="/hello/{id}")
public String sayHello(@PathVariable(value="id") String id) { } 
```



### @ResponseBody

@responseBody注解的作用是将controller的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到response对象的body区

**通常用来返回JSON数据或者是XML数据**



**注意**：在使用此注解之后不会再走视图处理器，而是直接将数据写入到输入流中，他的效果等同于通过response对象输出指定格式的数据。



### @RestController

 简单来说就是结合了 @Controller注解和@ResponseBody注解



### @RequestHeader

**作用**：获取指定请求头的值 

```java
@RequestMapping(path="/hello")    
public String sayHello(@RequestHeader(value="Accept") String header) {  }
```



### @CookieValue

**作用**：用于获取指定cookie的名称的值

```java
@RequestMapping(path="/hello")    
public String sayHello(@CookieValue(value="") String cookieValue) {}
```



### @ModelAttribute  

**作用** 

1. 出现在方法上：表示当前方法会在控制器方法执行前线执行。

2. 出现在参数上：获取指定的数据给参数赋值。 



**应用** ： 当提交表单数据，不是完整的实体数据时，保证没有提交的字段使用数据库原来的数据

```java
//当修饰的方法有返回值时，返回的数据会作为参数传递到调用的方法中
@ModelAttribute    
public User showUser() {     
    User user = new User();     
    return user;    
}          
@RequestMapping(path="/updateUser")   
public String updateUser(User user) {   }
```



```java
//当修饰方法没有返回值时，使用map集合封装
@ModelAttribute    
public void showUser(String name,Map<String, User> map) {                  
    User user = new User();      
    map.put("abc", user);   
 } 
@RequestMapping(path="/updateUser")
public String updateUser(@ModelAttribute(value="abc") User user) {   }
```



### @SessionAttributes

**作用**：控制器方法间的参数共享

1，存入Session

```java
@RequestMapping(path="/save")   
public String save(Model model) {    
    model.addAttribute("username", "root"); 
} 
```

2，获取

```java
@SessionAttributes(value= {"username"},types= {String.class})       
@RequestMapping(path="/find")   
public String find(ModelMap modelMap) {       
	String username = (String) modelMap.get("username");          
} 
```

3，删除

```java
@SessionAttributes(value= {"username"},types= {String.class})       
@RequestMapping(path="/delete")
public String delete(SessionStatus status) {
     status.setComplete();
}
```



