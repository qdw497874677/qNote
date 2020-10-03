[**首页**](https://github.com/qdw497874677/myNotes/blob/master/首页检索.md)

# 面试问题

## IOC

一个业务的架构主要由，javaBean（组件类）、业务逻辑处理（业务类，来协调实体类和组件类来完成业务逻辑）、POJO（简单实体类）

为了方便组件的扩展，一类组件往往用工厂模式。如果业务类对组件的控制力过强，就不是很灵活。将组件的创建和属性注入交给IOC容器来托管。变更组件和注入不同的属性都可以通过文件配置的方式，让IOC容器理解，可以减少变更的代码量。

控制翻转。是一种思想。将组件的创建和协调的工作交给Spring容器，在需要的地方让Spring容器把依赖的对象注入进来。降低了代码之间的耦合度，将组件的选择和属性的注入放到配置文件中，将组件的控制权交给IOC容器

### 原理

利用反射，通过类名等信息利用反射实例化对象



## Spring启动过程

通过ClassPathXmlApplicationContext来创建一个Spring容器。

ClassPathXmlApplicationContext通过多次继承才继承到ApplicationContext 接口

**ApplicationContext实际上就是一个BeanFactory**

回到ClassPathXmlApplicationContext，他里面有一个refresh方法（加锁的方法）来重新初始化ApplicationContext，第一次初始化也是从这开始。

~~~java
@Override
	public void refresh() throws BeansException, IllegalStateException {
		synchronized (this.startupShutdownMonitor) {
			// Prepare this context for refreshing.
			prepareRefresh();

			// Tell the subclass to refresh the internal bean factory.
			ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

			// Prepare the bean factory for use in this context.
			prepareBeanFactory(beanFactory);

			try {
				// Allows post-processing of the bean factory in context subclasses.
				postProcessBeanFactory(beanFactory);

				// Invoke factory processors registered as beans in the context.
				invokeBeanFactoryPostProcessors(beanFactory);

				// Register bean processors that intercept bean creation.
				registerBeanPostProcessors(beanFactory);

				// Initialize message source for this context.
				initMessageSource();

				// Initialize event multicaster for this context.
				initApplicationEventMulticaster();

				// Initialize other special beans in specific context subclasses.
				onRefresh();

				// Check for listener beans and register them.
				registerListeners();

				// Instantiate all remaining (non-lazy-init) singletons.
				finishBeanFactoryInitialization(beanFactory);

				// Last step: publish corresponding event.
				finishRefresh();
			}

			catch (BeansException ex) {
				if (logger.isWarnEnabled()) {
					logger.warn("Exception encountered during context initialization - " +
							"cancelling refresh attempt: " + ex);
				}

				// Destroy already created singletons to avoid dangling resources.
				destroyBeans();

				// Reset 'active' flag.
				cancelRefresh(ex);

				// Propagate exception to caller.
				throw ex;
			}

			finally {
				// Reset common introspection caches in Spring's core, since we
				// might not ever need metadata for singleton beans anymore...
				resetCommonCaches();
			}
		}
	}
~~~



1. 准备工作：prepareRefresh()。记录容器启动时间，标记已启动状态，处理配置文件占位符等等。
2. 加载配置。ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();配置的载体有xml文件、配置类、注解。将配置信息加载成BeanDefinition注册到容器容器中。还没有初始化。
   1. 这个注册就是向容器中的一个map中添加键值对，key是beanName，value是BeanDefinition对象。还有一个map保存别名到beanName的映射。
   2. 
3. 注册和执行  **BeanFactory容器后处理器**  实例。在实例化单例之前执行的。开发者可以通过实现类拓展。
   1. 注册：postProcessBeanFactory(beanFactory);
      1. 通过BeanFactoryPostProcessor的实现类，开发者可以修改BeanDefinition对象等操作。
      2. 这个接口的实现类怎么提供给spring呢。一个是可以将实现类通过xml配置bean的方式写到配置文件中。在容器启动时，会检查所有BeanDefinition信息，先将这个接口的实现类抽取出来，通过反射创建实例，执行接口方法。
   2. 执行：invokeBeanFactoryPostProcessors(beanFactory);
      1. 调用
4. 注册 Bean后处理器  实例。registerBeanPostProcessors(beanFactory);bean后处理器的实例中有两个方法会在每个bean初始化的前后去执行。
5. 国际化处理：initMessageSource();
6. 初始化当前 ApplicationContext 的事件广播器：initApplicationEventMulticaster();
7. 初始化一些特殊bean：onRefresh();在初始化单例bean之前。
8. 注册事件监听器：registerListeners();监听器需要实现 ApplicationListener 接口
9. 初始化所有单例bean：finishBeanFactoryInitialization(beanFactory);懒加载的除外。
10. 广播事件，初始化完成：finishRefresh();



## AOP概念

面向切面编程。

在方法或者异常前后去做一些逻辑操作。这样可以把一些分散的组件从业务代码中提取出来，使这段代码可复用，并提高可拓展性。

实现AOP分为两种：

- 动态代理：Spring中用动态代理实现AOP
  - 针对实现了接口的类用JDK默认的动态代理。（基于接口）
  - CGLIB动态代理。（基于类）
    - 
- 静态织入（静态代理）：让编译器在编译期间，织入代码。

应用场景：

- 记录日志
- 权限控制
- 缓存优化（第一次调用查数据库，第二次调用返回缓存中的数据）
- 事务管理（方法前开启事务，调用完成后提交关闭事务）

### 使用

加上注解@Aspect、@Component标识切面组件

在切面组件中定义各种通知方法，定义切点，在通知方法上定义连接点



## Spring常用注解

- 标识组件
  - @Component
  - @Repository
  - @Service
  - @Controller
- 自动装配Bean
  - @Autowired





## SpringMVC

MVC是一种设计模式，将web层进行解耦（**用户**与**控制器**之间发送请求、返回响应；**控制器**与模型之间数据处理、返回结果；**控制器**与**视图**之间推送模型，响应结果）。SpringMVC就是在Spring平台上的实现的MVC开发框架。

主要核心就是：DispatcherServlet把收到的请求分别依次委托给各个组件去处理：HanderMapping（处理器映射器）、HanderAdapter（处理器适配器）去做处理返回ModelAndView、ViewResolver（视图解析器）把ModelAndView解析为View、View（视图）最后处理把最终结果返回给DispatcherServlet。DispatcherServlet对请求作出响应。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190630145911981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdGlhbnhpYW5nX2thb2xh,size_16,color_FFFFFF,t_70)



## SpringMVC常用注解

- 绑定请求映射地址
  - @RequestMapping
  - @GetMapping
  - @PostMapping
- 返回数据而不是返回页面
  - @ResponseBody
- 接收参数
  - @RequestParam：获取url后面username="qdw"这种形式的参数。
  - @RequestBody：从body中获取参数
  - @PathVariable：获取在url地址中的一部分。在mapping中的url为/edit/{id}/{name}。



## Springboot

简化了Spring中的各种配置，不需要xml配置文件来配置各种bean的关系了

SpringBoot的配置原理

约定大于配置

通过application.yml文件

### 简化Bean配置

怎么让jar中的类作为Bean让Spring管理呢？

通过注解@Configuration 和@Bean

在加上@Configuration的类（表示配置类）中的一个方法上加上@Bean，方法返回的就作为Bean让容器管理了。

### 简化部署

SpringBoot内置了tomcat，将项目打成jar包可以直接启动web项目

### 怎么配置（待更新！！！）



### 配置原理（待更新！！！）



## 动态代理详解（待更新！！）

https://www.jianshu.com/p/aaeb2355ec5c

### JDK Proxy

~~~java
public static Object newProxyInstance(ClassLoader loader,
                                      Class<?>[] interfaces,
                                      InvocationHandler h)
~~~



态代理类（Proxy）类中的创建代理对象的方法，下面介绍一下方法的三个参数：

- ClassLoader loader：方法需要动态生成一个类，这个类实现了A和B两个接口，然后创建这个类的对象。需要生成一个类，而且这个类也需要加载到方法区中，所以我们需要一个ClassLoader来加载该类

- Class<?>[] interfaces：我们需要代理对象实现的数组

- InvocationHandler h：调用处理器



## spring循环依赖

什么是循环依赖：两个或两个以上的Bean互相持有对方，最终形成闭环。比如A中有B属性，B中有C属性，C中有A属性。

### Spring怎么处理

Spring的单例对象的初始化分为三步：

1. createBeanInstance：实例化，就是调用对象的构造方法实例化对象
2. populateBean：填充属性，主要是填充对其他bean的依赖
3. initializeBean：调用spring xml中的init方法

循环依赖主要发生在一、二中，也就是构造器循环依赖和field循环依赖。

Spring为了解决单例的循环依赖问题使用了**三级缓存**：

1. 三级缓存：singletonFactories：单例对象工厂的cache
2. 二级缓存：earlySingletonObjects：提前曝光的单例对象的cache
3. 一级缓存：singletonObjects：单例对象的cache

从缓存中获取bean用getSingleton()，先从一级缓存然后二级然后三级。

在一个bean A创建的第一步就把自己放到三级缓存中了，发现自己依赖B，从缓存中没找到创建B把B获取，B创建的时候把自己放入三级缓存，发现自己依赖A就去缓存中获取到不完整的A，然后B完成所有的初始化进入了一级缓存。A拿到了完整的B（B获取到了A的引用）

所以Spring不能解决构造器的循环引用



## Bean的作用域

五种作用域

| 作用域      | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| singleton   | 在spring IoC容器仅存在一个Bean实例，Bean以单例方式存在，bean作用域范围的默认值。 |
| prototype   | 每次从容器中调用Bean时，都返回一个新的实例，即每次调用getBean()时，相当于执行newXxxBean()。 |
| request     | 每次HTTP请求都会创建一个新的Bean，该作用域仅适用于web的Spring WebApplicationContext环境。 |
| session     | 同一个HTTP Session共享一个Bean，不同Session使用不同的Bean。该作用域仅适用于web的Spring WebApplicationContext环境。 |
| application | 限定一个Bean的作用域为`ServletContext`的生命周期。该作用域仅适用于web的Spring WebApplicationContext环境。 |



## Bean的生命周期

从创建Spring容器开始，到Spring容器销毁Bean

- 实例化BeanFactoryPostProcessor实现类
- 执行BeanFactoryPostProcessor的postProcessBeanFactory方法
- 实例化BeanPostProcessor实现类
- 实例化
- 执行前置方法
- 执行Bean的构造器
- 执行前置方法
- 为Bean注入属性
- 设置Bean名字
- 设置Bean工厂
- 执行初始化前置方法



## Spring事务

service实现类或方法中加上@Transactional注解后，在service实现类在被调用时注入的就是service的代理对象



## 用到的设计模式

- **工厂设计模式** : Spring使用工厂模式通过 `BeanFactory`、`ApplicationContext` 创建 bean 对象。
- **代理设计模式** : Spring AOP 功能的实现。
- **单例设计模式** : Spring 中的 Bean 默认都是单例的。
- **模板方法模式** : Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- **包装器设计模式** : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- **观察者模式:** Spring 事件驱动模型就是观察者模式很经典的一个应用。
- **适配器模式** :Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller`。



## 过滤器和拦截器的区别

https://www.jianshu.com/p/7bd0cad17f23

# @Scheduled

在Application启动类上添加@EnableScheduling注解开启定时

在组件中用@Scheduled启动定时任务

8种参数

### cron

接收一个cron表达式，是一个字符串，以5或者6个空格隔开，分开共6或7个域，每个域代表一个含义

~~~
[秒] [分] [小时] [日] [月] [周] [年]
~~~

可以省略年

| 序号 | 说明 | 必填 | 允许填写的值   | 允许的通配符  |
| ---- | ---- | ---- | -------------- | ------------- |
| 1    | 秒   | 是   | 0-59           | , - * /       |
| 2    | 分   | 是   | 0-59           | , - * /       |
| 3    | 时   | 是   | 0-23           | , - * /       |
| 4    | 日   | 是   | 1-31           | , - * ? / L W |
| 5    | 月   | 是   | 1-12 / JAN-DEC | , - * /       |
| 6    | 周   | 是   | 1-7 or SUN-SAT | , - * ? / L # |
| 7    | 年   | 否   | 1970-2099      | , - * /       |

###### 通配符说明:

- *****表示所有值。例如在分上，*，表示每分钟都会触发。
- **?**表示不指定值。例如，0 0 0 10 * ?，表示每月10号0点0分0秒触发，不关心是周几。
- **-**表示区间。例如在小时上，10-12，表示10、11、12都会触发。
- **,**表示多个值。例如在周上，MON,WED,FRI，表示周一周三周五触发。
- **/**用于递增触发。例如在秒上，5/15，表示5秒开始，每15秒触发（5,20,35,50）。在日上，1/3，表示每月1号开始，每三天触发一次。
- **L**表示最后。例如在日上，L，表示月的最后一天。在周上，6L，表示月的最后一个周六触发。
- **W**表示离指定日期最近的那个工作日。例如在日上，15W，表示离15号最近的工作日触发。
- **#**表示第几个。例如在周上，6#3，表示每个月的第三个周六。

支持占位符

示例

每隔5秒执行一次：*/5 * * * * ?

每隔1分钟执行一次：0 */1 * * * ?

每天23点执行一次：0 0 23 * * ?

每天凌晨1点执行一次：0 0 1 * * ?

每月1号凌晨1点执行一次：0 0 1 1 * ?

每月最后一天23点执行一次：0 0 23 L * ?

每周星期六凌晨1点实行一次：0 0 1 ? * L

在26分、29分、33分执行一次：0 26,29,33 * * * ?

每天的0点、13点、18点、21点都执行一次：0 0 0,13,18,21 * * ?



### zone

接收时区，类型是：java.util.TimeZone#ID

### fixedDelay

上一次执行完毕时间点之后多长时间再执行

~~~java
@Scheduled(fixedDelay = 5000) // 上次执行完5秒后执行
~~~

### fixedDelayString

与上面意思相同，只是通过字符串，支持占位符。

~~~java
@Scheduled(fixedDelay = "5000") // 上次执行完5秒后执行
~~~

### fixedRate

上一次执行时间点之后多长时间再执行，会重复

### fixedRateString

字符串的上面的

### initialDelay

第一次延迟多久执行

~~~java
@Scheduled(initialDelay=1000, fixedRate=5000) //第一次延迟1秒后执行，之后按fixedRate的规则每5秒执行一次
~~~







## Spring和SpringMVC的启动过程



## Spring用到的设计模式

https://www.cnblogs.com/wangwudi/p/12324974.html



## Bean的作用域

- singleton : 唯一 bean 实例，Spring 中的 bean 默认都是单例的。
- prototype : 每次请求都会创建一个新的 bean 实例。
- request : 每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP request内有效。
- session : 每一次HTTP请求都会产生一个新的 bean，该bean仅在当前 HTTP session 内有效。
- global-session：全局session作用域，仅仅在基于portlet的web应用中才有意义，Spring5已经没有了。Portlet是能够生成语义代码(例如：HTML)片段的小型Java Web插件。它们基于portlet容器，可以像servlet一样处理HTTP请求。但是，与 servlet 不同，每个 portlet 都有不同的会话



## Bean的生命周期

- Bean 容器找到配置文件中 Spring Bean 的定义。
- Bean 容器利用 Java Reflection API 创建一个Bean的实例。
- 如果涉及到一些属性值 利用 `set()`方法设置一些属性值。
- 如果 Bean 实现了 `BeanNameAware` 接口，调用 `setBeanName()`方法，传入Bean的名字。
- 如果 Bean 实现了 `BeanClassLoaderAware` 接口，调用 `setBeanClassLoader()`方法，传入 `ClassLoader`对象的实例。
- 如果Bean实现了 `BeanFactoryAware` 接口，调用 `setBeanClassLoader()`方法，传入 `ClassLoade` r对象的实例。
- 与上面的类似，如果实现了其他 `*.Aware`接口，就调用相应的方法。
- 如果有和加载这个 Bean 的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessBeforeInitialization()` 方法
- 如果Bean实现了`InitializingBean`接口，执行`afterPropertiesSet()`方法。
- 如果 Bean 在配置文件中的定义包含 init-method 属性，执行指定的方法。
- 如果有和加载这个 Bean的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessAfterInitialization()` 方法
- 当要销毁 Bean 的时候，如果 Bean 实现了 `DisposableBean` 接口，执行 `destroy()` 方法。
- 当要销毁 Bean 的时候，如果 Bean 在配置文件中的定义包含 destroy-method 属性，执行指定的方法。

## Spring对事务的支持

spring支持两种方式的事务管理

- 编程式事务管理
- 声明式事务管理：推荐使用。通过AOP实现。最常用使用@Transactional的全注解方式

在service中的一个方法上添加@Transactional注解，拦截的这个方法内抛出了指定异常后，事务会自动回滚。（如果方法内捕捉了，注解就捕获不到了，就不会自动回滚）

默认下只会处理。Error和RuntimeException及其子类的UNChecked异常。一般的Exception这些Checked异常不会发生回滚。

## 事务的传播属性

Spring定义了7中传播行为。主要就是当一个事务方法去调用另一个事务方法，处理这些事务之间的关系。

两个常用的。

- 默认是REQURED：被调用的事务会在现有的事务内运行。
- REQURED_NEW：调用的方法要启动一个新的事务来执行这个方法，当前自己的事务先挂起，当调用的新创建的事务结束后，自己的再继续，



## Spring框架是怎么解决Bean之间的循环依赖的





## SpringMVC单例Bean线程安全怎么解决



- 使用ThreadLocal，ThreadLocal会为每一个线程提供一个独立的变量副本，这样在多线程对数据访问就不会出现冲突。因为每一个线程都拥有自己的变量副本，因此也就不需要同步该变量。ThreadLocal提供了线程安全的共享对象，在编写多线程代码时，可以把不安全的变量封装进ThreadLocal。
- 如果时web应用，可以使用Spring Bean的作用域中的request,在controller类前面加上@Scope("****")，表明每次请求都会生成一个新的Bean对象。这样也能起到线程安全的作用。
- 使用线程同步，关键字synchronized当线程较多时，当一个线程调用该方法时，其他想要调用此方法的线程就要block ，多线程并发量大的时候会对性能有一定的影响。



## 单例Bean的优势

由于不会每次都新创建新对象所以有一下几个性能上的优势：

1. 减少了新生成实例的消耗

新生成实例消耗包括两方面，第一，spring会通过反射或者cglib来生成bean实例这都是耗性能的操作，其次给对象分配内存也会涉及复杂算法。

2. 减少jvm垃圾回收

由于不会给每个请求都新生成bean实例，所以自然回收的对象少了。

3. 可以快速获取到bean

因为单例的获取bean操作除了第一次生成之外其余的都是从缓存里获取的所以很快。

劣势：不能做到线程安全。





## 



# AOP

## 五种通知

配置 对方法切入

~~~java
@Aspect
@Component
public class TransactionAop {
	// 定义切点，之后注解这个切点后，就对切点声明的范围切入
    @Pointcut("execution(* com.jiuxian..service.*.*(..))")
    public void pointcut() {
    }

    @Before("pointcut()")
    public void beginTransaction() {
        System.out.println("before beginTransaction");
    }

    @After("pointcut()")
    public void commit() {
        System.out.println("after commit");
    }

    @AfterReturning("pointcut()", returning = "returnObject")
    public void afterReturning(JoinPoint joinPoint, Object returnObject) {
        System.out.println("afterReturning");
    }

    @AfterThrowing("pointcut()")
    public void afterThrowing() {
        System.out.println("afterThrowing afterThrowing  rollback");
    }

    @Around("pointcut()")
    public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
        try {
            System.out.println("around");
            return joinPoint.proceed();
        } catch (Throwable e) {
            e.printStackTrace();
            throw e;
        } finally {
            System.out.println("around");
        }
    }
}
~~~

配置 对注解切入

~~~java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Log {
    String value() default "";
}
~~~

~~~java
@Aspect
@Component
public class AnnotationAop {

    @Pointcut(value = "@annotation(log)", argNames = "log")
    public void pointcut(Log log) {
    }

    @Around(value = "pointcut(log)", argNames = "joinPoint,log")
    public Object around(ProceedingJoinPoint joinPoint, Log log) throws Throwable {
        try {
            System.out.println(log.value());
            System.out.println("around");
            return joinPoint.proceed();
        } catch (Throwable throwable) {
            throw throwable;
        } finally {
            System.out.println("around");
        }
    }
}
  @Before("@annotation(com.jiuxian.annotation.Log)")
  public void before(JoinPoint joinPoint) {
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        Method method = signature.getMethod();
        Log log = method.getAnnotation(Log.class);
        System.out.println("注解式拦截 " + log.value());
    }
~~~

service

~~~java
public interface UserService {
    String save(String user);
    void testAnnotationAop();
}
@Service
public class UserServiceImpl implements UserService {
    @Override
    public String save(String user) {
        System.out.println("保存用户信息");
        if ("a".equals(user)) {
            throw new RuntimeException();
        }
        return user;
    }

    @Log(value = "test")
    @Override
    public void testAnnotationAop() {
        System.out.println("testAnnotationAop");
    }
}
~~~

测试

~~~java
@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringbootAopApplicationTests {

    @Resource
    private UserService userService;

    @Test
    public void testAop1() {
        userService.save("张三");
        Assert.assertTrue(true);
    }

    @Test
    public void testAop2() {
        userService.save("a");
    }
    
    @Test
    public void testAop3() {
        userService.testAnnotationAop();
    }
}
~~~







## Introduction

让一个service有另一个service的方法

配置aop

~~~java
@Aspect
@Component
public class IntroductionAop {

    @DeclareParents(value = "com.jiuxian..service..*", defaultImpl = DoSthServiceImpl.class)
    public DoSthService doSthService;

}
~~~

service

~~~java
public interface DoSthService {

    void doSth();
}

@Service
public class DoSthServiceImpl implements DoSthService {

    @Override
    public void doSth() {
        System.out.println("do sth ....");
    }
    
}

public interface UserService {

    void testIntroduction();
}

@Service
public class UserServiceImpl implements UserService {

    @Override
    public void testIntroduction() {
        System.out.println("do testIntroduction");
    }
}
~~~

测试

通过userService，可以强转为另一个service的接口

~~~java
@Test
public void testIntroduction() {
    userService.testIntroduction();
    //Aop 让UserService方法拥有 DoSthService的方法
    DoSthService doSthService = (DoSthService) userService;
    doSthService.doSth();
}
~~~

