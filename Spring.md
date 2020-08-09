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

#### 基于xml形式的

配置文件

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

第一个*：表示返回值
第二个*：表示类
第三个*：表示方法名
第四个..：表示参数
//配置切入点表达式 id属性用于指定表达式的唯一标识
//expression属性用于指定表达式内容      
```

