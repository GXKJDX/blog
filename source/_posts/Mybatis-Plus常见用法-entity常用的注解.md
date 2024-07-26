---
title: 一、Mybatis-Plus常见用法-entity常用的注解
date: 2024-07-24 22:32:50
tags: mybatisplus
---
### Mybatis-Plus常见用法-entity常用的注解

### 1常用注解

### 1. @TableName

- **作用**：指定数据库表名。
- **用法**：

```java
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.Data;

@Data
@TableName("user")  // 数据库表名
public class User {
}
```

### 2. @TableId

- **作用**：指定主键字段，并可以配置主键生成策略。
- **用法**：

```java
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.IdType;
import lombok.Data;

@Data
public class User {
    @TableId(value = "id", type = IdType.AUTO)  // 主键生成策略
    private Long id;
}
```

常见的主键生成策略有：

- `IdType.AUTO`：数据库ID自增。
- `IdType.NONE`：未设置主键。
- `IdType.INPUT`：手动输入ID。
- `IdType.ID_WORKER`：默认的雪花算法。
- `IdType.UUID`：全局唯一ID。

* `IdType.ASSIGN_ID`：基于雪花算法（Snowflake）的 ID 生成器

### 3. @TableField

- **作用**：指定数据库表中的字段，以及配置字段的相关属性。
- **用法**：

```java
import com.baomidou.mybatisplus.annotation.TableField;
import lombok.Data;

@Data
public class User {
    @TableId
    private Long id;

    @TableField("user_name")  // 指定数据库字段名，一般命名时要求类中字段和数据库同名，如：user_name -> userName
    private String name;

    @TableField(select = false)  // 查询时不返回该字段，用的比较少，一般返回的是null
    private String email;

    @TableField(fill = FieldFill.INSERT)  // 自动填充策略
    private Date createTime;

	@TableField(exist = false) // 表明字段在表中不存在
	private List<ActiveBorderRelation> listActiveBorderRelation;
    
    @TableField(fill = FieldFill.INSERT)  // 自动填充策略
    private Integer deleted;
}
```

常见的自动填充策略有：

- `FieldFill.DEFAULT`：默认不处理。
- `FieldFill.INSERT`：插入时填充字段。
- `FieldFill.UPDATE`：更新时填充字段。
- `FieldFill.INSERT_UPDATE`：插入和更新时填充字段。

配置自动填充处理器

在项目中创建一个类，实现 `MetaObjectHandler` 接口，用于自动填充字段的默认值。

```java
import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import org.apache.ibatis.reflection.MetaObject;
import org.springframework.stereotype.Component;

@Component
public class MyMetaObjectHandler implements MetaObjectHandler {
    @Override
    public void insertFill(MetaObject metaObject) {
        // 插入时填充逻辑删除字段的默认值为0
        this.setFieldValByName("deleted", 0, metaObject);
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        // 更新操作时的填充策略，如果有需要可以在这里定义
    }
}
```

### 4. @Version

- **作用**：用于乐观锁实现，指定版本号字段。
- **用法**：

```java
import com.baomidou.mybatisplus.annotation.Version;
import lombok.Data;

@Data
public class User {
    @TableId
    private Long id;
    private String name;
    private Integer age;
    private String email;

    @Version
    private Integer version;  // 版本号字段
}
```

### 乐观锁工作原理

1. **查询数据**：首先查询需要更新的数据，同时获取当前的版本号。
2. **更新数据**：在更新数据时，带上当前版本号。
3. **版本号匹配**：MyBatis-Plus 在生成的 SQL 语句中，会将 `WHERE` 条件添加版本号的判断，即 `UPDATE user SET name=?, age=?, version=?+1 WHERE id=? AND version=?`。
4. **版本号校验**：如果 `WHERE` 条件中版本号匹配，则更新成功，同时版本号自动递增；如果版本号不匹配，则更新失败，返回 `0`，表示数据已经被其他事务修改过

### 5. @TableLogic

- **作用**：用于逻辑删除，实现软删除功能。
- **用法**：

```java
import com.baomidou.mybatisplus.annotation.TableLogic;
import lombok.Data;

@Data
public class User {
    @TableId
    private Long id;
    private String name;
    private Integer age;
    private String email;

    @TableLogic
    private Integer deleted;  // 逻辑删除字段
}
```

在 `application.yml` 中配置逻辑删除的值：

```yaml
mybatis-plus:
  global-config:
    db-config:
      logic-delete-value: 1
      logic-not-delete-value: 0
```

### 插入数据和逻辑删除数据

在插入数据时，`deleted` 字段会自动填充为 `0`，无需手动设置。

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {
    @Autowired
    private UserMapper userMapper;

    public void addUser() {
        User user = new User();
        user.setName("Alice");
        user.setAge(30);
        user.setEmail("alice@example.com");
        userMapper.insert(user);  // 插入时，deleted 字段自动填充为 0
    }
	
    public void deleteUser(Long id) {
        userMapper.deleteById(id);  // 逻辑删除时，deleted 字段自动更新为 1
    }
}
```