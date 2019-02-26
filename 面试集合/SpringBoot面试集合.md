### 1. Spring Boot 的优点？

- 独立运行
- **简化配置**
- **自动配置**
- 无代码生成和XML配置
- 应用监控
- 上手容易
- 没有单独的Web服务器需要。这意味着你不再需要启动Tomcat，Glassfish或其他任何东西。

### 2. **Spring Boot、Spring MVC 和 Spring 有什么区别**

- SpringFrame
  - SpringFramework 最重要的特征是依赖注入。所有 SpringModules 不是依赖注入就是 IOC 控制反转。
  - 当我们恰当的使用 DI 或者是 IOC 的时候，我们可以开发松耦合应用。松耦合应用的单元测试可以很容易的进行。
- SpringMVC
  - Spring MVC 提供了一种**分离式**的方法来开发 Web 应用。通过运用像 DispatcherServelet，MoudlAndView 和 ViewResolver 等一些简单的概念，开发 Web 应用将会变的非常简单。
- SpringBoot
  - Spring 和 SpringMVC 的问题在于需要配置大量的参数。
  - Spring Boot 通过一个**自动配置和启动**的项来目解决这个问题。为了更快的构建产品就绪应用程序，Spring Boot 提供了一些非功能性特征。

### **3. 如何使用Spring Boot实现分页和排序**

- 使用Spring Boot实现分页非常简单。使用Spring Data-JPA可以实现将可分页的**org.springframework.data.domain.Pageable**，传递给存储库方法。

### 4. **什么是Swagger？**

- Swagger广泛用于可视化API，使用Swagger UI为前端开发人员提供在线沙箱。Swagger是用于生成RESTful Web服务的可视化表示的工具，规范和完整框架实现。它使文档能够以与服务器相同的速度更新。当通过Swagger正确定义时，消费者可以使用最少量的实现逻辑来理解远程服务并与其进行交互。因此，Swagger消除了调用服务时的猜测。

### 5. **如何使用Spring Boot实现异常处理**

- Spring提供了一种使用**ControllerAdvice**处理异常的非常有用的方法。 我们通过实现一个ControlerAdvice类，来处理控制器类抛出的所有异常。

 ### 6. **springboot自动配置的原理**

- 在spring程序main方法中 添加@SpringBootApplication或者@EnableAutoConfiguration。
- 会自动去maven中读取每个starter中的spring.factories文件  该文件里配置了所有需要被创建spring容器中的bean

### 7. **JPA 和 Hibernate 有哪些区别？**

- JPA 是一个规范或者接口

- Hibernate 是 JPA 的一个实现