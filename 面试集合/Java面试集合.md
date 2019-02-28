### 1. 乐观锁与悲观锁

- 悲观锁：synchronized
- 乐观锁：是一种思想，具体的实现是CAS

### 2. Java与C#的不同

- 平台不同：
  - c#  .NET平台， java，  JVM

- C#里没有checked exception

- java中没有传入引用，ref这样的类型修饰符

- C#的interface中不能有字段定义

### 3. ArrayList LinkedList Vector

- arrayList ，基于动态数组实现的，适合查找

- linkedList ，基于双向链表实现的，适合插入、删除

- vector, 是synchronized修饰的，适合在多线程中使用

- arrayList和linkedList都实现了list接口，实现了Serializable接口

### 4. Java和C++面向对象的不同

- 继承
  - java只支持单继承
  - java中方法默认是虚的，c++中只能继承虚函数
  - java只有public继承
- 重载运算符
  - C++中可以重载运算符
  - java中没有，只有String重载了+
- 泛型：
  - C++模板采用代码生成技术，运行时能够保留类型信息
  - java采用类型擦除，类型信息在运行时就没有了
  - C++模板可以使基本类型，java模板不可以
- 异常处理
  - C++没有也不需要finally块
- 对象生命周期控制
  - C++主要靠手动申请
  - java依靠GC
- 构造与析构
  - C++有析构函数

### 5. jvm关于设置新生代、老年代大小的参数

- **-XX:NewSize**：新生代内存
- **-Xmn**：新生代最大内存
- **-XX:SurvivorRatio：**：一个新生代与一个Survivor的比值
- **-XX:NewRatio**：老年代与新生代的比值，默认为4，在设置了**-Xmn**的情况下，该值会被忽略
- **XX:OldSize**：老年代的大小
- **-XX:PermSize**：永久带的大小

