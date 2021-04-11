### 1.Spring 的 AOP 简介

#### 1.1 什么是 AOP 

AOP 为 Aspect Oriented Programming 的缩写，意思为面向切面编程，是通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。

AOP 是 OOP 的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。



### 2. 基于 XML 的 AOP 开发

#### 2.1 快速入门

①导入 AOP 相关坐标

②创建目标接口和目标类（内部有切点）

③创建切面类（内部有增强方法）

④将目标类和切面类的对象创建权交给 spring

⑤在 applicationContext.xml 中配置织入关系

⑥测试代码



①导入 AOP 相关坐标

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.4</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.4</version>
        </dependency>
    </dependencies>
```

②创建目标接口和目标类（内部有切点）

```java
public interface TargetInterface {
    public void method();
    public void open();
    public void close();
}

public class Target implements TargetInterface {
    public void method(){
        System.out.println("method running...");
    }
    public void open(){
        System.out.println("open running...");
    }
    public void close(){
        System.out.println("close running...");
    }
}
```

③创建切面类（内部有增强方法）

```java
public class MyAspect {
    public void before(){
        System.out.println("before reinforcement");
    }
    public void after(){
        System.out.println("after reinforcement");
    }

    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("around before...");
        Object obj=proceedingJoinPoint.proceed();
        System.out.println("around after...");
        return obj;
    }
    public void afterThrowing(){
        System.out.println("Throw Exception...");
    }

    public void afterFinal(){
        System.out.println("afterFinal...");
    }
}
```

④将目标类和切面类的对象创建权交给 spring

```xml
    <!--配置目标类-->
	<bean id="target" class="com.itheima.aop.Target"></bean>
    <!--配置切面类-->
    <bean id="myAspect" class="com.itheima.aop.MyAspect"></bean>
```

⑤在 applicationContext.xml 中配置织入关系

导入aop命名空间:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 					 				http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
```



配置切点表达式和前置增强的织入关系:

```xml
    <aop:config>
        <aop:aspect ref="myAspect">
<!--            <aop:after method="afterFinal" pointcut="execution(public void com.itheima.aop..Target.*(..))"></aop:after>-->
<!--            <aop:around method="around" pointcut="execution(public void com.itheima.aop..Target.*(..))"></aop:around>-->
            <aop:pointcut id="myPointcut" expression="execution(public void com.itheima.aop..Target.*(..))"/>
<!--            <aop:after-throwing method="afterThrowing" pointcut="execution(public void com.itheima.aop..Target.*(..))"></aop:after-throwing>-->
            <aop:after-returning method="after" pointcut-ref="myPointcut"></aop:after-returning>
<!--            <aop:before method="before" pointcut="execution(public void com.itheima.aop.Target.method())"></aop:before>-->
        </aop:aspect>
    </aop:config>
```



测试类：

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class AopTest {
    @Autowired
    private TargetInterface target; //为什么当注入的bean：target继承了TargetInterface后，这里必须使用TargetInterface的声明
    // 当换成Target类就不行了：Unsatisfied dependency expressed through field 'target'; nested exception is org.springframework.beans.factory.BeanNotOfRequiredTypeException: Bean named 'target' is expected to be of type 'com.itheima.aop.Target' but was actually of type 'com.sun.proxy.$Proxy15'

    @Autowired
    private Target testTarget;
    @Test
    public void test1(){
        target.method();
        target.open();
        target.close();
    }
}
```



- 运行结果

```java
method running...
after reinforcement
open running...
after reinforcement
close running...
after reinforcement
```



#### 2.2 XML 配置 AOP 详解

##### 1) 切点表达式的写法

表达式语法：

```java
execution([修饰符] 返回值类型 包名.类名.方法名(参数))
```

- 访问修饰符可以省略

- 返回值类型、包名、类名、方法名可以使用星号*  代表任意

- 包名与类名之间一个点 . 代表当前包下的类，两个点 .. 表示当前包及其子包下的类

- 参数列表可以使用两个点 .. 表示任意个数，任意类型的参数列表

例如：

```xml
execution(public void com.itheima.aop.Target.method())	
execution(void com.itheima.aop.Target.*(..))
execution(* com.itheima.aop.*.*(..))
execution(* com.itheima.aop..*.*(..))
execution(* *..*.*(..))
```

##### 2) 通知的类型

通知的配置语法：

```xml
<aop:通知类型 method=“切面类中方法名” pointcut=“切点表达式"></aop:通知类型>
```

![](/Users/hulang/share/03-就业课(2.1)-Spring/day03_ AOP简介/笔记/img\图片5.png)

##### 3) 切点表达式的抽取

当多个增强的切点表达式相同时，可以将切点表达式进行抽取，在增强中使用 pointcut-ref 属性代替 pointcut 属性来引用抽取后的切点表达式。

```xml
<aop:config>
    <!--引用myAspect的Bean为切面对象-->
    <aop:aspect ref="myAspect">
        <aop:pointcut id="myPointcut" expression="execution(* com.itheima.aop.*.*(..))"/>
        <aop:before method="before" pointcut-ref="myPointcut"></aop:before>
    </aop:aspect>
</aop:config>
```

#### 

### 3.基于注解的 AOP 开发

#### 3.1 快速入门

基于注解的aop开发步骤：

①创建目标接口和目标类（内部有切点）

②创建切面类（内部有增强方法）

③将目标类和切面类的对象创建权交给 spring

④在切面类中使用注解配置织入关系

⑤核心配置类写上开启扫包和自动代理注解

⑥测试

- 创建目标接口和目标类（内部有切点）：

```java
public interface TargetInterface {
    public void method();
    public void open();
    public void close();
}

@Component("target")
public class Target implements TargetInterface {
    public void method(){
        System.out.println("method running...");
    }
    public void open(){
        System.out.println("open running...");
    }
    public void close(){
        System.out.println("close running...");
    }
}

```

- 创建切面类（内部有增强方法） 并使用注解配置织入关系

```java
@Component("myAspect")
@Aspect
public class MyAspect {

    @Before("MyAspect.myPoint()")
    public void before() {
        System.out.println("before reinforcement");
    }

    @AfterReturning("myPoint()")
    public void after() {
        System.out.println("after reinforcement");
    }

    @Around("execution(public void com.itheima.anno.*.*(..))")
    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("around before...");
        Object obj = proceedingJoinPoint.proceed();
        System.out.println("around after...");
        return obj;
    }

    public void afterThrowing() {
        System.out.println("Throw Exception...");
    }

    public void afterFinal() {
        System.out.println("afterFinal...");
    }

    @Pointcut("execution(public void com.itheima.anno.*.*(..))")
    public void myPoint(){}
}
```



- 核心配置类写上开启扫包和自动代理注解

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ComponentScan("com.itheima.anno")
@ContextConfiguration(classes = {AnnoTest.class})
@EnableAspectJAutoProxy
//@ContextConfiguration("classpath:applicationContext-anno.xml")
public class AnnoTest {
    @Autowired
    private TargetInterface target;

    @Test
    public void test1(){
        target.method();
    }
}
```

- 运行结果：

```java
around before...
before reinforcement
method running...
after reinforcement
around after...
```



