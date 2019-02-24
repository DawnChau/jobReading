### Spring 的 AOP 理解

可用于**权限认证、日志、事务处理**。

- AOP实现的关键在于AOP框架自动创建的AOP代理，AOP代理主要分为**静态代理和动态代理**。静态代理的代表为**AspectJ**；动态代理则以**Spring AOP**为代表。

- AspectJ是静态代理的增强，所谓静态代理，就是**AOP框架会在编译阶段生成AOP代理类，因此也称为编译时增强**，他会**在编译阶段将AspectJ织入到Java字节码中，运行的时候就是增强之后的AOP对象**。

- Spring AOP使用的动态代理，所谓的动态代理就是说**AOP框架不会去修改字节码，而是每次运行时在内存中临时为方法生成一个AOP对象**，这个AOP对象包含了目标对象的全部方法，并且在特定的切点做了增强处理，并回调原对象的方法。Spring AOP中的动态代理主要有两种方式，**JDK动态代理和CGLIB动态代理**：
  - JDK动态代理通过**反射**来接收被代理的类，并且**要求被代理的类必须实现一个接口**。JDK动态代理的核心是**InvocationHandler接口和Proxy类**。生成的代理对象的方法调用都会委托到**InvocationHandler.invoke()**方法，当我们调用代理类对象的方法时，这个“调用”会转送到invoke方法中，代理类对象作为proxy参数传入，参数method标识了我们具体调用的是代理类的哪个方法，args为这个方法的参数。
  - **如果目标类没有实现接口**，那么Spring AOP会选择使用CGLIB来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，**可以在运行时动态的生成指定类的一个子类对象，并覆盖其中特定方法，覆盖方法时可以添加增强代码，从而实现AOP**。**CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的。**

- 静态代理与动态代理区别在于生成AOP代理对象的时机不同，相对来说**AspectJ的静态代理方式具有更好的性能，但是AspectJ需要特定的编译器进行处理，而Spring AOP则无需特定的编译器处理**。

### Spring 的 IoC的理解

- IOC就是控制反转。就是对象的创建权反转交给Spring，由容器控制程序之间的依赖关系，作用是实现了程序的解耦合，而非传统实现中，由程序代码直接操控。(依赖)控制权由应用代码本身转到了外部容器，由容器根据配置文件去创建实例并管理各个实例之间的依赖关系，控制权的转移，是所谓反转，并且由容器动态的将某种依赖关系注入到组件之中。**BeanFactory 是Spring IoC容器的具体实现与核心接口**，提供了一个先进的配置机制，使得任何类型的对象的配置成为可能，用来包装和管理各种bean。

- 最直观的表达就是，IOC让对象的创建不用去new了，可以由spring自动生产，**这里用的就是java的反射机制**，通过反射在运行时动态的去创建、调用对象。spring就是根据配置文件在运行时动态的去创建对象，并调用对象的方法的。

- Spring的IOC有三种注入方式 ： 
  - 第一是根据属性注入，也叫set方法注入； 
  - 第二种是根据构造方法进行注入； 
  - 第三种是根据注解进行注入。

- IoC，控制反转：将对象交给容器管理，你只需要在spring配置文件总配置相应的bean，以及设置相关的属性，让spring容器生成类的实例对象以及管理对象。在spring容器启动的时候，spring会把你在配置文件中配置的bean都初始化以及装配好，然后在你需要调用的时候，就把它已经初始化好的那些bean分配给你需要调用这些bean的类。就是将对象的控制权反转给spring容器管理。

- DI机制（Dependency Injection，依赖注入）：可以说是IoC的其中一个内容，在容器实例化对象的时候主动的将被调用者（或者说它的依赖对象）注入给调用对象。比如对象A需要操作数据库，以前我们总是要在A中自己编写代码来获得一个Connection对象，有了 spring我们就只需要告诉spring，A中需要一个Connection，至于这个Connection怎么构造，何时构造，A不需要知道。在系统运行时，spring会在适当的时候制造一个Connection，然后像打针一样，注射到A当中，这样就完成了对各个对象之间关系的控制。

- IoC让相互协作的组件保持松散的耦合，而AOP编程允许你把遍布于应用各层的功能分离出来形成可重用的功能组件。

### BeanFactory和ApplicationContext有什么区别？

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