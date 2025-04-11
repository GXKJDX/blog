---
title: 一、SpringBoot Java Bean Validation 注解
date: 2024-08-27 22:32:50
tags: SpringBoot
---
常用的 Java Bean Validation 注解包括：

- 校验空值：`@NotNull`, `@NotEmpty`, `@NotBlank`, `@Null`
- 校验长度：`@Size`, `@Length`
- 校验数字：`@Min`, `@Max`, `@DecimalMin`, `@DecimalMax`, `@Range`, `@Digits`
- 校验日期：`@Past`, `@Future`
- 校验格式：`@Email`, `@Pattern`
- 校验布尔值：`@AssertTrue`, `@AssertFalse`
- 校验对象嵌套：`@Valid`

### **1. 空值校验相关注解**

**`@NotNull`**

- 校验字段值是否为 `null`，不允许为 `null`。

- 适用类型：所有对象类型。

- 示例：

  ```java
  @NotNull(message = "用户名不能为空")
  private String userName;
  ```

**`@NotEmpty`**

- 校验字段是否为空，适用于 **字符串、集合、数组、Map** 等类型，不能为空或长度为0。

- 适用类型：字符串、集合、数组、Map。

- 示例：

  ```java
  @NotEmpty(message = "用户名不能为空")
  private String userName;
  ```

**`@NotBlank`**

- 校验字段是否为非空字符串，并且不能为空或只包含空白字符。

- 适用类型：字符串。

- 示例：

  ```java
  @NotBlank(message = "用户名不能为空")
  private String userName;
  ```

**`@Null`**

- 校验字段值是否为 `null`，通常用于校验不允许赋值的字段。

- 适用类型：所有对象类型。

- 示例：

  ```java
  @Null(message = "字段必须为空")
  private String someField;
  ```

------

### **2. 长度校验相关注解**

**`@Size`**

- 校验字符串、集合、数组的长度。

- 适用类型：字符串、集合、数组。

- 示例：

  ```java
  @Size(min = 5, max = 20, message = "用户名长度必须在5到20之间")
  private String userName;
  ```

**`@Length`** (Hibernate Validator)

- 校验字符串的长度，类似于 `@Size`，但它专门用于字符串长度校验。

- 适用类型：字符串。

- 示例：

  ```java
  @Length(min = 5, max = 10, message = "密码长度必须在5到10之间")
  private String password;
  ```

------

### **3. 数值范围校验相关注解**

**`@Min`**

- 校验字段值是否大于或等于指定的最小值。

- 适用类型：数值类型（`int`, `long`, `double`）。

- 示例：

  ```java
  @Min(value = 18, message = "年龄不能小于18岁")
  private int age;
  ```

**`@Max`**

- 校验字段值是否小于或等于指定的最大值。

- 适用类型：数值类型（`int`, `long`, `double`）。

- 示例：

  ```java
  @Max(value = 100, message = "年龄不能大于100岁")
  private int age;
  ```

**`@DecimalMin` 和 `@DecimalMax`**

- 校验小数类型字段的最小值和最大值。

- 适用类型：`BigDecimal` 或 `double` 类型。

- 示例：

  ```java
  @DecimalMin(value = "0.0", inclusive = true, message = "金额不能小于0")
  private BigDecimal amount;
  ```

**`@Range`** (Hibernate Validator)

- 校验字段值是否在指定范围内，适用于数值类型。

- 适用类型：数值类型（`int`, `long`, `BigDecimal` 等）。

- 示例：

  ```java
  @Range(min = 1, max = 100, message = "数值必须在1到100之间")
  private int rangeValue;
  ```

**`@Digits`**

- 校验数字是否符合规定的整数位和小数位的范围。

- 适用类型：`BigDecimal`、`double` 等数值类型。

- 示例：

  ```java
  @Digits(integer = 10, fraction = 2, message = "价格必须是数字，且最多两位小数")
  private BigDecimal price;
  ```

------

### **4. 日期相关校验注解**

**`@Past`**

- 校验日期是否是过去的日期。

- 适用类型：`java.util.Date` 或 `java.time` 类。

- 示例：

  ```java
  @Past(message = "出生日期必须是过去的日期")
  private Date birthDate;
  ```

**`@Future`**

- 校验日期是否是未来的日期。

- 适用类型：`java.util.Date` 或 `java.time` 类。

- 示例：

  ```java
  @Future(message = "交付日期必须是未来日期")
  private Date deliveryDate;
  ```

------

### **5. 格式校验相关注解**

**`@Email`**

- 校验字段值是否为有效的电子邮件格式。

- 适用类型：`String`。

- 示例：

  ```java
  @Email(message = "请输入有效的邮箱地址")
  private String email;
  ```

**`@Pattern`**

- 校验字段值是否符合指定的正则表达式。

- 适用类型：`String`。

- 示例：

  ```java
  @Pattern(regexp = "^[a-zA-Z0-9_]+$", message = "用户名只能包含字母、数字和下划线")
  private String userName;
  ```

------

### **6. 布尔值校验注解**

**`@AssertTrue`**

- 校验字段值是否为 `true`。

- 适用类型：`boolean` 或 `Boolean`。

- 示例：

  ```java
  @AssertTrue(message = "必须同意用户协议")
  private boolean agreeTerms;
  ```

**`@AssertFalse`**

- 校验字段值是否为 `false`。

- 适用类型：`boolean` 或 `Boolean`。

- 示例：

  ```java
  @AssertFalse(message = "此操作不可执行")
  private boolean operationForbidden;
  ```

------

### **7. 对象校验相关注解**

`@Valid`

- 用于嵌套对象的校验。

- 适用类型：嵌套对象。

- 示例：

  ```java
  public class User {
      @Valid
      private Address address; // Address 类也有自己的校验规则
  }
  ```

------

### **8. 其他校验注解**

`@NotBlank` (Spring)

- 校验字段是否为非空且不为仅空白字符。

- 适用类型：`String`。

- 示例：

  ```java
  @NotBlank(message = "用户名不能为空")
  private String userName;
  ```



### 常见的使用场景和示例

#### 1. **校验请求参数（请求体）**

在 Spring Boot 控制器中，常见的场景是校验 POST 或 PUT 请求中的请求体。假设你有一个用户注册接口，需要校验用户信息：

**User.java（实体类）**:

```java
import javax.validation.constraints.*;

public class User {

    @NotNull(message = "用户名不能为空")
    @Size(min = 5, max = 30, message = "用户名长度必须在5到30之间")
    private String userName;

    @NotNull(message = "密码不能为空")
    @Size(min = 8, max = 20, message = "密码长度必须在8到20之间")
    private String password;

    @Email(message = "邮箱格式不正确")
    private String email;

    // getters and setters
}
```

**UserController.java（控制器）**:

```java
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.*;
import javax.validation.Valid;

@RestController
@RequestMapping("/users")
public class UserController {

    // 通过@Valid触发校验
    @PostMapping("/register")
    public String register(@Valid @RequestBody User user) {
        // 如果校验失败，会自动返回400错误，提示校验信息
        return "用户注册成功!";
    }
}
```

#### 2. **校验查询参数**

除了请求体中的数据，很多时候我们也需要校验查询参数。例如，校验用户的查询条件（如分页参数、排序字段等）：

**QueryParams.java（查询参数类）**:

```java
import javax.validation.constraints.*;

public class QueryParams {

    @Min(value = 1, message = "页码不能小于1")
    private int page;

    @Min(value = 1, message = "每页记录数不能小于1")
    @Max(value = 100, message = "每页记录数不能超过100")
    private int size;

    @Pattern(regexp = "name|email|phone", message = "排序字段不合法")
    private String sortBy;

    @Pattern(regexp = "asc|desc", message = "排序方式不合法")
    private String sortOrder;

    // getters and setters
}
```

**UserController.java（控制器）**:

```java
@RestController
@RequestMapping("/users")
public class UserController {

    // 通过@Validated触发校验（校验查询参数）
    @GetMapping("/list")
    public List<User> getUserList(@Validated @ModelAttribute QueryParams queryParams) {
        // 查询逻辑
        return userService.getUserList(queryParams);
    }
}
```

在这个例子中，`@Validated` 触发了对 `QueryParams` 的校验，确保 `page`、`size` 和 `sortBy` 等字段符合预期的规则。

#### 3. **校验嵌套对象**

当一个对象内嵌有其他对象时，可以使用 `@Valid` 或 `@Validated` 来递归触发嵌套对象的校验。

**Address.java（嵌套对象）**:

```java
import javax.validation.constraints.*;

public class Address {

    @NotNull(message = "省份不能为空")
    private String province;

    @NotNull(message = "城市不能为空")
    private String city;

    @NotNull(message = "街道不能为空")
    private String street;

    // getters and setters
}
```

**User.java（包含嵌套对象）**:

```java
import javax.validation.constraints.*;

public class User {

    @NotNull(message = "用户名不能为空")
    private String userName;

    @Valid  // 触发嵌套对象Address的校验
    private Address address;  // 地址对象

    // getters and setters
}
```

**UserController.java（控制器）**:

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @PostMapping("/create")
    public String createUser(@Valid @RequestBody User user) {
        // 当User的address字段校验失败时，会自动返回400错误
        return "用户创建成功!";
    }
}
```

在这个示例中，`User` 类有一个嵌套的 `Address` 类，通过 `@Valid` 注解，Spring 会自动校验 `Address` 类中的字段。

#### 4. **校验更新操作的字段（使用校验分组）**

在某些情况下，针对不同操作（如创建、更新）会有不同的校验规则。可以通过校验分组来实现不同场景下的校验。

**CreateGroup.java（分组接口）**:

```java
public interface CreateGroup {}
```

**UpdateGroup.java（分组接口）**:

```java
public interface UpdateGroup {}
```

**User.java（实体类）**:

```java
import javax.validation.constraints.*;

public class User {

    @NotNull(message = "用户名不能为空", groups = {CreateGroup.class, UpdateGroup.class})
    @Size(min = 5, max = 30, message = "用户名长度必须在5到30之间", groups = {CreateGroup.class, UpdateGroup.class})
    private String userName;

    @NotNull(message = "密码不能为空", groups = {CreateGroup.class})
    @Size(min = 8, max = 20, message = "密码长度必须在8到20之间", groups = {CreateGroup.class})
    private String password;

    @Email(message = "邮箱格式不正确", groups = {UpdateGroup.class})
    private String email;

    // getters and setters
}
```

**UserController.java（控制器）**:

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @PostMapping("/create")
    public String createUser(@Validated(CreateGroup.class) @RequestBody User user) {
        // 仅校验CreateGroup中的字段
        return "用户创建成功!";
    }

    @PutMapping("/update")
    public String updateUser(@Validated(UpdateGroup.class) @RequestBody User user) {
        // 仅校验UpdateGroup中的字段
        return "用户更新成功!";
    }
}
```

通过不同的校验分组，你可以在创建用户时校验用户名和密码，但在更新用户时只校验邮箱。

#### 5. **校验集合对象**

如果一个字段是集合类型，且集合内的每个元素都需要校验，可以使用 `@Valid` 注解来校验集合中的每个元素。

**User.java（包含集合）**:

```java
import javax.validation.constraints.*;
import java.util.List;

public class User {

    @NotNull(message = "用户名不能为空")
    private String userName;

    @Valid  // 校验集合中的每个元素
    @NotEmpty(message = "角色列表不能为空")
    private List<@NotNull(message = "角色不能为空") String> roles;  // 角色列表

    // getters and setters
}
```

**UserController.java（控制器）**:

```
@RestController
@RequestMapping("/users")
public class UserController {

    @PostMapping("/create")
    public String createUser(@Valid @RequestBody User user) {
        // 校验失败时会返回400错误，提示哪些角色无效
        return "用户创建成功!";
    }
}
```

在这个例子中，如果 `roles` 中有 `null` 或者是空的，Spring 会根据注解规则抛出验证异常。

#### 6. **校验方法参数（局部验证）**

除了全局校验（如类上的注解），你还可以在方法参数上进行校验。

**UserService.java（服务类）**:

```java
import javax.validation.constraints.*;

public class UserService {

    public void updateUserInfo(@NotNull @Size(min = 5) String userName) {
        // 如果userName不符合条件，会抛出ConstraintViolationException
        System.out.println("用户名更新为：" + userName);
    }
}
```

### 总结

常见的校验注解使用示例包括：

1. **校验请求参数**：在 Spring 控制器中使用 `@Valid` 校验请求体或 `@Validated` 校验查询参数。
2. **校验嵌套对象**：通过在父类对象字段上使用 `@Valid` 来校验嵌套对象。
3. **校验分组**：使用不同的分组来为不同的操作（如创建、更新）定义不同的校验规则。
4. **校验集合对象**：使用 `@Valid` 校验集合中的每个元素。
5. **校验方法参数**：方法级别的校验，用于校验方法传入的参数。