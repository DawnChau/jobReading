## 4-2 JDK的命令行工具

### 4-2-1 jps：虚拟机进程状况工具

- 同时启动多个虚拟机进行，需要用jps查看

### 4-2-2 jstat：虚拟机统计信息监视工具

### 4-2-3 jinfo：Java配置信息工具

- jps -v 可以查看虚拟机启动时显示指定的参数列表
- 非显示指定的默认的只能用jinfo -flag

### 4-2-4 jmap：Java内存映像工具

- 获取dump文件
- 查询finalize执行队列，Java堆和永久带的详细信息

### 4-2-5 jhat：虚拟机堆转存储快照分析工具

- 不常用
- 内置http服务器，可以在浏览器上查看

### jstack：Java堆栈跟踪工具

- 生成虚拟机当前时刻的线程快照
- 生成线程快照的目的是定位线程出现长时间停顿的原因，如线程死锁，死循环，请求外部资源导致线程长时间停顿等原因

