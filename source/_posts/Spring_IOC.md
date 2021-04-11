## 使用com.alibaba.druid连接mysql数据库时异常：

com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: Could not create connection to database server [duplicate]

Server version: 8.0.23

解决方法：

mysql驱动的版本指定为8.0.11 （报错前为5.1.39）

```xml
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.11</version>
        </dependency>
```





## 使用com.zaxxer.hikari.HikariDataSource连接mysql数据库时异常：

Exception in thread "main" java.lang.RuntimeException: Failed to get driver instance for jdbcUrl=jdbc:mysql://localhost:3306/learnjdbc?useSSL=false

Caused by: java.sql.SQLException: No suitable driver

原因：居然是maven依赖里没有写mysql-connector-java驱动。。。服了



spring容器配置文件applicationContext.xml中加载属性配置文件jdbc.properties，需要在applicationContext.xml中的配置如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
  
		<!--    加载配置文件-->
    <context:property-placeholder location="classpath:jdbc.properties"/>
  
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
</beans>
```



## 2. Spring注解开发

### 2.1 Spring原始注解

Spring是轻代码而重配置的框架，配置比较繁重，影响开发效率，所以注解开发是一种趋势，注解代替xml配置文件可以简化配置，提高开发效率。 

Spring原始注解主要是替代<Bean>的配置

| 注解           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| @Component     | 使用在类上用于实例化Bean                                     |
| @Controller    | 使用在web层类上用于实例化Bean                                |
| @Service       | 使用在service层类上用于实例化Bean                            |
| @Repository    | 使用在dao层类上用于实例化Bean                                |
| @Autowired     | 使用在字段上用于根据类型依赖注入                             |
| @Qualifier     | 结合@Autowired一起使用用于根据名称进行依赖注入  @Qualifier("id") |
| @Resource      | 相当于@Autowired+@Qualifier，按照名称进行注入  @Resource(name="id") |
| @Value         | 注入普通属性                                                 |
| @Scope         | 标注Bean的作用范围                                           |
| @PostConstruct | 使用在方法上标注该方法是Bean的初始化方法                     |
| @PreDestroy    | 使用在方法上标注该方法是Bean的销毁方法                       |






```java
@Component("userDao")
//相当于<bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
public class UserDaoImpl implements UserDao {
    public void save() {
        System.out.println("save running...");
    }
}

@Component("userService")
public class UserServiceImpl implements UserService {


    //<property name="userDao" ref="userDao"></property>
    @Autowired //按照数据类型从Spring容器中进行匹配的
    @Qualifier("userDao")  //是按照id值从容器中进行匹配的 但是主要此处@Qualifier结合@Autowired一起使用
    private UserDao userDao;

    public void save() {
        userDao.save();
    }
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
}
```



还要在xml文件中配置：

```xml
<!--  配置组件扫描，注入bean的时候会去扫描com.itheima包下的类及子包文件寻找对应的bean--> 
<context:component-scan base-package="com.itheima"></context:component-scan>
```

测试代码：

```java
public class UserController {
    public static void main(String[] args) {
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = app.getBean(UserService.class);
        userService.save();
    }

}
```



### 2.2 Spring新注解

| 注解            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| @Configuration  | 用于指定当前类是一个 Spring   配置类，当创建容器时会从该类上加载注解 |
| @ComponentScan  | 用于指定 Spring   在初始化容器时要扫描的包。   作用和在 Spring   的 xml 配置文件中的   <context:component-scan   base-package="com.itheima"/>一样 |
| @Bean           | 使用在方法上，标注将该方法的返回值存储到   Spring   容器中   |
| @PropertySource | 用于加载.properties   文件中的配置                           |
| @Import         | 用于导入其他配置类                                           |

@PropertySource及引入外部bean用法：

```java
@PropertySource("classpath:jdbc.properties")
public class DataSourceConfiguration {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;


    @Bean("dataSource")
    public DataSource getDataSource() throws PropertyVetoException {
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        dataSource.setDriverClass(driver);
        dataSource.setJdbcUrl(url);
        dataSource.setUser(username);
        dataSource.setPassword(password);
        return dataSource;
    }
}
```

@ComponentScan用法，@Import用法：

```java
@Configuration
@ComponentScan("com.itheima")
@Import({DataSourceConfiguration.class})
public class SpringConfiguration {

}
```

测试获得bean：

```java
        ApplicationContext app = new AnnotationConfigApplicationContext(SpringConfiguration.class);
        UserService userService = app.getBean(UserService.class); //获得userService
        userService.save();
        DataSource dataSource=app.getBean(DataSource.class); //获得dataSource
        System.out.println(dataSource.getConnection());
```







## 3. Spring整合Junit

### 3.1 原始Junit测试Spring的问题

在测试类中，每个测试方法都有以下两行代码：

```java
 ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
 IAccountService as = ac.getBean("accountService",IAccountService.class);
```

这两行代码的作用是获取容器，如果不写的话，直接会提示空指针异常。所以又不能轻易删掉。

### 3.2 上述问题解决思路

让SpringJunit负责创建Spring容器，但是需要将配置文件的名称告诉它

将需要进行测试Bean直接在测试类中进行注入

### 3.3 Spring集成Junit步骤

①导入spring集成Junit的坐标

②使用@Runwith注解替换原来的运行期

③使用@ContextConfiguration指定配置文件或配置类

④使用@Autowired注入需要测试的对象

⑤创建测试方法进行测试

### 3.4 Spring集成Junit代码实现

①导入spring集成Junit的坐标

```xml
<!--此处需要注意的是，spring5 及以上版本要求 junit 的版本必须是 4.12 及以上-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.0.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```

②使用@Runwith注解替换原来的运行期

```java
@RunWith(SpringJUnit4ClassRunner.class)
public class SpringJunitTest {
}
```

③使用@ContextConfiguration指定配置文件或配置类

```java
@RunWith(SpringJUnit4ClassRunner.class)
//加载spring核心配置文件，xml文件的方式
//@ContextConfiguration(value = {"classpath:applicationContext.xml"})
//加载spring核心配置类,注解的方式
@ContextConfiguration(classes = {SpringConfiguration.class})
public class SpringJunitTest {
}
```

④使用@Autowired注入需要测试的对象

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {SpringConfiguration.class}) //注意classes后面是数组
public class SpringJunitTest {
    @Autowired
    private UserService userService;
}
```

⑤创建测试方法进行测试

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {SpringConfiguration.class})
public class SpringJunitTest {
    @Autowired
    private UserService userService;
    @Test
    public void testUserService(){
   	 userService.save();
    }
}
```

Spring集成Junit步骤

①导入spring集成Junit的坐标

②使用@Runwith注解替换原来的运行期

③使用@ContextConfiguration指定配置文件或配置类

④使用@Autowired注入需要测试的对象

⑤创建测试方法进行测试



