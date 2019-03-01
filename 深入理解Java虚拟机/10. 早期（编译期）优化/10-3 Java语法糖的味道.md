## 10-3 Java语法糖的味道

### 10-3-1 泛型与类型擦除

- 两个方法如果有相同的名称和特征签名，但是返回值不同，那他们也是可以合法地共存于一个Class文件中

### 10-3-2 自动装箱，拆箱与遍历循环

- 装箱实际上调用的是Integer.valueOf()方法
- 遍历循环实际上是迭代器

```java
Integer a = 1;
Integer b = 2;
Integer c = 3;
Integer d = 3;
Integer e = 321;
Integer f = 321;
Long g = 3L;
c == d; // true
e == f; //false
c == (a+b); //true
c.equals(a+b); //true
g == (a+b); //true
g.equals(a+b); //false   
```

