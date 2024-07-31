---
title: 四、Mybatis-Plus常见用法-entity常用的注解
date: 2024-07-28 22:32:50
tags: mybatisplus
---
MyBatis-Plus 提供了丰富的条件构造器方法，用于构建复杂的 SQL 查询条件。以下是常用条件构造器方法的详细说明和示例：

### 常用条件构造器方法

#### 1. eq(String column, Object val)

等于

```java
queryWrapper.eq("status", 1);
```

#### 2. ne(String column, Object val)

不等于

```java
queryWrapper.ne("status", 0);
```

#### 3. gt(String column, Object val)

大于

```java
queryWrapper.gt("age", 18);
```

#### 4. ge(String column, Object val)

大于等于

```java
queryWrapper.ge("age", 18);
```

#### 5. lt(String column, Object val)

小于

```java
queryWrapper.lt("age", 60);
```

#### 6. le(String column, Object val)

小于等于

```java
queryWrapper.le("age", 60);
```

#### 7. between(String column, Object val1, Object val2)

介于

```java
queryWrapper.between("age", 18, 60);
```

#### 8. like(String column, Object val)

模糊查询

```java
queryWrapper.like("name", "John");
```

#### 9. notLike(String column, Object val)

非模糊查询

```java
queryWrapper.notLike("name", "John");
```

#### 10. likeLeft(String column, Object val)

左模糊查询

```java
queryWrapper.likeLeft("name", "ohn");
```

#### 11. likeRight(String column, Object val)

右模糊查询

```java
queryWrapper.likeRight("name", "Joh");
```

#### 12. isNull(String column)

字段为 NULL

```java
queryWrapper.isNull("email");
```

#### 13. isNotNull(String column)

字段不为 NULL

```java
queryWrapper.isNotNull("email");
```

#### 14. in(String column, Collection<?> value)

IN 查询

```java
queryWrapper.in("id", Arrays.asList(1, 2, 3));
```

#### 15. notIn(String column, Collection<?> value)

NOT IN 查询

```java
queryWrapper.notIn("id", Arrays.asList(1, 2, 3));
```

#### 16. or()

或者

```java
queryWrapper.eq("status", 1).or().eq("status", 2);
```

#### 17. and(Consumer<Wrapper<T>> consumer)

并且

```java
queryWrapper.and(wrapper -> wrapper.eq("status", 1).like("name", "John"));
```

#### 18. orderByAsc(String... columns)

升序排序

```java
queryWrapper.orderByAsc("age", "name");
```

#### 19. orderByDesc(String... columns)

降序排序

```java
queryWrapper.orderByDesc("create_time");
```

#### 20. groupBy(String... columns)

分组

```java
queryWrapper.groupBy("department_id");
```

#### 21. having(String sqlHaving, Object... params)

分组后过滤

```java
queryWrapper.groupBy("department_id").having("count(id) > ?", 5);
```

#### 22. set(boolean condition, String column, Object val)

设置字段值

```java
UpdateWrapper<User> updateWrapper = new UpdateWrapper<>();
updateWrapper.set("name", "John").set("age", 25);
```

#### 23. setSql(String sql)

拼接 SET SQL

```java
updateWrapper.setSql("name = 'John', age = 25");
```

### 示例

以下是一个完整的示例，展示如何使用上述条件构造器方法构建复杂查询条件：

```java
@Autowired
private UserMapper userMapper;

public List<User> getUsersByConditions() {
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    queryWrapper.eq("status", 1)
                .ne("type", 0)
                .gt("age", 18)
                .ge("score", 60)
                .lt("height", 200)
                .le("weight", 100)
                .between("age", 20, 30)
                .like("name", "John")
                .notLike("address", "New York")
                .likeLeft("email", "example.com")
                .likeRight("username", "user")
                .isNull("phone")
                .isNotNull("email")
                .in("id", Arrays.asList(1, 2, 3))
                .notIn("role", Arrays.asList("admin", "guest"))
                .or()
                .eq("gender", "male")
                .and(wrapper -> wrapper.eq("active", 1).like("nickname", "johnny"));
    
    return userMapper.selectList(queryWrapper);
}
```