# Spring

## 简介

Spring是分层的 Java SE/EE应用 full-stack **轻量级开源框架**，

以 **IoC（Inverse Of Control： 反转控制）和 AOP（Aspect Oriented Programming：面向切面编程）为内核**，

提供了展现层 Spring MVC 和持久层 Spring JDBC 以及业务层事务管理等众多的企业级应用技术，还能整合开源世界众多 著名的第三方框架和类库，逐渐成为使用最多的Java EE 企业应用开源框架



## 优势

**1，方便解耦，简化开发**  

通过 Spring提供的 **IoC容器**，可以将**对象间的依赖关系交由 Spring进行控制**，**避免硬编码所造成的过度程序耦合**。用户也不必再为单例模式类、属性文件解析等这些很底层的需求编写代码，可以更专注于上层的应用

**2，AOP编程的支持**  

通过 Spring的 **AOP 功能**，**方便进行面向切面的编程**

**3，声明式事务的支持**  

可以将我们从单调烦闷的事务管理代码中解脱出来，通过**声明式方式灵活的进行事务的管理**， 提高开发效率和质量。 

**4，方便程序的测试** 

可以用非容器依赖的编程方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可做的事情

**5，方便集成各种优秀框架**  

Spring可以降低各种框架的使用难度，提供了对各种优秀框架（Struts、Hibernate、Hessian、Quartz 等）的直接支持

 **6，降低 JavaEE API的使用难度**  

Spring对 JavaEE API（如 JDBC、JavaMail、远程调用等）进行了薄薄的封装层，使这些 API 的 使用难度大为降低。

**7，Java源码是经典学习范例** 

 Spring的源代码设计精妙、结构清晰、匠心独用，处处体现着大师对Java 设计模式灵活运用以 及对 Java技术的高深造诣。它的源代码无意是 Java 技术的最佳实践的范例



## 什么是AOP？

AOP  , 直译过来就是 面向切面编程。

AOP 是一种编程思想，是面向对象编程（OOP）的一种补充。面向对象编程将程序抽象成各个层次的对象，而面向切面编程是将程序抽象成各个切面。

AOP 要达到的效果是，保证开发者不修改源代码的前提下，去为系统中的业务组件添加某种通用功能

### 分类

- 静态 AOP 实现， AOP 框架在编译阶段对程序源代码进行修改，生成了静态的 AOP 代理类（生成的 *.class 文件已经被改掉了，需要使用特定的编译器），比如 AspectJ。
- 动态 AOP 实现， AOP 框架在运行阶段对动态生成代理对象（在内存中以 JDK 动态代理，或 CGlib 动态地生成 AOP 代理类），如 SpringAOP。



> 那么这里就涉及到静态代理和动态代理

静态代理

代理类和被代理的类实现了同样的接口，代理类同时持有被代理类的引用，这样，当我们需要调用被代理类的方法时，可以通过调用代理类的方法来做到

动态代理

动态代理的根据实现方式的不同可以分为 ：JDK 动态代理 和 CGlib 动态代理



JDK 动态代理：利用反射机制生成一个实现代理接口的类，在调用具体方法前调用InvokeHandler来处理

```java
public class Work implements InvocationHandler {
    private Object object;

    public Work(Object object) {
        this.object = object;
    }
   //定义类实现InvocationHandler接口，并实现invoke方法 
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("object: " + object.getClass().getSimpleName());//被代理的对象名
        System.out.println("proxy: " + proxy.getClass().getSimpleName());//代理对象名

        if ("meeting".equals(method.getName())) {
            System.out.println("代理先准备会议材料...");
            return method.invoke(object, args);
        } 
        return null;
    }
}

//调用
//这样当使用proxy调用meeting方法时，实际上是通过反射产生的代理类执行
Leader leader = new Leader();
IWork proxy = (IWork) Proxy.newProxyInstance(Leader.class.getClassLoader(),
                		new Class[]{IWork.class}, new WorkInvocationHandler(leader));
proxy.meeting();
```



CGlib 动态代理：利用ASM（开源的Java字节码编辑库，操作字节码）开源包，将代理对象类的class文件加载进来，通过修改其字节码生成子类来处理



区别：

JDK代理只能对实现接口的类生成代理；

CGlib是针对类实现代理，对指定的类生成一个子类，并覆盖其中的方法，这种通过继承类的实现方式，不能代理final修饰的类

### Aop术语

- 通知（Advice）: AOP 框架中的增强处理。通知描述了切面何时执行以及如何执行增强处理，即**被用来增强方法的方法**
- 连接点（join point）: 连接点表示应用执行过程中能够插入切面的一个点，这个点可以是方法的调用、异常的抛出。在 Spring AOP 中，连接点总是方法的调用，即**被增强的方法**
- 切点（PointCut）: 可以插入增强处理的连接点，即**没有被增强的方法**。
- 切面（Aspect）: 切面是通知和切点的结合。
- 引入（Introduction）：引入允许我们向现有的类添加新的方法或者属性。
- 织入（Weaving）: 将增强处理添加到目标对象中，并创建一个被增强的对象，这个过程就是织入。
- Target(目标对象):   代理的目标对象
- Proxy（代理）:   一个类被 AOP 织入增强后，就产生一个结果代理类



### AOP的使用

首先得明确切入点表达式的写法

关键字：execution(表达式)

```java
表达式：访问修饰符  返回值  包名.包名.包名...类名.方法名(参数列表)

标准的表达式写法：
public void com.itheima.service.impl.AccountServiceImpl.saveAccount(int a,int b)
    
//访问修饰符可以省略
//包名可以使用..表示当前包及其子包
2 * *..AccountServiceImpl.saveAccount()
```



额外：环绕通知

1，它是spring框架**为我们提供的一种可以在代码中手动控制增强方法何时执行的方式。**

2，Spring框架为我们提供了一个**接口：ProceedingJoinPoint**。

3，该接口有一个方法**proceed()**，此方法就相当于**明确调用切入点方法**。

4，该接口可以作为环绕通知的方法参数，在程序执行时，spring框架会为我们提供该接口的实现类供我们使用

（下面有两种形式的环绕通知配置）



#### 基于xml形式的

1，配置文件

```xml
//配置前置通知：在切入点方法执行之前执行
<aop:before method="" pointcut-ref="pt1" ></aop:before>
//配置后置通知：在切入点方法正常执行之后值。它和异常通知永远只能执行一个
<aop:after-returning method="" pointcut-ref="pt1"></aop:after-returning>-->
//配置异常通知：在切入点方法执行产生异常之后执行。它和后置通知永远只能执行一个
<aop:after-throwing method="" pointcut-ref="pt1"></aop:after-throwing>-->
//配置最终通知：无论切入点方法是否正常执行它都会在其后面执行
<aop:after method="" pointcut-ref="pt1"></aop:after>


//此标签写在aop:aspect标签内部只能当前切面使用。
//它还可以写在aop:aspect外面，此时就变成了所有切面可用
<aop:pointcut id="pt1" 
            expression="execution(* com.tutu.test1.buy(..))">
</aop:pointcut>
//这样写就说明是这个包下的所有类的所有方法执行时都会执行通知
```

2，开启aop支持

```xml
<aop:aspectj-autoproxy/> 
```



环绕通知配置文件

```xml
<aop:config>
<!--aop:pointcut：就是配置各种通知-->    
<aop:pointcut expression="execution()" id="pt1"/>  
    <aop:aspect id="txAdvice" ref="txManager"> 
        <!-- 配置环绕通知 -->  
        <aop:around method="" pointcut-ref="pt1"/> 
   	</aop:aspect> 
</aop:config> 
<!--method：指定通知中方法的名称--> 
<!--pointct：定义切入点表达式-->  
<!--pointcut-ref：指定切入点表达式的引用--> 
```

使用

```java
public Object aroundPringLog(ProceedingJoinPoint pjp){
        Object rtValue = null;
        try{
            Object[] args = pjp.getArgs();//得到方法执行所需的参数

            System.out.println("Logger类中的aroundPringLog前置");

            rtValue = pjp.proceed(args);//明确调用业务层方法（切入点方法）

            System.out.println("Logger类中的aroundPringLog后置");

            return rtValue;
        }catch (Throwable t){
            System.out.println("Logger类中的aroundPringLog异常");
            throw new RuntimeException(t);
        }finally {
            System.out.println("Logger类中的aroundPringLog最终");
        }
    }
}
```



#### 基于注解形式的

1，导入jar包

2，配置通知类

```java
@Component("txManager") 
@Aspect //表明当前类是一个切面类
public class TransactionManager { 
    //开启事务  
    @Before("execution(* com.service.impl.*.*(..))") 
    public void beginTransaction() {}
    //提交事务  
    @AfterReturning("execution(* com.service.impl.*.*(..))")  
    public void commit() {  }
    //回滚事务
    @AfterThrowing("execution(* com.service.impl.*.*(..))") 
    public void rollback() { }
    //释放资源  
    @After("execution(* com.service.impl.*.*(..))")  
    public void release() { }
}
```

3，开启spring对aop支持

```java
@EnableAspectJAutoProxy
puclic class MainApplication{}
```



环绕通知配置

```java
@Around("execution(* com.itheima.service.impl.*.*(..))") 
public Object transactionAround(ProceedingJoinPoint pjp) { 
  //定义返回值   
  Object rtValue = null;  
   try { 
   		//获取方法执行所需的参数    
		Object[] args = pjp.getArgs(); 
   		//前置通知：开启事务    
		beginTransaction(); 
   		//执行方法    
		rtValue = pjp.proceed(args); 
  		//后置通知：提交事务    
		commit();   
   }catch(Throwable e) { 
   		//异常通知：回滚事务    
		rollback();   
    	e.printStackTrace();   
   }finally {    
    	//最终通知：释放资源   
		release();  
   }   
return rtValue; 
```



#### 切入点表达式注解

```java
//里面还有很多属性进行设置
@Pointcut("execution(* com.itheima.service.impl.*.*(..))") 
private void pt1() {} 

@Around("pt1()")//注意：千万别忘了写括号，参数有的话也要带上 
public Object transactionAround(ProceedingJoinPoint pjp) {}
```



## 什么是IOC？

控制反转（Inversion of Control，缩写为IoC，是面向对象编程中的一种设计原则，**可以用来减低计算机代码之间的耦合度**，把创建对象的权力交给框架

通过控制反转，对象在被创建的时候，由一个调控系统内所有对象的外界实体将其所依赖的对象的引用传递给它。也可以说，依赖被注入到对象中

> 其中最常见的方式叫做**依赖注入**（Dependency Injection，简称**DI**），还有一种方式叫“依赖找”（DependencyLookup）

```java
//一般的声明对象方式
private AccountDao dao = new AccountDaoImpl();

//控制反转，把这个类的权利交给其他类，让这个类去创建对象
private AccountDao dao = (AccountDao) beanFactory.getBean("accountDao");
```



### 耦合性

1，耦合性(Coupling)，也叫**耦合度**，**是对模块间关联程度的度量**

2，**耦合的强弱**取决于模块间接口的复杂性、调用模块的方式以及通过界面传送数据的多少

3，**模块间的耦合度**是指模块之间的依赖关系，包括控制关系、调用关系、数据传递关系。模块间联系越多，其耦合性越强，同时表明其独立性越差

（**划分模块的一个 准则就是高内聚低耦合**）



### 耦合分类

（1） 内容耦合

当一个模块**直接修改或操作另一个模块的数据**时，或一个模块不通过正常入口而转入另 一个模块时，这样的耦合被称为内容耦合。内容耦合是最高程度的耦合，应该避免使用之

（2） 公共耦合

两个或两个以上的模块**共同引用一个全局数据项**，这种耦合被称为公共耦合。在具有大 量公共耦合的结构中，确定究竟是哪个模块给全局变量赋了一个特定的值是十分困难的

（3） 外部耦合 

一组模块都访问同一全局简单变量而不是同一全局数据结构，而且不是通过参数表传 递该全局变量的信息，则称之为外部耦合

（4） 控制耦合 

一个模块通过接口向另一个模块传递一个控制信号，接受信号的模块根据信号值而进 行适当的动作，这种耦合被称为控制耦合

（5） 标记耦合 

若一个模块 A 通过接口向两个模块 B 和 C 传递一个公共参数，那么称模块 B 和 C 之间 存在一个标记耦合

（6） 数据耦合

模块之间通过参数来传递数据，那么被称为数据耦合。数据耦合是最低的一种耦合形 式，系统中一般都存在这种类型的耦合，因为为了完成一些有意义的功能，往往需要将某些模块的输出数据作为另

一些模块的输入数据

（7） 非直接耦合 

两个模块之间没有直接关系，它们之间的联系完全是通过主模块的控制和调用来实 现的

​                                                                                                                                                                                                                                                                                                                                                                                                                               

**总结**：  耦合是影响软件复杂程度和设计质量的一个重要因素，在设计上我们应采用以下原则：

如果模块间必须 存在耦合，就尽量使用数据耦合，少用控制耦合，限制公共耦合的范围，尽量避免使用内容耦合



### 内聚与耦合 

**内聚标志一个模块内各个元素彼此结合的紧密程度**，它是信息隐蔽和局部化概念的自然扩展。

内聚是从 功能角度来度量模块内的联系，一个好的内聚模块应当恰好做一件事。它描述的是模块内的功能联系。耦合是软件结构中各模块之间相互连接的一种度量，耦合强弱取决于模块间接口的复杂程度、进入或访问一个模块的点以及通过接口的数据。 程序讲究的是低耦合，高内聚。就是同一个模块内的各个元素之间要高度紧密，但是各个模块之 间的相互依存度却要不那么紧密。 内聚和耦合是密切相关的，同其他模块存在高耦合的模块意味着低内聚，而高内聚的模块意味着该模块同其他模块之间是低耦合。在进行软件设计时，应力争做到高内聚，低耦合。 



### 工厂模式解耦

```java
//什么是工厂
//工厂就是负责给我们从容器中获取指定对象的类
package com.tutu.factury;

import java.io.IOException;
import java.io.InputStream;
import java.util.Enumeration;
import java.util.HashMap;
import java.util.Map;
import java.util.Properties;

public class beanFactory {
    private static Properties properties;
    //定义一个Map,用于存放我们要创建的对象。我们把它称之为容器
    private static Map<String,Object> beans;

    static {
        properties = new Properties();
        InputStream is = beanFactory.class.getClassLoader().getResourceAsStream("Bean.properties");
        try {
            properties.load(is);
            beans =  new HashMap<String, Object>();
            //取出配置文件中所有的Key
            Enumeration keys = properties.keys();
            while (keys.hasMoreElements()){
                //取出每个Key
                String key = keys.nextElement().toString();
                //根据key获取value
                String beanPath = properties.getProperty(key);
                //反射创建对象
                Object value = Class.forName(beanPath).newInstance();
            }
        } catch (IOException | ClassNotFoundException e) {
            //初始化异常
            throw new ExceptionInInitializerError("初始化properties失败！");
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        }
    }
    public static Object getBean(String BeanName){
        return beans.get(BeanName);
    }
}



accountService=com.tutu.service.Impl.AccountServiceImpl
accountDao=com.tutu.dao.impl.AccountDaoImpl


private AccountDao dao = (AccountDao) beanFactory.getBean("accountDao");


```



### Bean

#### 创建Bean的三种方式

Bean标签属性

```properties
factory-bean:工厂类的名称
factory-method:工厂类种的方法   
id: Bean的id
class: Bean所对应的类
scope: 指定bean的作用范围
init-method: 在bean对象创建时执行的方法
destroy-method: 在bean对象销毁时执行的方法
```

第一种：使用默认构造

在spring的配置文件中使用bean标签，配以**id和class属性之后**，且**没有其他属性和标签时**，采用的就是默认构造函数创建bean对象，此时如果**类**中**没有默认构造函数，则对象无法创建**

```xml
<bean id="accountdao" class="com.dao.impl.AccountdaoImpl"></bean>
```

第二种：使用普通工厂类中的方法创建

一般这种方式是针对，很多无法修改的jar包，**其中很多构造方法都被使用**，所以需要一个**工厂类，用来创建需要类的对象**

```xml
<bean id="instanceFactory" class="com.factory.InstanceFactory"></bean>
<bean id="accountdao" 
        factory-bean="Factory" factory-method="getAccountdaoe"></bean>
```

工厂类

```java
public class Factory{
    public void getAccountdaoe(){ return new AccountDaoImpl();}
}
```

第三种：使用工厂中的静态方法创建对象

```xml
<bean id="accountService" class="com.factory.StaticFactory" 
                            factory-method="getAccountService"></bean>
```

工厂类

```java
public class Factory{
    public void static getAccountdaoe(){ return new AccountDaoImpl();}
}

```

#### Bean的作用范围

bean标签的**scope**属性：

​            作用：**用于指定bean的作用范围**

​            取值： 常用的就是单例的和多例的

```properties
singleton():单例的（默认值）
prototype():多例的
request():作用于web应用的请求范围
session():作用于web应用的会话范围
global-session:作用于集群环境的会话范围（全局会话范围）
                //当不是集群环境时，它就是session
```

例子

```xml
<bean id="" class="" scope="prototype"></bean>
```



#### Bean的生命周期

**单例对象**

​                出生：当**容器创建**时对象出生

​                存活：只要**容器还在，对象一直存活**

​                死亡：**容器销毁**，对象消亡

​                总结：单例对象的生命周期和容器相同

**多例对象**

​                出生：当我们**使用对象时**spring框架为我们创建

​                存活：对象只要是在**使用过程中就一直活着**

​                死亡：**当对象长时间不用，且没有别的对象引用时，由Java的垃圾回收器回收**

```
<bean scope="" init-method="init" destroy-method="destroy"></bean>
```





### IOC的使用

1，xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

2，把对象的创建交给spring来管理

```xml
<bean id="accountDao" class="com.tutu.dao.Impl.AccountDaoImpl"></bean>
```



3，获取ioc容器

方式一

```java
ApplicationContext ac =new ClassPathXmlApplicationContext("bean.xml");
//path:指的是xml文件的绝对路径 C:\\Users\\Desktop\\bean.xml
ApplicationContext ac = new FileSystemXmlApplicationContext("path");
//它是用于读取注解创建容器的
AnnotationConfigApplicationContext


IAccountService as  = (IAccountService)factory.getBean("accountService");
```

方式二

```java
//注意导包
import org.springframework.core.io.Resource;

Resource resource = new ClassPathResource("bean.xml");
BeanFactory factory = new XmlBeanFactory(resource);

AccountDao dao = factory.getBean("accountDao", AccountDao.class);
```

4，使用容器