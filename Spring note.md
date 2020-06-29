# Spring 笔记

## 1. General

**POJO**：Plain Ordinary Java Object，简单的Java对象，实际就是普通JavaBeans。与EJB相对。

**JavaBeans**：Java中一种特殊的类，可以将多个对象封装到一个对象（bean）中。特点是可序列化，提供无参构造器，提供getter方法和setter方法访问对象的属性。

**BeanFactory**: 负责生产和管理bean的一个工厂。

**XML**：被设计用来结构化、存储以及传输信息。常用在配置文件。

**web.xml**：用来初始化配置信息。当要启动某个web项目时，服务器软件或容器如（tomcat）会第一步加载项目中的web.xml文件，通过其中的各种配置来启动项目。其加载的过程中顺序依次为：context-param >> listener >> fileter >> servlet。

**pom.xml**：POM 是 Project Object Model 的缩写，即项目对象模型。pom.xml 就是 maven 的配置文件，用以描述项目的各种信息。

**注解**：代码中的特殊标记，这些标记可以在编译、类加载、运行时被读取，并执行相对应的处理。注解代替xml配置是一种趋势。

**application.properties**：配置文件，SpringBoot启动时也会默认加载application.yml配置文件。.propertie和.yml配置文件同时存在时，SpringBoot会优先加载.yml（SpringBoot会把.yml转化为.properties文件）。

**Spring Boot**：Spring Boot 最大的特点是无需 XML 配置文件，能自动扫描包路径装载并注入对象，并能做到根据 classpath 下的 jar 包自动配置。全部采用注解形式，内置 Http 服务器（ Jetty 和 Tomcat ），最终以 java 应用程序进行执行。

**Filter**：过滤器，可对用户访问进行预处理，session管理、权限访问控制、修改字符编码。当你有一堆东西的时候，你只希望选择符合你要求的某一些东西，定义这些要求的工具，就是过滤器。

**Interceptor**：拦截器，依赖于SpringMVC框架。在实现上,基于Java的反射机制，属于AOP的一种运用，就是在一个方法前调用一个方法，或者在方法后调用一个方法。在一个流程正在进行的时候，你希望干预它的进展，甚至终止它进行，这是拦截器做的事情。

**Listener**：监听器，在应用时要将监听器对象添加到事件源，在事件产生时，事件源对象会将事件反馈给监听器。当一个事件发生的时候，你希望获得这个事件发生的详细信息，而并不想干预这个事件本身的进程，这就要用到监听器。

<img src="https://mrbird.cc/img/32361-20180530095349427-444141538.png" alt="32361-20180530095349427-444141538.png" style="zoom: 67%;" />

**Java反射机制**：当每个类被装载时，会自动创建一个对应的**Class类**对象（反射类），用来创建这个类的所有对象。可以使用  Class c=Class.forName("MyObject")来获得Class对象。用Class类的newInstance()来创建实例对象，这种方式被应用于工厂模式。

**联系**：目前类的调用方法总共有3种：1.自己创建；2.工厂模式；3.外部注入。其中外部注入即为控制反转/依赖注入模式（IoC/DI）。我们可以用3个形象的东西来分别表示它们，就是new、get、set。顾名思义，new表示自己创建，get表示主动去取（即工厂），set表示是被别人送进来的（即注入）,其中get和set分别表示了主动去取和等待送来两种截然相反的特性。

**消息队列**：消息队列的主要特点是异步处理，主要目的是减少请求响应时间和解耦。所以主要的使用场景就是将比较耗时而且不需要即时（同步）返回结果的操作作为消息放入消息队列。

**AJAX**：一种利用 JavaScript 向服务端发起请求，并获得服务端响应的技术。它的特点是异步请求，局部刷新。AJAX最核心的依赖是浏览器提供的XMLHttpRequest（XHR）对象，是这个对象使得浏览器可以发出HTTP请求与接收HTTP响应。

**XSS攻击**：跨站脚本攻击(Cross Site Scripting)。恶意攻击者往Web页面里插入恶意Script代码，当用户浏览该页之时，嵌入其中Web里面的Script代码会被执行，从而达到恶意攻击用户的目的。使用Jsoup可以有效的过滤不安全的代码。

**代码生成器**：通过代码生成器可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码。MyBatis-Plus 包含代码生成器。

**分页**：数据量比较大的时候，不一次取所有数据，每条SQL取若干行的数据。（order+offset+limit）

**分表**：数据库的水平切分：把数据按行分配到不同的数据库实例上；垂直切分：按列分布到不同数据库上。



## 2. 相关工具

**Maven**：Maven提供了一种思想让团队更科学的管理、构建项目。用配置文件的方式对项目的描述、名称、版本号、项目依赖等等信息进行描述。同时Maven提供了仓库的概念，让这些依赖项放进仓库中，从仓库中去取，也不用考虑这些jar包的依赖关系。

**JDBC**：用于连接多种数据库，执行SQL语句的Java API。JDBC只是一个规范，里面只定义了接口，由各种数据库去实现。。

**MyBatis**：一种ORM框架（Object-Relational Mapping），封装了JDBC。MyBatis注重于POJO类与SQL的映射。而Hibernate注重于POJO类与数据表的映射。

**Druid**：一种数据库连接池。因为建立数据库连接是一个非常耗时、耗资源的行为，所以通过连接池预先同数据库建立一些连接，放在内存中，应用程序需要建立数据库连接时直接到连接池中申请一个就行，用完后再放回去，极大的提高了数据库连接的性能问题。

**Redis**：运行速度很快，并发很强的跑在内存上的NoSql数据库，支持键到五种数据类型的映射。常用场景：缓存、排行榜、计数器、简单消息队列、关系型数据库中被频繁访问的数据。

**Log4j**：一个功能强大的日志组件。常用场景：调试代码、查看系统运行状态、记录访问信息。

**Swagger**：提供了一套通过代码和注解自动生成文档的方法，帮助我们设计、构建、记录以及使用 Rest API。

**Shiro/SpringSecurity**：安全访问控制框架。

**Jackson**：Java的高性能JSON处理器。

**ElasticSearch**：Elasticsearch 在 Lucene 的基础上进行封装。原理：读取内容—>分词—>建立反向索引。Elasticsearch 把操作都封装成了基于HTTP的 API。索引对应一个数据库，类型对应一个表定义，文档对应一条数据。

**docker**：一种容器技术，思想是将开发环境一起打包镜像，避免部署时的环境问题。dockerfile记录镜像的制作步骤。镜像、容器、仓库的概念可以类比代码、进程、github。



## 3. Spring

**Main function**: 

- Managing services and objects by ApplicationContext (DI)
- Data Access
- Build web application by Spring MVC

**IOC**：Spring容器里面有着各种Bean，Bean是我们开发的一个类，我们通过注解/配置文件的方式把Bean注入到Spring容器中。注入到Spring容器中后，Spring容器就有了这个Bean的控制权，后面我们每次要用到容器中的某个Bean时，对象由Spring容器来创建（即所谓控制反转），我们直接使用@Autowired引入这个实例即可，降低了耦合。Bean在Spring容器中默认只创建一个实例（Singleton）。

既然Spring容器需要有Bean的生产和控制权，那么就需要用到工厂模式 + Java反射机制。IOC容器即是一个工厂，这个工厂生产的对象就是Bean，我们利用反射机制根据Bean类名来生产相应的对象。

**AOP**：我们经常需要用到一些Bean所共用的同时与业务逻辑无关的功能，比如日志、安全等功能。这时候我们就涉及到Spring的AOP特性。我们通过动态代理的方式把这些功能织入到目标类中。所谓AOP，其实就是在一些类方法的前后做一些通用的操作，那就涉及到代理。

首先我们使用一个代理类伪装成目标类，代理类包括了我们需要添加的功能，同时还会截取目标类的方法调用；我们对目标类的调用都转变成了对代理类的调用，因此我们就首先执行了需要添加的功能，然后再把对目标类的方法调用转发给真正的目标类执行。

**JDK动态代理**：代理类与委托类**实现同一接口**，主要是通过代理类实现InvocationHandler并重写invoke方法来进行动态代理的，在invoke方法中将对方法进行增强处理。

**CGLIB动态代理**：代理类**继承委托类**，并为其中的委托方法创建两个方法：一个是与委托方法签名相同的方法，它在方法中会通过super调用委托方法；另一个是代理类独有的方法。

**Spring ApplicationContext 容器**：Application Context 是 BeanFactory 的子接口，也被成为 Spring 上下文。ApplicationContext 包含 BeanFactory 所有的功能，相对于 BeanFactory，ApplicationContext 会更加优秀。

**Spring Bean**：bean 是一个被实例化，组装，并通过 Spring IoC 容器所管理的对象。这些 bean 是由用容器提供的配置元数据创建的，例如，在 XML 的表单中的定义。

> DO（ Data Object）：与数据库表结构一一对应，通过DAO层向上传输数据源对象。
>
> **DTO**（ Data Transfer Object）：数据传输对象，Service或Manager向外传输的对象。
>
> BO（ Business Object）：业务对象。 由Service层输出的封装业务逻辑的对象。
>
> AO（ Application Object）：应用对象。 在Web层与Service层之间抽象的复用对象模型，极为贴近展示层，复用度不高。
>
> VO（ View Object）：显示层对象，通常是Web向模板渲染引擎层传输的对象。






## **4. SpringMVC**

**Model**：模型，存取的数据，完成业务逻辑，由JavaBean构成（**service，dao，entity**）。

**View**：视图，用来展示模型中的数据的网页（html，jsp）。

**Controller**：控制器，接收请求—>调用模型—>根据结果派发页面（servlet）。



**DAO层**：与数据库进行联络的一些任务都封装在此。

**Service层**：负责业务模块的逻辑应用设计，Service层调用数据访问层。

**Controller层**：负责具体的业务模块流程的控制，控制层调用Service层。controller层不应该包含业务逻辑，controller的功能应该有以下五点：参数**校验**，调用service层接口**实现**业务逻辑，**转换**业务／数据对象，**组装返回**对象，异常处理。



## 5. 前后端分离

**SPA**：Single Page Application，动态地重写页面的部分来与用户交互。

**SSR**：Server-Side Rendering，将框架及网站页面代码发送到浏览器，然后在浏览器中生成和操作DOM。

**MVC模式**：在MVC中，视图依赖于模型，渲染视图的过程在服务器端完成，性能比较差。

**前后端分离**：REST的形式，前端发送AJAX请求，服务端接收请求并返回JSON数据至前端，在前端渲染界面。

**REST**：resource REpresentational State Transfer，URL定位资源，用HTTP动词（GET,POST,DELETE,DETC）描述操作。实际上是一种设计风格。相比于MVC中在服务器端渲染数据至HTML，RESTful controller以JSON的形式返回一个对象给前端。

**序列化**：后端将前端传来的JSON转换为POJO对象，或者后端将一个POJO对象转换为JSON返回给前端，这个转换过程称为序列化。一般用@ResponseBody/@RestController实现。

**CORS技术**：Cross Origin Resource Sharing（跨域资源共享），用来解决跨域问题，后端只需添加相关响应头信息，前端即可实现发出AJAX跨域请求。

**登录问题**：后端生成一个token给用户，并放入内存（JVM/Redis）中，同时返回给前端。前端将token写入cookie，每次请求都将token一起发给后端。后端提供一个AOP切面，用于拦截所有Controller方法，在切面判断token的有效性，用户登出时清除掉cookie中的token。



## 7. 安全权限

**Authentication**：用户验证。

**Authorization**：授权。

**Cookie**：用来辨别用户身份，加密后存储在客户端。

- 使用Cookie保存曾登录过的用户信息
- 使用Cookie保存session或者token，向后端发送请求时附上Cookie，以此来记录用户的状态。
- 记录和分析用户行为。

**Session**：通过客户端记录用户的状态，存储在服务器端。使用sessionId来识别特定的用户。

- 流程：前端向后端发送用户名和密码 —> 后端创建一个Session返回给前端 —> 前端将Session写入Cookie   —> 若用户保持登录状态，后续的请求都将附上Cookie —> 后端比较Session信息，以验证用户身份。 

**Token**：不需要后端存放Session信息，只需要在前端保存后端返回给前端的Token就可以了，扩展性比较强。常用JWT（JSON Web Token），由Header（元数据）、Payload（实际数据）、Signature三部分组成。

- 流程：后端通过JWT的三部分创建一个Token，并将Token返回给前端 —> 前端将Token保存在Cookie中  —> 后续前端发出的**请求头**都将附上这个Token —> 后端验证签名，从Token中得到用户信息。

**OAuth**：一种授权机制，最终目的是为第三方应用颁发一个由时效性的Token，使得第三方可以获取相关资源。



## 8. 消息队列

相当于一个存放消息的容器，主要目的是通过异步处理提高系统性能、削峰，降低系统耦合度。

- 通过异步处理提高系统性能、削峰：

<img src="https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-11/Asynchronous-message-queue.png" alt="通过异步处理提高系统性能" style="zoom:80%;" />

- 降低系统耦合度

<img src="https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-11/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97-%E8%A7%A3%E8%80%A6.png" alt="解耦" style="zoom: 80%;" />











