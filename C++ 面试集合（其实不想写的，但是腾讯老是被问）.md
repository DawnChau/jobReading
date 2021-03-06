## C++ 面试集合（其实不想写的，但是腾讯老是被问）

### 1. C++多态

- 动态多态
  - 虚函数类有一维的虚函数表叫做虚表
  - 类的对象有一个虚表指针
- 静态多态
  - 函数重载（编译期确定）
  - 泛型编程

### 2. Linux 怎么根据进程名杀掉进程

- pkill 进程名
- kill all 进程名
- awk '{print("kill -9 ",$2)}'：生成一条“kill -9 ”+进程号的shell脚本

### 3. C++中指针和引用的区别

- 引用只是一个别名，还是变量本身，对引用的任何操作就是对变量本身进行操作，以达到修改变量的目的
- 指针传参的时候，还是值传递，指针本身的值不可以修改，需要通过解引用才能对指向的对象进行操作
- 引用传参的时候，传进来的就是变量本身，因此变量可以被修改

### 4. const知道吗？解释一下其作用

- const修饰类的成员变量，表示常量不可能被修改
- const修饰类的成员函数，表示该函数不会修改类中的数据成员，不会调用其他非const的成员函数

### 5. STL中map和set的原理（关联式容器）

- map和set的底层实现主要通过红黑树来实现

### 6. STL中的vector的实现，是怎么扩容的

- vector就是一个动态增长的数组，里面有一个指针指向一片连续的空间，当空间装不下的时候，会申请一片更大的空间，将原来的数据拷贝过去，并释放原来的旧空间。当删除的时候空间并不会被释放，只是清空了里面的数据。对比array是静态空间一旦配置了就不能改变大小。
- vector的动态增加大小的时候，并不是在原有的空间上持续新的空间（无法保证原空间的后面还有可供配置的空间），而是以原大小的两倍另外配置一块较大的空间，然后将原内容拷贝过来，并释放原空间。

### 7. STL中unordered_map和map的区别

- map是STL中的一个关联容器，提供键值对的数据管理。底层通过**红黑树来**实现，实际上是二叉排序树和非严格意义上的**二叉平衡树**。所以在`map`内部所有的数据都是有序的，且`map`的查询、插入、删除操作的时间复杂度都是`O(logN)`

- unordered_map和map类似，都是存储key-value对，可以通过key快速索引到value，不同的unordered_map不会根据key进行排序。unordered_map底层是一个防冗余的哈希表，存储时根据key的hash值判断元素是否相同，即unoredered_map内部是无序的

### 8. 构造函数为什么一般不定义为虚函数？而析构函数一般写成虚函数的原因

- 构造函数不能声明为虚函数
  - 因为创建一个对象时需要确定对象的类型，而虚函数是在运行时确定其类型的。而在构造一个对象时，由于对象还未创建成功，编译器无法知道对象的实际类型，是类本身还是类的派生类等等
  - 虚函数的调用需要虚函数表指针，而该指针存放在对象的内存空间中；若构造函数声明为虚函数，那么由于对象还未创建，还没有内存空间，更没有虚函数表地址用来调用虚函数即构造函数了
- 析构函数最好声明为虚函数
  - 如果析构函数不被声明成虚函数，则编译器实施静态绑定，在删除指向派生类的基类指针时，只会调用基类的析构函数而不调用派生类析构函数，这样就会造成派生类对象析构不完全。



