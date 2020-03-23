

# Spring 实践

## 1. Spring 常见注解

@Controller：表示该类是一个Spring MVC Controller处理器，用来创建处理HTTP请求的对象。

@RestController：代替@Controller+@ResponseBody，以JSON的形式渲染返回给调用者。RestController是你返回什么就在页面显示什么，Controller会解析成JSP或者其他前端页面格式。

@Autowired：用来装配bean，可以写在字段上（类成员变量），或者方法上。当前对象如果需要依赖另一个对象，只要使用@Autowired注解，Spring就会自动帮你安装上。

@Resource：与@Autowired相似，都可以用来装配bean。

@Controller：用于标注控制层组件。

@Service：用于标注业务层组件。

@Repository：用于标注数据访问层组件。

@Component：泛指组件，与三个组件等效。被@controller 、@service、@repository 、@component 注解的类，都会把这些类注入进spring容器中进行管理。

@Bean：用于告诉方法，产生一个Bean对象，然后这个Bean对象交给Spring管理。 当你引用了一个库，里面的某个类你想要把它变成Bean，但是你改不到它的源码不能加注解，只能写一个方法，加上Bean注解，return这个类的实例给Spring。Bean是一个方法级别上的注解，主要用在@Configuration注解的类里。

>@Controller、@Service、@Repository、@Component、@Bean作用是往IOC容器里放东西；
>
>而@Autowire、@Resource作用是从IOC容器里拿东西出来用。

@RequestMapping：提供HTTP请求的映射信息。类定义处URL相对于根目录，方法处URL相对于类定义处。

- @GetMapping：@RequestMapping(method = RequestMethod.GET) 的简写
- @PostMapping
- @PutMapping
- @DeleteMapping
- @PatchMapping

@RequestParam：用在方法的参数前面，用于将请求参数区数据映射到功能处理方法的参数上。

@PathVariable ：@RequestParam 和 @PathVariable 注解是用于从request中接收请求的，两个都可以接收参数，不同的是@RequestParam 是从request里面拿取值，而 @PathVariable 是从一个URI模板里面来填充。

@CrossOrigin：解决跨域

@Transactional：原子事务

@SpringBootApplication：包含了以下 3 个主要注解，若没有自定义配置的需求，则使用 SpringBootApplication 注解就可以了。

- @Configuration：用来代替 applicationContext.xml 配置文件，所有配置文件里面能做到的事情都可以通过这个注解所在类来进行注册。
- @ComponentScan：用来代替配置文件中的 component-scan 配置，开启组件扫描，即自动扫描包路径下的 Component 注解进行注册 bean 实例到 context 中。
- @EnableAutoConfiguration：用来提供自动配置。



## 2. SpringBoot 项目结构

启动类：com.demo.xxxApplication.java

Entity类：com.demo.pojo/domain

DAO类：com.demo.mapper

Service类：com.demo.service

Service Implements类：com.demo.service.serviceImpl

Controller类：com.demo.controller

常量接口类：com.demo.constant

utils类：com.demo.utils

Config类：com.demo.config

DTO类：com.demo.dto



Aspect类、Filter类



## 3. 后台获取请求参数

例：localhost:8080/student/101?param1=10&param2=20

1. @RequestParam(value="param1")
2. @PathVariable(value="id") 这个注解能够配合@RequesetMapping("/hello/{id}")识别URL里面的一个模板
3. 接收对象：@RequestBody将前端传来的JsonString反序列化为对象（POST）
4. 另外，在Mapper接口方法中加上@Param(" ")来配合#{}完成对SQL语句的映射

> 大概流程：前端对Controller发出HTTP请求，后台通过URL映射到一个Controller方法，该Controller方法获取URL中的参数，并调用一个Service层的方法，最后映射到一个Mapper接口类方法，对数据库进行操作。
>
> HTTPRequest ——> Controller ——> Service ——> Mapper ——> DataBase ——> MyBatis maps data to POJO ——> return the POJO to Service ——> Controller turn POJO to JSON and return to browser



## 4. Spring AOP

#### 切入点函数：

@Pointcut：定义切入点函数，并匹配到一个类或者方法上。有以下几种过滤方式（与通配符.*+配合）：

- within("包名或者类名")
- execution(\<scope> \<return-type> \<fully-qualified-class-name>.*(parameters))
- @annotation("使用了某个注解的方法")

#### 五种通知函数：

@Before：前置通知，通过joinPoint 参数，可以获取目标对象的信息,如类名称,方法参数,方法名称等

@AfterReturning：后置通知，在目标函数执行完成后执行，并可以获取到目标函数最终的返回值returnVal

@AfterThrowing：异常通知，在异常时才会被触发，并由throwing来声明一个接收异常信息的变量

@After：最终通知，类似于finally，只要应用了无论什么情况下都会执行。

@Around：环绕通知，既可以在目标方法前执行也可在目标方法之后执行，同时可以**控制目标方法**是否指向执行

- JoinPoint对象：封装了方法信息， getSignature()、getArgs()、getTarget()、getThis() 
- Signature对象：封装了署名信息，在该对象中可以获取到目标方法名,所属类的Class等信息
- ProceedingJoinPoint对象：只用在@Around中，与JoinPoint相比添加了proceed()方法。

#### 通知传递参数：

在Spring AOP中，除了execution和bean指示符不能传递参数给通知方法，其他指示符都可以将匹配的方法相应参数或对象自动传递给通知方法。

#### Aspect优先级：

实现org.springframework.core.Ordered 接口，该接口用于控制切面类的优先级，同时重写getOrder方法，定制返回值，返回值(int 类型)越小优先级越大。



## 5. MyBatis

主要作用为执行SQL语句，处理POJO类与数据库字段的映射。

简单的语句只需要使用@Insert、@Update、@Delete、@Select这4个注解即可，动态SQL语句需要使用@InsertProvider、@UpdateProvider、@DeleteProvider、@SelectProvider等注解。



## 6. Redis

Spring Cache是作用在方法上的，其核心思想是：当我们在调用一个缓存方法时会把该方法参数和返回结果作为一个键值对存放在缓存中，等到下次利用同样的参数来调用该方法时将不再执行该方法，而是直接从缓存中获取结果进行返回。

> 对象必须实现**序列化**，才能存储到Redis。

#### 主要注解：

@Cacheable：可以标记在一个方法上，也可以标记在一个类上，表示支持缓存。对于一个支持缓存的方法，Spring会在其被调用后将其返回值缓存起来，以保证下次利用同样的参数来执行该方法时可以直接从缓存中获取结果，而不需要再次执行该方法。以键值对进行缓存。

- value属性：指定Cache名称，是必须指定的，表示当前方法的返回值会被缓存在哪个Cache上。
- key属性：\#参数名或者#p参数index，Spring还为我们提供了一个root对象可以用来生成key。类似JoinPoint

@CachePut：对于使用@Cacheable标注的方法，Spring在每次执行前都会检查Cache中是否存在相同key的缓存元素，如果存在就不再执行该方法，而是直接从缓存中获取结果进行返回。而使用@CachePut标注的方法在执行前不会去检查缓存中是否存在之前执行过的结果，而是每次都会执行该方法，并将执行结果以键值对的形式存入指定的缓存中。

@CacheEvict：用来标注在需要清除缓存元素的方法或类上。

#### 键的生成策略：

自定义策略：通过Spring的EL表达式来指定我们的key，即用\#参数名或者#p参数index的方式

默认策略：通过KeyGenerator生成，可修改

- 如果方法没有参数，则使用0作为key。
- 如果只有一个参数的话则使用该参数作为key。
- 如果参数多余一个的话则使用所有参数的hashCode作为key。

#### Redis命令：

String：SET key value ；GET key

Hash：HMSET key field1 "value1" field2 "value2" ；HGET key field

List：LPUSH key value ；LRANGE key start stop

Set：SADD key value ；SMEMBERS key

ZSet：ZADD key score value ；ZRANGEBYSCORE key start stop WITHSCORES 



## 7. Test

#### JUnit：

JUnit5中包含了几个比较重要的注解：@BeforeAll、@AfterAll、@BeforeEach、@AfterEach和@Test。其中， @BeforeAll和@AfterAll在每个类加载的开始和结束时运行，必须为静态方法；而@BeforeEach和@AfterEach则在每个@Test方法开始之前和结束之后运行。

#### Assert：

1. assertEquals("message",A,B)，判断A对象和B对象是否相等，此判断在比较两个对象时调用了equals()方法。

2. assertSame("message",A,B)，判断A对象与B对象是否相同，使用的是==操作符。


#### MockMvc：

1. 模拟一个get请求：

   mockMvc.perform(MockMvcRequestBuilders.get("/hello?name={name}","mrbird"))

2. 模拟一个post请求：

   mockMvc.perform(MockMvcRequestBuilders.post("/user/{id}", 1))

3. 模拟请求参数：

   mockMvc.perform(MockMvcRequestBuilders.get("/hello").param("message", "hello"))

   也可以直接使用MultiValueMap构建参数：new LinkedMultiValueMap<String, String>()

4. 模拟发送JSON参数：

   mockMvc.perform(MockMvcRequestBuilders.post("/user/save").content(jsonStr.getBytes()));




## 8. Swagger

提供了一套通过代码和注解自动生成文档的方法，帮助我们设计、构建、记录以及使用 Rest API。

@Api：修饰整个类，描述整个Controller的作用。

@ApiOperation：用value属性描述一个类的某个方法。

@ApiParam：单个参数描述。

@ApiIgnore：使用该注解忽略这个API。



## 9. Shiro

我们基本只要跟Subject交互就行了：Shiro把用户的数据封装成标识token给Subject，然后通过Subject可以获取到用户的认证信息（AuthenticationInfo）和授权信息（AuthorizationInfo）。Subject把调用委托给SecurityManager，SecurityManager又委托给内部的Authenticator和Authortizer。最后通过Realm获取用户的认证信息和授权信息完成认证和授权。

#### 权限控制：

权限控制：根据不同用户的权限判断其是否有访问相应资源的权限。

三个核心元素：用户、角色、权限。一个用户对应若干角色，一个角色对应若干权限。

#### Session管理：

在Shiro中我们可以通过org.apache.shiro.session.mgt.eis.SessionDAO对象的getActiveSessions()方法方便的获取到当前所有有效的Session对象。通过这些Session对象，我们可以查看当前系统的在线人数，查看这些在线用户的一些基本信息，强制让某个用户下线等。

#### **Shiro 前后端分离**：

传统结构项目中，Shiro从cookie中读取sessionId以此来维持会话，在前后端分离的项目中，我们选择在ajax的请求头中传递sessionId，因此需要重写Shiro获取sessionId的方式。自定义MySessionManager类继承DefaultWebSessionManager类，重写getSessionId方法。登入失败，登入地址，前后端分离，不应该直接跳转页面，而是返回响应结果。

我们需要编写MyShiroRealm类，用于自定义权限匹配和账号密码匹配。

