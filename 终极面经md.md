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





