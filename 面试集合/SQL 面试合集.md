## SQL 面试合集

### 1. union 和 union all 的区别

- union 会去重排序，union all 不会
- union all 的性能更好

### 2. join的类型

- inner join
- outer join 
- left join

### 3. null的判断

- 用 is 不用 =

### 4. 去重的方式

- union all + where

### 5. ACID原则

- Atomicity 原子性
- Consistency 一致性
- Isolation 隔离性
- Durability 持久性

### 6. where 和 having 的区别

- 如果没有 group by，having 和 where 是等价的
- 如果使用了group ,where 在group by 之前过滤,having 对group by 之后数据进行过滤

### 7. char 和 varchar的区别

- 前者不可变
- 后者可变

### 8. 什么是存储过程

- 预编译的SQL语句，允许模块化设计
- 需要多次执行的SQL，使用存储过程更快

### 9.