### 1. Spring 的 AOP 理解

可用于**权限认证、日志、事务处理**。

- AOP实现的关键在于AOP框架自动创建的AOP代理，AOP代理主要分为**静态代理和动态代理**。静态代理的代表为**AspectJ**；动态代理则以**Spring AOP**为代表。

- AspectJ是静态代理的增强，所谓静态代理，就是**AOP框架会在编译阶段生成AOP代理类，因此也称为编译时增强**，他会**在编译阶段将AspectJ织入到Java字节码中，运行的时候就是增强之后的AOP对象**。

- Spring AOP使用的动态代理，所谓的动态代理就是说**AOP框架不会去修改字节码，而是每次运行时在内存中临时为方法生成一个AOP对象**，这个AOP对象包含了目标对象的全部方法，并且在特定的切点做了增强处理，并回调原对象的方法。Spring AOP中的动态代理主要有两种方式，**JDK动态代理和CGLIB动态代理**：
  - JDK动态代理通过**反射**来接收被代理的类，并且**要求被代理的类必须实现一个接口**。JDK动态代理的核心是**InvocationHandler接口和Proxy类**。生成的代理对象的方法调用都会委托到 **InvocationHandler.invoke()** 方法，当我们调用代理类对象的方法时，这个“调用”会转送到invoke方法中，代理类对象作为proxy参数传入，参数method标识了我们具体调用的是代理类的哪个方法，args为这个方法的参数。
  - **如果目标类没有实现接口**，那么Spring AOP会选择使用CGLIB来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，**可以在运行时动态的生成指定类的一个子类对象，并覆盖其中特定方法，覆盖方法时可以添加增强代码，从而实现AOP**。**CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的。**

- 静态代理与动态代理区别在于生成AOP代理对象的时机不同，相对来说**AspectJ的静态代理方式具有更好的性能，但是AspectJ需要特定的编译器进行处理，而Spring AOP则无需特定的编译器处理**。

### 2. Spring 的 IoC的理解

- IOC就是控制反转。就是对象的创建权反转交给Spring，由容器控制程序之间的依赖关系，作用是实现了程序的解耦合，而非传统实现中，由程序代码直接操控。(依赖)控制权由应用代码本身转到了外部容器，由容器根据配置文件去创建实例并管理各个实例之间的依赖关系，控制权的转移，是所谓反转，并且由容器动态的将某种依赖关系注入到组件之中。**BeanFactory 是Spring IoC容器的具体实现与核心接口**，提供了一个先进的配置机制，使得任何类型的对象的配置成为可能，用来包装和管理各种bean。
- 最直观的表达就是，IOC让对象的创建不用去new了，可以由spring自动生产，**这里用的就是java的反射机制**，通过反射在运行时动态的去创建、调用对象。spring就是根据配置文件在运行时动态的去创建对象，并调用对象的方法的。
- Spring的IOC有三种注入方式 ： 
  - 第一是根据属性注入，也叫set方法注入； 
  - 第二种是根据构造方法进行注入； 
  - 第三种是根据注解进行注入。
- IoC，控制反转：将对象交给容器管理，你只需要在spring配置文件总配置相应的bean，以及设置相关的属性，让spring容器生成类的实例对象以及管理对象。在spring容器启动的时候，spring会把你在配置文件中配置的bean都初始化以及装配好，然后在你需要调用的时候，就把它已经初始化好的那些bean分配给你需要调用这些bean的类。就是将对象的控制权反转给spring容器管理。
- DI机制（Dependency Injection，依赖注入）：可以说是IoC的其中一个内容，在容器实例化对象的时候主动的将被调用者（或者说它的依赖对象）注入给调用对象。比如对象A需要操作数据库，以前我们总是要在A中自己编写代码来获得一个Connection对象，有了 spring我们就只需要告诉spring，A中需要一个Connection，至于这个Connection怎么构造，何时构造，A不需要知道。在系统运行时，spring会在适当的时候制造一个Connection，然后像打针一样，注射到A当中，这样就完成了对各个对象之间关系的控制。
- IoC让相互协作的组件保持松散的耦合，而AOP编程允许你把遍布于应用各层的功能分离出来形成可重用的功能组件。
- IOC 或 依赖注入把应用的**代码量降到最低**。它使应用容易测试，单元测试不再需要单例和JNDI查找机制。**最小的代价和最小的侵入性使松散耦合得以实现**。IOC容器支持加载服务时的**饿汉式初始化和懒加载**。

### 3. BeanFactory和ApplicationContext有什么区别？

- BeanFactory和ApplicationContext是Spring的两大核心接口，而其中**ApplicationContext是BeanFactory的子接口**。它们都可以当做Spring的容器，生成Bean实例的，并管理容器中的Bean。
  - BeanFactory：是Spring里面最底层的接口，提供了最简单的容器的功能，负责读取bean配置文档，管理bean的加载与实例化，维护bean之间的依赖关系，负责bean的生命周期，**但是无法支持spring的aop功能和web应用。**
  - ApplicationContext接口作为BeanFactory的派生，因而具有BeanFactory所有的功能。而且ApplicationContext还在功能上做了扩展，以一种更面向框架的方式工作以及对上下文进行分层和实现继承，相较于BeanFactorty，ApplicationContext还提供了以下的功能： 
    - **默认初始化所有的Singleton**，也可以通过配置取消预初始化。
    - 继承MessageSource，因此支持国际化。
    - 资源访问，比如访问URL和文件。
    - 事件机制。
    - 同时加载多个配置文件。
    - 以**声明式**方式启动并创建Spring容器。
    - **载入多个（有继承关系）上下文** ，使得每一个上下文都专注于一个特定的层次，比如应用的web层。
- BeanFactroy采用的是**延迟加载**形式来注入Bean的，**即只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化**，这样，我们就不能发现一些存在的Spring的配置问题。如果Bean的某一个属性没有注入，BeanFacotry加载后，直至第一次使用调用getBean方法才会抛出异常。
- 而ApplicationContext则相反，**它是在容器启动时，一次性创建了所有的Bean**。这样，在容器启动时，我们就可以发现Spring中存在的配置错误，这样有利于检查所依赖属性是否注入。 ApplicationContext启动后预载入所有的单实例Bean，通过预载入单实例bean ,确保当你需要的时候，你就不用等待，因为它们已经创建好了。
- 相对于基本的BeanFactory，**ApplicationContext 唯一的不足是占用内存空间。当应用程序配置Bean较多时，程序启动较慢**。
- BeanFactory通常**以编程的方式被创建**，**ApplicationContext还能以声明的方式创建，如使用ContextLoader**。
- BeanFactory和ApplicationContext都支持BeanPostProcessor、BeanFactoryPostProcessor的使用，但两者之间的区别是：**BeanFactory需要手动注册，而ApplicationContext则是自动注册**。

### 4. Spring事务的实现方式和实现原理

- 划分处理单元——IOC：
  - 由于spring解决的问题是对单个数据库进行局部事务处理的，具体的实现首先用**spring中的IOC划分了事务处理单元**。并且将**对事务的各种配置放到了ioc容器中**（设置事务管理器，设置事务的传播特性及隔离机制）。
- AOP拦截需要进行事务处理的类：
  - Spring事务处理模块是通过AOP功能来实现声明式事务处理的，具体操作（比如事务实行的配置和读取，事务对象的抽象），用**TransactionProxyFactoryBean接口来使用AOP功能，生成proxy代理对象，通过TransactionInterceptor完成对代理方法的拦截，将事务处理的功能编织到拦截的方法中。** 
  - 读取ioc容器事务配置属性，转化为spring事务处理需要的内部数据结构（TransactionAttributeSourceAdvisor），转化为TransactionAttribute表示的数据对象。 
- 对事务处理实现（事务的生成、提交、回滚、挂起）：
  - spring委托给具体的事务处理器实现。实现了一个抽象和适配。适配的具体事务处理器：DataSource数据源支持、**hibernate数据源事务处理支持**、**JDO数据源事务处理支持，JPA、JTA数据源事务处理支持**。这些支持都是通过设计PlatformTransactionManager、AbstractPlatforTransaction一系列事务处理的支持。为常用数据源支持提供了一系列的TransactionManager。

- 结合：**PlatformTransactionManager实现了TransactionInterception接口**，让其与TransactionProxyFactoryBean结合起来，形成一个Spring声明式事务处理的设计体系。

### 5. 解释一下Spring AOP里面的几个名词

- 切面（Aspect）：一个关注点的模块化，这个关注点可能会横切多个对象。事务管理是J2EE应用中一个关于横切关注点的很好的例子。 在Spring AOP中，切面可以使用通用类（基于模式的风格） 或者在普通类中以 @Aspect 注解（@AspectJ风格）来实现。**通俗讲是切入点和增强的结合。可以由多个切入点和多个增强组成。**

- 连接点（Joinpoint）：在程序执行过程中某个特定的点，比如某方法调用的时候或者处理异常的时候。 在Spring AOP中，**一个连接点 总是 代表一个方法的执行。** 通过声明一个org.aspectj.lang.JoinPoint类型的参数可以使增强（Advice）的主体部分获得连接点信息。**通俗讲就是方法。**

- 增强（Advice）：在切面的某个特定的连接点（Joinpoint）上执行的动作。增强有各种类型，其中包括“around”、“before”和“after”等增强。 增强的类型将在后面部分进行讨论。许多AOP框架，包括Spring，都是以拦截器做增强模型， 并维护一个以连接点为中心的拦截器链。**翻译为增强更合适。**

- 切入点（Pointcut）：**匹配连接点（Joinpoint）的断言。**增强和一个切入点表达式关联，并在满足这个切入点的连接点上运行（例如，当执行某个特定名称的方法时）。 切入点表达式如何和连接点匹配是AOP的核心：Spring缺省使用AspectJ切入点语法。**通俗讲就是要拦截哪些连接点。**

- 引入（Introduction）：（也被称为内部类型声明（inter-type declaration））。声明额外的方法或者某个类型的字段。  **Spring允许引入新的接口（以及一个对应的实现）到任何被代理的对象。** 例如，你可以使用一个引入来使bean实现 IsModified 接口，以便简化缓存机制。

- 目标对象（Target Object）： 被一个或者多个切面（aspect）所增强（advise）的对象。也有人把它叫做 被增强（advised） 对象。 既然Spring AOP是通过运行时代理实现的，**这个对象永远是一个 被代理（proxied） 对象。**

- 织入（Weaving）：**把切面（aspect）连接到其它的应用程序类型或者对象上，并创建一个被增强（advised）的对象。 这些可以在编译时（例如使用AspectJ编译器），类加载时和运行时完成。 Spring和其他纯Java AOP框架一样，在运行时完成织入。**
- 切入点（pointcut）和连接点（join point）匹配的概念是AOP的关键，这使得AOP不同于其它仅仅提供拦截功能的旧技术。 切入点使得定位增强（advice）可独立于OO层次。 例如，一个提供声明式事务管理的around增强可以被应用到一组横跨多个对象中的方法上（例如服务层的所有业务操作）。

### 6. 增强有哪些类型

- 前置增强（Before advice）：在某连接点（join point）之前执行的增强，但这个增强不能阻止连接点前的执行（除非它抛出一个异常）。

- 返回后增强（After returning advice）：在某连接点（join point）正常完成后执行的增强：例如，一个方法没有抛出任何异常，正常返回。 
- 抛出异常后增强（After throwing advice）：在方法抛出异常退出时执行的增强。 
- 后增强（After (finally) advice）：当某连接点退出的时候执行的增强（不论是正常返回还是异常退出）。 
- 环绕增强（Around Advice）：包围一个连接点（join point）的增强，如方法调用。这是最强大的一种增强类型。 环绕增强可以在方法调用前后完成自定义的行为。它也会选择是否继续执行连接点或直接返回它们自己的返回值或抛出异常来结束执行。 

### 7. 什么是spring？

- Spring 是个java**企业级应用的开源开发框架**。Spring主要用来开发Java应用，但是有些扩展是针对构建J2EE平台的web应用。Spring 框架目标是**简化Java企业级应用开发，并通过POJO为基础的编程模型促进良好的编程习惯**。

### 8. 使用Spring框架的好处是什么？

- 轻量：Spring 是轻量的，基本的版本大约2MB。
- 控制反转：Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。
- 面向切面的编程(AOP)：Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开。
- 容器：Spring 包含并管理应用中对象的生命周期和配置。
- MVC框架：Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。
- 事务管理：Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）。
- 异常处理：**Spring 提供方便的API把具体技术相关的异常（比如由JDBC，Hibernate or JDO抛出的）转化为一致的unchecked 异常。**

### 9. 解释JDBC抽象和DAO模块

- 通过使用JDBC抽象和DAO模块，保证数据库代码的简洁，**并能避免数据库资源错误关闭导致的问题**，**它在各种不同的数据库的错误信息之上，提供了一个统一的异常访问层。它还利用Spring的AOP 模块给Spring应用中的对象提供事务管理服务。**

### 10. 解释Spring支持的几种bean的作用域

- Spring框架支持以下五种bean的作用域：
  - singleton : **bean在每个Spring ioc 容器中只有一个实例**。**（不是线程安全的）**
  - prototype：**一个bean的定义可以有多个实例**。
  - request：每次http请求都会创建一个bean，该作用域仅在基于web的Spring ApplicationContext情形下有效。
  - session：在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。
  - global-session：在一个全局的HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。

- 缺省的Spring bean 的作用域是Singleton。

### 11. 哪些是重要的bean生命周期方法？ 你能重载它们吗

- 有两个重要的bean 生命周期方法，第一个是**setup** ， 它是在容器加载bean的时候被调用。第二个方法是 **teardown**  它是在容器卸载类的时候被调用。

- The bean 标签有两个重要的属性（init-method和destroy-method）。用它们你可以自己定制初始化和注销方法。它们也有相应的注解（**@PostConstruct和@PreDestroy）**。

### 12. 怎样开启注解装配

- 注解装配在默认情况下是不开启的，为了使用注解装配，我们必须在Spring配置文件中配置 `<context:annotation-config/>`元素。

