---
title: 二、Mybatis-Plus常见用法-Service_impl常用的基础方法
date: 2024-07-24 22:32:50
tags: mybatisplus
---
### Mybatis-Plus常见用法-Service_impl常用的基础方法

#### 2Crud相关操作

在 MyBatis-Plus 中，`IService` 和 `ServiceImpl` 提供了一些常用的 CRUD 方法，这些方法大大简化了对数据库的基本操作。下面是 `IService` 中常用的 CRUD 方法：

### 1. 插入操作

- **insert(T entity)**：插入一条记录。

  ```java
  public boolean saveUser(User user) {
      return userService.save(user);
  }
  ```

- **saveBatch(Collection<T> entityList)**：批量插入记录。

  ```java
  public boolean saveUsers(List<User> users) {
      return userService.saveBatch(users);
  }
  ```

### 2. 删除操作

- **removeById(Serializable id)**：根据 ID 删除一条记录（逻辑删除）。

  ```java
  public boolean removeUserById(Long id) {
      return userService.removeById(id);
  }
  ```

- **removeByMap(Map<String, Object> columnMap)**：根据 columnMap 条件删除记录。

  ```java
  public boolean removeUsersByMap(Map<String, Object> columnMap) {
      return userService.removeByMap(columnMap);
  }
  ```

- **remove(Wrapper<T> wrapper)**：根据条件删除记录。

  ```java
  public boolean removeUsers(LambdaQueryWrapper<User> wrapper) {
      return userService.remove(wrapper);
  }
  ```

- **removeBatchByIds(Collection<? extends Serializable> idList)**：批量删除记录（逻辑删除）。

  ```java
  public boolean removeUsersByIds(List<Long> ids) {
      return userService.removeBatchByIds(ids);
  }
  ```

### 3. 更新操作

- **updateById(T entity)**：根据 ID 更新一条记录。

  ```java
  public boolean updateUser(User user) {
      return userService.updateById(user);
  }
  ```

- **update(T entity, Wrapper<T> updateWrapper)**：根据 whereWrapper 条件更新记录。

  ```java
  public boolean updateUser(User user, LambdaUpdateWrapper<User> updateWrapper) {
      return userService.update(user, updateWrapper);
  }
  ```

- **updateBatchById(Collection<T> entityList)**：批量更新记录。

  ```java
  public boolean updateUsers(List<User> users) {
      return userService.updateBatchById(users);
  }
  ```

### 4. 查询操作

- **getById(Serializable id)**：根据 ID 查询一条记录。

  ```java
  public User getUserById(Long id) {
      return userService.getById(id);
  }
  ```

- **listByIds(Collection<? extends Serializable> idList)**：根据 ID 集合批量查询记录。

  ```java
  public List<User> getUsersByIds(List<Long> ids) {
      return userService.listByIds(ids);
  }
  ```

- **listByMap(Map<String, Object> columnMap)**：根据 columnMap 条件查询记录。

  ```java
  public List<User> getUsersByMap(Map<String, Object> columnMap) {
      return userService.listByMap(columnMap);
  }
  ```

- **getOne(Wrapper<T> queryWrapper)**：根据 wrapper 条件查询一条记录。

  ```java
  public User getOneUser(LambdaQueryWrapper<User> queryWrapper) {
      return userService.getOne(queryWrapper);
  }
  ```

- **list(Wrapper<T> queryWrapper)**：根据 wrapper 条件查询多条记录。

  ```java
  public List<User> getUsers(LambdaQueryWrapper<User> queryWrapper) {
      return userService.list(queryWrapper);
  }
  ```

- **page(Page<T> page, Wrapper<T> queryWrapper)**：根据 wrapper 条件分页查询多条记录。

  ```java
  public IPage<User> getUsersPage(Page<User> page, LambdaQueryWrapper<User> queryWrapper) {
      return userService.page(page, queryWrapper);
  }
  ```

- **count(Wrapper<T> queryWrapper)**：根据 wrapper 条件查询记录数量。

  ```java
  public int countUsers(LambdaQueryWrapper<User> queryWrapper) {
      return userService.count(queryWrapper);
  }
  ```

- **list()**：查询所有记录。

  ```java
  public List<User> getAllUsers() {
      return userService.list();
  }
  ```

- **page(Page<T> page)**：分页查询所有记录。

  ```java
  public IPage<User> getUsersPage(Page<User> page) {
      return userService.page(page);
  }
  ```

### 示例代码

以下是一个完整的 `UserService` 实现示例：

```java
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.core.conditions.update.LambdaUpdateWrapper;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import org.springframework.stereotype.Service;
import java.util.List;
import java.util.Map;

@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {
    @Override
    public boolean saveUser(User user) {
        return save(user);
    }

    @Override
    public boolean saveUsers(List<User> users) {
        return saveBatch(users);
    }

    @Override
    public boolean removeUserById(Long id) {
        return removeById(id);
    }

    @Override
    public boolean removeUsersByMap(Map<String, Object> columnMap) {
        return removeByMap(columnMap);
    }

    @Override
    public boolean removeUsers(LambdaQueryWrapper<User> wrapper) {
        return remove(wrapper);
    }

    @Override
    public boolean removeUsersByIds(List<Long> ids) {
        return removeBatchByIds(ids);
    }

    @Override
    public boolean updateUser(User user) {
        return updateById(user);
    }

    @Override
    public boolean updateUser(User user, LambdaUpdateWrapper<User> updateWrapper) {
        return update(user, updateWrapper);
    }

    @Override
    public boolean updateUsers(List<User> users) {
        return updateBatchById(users);
    }

    @Override
    public User getUserById(Long id) {
        return getById(id);
    }

    @Override
    public List<User> getUsersByIds(List<Long> ids) {
        return listByIds(ids);
    }

    @Override
    public List<User> getUsersByMap(Map<String, Object> columnMap) {
        return listByMap(columnMap);
    }

    @Override
    public User getOneUser(LambdaQueryWrapper<User> queryWrapper) {
        return getOne(queryWrapper);
    }

    @Override
    public List<User> getUsers(LambdaQueryWrapper<User> queryWrapper) {
        return list(queryWrapper);
    }

    @Override
    public IPage<User> getUsersPage(Page<User> page, LambdaQueryWrapper<User> queryWrapper) {
        return page(page, queryWrapper);
    }

    @Override
    public int countUsers(LambdaQueryWrapper<User> queryWrapper) {
        return count(queryWrapper);
    }

    @Override
    public List<User> getAllUsers() {
        return list();
    }

    @Override
    public IPage<User> getUsersPage(Page<User> page) {
        return page(page);
    }
}
```