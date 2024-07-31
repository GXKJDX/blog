### Mapper层自定义SQL

在 MyBatis-Plus 中，可以通过注解和 XML 配置文件两种方式编写自定义 SQL。以下是两种方式的详细说明和示例。

#### 使用注解编写自定义 SQL

通过注解在 Mapper 接口中直接编写 SQL 语句。

##### 使用 `@Select` 注解

```java
@Mapper
public interface UserMapper extends BaseMapper<User> {
    @Select("SELECT * FROM user WHERE status = #{status}")
    List<User> selectUsersByStatus(@Param("status") int status);
}
```

##### 使用 `@Insert` 注解

```java
@Mapper
public interface UserMapper extends BaseMapper<User> {
    @Insert("INSERT INTO user(name, age, email) VALUES(#{name}, #{age}, #{email})")
    int insertUser(@Param("name") String name, @Param("age") int age, @Param("email") String email);
}
```

##### 使用 `@Update` 注解

```java
@Mapper
public interface UserMapper extends BaseMapper<User> {
    @Update("UPDATE user SET name = #{name} WHERE id = #{id}")
    int updateUserName(@Param("id") Long id, @Param("name") String name);
}
```

##### 使用 `@Delete` 注解

```java
@Mapper
public interface UserMapper extends BaseMapper<User> {
    @Delete("DELETE FROM user WHERE id = #{id}")
    int deleteUserById(@Param("id") Long id);
}
```

#### 使用 XML 配置文件编写自定义 SQL

在 `resources/mapper` 目录下创建一个 `UserMapper.xml` 文件：

```xml
<mapper namespace="com.example.mapper.UserMapper">

    <select id="selectUsersByStatus" resultType="com.example.entity.User">
        SELECT * FROM user WHERE status = #{status}
    </select>

    <insert id="insertUser">
        INSERT INTO user(name, age, email) VALUES(#{name}, #{age}, #{email})
    </insert>

    <update id="updateUserName">
        UPDATE user SET name = #{name} WHERE id = #{id}
    </update>

    <delete id="deleteUserById">
        DELETE FROM user WHERE id = #{id}
    </delete>

</mapper>
```

在 Mapper 接口中引用这些方法：

```java
@Mapper
public interface UserMapper extends BaseMapper<User> {
    List<User> selectUsersByStatus(@Param("status") int status);
    int insertUser(@Param("name") String name, @Param("age") int age, @Param("email") String email);
    int updateUserName(@Param("id") Long id, @Param("name") String name);
    int deleteUserById(@Param("id") Long id);
}
```
### 分页器

#### 配置分页插件

创建一个配置类来配置分页插件：

```java
@Configuration
public class MybatisPlusConfig {
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }
}
```

### 分页查询示例

假设我们有一个用户表 `user`，我们需要对用户数据进行分页查询。以下是具体实现步骤。

#### 定义实体类

定义一个用户实体类 `User`：

```java
@Data
@TableName("user")
public class User {
    @TableId
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

#### 定义 Mapper 接口

在 Mapper 接口中继承 `BaseMapper`，MyBatis-Plus 会自动生成常用的 CRUD 方法：

```java
@Mapper
public interface UserMapper extends BaseMapper<User> {
}
```

#### Service 层实现分页查询

在 Service 层中实现分页查询逻辑：

```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    public IPage<User> getUsersPage(int currentPage, int pageSize) {
        Page<User> page = new Page<>(currentPage, pageSize);
        return userMapper.selectPage(page, null);
    }
}
```

#### Controller 层调用分页查询

在 Controller 层中调用分页查询：

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping("/page")
    public IPage<User> getUsersPage(@RequestParam int currentPage, @RequestParam int pageSize) {
        return userService.getUsersPage(currentPage, pageSize);
    }
}
```

### 分页查询示例说明

1. **配置分页插件**： 在 `MybatisPlusConfig` 类中，通过 `PaginationInterceptor` 配置分页插件，使其在项目中生效。
2. **定义实体类**： 在 `User` 类中使用 `@TableName` 注解指定数据库表名，使用 `@TableId` 注解指定主键。
3. **定义 Mapper 接口**： 在 `UserMapper` 接口中继承 `BaseMapper<User>`，使 MyBatis-Plus 提供的 CRUD 方法可用。
4. **Service 层实现分页查询**： 在 `UserService` 类中，使用 `Page<User>` 对象封装分页参数，通过 `userMapper.selectPage(page, null)` 方法执行分页查询。
5. **Controller 层调用分页查询**： 在 `UserController` 类中，通过 `UserService` 调用分页查询方法，并通过 RESTful API 返回分页结果。
