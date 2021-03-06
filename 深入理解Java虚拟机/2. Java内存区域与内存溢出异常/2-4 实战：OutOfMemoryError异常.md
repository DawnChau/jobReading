## 2-4 实战：OutOfMemoryError异常

### 2-4-2 虚拟机栈和本地方法栈溢出

- -Xss 设置方法栈大小
- 单线程下，无论是由于栈帧太大还是虚拟机容量太小，内存无法分配时候，抛出的都是stackOverFlow异常

### 2-4-3 方法区和运行时常量池溢出

- Spring.intern() 是Native方法，作用是：
  - 常量池包含一个等于此String的字符串，返回
  - 否则，将此String对象包含的字符串加入常量池，并返回引用
- JDK 1.7  的 intern()，如果该字符串是首次出现，不会复制，只会在常量池里放引用
- JDK 1.6 的 intern()会复制，因此返回的不是同一个对象

### 2-4-4 本机直接内存溢出

- 明显特征是Heap Dump中不会看到异常
- 程序间接使用了NIO