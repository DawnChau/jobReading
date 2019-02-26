## 12-4 Java与线程

### 12-4-1 线程的实现

- 程序一般不会直接去使用内核线程，而是去使用内核线程的一种高级接口—轻量级进程LWP
- 当前虚拟机采用的方式是使用用户线程+轻量级进程的混合体

### 12-4-2 Java线程调度

- 协同式线程调度
  - 线程的执行时间由线程本身来控制
- 抢占式线程调度
  - 抢占
  - Thread.yield() 可以让出执行时间

### 12-4-3 状态转换

- 新建
- 运行
- 无限期等待
  - 没设置超时参数的Objec.wait()
  - 没设置线程参数的Thread.join()
  - LockSupport.park()
- 限期等待
  - Thread.sleep()
  - 设置超时参数的Objec.wait()
  - 设置线程参数的Thread.join()
  - LockSupport.parkNanos()
  - LockSupport.parkUtil()
- 阻塞
  - sleep不会让出系统资源
- 结束