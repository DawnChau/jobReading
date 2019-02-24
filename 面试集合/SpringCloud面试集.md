### SpringCloud和Dubbo

- SpringCloud和Dubbo都是现在主流的微服务架构
- SpringCloud是Apache旗下的Spring体系下的微服务解决方案。Dubbo是阿里系的分布式服务治理框架
- 从技术维度上，其实SpringCloud远远的超过Dubbo，Dubbo本身只是实现了服务治理，而SpringCloud现在以及有21个子项目以后还会更多，所以其实很多人都会说Dubbo和SpringCloud是不公平的。但是由于RPC以及注册中心元数据等原因，在技术选型的时候我们只能二者选其一，所以我们常常为用他俩来对比。
- 服务的调用方式**Dubbo使用的是RPC远程调用**，**而SpringCloud使用的是 Rest API**，其实更符合微服务官方的定义
- **服务的注册中心来看，Dubbo使用了第三方的ZooKeeper作为其底层的注册中心，实现服务的注册和发现，SpringCloud使用Spring Cloud Netflix Eureka实现注册中心，当然SpringCloud也可以使用ZooKeeper实现，但一般我们不会这样做**
- 服务网关，Dubbo并没有本身的实现，只能通过其他第三方技术的整合，而**SpringCloud有Zuul路由网关**，作为路由服务器，进行消费者的请求分发，**SpringCloud还支持断路器，与git完美集成分布式配置文件支持版本控制**，事务总线实现配置文件的更新与服务自动装配等等一系列的微服务架构要素

### Rest和RPC对比

- 其实如果仔细阅读过微服务提出者马丁福勒的论文的话可以发现其定义的服务间通信机制就是Http Rest。**RPC最主要的缺陷就是服务提供方和调用方式之间依赖太强，我们需要为每一个微服务进行接口的定义，并通过持续继承发布，需要严格的版本控制才不会出现服务提供和调用之间因为版本不同而产生的冲突**，而REST是轻量级的接口，**服务的提供和调用不存在代码之间的耦合**，只是通过一个约定进行规范，但也有可能出现文档和接口不一致而导致的服务集成问题，但可以通过swagger工具整合，是代码和文档一体化解决，所以REST在分布式环境下比RPC更加灵活

### Spring Boot 和 Spring Cloud

- SpringBoot是Spring推出用于**解决传统框架配置文件冗余，装配组件繁杂的基于Maven的解决方案，旨在快速搭建单个微服务**，**而SpringCloud专注于解决各个微服务之间的协调与配置，服务之间的通信，熔断，负载均衡等**
- 技术维度并相同，并且**SpringCloud是依赖于SpringBoot的**，而SpringBoot并不是依赖与SpringCloud，甚至还可以和Dubbo进行优秀的整合开
- SpringBoot专注于快速方便的开发单个个体的微服务
- SpringCloud是关注**全局的微服务协调整理治理框架**，整合并管理各个微服务，为各个微服务之间提供，配置管理，服务发现，断路器，路由，事件总线等集成服务
  SpringBoot不依赖于SpringCloud，SpringCloud依赖于SpringBoot，属于依赖关系
- **SpringBoot专注于快速，方便的开发单个的微服务个体，SpringCloud关注全局的服务治理框架**

