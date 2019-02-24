## 6-3 Class 类文件结构

- 只要符合java字节码的语言，都可以运行在JVM虚拟机上

- Class 文件中只有两种数据类型：**无符号数**和**表**

### 6-3-1 魔数与Class文件的版本

- 每个Class文件的头四个字节被称为魔数，唯一作用是确定这个文件能否成为一个能被虚拟机接受的Class文件。
- 不使用扩展名是因为扩展名可以改动
- 0xCAFEBABE

### 6-3-2 常量池

- 主次版本号之后是常量池，Class文件的资源仓库
- 常量池容量从1开始计数
  - 字面量：字符串，常量
  - 符号引用
    - 类和接口的全限定名
    - 字段的名称和描述符
    - 方法的名称和描述符
- CONSTANT_Utf8_info型常量的最大长度就是Java中方法，字段名的最大长度，即65535。

### 6-3-3 访问标志

- 常量池之后跟着访问标志

### 6-3-4 类索引，父类索引与接口索引集合

- Class 文件根据这三个数据来确定类的继承关系
- 除了 java.lang.Object 之外，其余Java类的父类索引都不为0

### 6-3-5 字段表集合

- 字段表用于描述接口或类中声明的变量。
- 不包括方法内部声明的局部变量
- 字段表集合中不会列出从超类或者父类接口中继承而来的字段。

### 6-3-6 方法表集合

- 如果父类方法在子类中没有被重写，方法表集合中就不会出现来自父类的方法信息。
- 重载要求参数个数或者顺序或者类型不同
- 返回值不同并不算重载

### 6-3-7 属性表集合

- 字段表，方法表都可以携带自己的属性表集合
  - code属性：即代码
    - java虚拟机执行字节码是基于栈的体系结构，但是与一般基于堆栈的零字节指令不太一样，某些指令（invokspecial）后面还会带有参数。
    - 实例方法的局部变量表中至少存在一个指向当前对象实例的局部变量
  - Exception属性
  - LineNumberTable 属性
  - LocalVariableTable 属性
  - SourceFIle 属性
  - ConstantValue 属性
    - 非static类型的变量（实例变量）赋值实在实例构造器`<init>`方法中进行的
    - 类变量的初始化
      - 在类构造器的`<clinit>`方法中赋值
      - 使用ConstantValue属性
        - Sun Javac 编译器：
          - static final 的或者字符串，生成ConstantValue 属性来初始化
          - 其他的在`<clinit>`中初始化
    - ConstantValue属性的字段必须设置`ACC_STATIC`标志
  - InnerClasses属性
  - Deprecated及Synthetic属性
    - 所有由非用户代码产生的类，方法，字段都应当至少设置Synthetic属性和`ACC_SYNTHETIC`标志，唯一的例外是`<init>`和`<clinit>`
  - StackMapTable 属性
  - Signature 属性
    - Java的泛型是伪泛型，运行期反射无法获得泛型信息，Signature属性就是为了弥补这个缺陷而增设的
  - BootstrapMethods 属性

