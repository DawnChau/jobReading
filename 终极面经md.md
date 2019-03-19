## 终极面经（我不会的，准备整理1000道）

### 1. v-if v-show的区别

- v-show display:none
- v-if 删除增加DOM元素

### 2. Spring 循环引用问题

- 构造器注入造成的是无法解决的
- Setter注入必须注入单例
  - Spring 容器创建Bean后会返回 ObjectFactory，用于返回一个创建中的Bean

### 3. 如何防止表单的重复提交

- 客户端在提交之后进行重定向
- Session中存放特殊的字符标志串
  - 放在表单的隐藏字段里
  - XSRF保护
- 使用Cookie记录提交状态
- 在数据库里添加约束（最好）

### 4. MySQL 索引失效的场景

- or
- like %
- 字符串没有加引号
- 数据库认为全表扫描比索引更快
- 联合索引顺序不对

### 5. 拥塞窗口为什么要使用慢启动算法

- 慢启动不慢，指数增长
- 上限64K
- 作用是最大限度的使用网络资源

### 6. JSP 九大内置对象

- **out**
- **request response**
- **session**
- page  pageContext
- application
- exception
- config

### 7. 僵尸进程和孤儿进程

- 孤儿进程：父进程退出。子进程孩子运行，将会交给init托管
- 僵尸进程：子进程退出，父进程没有调用wait或者waitpid获取子进程的状态信息，子进程的状态描述符仍然保存在系统中，这种进程称为僵尸进程

### 8. ArrayList 的 subList 方法

- 返回的是原List的一个视图
- 内部保存数据的地址是一样的、
- 子类结构性修改，父类也会
- 父类结构性修改，子类报并发修改错误

### 9. MySQL 行锁的实现

- 通过给索引上的索引项加锁来实现
- 索引检索数据才使用行锁，否则使用表锁
- 即使访问的不同行，但是使用了相同的索引，也会出现冲突

### 10. 堆外内存何时会溢出

- **Direct Memory：NIO**
  - －XX:MaxDirectMemorySize 调整大小
- 线程堆栈：
  - －Xss调整大小
- Socket 缓冲区：
- JNI代码使用本地库
- Java 虚拟机的GC也会占用内存

### 11. 责任链模式

- 每个对象持有下一个对象的引用，请求在链上传递，知道某一个对象决定处理该请求
- 客户端不知道请求被谁处理
- 引用场景：
  - Servlet 的 Filter
  - try catch

### 12. Mybatis 和 Hibernate 的区别

- Hibernate：
  - 优点：
    - 全自动化，通过对象关系实现对数据库的操作
    - OR映射能力强，
    - 有更好的二级缓存，可以使用第三方缓存
    - 日志系统健全
    - 数据库移植性好
  - 缺点：
    - 学习门槛高
    - **无法直接维护sql**
- MyBatis：
  - 优点：
    - **可以直接维护sql，满足复杂的查询需求**
    - sql写在xml里，使得sql与代码解耦，并且避免sql注入
    - 速度相比较Hibernate更快
  - 缺点：
    - sql依赖数据库，导致数据库移植性比较差
    - 日志功能不完善
    - 关联表比较多时候，需要写大量的SQL

### 13. 线程状态

- New
- Runnable 执行了start()
- Running
- Blocked 
  - 等待阻塞 wait
  - 同步阻塞 synchronized
  - 其他阻塞 sleep  或者发出IO请求
- Dead

### 14. 如何编写一段代码引发内存泄露

- 长字符串的String.intern()
- 未关闭已经打开的流 BufferedReader
- 未关闭已经打开的连接 Connection
- 静态变量持有引用（JVM生命周期期间用于不会被GC收集）

### 15. CGLIB 和 JDK 的区别

- cglib：
  - 可以直接代理类
  - 不能代理final 类
- jdk：
  - 必须有顶层接口
  - 使用反射完成
  - 常见例子：Mybatis 的 Mapper文件

### 16. Spring 中有哪些单例

- Bean

### 17. clone 深拷贝和浅拷贝

- 浅拷贝：
  - 不拷贝对象中的引用变量（只拷贝栈，不拷贝堆）
- 深拷贝
  - 实现Serializable 通过 IO 流序列化，反序列化来实现对象的深拷贝
  - 实现Clonable，对其中的每个引用变量也进行拷贝

### 18. Nginx 反向代理

- 修改nginx.conf
  - upstream 中配置 server 【IP + 端口】
  - server / location / proxy_pass 会去 upstream 中找到真正的地址

### 19. Java中的枚举类型

- 本质上是普通类
- 编译器自动添加 values 和 valueOf 方法
- 每个枚举常量是静态常量字段，使用内部类实现
- 枚举常量通过静态代码块来进行初始化

### 20. Redis 分布式锁

- SETNX key val 
  - val 是uuid，用于删除锁时候做判断
- expire key timeout 
  - 给锁设置超时时间
- delete key

### 21. Spring IOC 的启动过程

- 加载配置信息
- 解析配置信息
- Bean 实例化
- BeanPostProcessor
  - 扩展对象（AOP）
- Bean 的初始化
- Bean 的销毁

### 22. MySQL 优化

- 为搜索字段建立索引
- 避免使用select *
- 当只需要一条数据的时候，使用limit 1
- **加缓存**
- 每张表设置ID
- 使用enum而不是varchar
- 选择合适的引擎
- 拆分大的DELETE 和 UPDATE  用 LIMIT（因为会锁表）

### 23. Redis 数据类型

- String
- Hash
- List
- Set
- Sorted Set

### 24. ReentrantLock 和 Synchronized 的区别

- synchronized是语法层面的，ReentrantLock是API层面的
- ReentrantLock 等待可中断
  - 使用tryLock 尝试锁定
  - 可以实现公平锁
  - 可以锁定多个条件

### 25. 大端模式 小端模式

- 大端模式：数据的高位存储在内存的低字节位
- 小端模式：数据的高位存储咋内存的高字节位【小端模式更符合人的思想】

### 26. equals与hashcode方法关系

- 重写equals 必须重写hashcode
- equals相等，hashcode必定相等
- equals不相等，hashcode也可能相等

### 27. MySQL 的引擎及区别

- InnoDB：
  - 对事务的支持
  - 行级锁
  - 外键约束
- MyISAM：
  - 表级锁
  - 保存了表的行数
  - 读操作多，且不需要事务，该引擎为首选

### 28. 反射如何创建对象

- 获取class对象
  - 对象.getClass()
  - 类.class
  - Class.forName()
- newInstance()

### 29. LRU Cache 的原理

- 最近最少使用
- 链表：
  - 新数据放到链表头部
  - 缓存命中时候，数据放到链表头部
  - 链表满时候，将链表尾部丢弃

### 30. LinkedHashMap 底层实现

- 和输入顺序一致
- 内部使用双向链表
- 自带LRUCache，开启之后，get会把其放到header前边
- 第一个访问 -> 第二个访问 -> 第三个访问 -> header -> 最久没被访问

### 31. 乐观锁，悲观锁的原理及应用场景

- 乐观锁：拿数据的时候认为别人不会修改，所以不会上锁，更新的时候会判断一下数据在这期间有没有被更改
  - 版本号
  - 时间戳
    - git的版本控制
- 悲观锁：拿数据的时候就上锁
  - DB的行锁，表锁

### 32. HashMap 的初始容量及扩容过程

- 负载因子 0.75
- 默认初始容量是16
- 扩容后增加为原来的一倍
- 初始容量  （需要存储的元素个数/ 负载因子） +1
- 扩容过程：
  - 创建新的数组
  - rehash
- 多并发条件下的扩容可能会产生循环链表，导致get的时候进行死循环

### 33. Callable 与 Future

- 一般和ExecutorService 配合使用
- Future 是一个接口，FutureTask是实现类。同时FutureTask还实现了Runnable
- **推荐使用FutureTask，集成了Runnable和Callable的优点**

### 34. 负载均衡的实现

- 随机
- 轮询
- 加权轮询
- 源地址哈希（静态映射）
- 目标地址哈希（静态映射）
- 一致性哈希
- 最少连接

### 35. AQS 的底层实现

- AQS 管理着资源 state
- AQS 管理着队列 CLH （双向链表）
- 所有Lock 锁都是AQS的实现
- 将node加入到CLH使用的是CAS，同时线程进入自旋状态
- 自旋时候，判断如果不合适，就调用LockSupport.park(this)挂起
- 前边的节点通过LockSupport.unpark(currentNode.next.thread)唤醒后续
- 重入锁的最主要逻辑就锁判断上次获取锁的线程是否为当前线程
- 读写锁的实现是将state分为高低16位，并且通过CAS更改

### 36. beanFactory 和 factoryBean 的区别

- factoryBean 是个 bean，但是这个Bean不是简单的Bean，而是一个能生产或者修饰对象的工厂Bean

### 37. Spring mvc的流程

- 用户请求DispatcherServlet
- DispatcherServlet去HandlerMapping 处查找Handler
- HandlerMapping 返回 HandlerExecutionChain
- DispatcherServlet 请求 HandlerAdapter 执行Handler
- Handler 执行结束后返回ModelAndView，并被HandlerAdapter 返回给DispatcherServlet
- DispatcherServlet 请求 ViewResolver 解析
- ViewResolver 返回 view对象
- DispatcherServlet 将 view 交给前端渲染

### 38. Spring Bean 的生命周期

- 实例化 new
- 按照Spring 上下文对Bean进行配置，IOC注入
- 如果实现BeanNameAware，调用setBeanName(String)，此处传递Spring 配置文件中的Bean的id
- 如果实现BeanFactoryAware，调用setBeanFactory(BeanFactory) 此处传递的是Spring 工厂自身
- 如果实现ApplicationContextAware，调用setApplicationContext(ApplicationContext)，传入Spring上下文
- 如果关联了BeanPostProcessor，则会调用postProcessBeforeInitialization(Object obj, String s)
- 调用 init-method
- 如果关联了BeanPostProcessor，则会调用postProcessAfterInitialization(Object obj, String s)
- 如果实现了DisposableBean，调用destroy()
- 调用destroy-method

### 39. ConcurrentHashMap的size方法的问题

- JDK 1.7 
  - 不加锁尝试三次，如果结果一致，就返回
  - 如果结果不一致，加锁
- JDK 1.8
  - 建议使用 mappingCount（），返回值是long
  - 核心是sumCount（）
  - 核心是通过对 baseCount 【volatile 类型】和 counterCell 进行 CAS 计算，最终通过baseCount 和遍历 countCell 数组得出size
  - 并发量很高的情况下，CAS 失败的线程使用counterCell记录元素个数的变化

### 40. Ajax 

- 异步
- 局部渲染
- XMLHttpRequest

### 41. JDK 11 新特性

- var 类型推断
- 字符串功能加强
- 集合增加of，copyOf【不可变返回true】
- HttpClient
- **ZGC**
  - GC 暂停时间不会超过10ms
  - 既能处理小堆，也能处理大堆
  - 使用读屏障，不使用写屏障
  - 着色指针

### 42. 工厂模式和简单工厂的区别

- 简单工厂模式：只有一个工厂
- 工厂模式：工厂也实现接口

### 43. 线程池初始化参数

- int corePoolSize => 核心线程数
- int maximumPoolSize 最大线程数
- long keepAliveTime 非核心闲置线程存活时间
- TimeUnit unit 时间单位
- BlockingQueue<Runnable> workQueue 阻塞队列
- ThreadFactory threadFactory 线程工厂
- RejectedExecutionHandler handler 异常处理策略
- **核心线程满了进入workQueue**
- **workQueue满了开启非核心线程**

### 44. Servlet 的单例多线程机制

- 通过线程池来响应多个请求
- 同一个servlet的多个请求也是多线程的

### 45. Java包里提供了哪些线程池

- newCachedThreadPool：复用
- newFIxedThreadPool：定长
- newScheduledThreadPool：定长
- newSingleThreadExecutor：单线程

### 46. 判断数组中出现次数超过该数组一半的元素

- leetcode 169

- 中位数，排序 logN
- res为第一个数，times记录次数，遍历数组，遇到一样的times++，不一样的times— ，当times等于0的时候，res重新指向



