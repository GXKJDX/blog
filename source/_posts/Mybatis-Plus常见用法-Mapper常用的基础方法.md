---
title: 三、Mybatis-Plus常见用法-Mapper常用的基础方法
date: 2024-07-24 22:32:50
tags: mybatisplus
---
### Mybatis-Plus常见用法-Mapper常用的基础方法

#### 3Crud相关操作-Mapper

在 MyBatis-Plus 中，`BaseMapper` 是一个通用的 Mapper 接口，它提供了一系列基础的 CRUD 方法，帮助开发者简化对数据库的操作。通过继承 `BaseMapper`，我们可以直接使用这些方法，而无需编写复杂的 SQL 语句。以下是 `BaseMapper` 中常用的方法：

#### 插入操作

- **insert(T entity)**：插入一条记录。

  ```java
  int insert(User user);
  ```

#### 删除操作

- **deleteById(Serializable id)**：根据 ID 删除一条记录。

  ```java
  int deleteById(Long id);
  ```
  
- **deleteByMap(Map<String, Object> columnMap)**：根据 columnMap 条件删除记录。

  ```java
  int deleteByMap(Map<String, Object> columnMap);
  ```
  
- **delete(Wrapper<T> wrapper)**：根据条件删除记录。

  ```java
  int delete(LambdaQueryWrapper<User> wrapper);
  ```
  
- **deleteBatchIds(Collection<? extends Serializable> idList)**：批量删除记录。

  ```java
  int deleteBatchIds(List<Long> ids);
  ```

#### 更新操作

- **updateById(T entity)**：根据 ID 更新一条记录。

  ```java
  int updateById(User user);
  ```
  
- **update(T entity, Wrapper<T> updateWrapper)**：根据条件更新记录。

  ```java
  int update(User user, LambdaUpdateWrapper<User> updateWrapper);
  ```

#### 查询操作

- **selectById(Serializable id)**：根据 ID 查询一条记录。

  ```java
  User selectById(Long id);
  ```
  
- **selectBatchIds(Collection<? extends Serializable> idList)**：根据 ID 集合批量查询记录。

  ```java
  List<User> selectBatchIds(List<Long> ids);
  ```
  
- **selectByMap(Map<String, Object> columnMap)**：根据 columnMap 条件查询记录。

  ```java
  List<User> selectByMap(Map<String, Object> columnMap);
  ```
  
- **selectOne(Wrapper<T> queryWrapper)**：根据条件查询一条记录。

  ```java
  User selectOne(LambdaQueryWrapper<User> queryWrapper);
  ```

- **selectList(Wrapper<T> queryWrapper)**：根据条件查询多条记录。

  ```java
  List<User> selectList(LambdaQueryWrapper<User> queryWrapper);
  ```

- **selectPage(Page<T> page, Wrapper<T> queryWrapper)**：根据条件分页查询多条记录。

  ```java
  IPage<User> selectPage(Page<User> page, LambdaQueryWrapper<User> queryWrapper);
  ```

- **selectCount(Wrapper<T> queryWrapper)**：根据条件查询记录数量。

  ```java
  int selectCount(LambdaQueryWrapper<User> queryWrapper);
  ```

### 使用示例

以下是一个 `UserMapper` 接口的示例：

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface UserMapper extends BaseMapper<User> {
}
```

在这个示例中，`UserMapper` 继承了 `BaseMapper<User>`，因此可以直接使用 `BaseMapper` 提供的各种 CRUD 方法，而无需在 `UserMapper` 中显式定义这些方法。

### 使用示例

以下是一个 `UserService` 类，通过 `UserMapper` 使用 `BaseMapper` 提供的 CRUD 方法：

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class UserService {
    @Autowired
    private UserMapper userMapper;

    public boolean saveUser(User user) {
        return userMapper.insert(user) > 0;
    }

    public boolean removeUserById(Long id) {
        return userMapper.deleteById(id) > 0;
    }

    public boolean updateUser(User user) {
        return userMapper.updateById(user) > 0;
    }

    public User getUserById(Long id) {
        return userMapper.selectById(id);
    }

    public List<User> getAllUsers() {
        return userMapper.selectList(null);
    }
}
```