# 对象转型

## 向上转型

在 Java 继承体系中，认为基类在上层，导出类在下层，因此向上转型的意思就是把子类对象转成父类类型，即将父类的引用指向子类对象。向下转型的意思就是把父类对象转成子类类型，即将子类的引用指向父类对象

**父类引用指向子类对象的时候，该引用能访问到的只是作为父类的那部分所拥有的属性和方法，至于作为子类的那部分该引用看不到。如果需要访问子类的所有方法和属性，则需要进行*对象转型*，即强制类型转换。**

## 向下转型

	float—>int；double—>float等等





# 动态绑定—多态

**动态绑定使得程序的可扩展性达到了最好**

## 满足条件

继承 + 重写 + 父类引用指向子类对象

## 动态绑定

动态绑定是在程序运行时确定的，子类继承父类了父类的成员，同时子类又有自己的私有属性和方法，然后父类又作为另一个类中的成员变量，逻辑如下：

```java
// 基类
Class Animal{
    String name;
    public void action(){
        ...
    }
}
// 子类
Class Cat extends Animal{
    String name;
    String addr;
    public void action(){
        ...
    }
}
class Person{
    String name;
    Animal animal;
    ...
}
public class Action{
    public static void main(String[] args){
        Animal a = new Cat();
		Person p = new Person(name, a);
    	a.action();	        
    }
}
```

如上，初始化Action对象并new对象时，如何判断执行执行哪个action()方法？如果是根据引用类型，则执行基类的action方法；而在动态绑定中（多态）中，采用的是根据实际类型，即在程序中new的谁则调用的就是谁的action方法。**深入理解该机制，当实际中找执行哪个action的过程中，在基类内部有个指针指向自己的action方法，当new某个集成于该基类的子类对象时，这个指针则改变指向，即指向刚new的这个子类的action方法，这就是动态绑定机制。**<u>只有当程序运行起期间，new出了对象后才会判断要调用哪个的方法。</u>

![动态绑定](/Users/walker/notebook/note/pic/动态绑定.png)

**子类的test方法会出现在基类的相同位置**













# 注解

*package：java.lang.annotation*

![Java注解](/Users/walker/notebook/note/pic/Java注解.jpg)

## meta-annotation

Java定义了4个标准的meta-annotation类型，被用来提供对其他annotation类型作说明：

- @Target
- @Retention
- @Documented
- @Inherited

### @Target

描述注解的使用范围，取值ElementType有：

+ CONSTRUCTOR:用于描述构造器 　
+ FIELD:用于描述域 　
+ LOCAL_VARIABLE:用于描述局部变量 　
+ METHOD:用于描述方法 　
+ PACKAGE:用于描述包 　
+ PARAMETER:用于描述参数 　
	 TYPE:用于描述类、接口(包括注解类型) 或enum声明	

### @Retention

定义了当前annotation被保留的时间长短，**表示需要在什么级别保存该注解信息，用于描述注解的生命周期（被描述的注解在什么范围内有效）**，取值RetentionPolicy有：

+ SOURCE：在原文件中有效
+ CLASS：在Class字节码文件中有效
+ RUNTIME：只在运行时有效，注解保存在JVM运行时刻,能够在运行时刻通过反射API来获取到注解的信息

### @Documented

是否将注解信息添加在Java文档中，可以被javadoc工具文档化

### @Inherited

表示某个被标注的类型是被继承的，如果一个使用好了@Inherited注解修饰的annotation类型被用于一个Class，则这个annotation也被用于该class的子类















# SpringBoot

## 启动类

@SpringBootApplication相当于

+ @Configuration（@SpringBootConfiguration点开查看发现里面还是应用了@Configuration）
+ @EnableAutoConfiguration
+ @ComponentScan

### @Configuration

+ 之前Spring的配置如下:

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"
       default-lazy-init="true">
    <!--bean定义-->
    <bean id="userService" class="..UserServiceImpl">
    	...
	</bean>
</beans>
```

+ 基于JavaConfig的配置：

```
@Configuration
public class DemoConfiguration{
    //bean定义
    @Bean
    public UserService userService(){
        return new UserServiceImpl();
    }
}
```

**任何一个标注了@Bean的方法，其返回值将作为一个bean定义并注册到Spring的IoC容器，方法名将默认成该bean定义的id**

### @ComponentScan

自动扫描并加载**符合条件**的组件，如@Component、@Repository和其他定义的bean，并将这些beans定义加载到Ioc容器。

![@EnableAutoConfiguration组件示意图](/Users/walker/notebook/note/pic/@EnableAutoConfiguration组件示意图.png)

可以通过basePackages等属性来细粒度定制该注解自动扫描的范围，如果不指定，框架会从声明@ComponentScan注解所在类的package进行扫描（这也是为什么默认情况下启动类在root package）

###@EnableAutoConfiguration

借助@Import的支持，手机和注册特定场景相关的bean定义

***@EnableAutoConfiguration也是借助@Import的帮助，将所有符合自动配置条件的bean定义加载到IoC容器***

@EnableAutoConfiguration注解中，最关键的要属`@Import(EnableAutoConfigurationImportSelector.class)`，借助EnableAutoConfigurationImportSelector，@EnableAutoConfiguration可以帮助SpringBoot应用将所有符合条件的@Configuration配置都加载到当前SpringBoot创建并使用的IoC容器

















# 公司Java内部服务流程

![公司Java服务流程](/Users/walker/notebook/note/pic/公司Java服务流程.png)

## 目前用到的公共组件

+ RabbitMQ
+ Memcached：Java中先从cache中获取数据，如果为空，则空DB获取，然后再写到cache中，如findById源码
+ JSON
+ RPC
+ RabbitMQ：MQ采用exchange的type默认为direct，即只通过routingkey将msg发送到对应的Queue，Queue的建立是由消费者决定的，两端只需要协调routingkey即可
+ JdbcDAO
+ Scheduled：马上用到，处理定时任务，用到MQ，使用@TaskService和@TaskMethod确定

## 服务流程

### 请求处理

通过@RestController和@xxxMappting确定SpringMVC接收HTTP请求，然后核心控制器拦截并分发请求，Controller调用Service处理。

![SpringMVC工作原理](/Users/walker/notebook/note/pic/SpringMVC工作原理.png)

### Ioc容器

控制反转（依赖注入）ioc，di，aop，控制bean的生命周期

### JDBCTemplate

公司框架没有用到类似于Hibernate、Mybatis这些框架

Spring中关于JDBC的一个辅助类JDBCTemplate，封装了原生的JDBC,可以较为方便的进行数据库操作

Spring提供了JdbcDaoSupport支持类，所有DAO继承这个类，就会自动获得JdbcTemplate（前提是注入DataSource）

```java
// 简单查询，按照ID查询，返回字符串	
public String searchUserName(int id) {
    String sql = "select username from user where id=?";	
    // 返回类型为String(String.class)
    return this.getJdbcTemplate().queryForObject(sql, String.class, id);
}
```

+ Spring 为每种持久化技术 提供一个支持类，在DAO 中注入 模板工具类
  * JDBC ：	org.springframework.jdbc.core.support.JdbcDaoSupport
  * Hibernate 3.0 ：org.springframework.orm.hibernate3.support.HibernateDaoSupport
  * MyBatis ：org.springframework.orm.ibatis.support.SqlMapClientDaoSupport
+ SptingBoot中直接注入JDBCTemplate也可以实现对数据库的访问：

```
@Service
public class UserServiceImpl implements UserService {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public void create(String name, Integer age) {
        jdbcTemplate.update("insert into USER(NAME, AGE) values(?, ?)", name, age);
    }

    @Override
    public void deleteByName(String name) {
        jdbcTemplate.update("delete from USER where NAME = ?", name);
    }

    @Override
    public Integer getAllUsers() {
        return jdbcTemplate.queryForObject("select count(1) from USER", Integer.class);
    }

    @Override
    public void deleteAllUsers() {
        jdbcTemplate.update("delete from USER");
    }
}
```

+ 公司的JdbcDAO组件在封装了JDBCTemplate，并在基础上丰富了操作数据库的方法；另一方面，JdbcDAO中调用相关操作方法时，会对组装SQL语句；也可以自己手动写类似于连表查询等这种湘桂灵活的SQL语句。



# UML图

**注：‘------’为虚线；'——'为实线**

+ 继承

  B继承A类：B————▷A

+ 实现

  B实现了A接口：B--------▷A

+ 依赖

  A依赖于B，如A的某个方法的参数为B：B-------->A

+ 关联

  A关联B，A的一个属性是B：B———>A