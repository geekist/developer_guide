# 一、spring validation介绍

## 1、介绍
Java API规范(JSR303)定义了Bean校验的标准validation-api，但没有提供实现。hibernate validation是对这个规范的实现，并增加了校验注解如@Email、@Length等。Spring Validation是对hibernate validation的二次封装，用于支持spring mvc参数自动校验。接下来，我们以spring-boot项目为例，介绍Spring Validation的使用。

## 2、注解举例

如下所示，一个NotEmpty的注解可以定义

```java
package javax.validation.constraints;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Repeatable;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import javax.validation.Constraint;
import javax.validation.Payload;

@Documented
@Constraint(
    validatedBy = {}
)
@Target({ElementType.METHOD, ElementType.FIELD, ElementType.ANNOTATION_TYPE, ElementType.CONSTRUCTOR, ElementType.PARAMETER, ElementType.TYPE_USE})
@Retention(RetentionPolicy.RUNTIME)
@Repeatable(NotEmpty.List.class)
public @interface NotEmpty {
    String message() default "{javax.validation.constraints.NotEmpty.message}";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};

    @Target({ElementType.METHOD, ElementType.FIELD, ElementType.ANNOTATION_TYPE, ElementType.CONSTRUCTOR, ElementType.PARAMETER, ElementType.TYPE_USE})
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    public @interface List {
        NotEmpty[] value();
    }
}
```
## 3、常用的注解

|验证注解	|验证的数据类型|	说明|
| ---- | ---- | ---- |
|@AssertFalse |	Boolean,boolean|	验证注解的元素值是false|
|@AssertTrue|	Boolean,boolean|	验证注解的元素值是true|
|@NotNull	|任意类型	|验证注解的元素值不是null|
|@Null	|任意类型	|验证注解的元素值是null|
|@Min(value=值)	|BigDecimal，BigInteger, byte,short, int, long，等任何Number或CharSequence（存储的是数字）子类型|	验证注解的元素值大于等于@Min指定的value值|
|@Max（value=值）|	和@Min要求一样	|验证注解的元素值小于等于@Max指定的value值|
|@DecimalMin(value=值)|	和@Min要求一样	|验证注解的元素值大于等于@ DecimalMin指定的value值|
|@DecimalMax(value=值)|	和@Min要求一样	|验证注解的元素值小于等于@ DecimalMax指定的value值|
|@Digits(integer=整数位数, fraction=小数位数)|	和@Min要求一样	|验证注解的元素值的整数位数和小数位数上限|
|@Size(min=下限, max=上限)	|字符串、Collection、Map、数组等|	验证注解的元素值的在min和max（包含）指定区间之内，如字符长度、集合大小|
|@Past	|java.util.Date,java.util.Calendar;Joda Time类库的日期类型	|验证注解的元素值（日期类型）比当前时间早|
|@Future	|与@Past要求一样	|验证注解的元素值（日期类型）比当前时间晚|
|@NotBlank	|CharSequence子类型	|验证注解的元素值不为空（不为null、去除首位空格后长度为0），不同于@NotEmpty，@NotBlank只应用于字符串且在比较时会去除字符串的首位空格|
|@Length(min=下限, max=上限)	|CharSequence子类型	|验证注解的元素值长度在min和max区间内|
|@NotEmpty	|CharSequence子类型、Collection、Map、数组	|验证注解的元素值不为null且不为空（字符串长度不为0、集合大小不为0）|
|@Range(min=最小值, max=最大值)	|BigDecimal,BigInteger,CharSequence, byte, short, int, long等原子类型和包装类型	验证注解的元素值在最小值和最大值之间|
|@Email(regexp=正则表达式,flag=标志的模式)	|CharSequence子类型（如String）	|验证注解的元素值是Email，也可以通过regexp和flag指定自定义的email格式|
|@Pattern(regexp=正则表达式,flag=标志的模式)	|String，任何CharSequence的子类型|	验证注解的元素值与指定的正则表达式匹配|
|@Valid	|任何非原子类型	|指定递归验证关联的对象如用户对象中有个地址对象属性，如果想在验证用户对象时一起验证地址对象的话，在地址对象上加@Valid注解即可级联验证|

# 二、使用 spring validation

## 1、引入依赖

如果spring-boot版本小于2.3.x，spring-boot-starter-web会自动传入hibernate-validator依赖。如果spring-boot版本大于2.3.x，则需要手动引入依赖：
```xml
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>6.0.1.Final</version>
</dependency>
```

## 2、在controller层做校验

### 2.1 Get请求常用的requestParam/PathVariable参数校验

GET请求一般会使用requestParam/PathVariable传参。此种类型的校验，必须在Controller类上标注@Validated注解，并在入参上声明约束注解(如@Min等)。如果校验失败，会抛出ConstraintViolationException异常。代码示例如下：
```java
@RequestMapping("/api/user")
@RestController
@Validated
public class UserController {
    // 路径变量
    @GetMapping("{userId}")
    public Result detail(@PathVariable("userId") @Min(10000000000000000L) Long userId) {
        // 校验通过，才会执行业务逻辑处理
        UserDTO userDTO = new UserDTO();
        userDTO.setUserId(userId);
        userDTO.setAccount("11111111111111111");
        userDTO.setUserName("xixi");
        userDTO.setAccount("11111111111111111");
        return Result.ok(userDTO);
    }

    // 查询参数
    @GetMapping("getByAccount")
    public Result getByAccount(@Length(min = 6, max = 20) @NotNull String  account) {
        // 校验通过，才会执行业务逻辑处理
        UserDTO userDTO = new UserDTO();
        userDTO.setUserId(10000000000000003L);
        userDTO.setAccount(account);
        userDTO.setUserName("xixi");
        userDTO.setAccount("11111111111111111");
        return Result.ok(userDTO);
    }
}
```
如果校验失败，会抛出MethodArgumentNotValidException异常，Spring默认会将其转为400（Bad Request）请求。

### 2.2 Post和Put请求常用requestBody参数校验

POST、PUT请求一般会使用requestBody传递参数，这种情况下，后端使用DTO对象进行接收。

```java
package com.yuya.common.entity.console.dto;

import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

import javax.validation.constraints.NotEmpty;
import java.io.Serializable;

@Data
public class ConsoleLoginDto implements Serializable {

    @ApiModelProperty(value = "登录用户名",required = true)
    @NotEmpty(message = "登录用户名 不能为空")
    private String username;

    @ApiModelProperty(value = "登录用户密码",required = true)
    @NotEmpty(message = "登录用户密码 不能为空")
    private String password;
    
    @ApiModelProperty(value = "验证码",required = true)
    @NotEmpty(message = "验证码 不能为空")
    private String code;
}
```

如果类的属性是对象，并且也需要进行验证，则要加上@valid注解

```java
import org.hibernate.validator.constraints.Length;
 
import javax.validation.Valid;
import javax.validation.constraints.NotBlank;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Pattern;
 
@Data
public class User {
 
    @NotBlank(message = "姓名不能为空")
    private String username;
 
    @NotBlank(message = "密码不能为空")
    @Length(min = 6, max = 16, message = "密码长度为6-16位")
    private String password;
 
    @Pattern(regexp = "0?(13|14|15|17|18|19)[0-9]{9}", message = "手机号格式不正确")
    private String phone;
 
    // 嵌套必须加 @Valid，否则嵌套中的验证不生效
 
    @Valid
    @NotNull(message = "userinfo不能为空")
    private UserInfo userInfo;
}
```



只要在controller中给DTO对象加上@Validated注解就能实现自动参数校验。比如，有一个保存User的接口，要求userName长度是2-10，account和password字段长度是6-20。如果校验失败，会抛出MethodArgumentNotValidException异常，Spring默认会将其转为400（Bad Request）请求。

```java
@PostMapping("/save")
public Result saveUser(@RequestBody @Validated UserDTO userDTO) {
    // 校验通过，才会执行业务逻辑处理
    return Result.ok();
}
```
这种情况下，使用@Valid和@Validated都可以。

# 三、统一异常处理

在Spring 3.2中，新增了@ControllerAdvice、@RestControllerAdvice 注解，可以用于定义@ExceptionHandler、@InitBinder、@ModelAttribute，并应用到所有@RequestMapping、@PostMapping， @GetMapping注解中。

## 1、@ControllerAdvice定义
```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface ControllerAdvice {
    @AliasFor("basePackages")
    String[] value() default {};

    @AliasFor("value")
    String[] basePackages() default {};

    Class<?>[] basePackageClasses() default {};

    Class<?>[] assignableTypes() default {};

    Class<? extends Annotation>[] annotations() default {};
}
```
@ControllerAdvice类似切面中，可以在controller处理的时候环绕执行相应的方法。

其主要功能是：

- 捕获全局的异常

- 进行数据的绑定

- 全局数据的预处理

## 2、编写全局处理类

```java
package com.yuya.common.config;

import javax.servlet.http.HttpServletResponse;

import org.apache.ibatis.transaction.TransactionException;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import com.yuya.common.exception.BusException;
import com.yuya.common.exception.ErrorEnum;
import com.yuya.common.exception.ExtendException;
import com.yuya.common.utils.ResponseUtils;


/**
 * 统一异常拦截，使用注解@ControllerAdvice实现
 */
@RestControllerAdvice
public class GlobalExceptionHandler {

    private Logger logger = LoggerFactory.getLogger(this.getClass());

    @ExceptionHandler(BusException.class)
    public void handleBusExp(HttpServletResponse response, Exception ex) throws TransactionException {
        ErrorEnum errorEnum = ((BusException) ex).getErrorEnum();
        ResponseUtils.sendFail(response, errorEnum);
    }

    @ExceptionHandler(ExtendException.class)
    public void handleExtendExp(HttpServletResponse response, Exception ex) throws TransactionException {
        ExtendException ee = (ExtendException) ex;
        ResponseUtils.sendFail(response, HttpServletResponse.SC_INTERNAL_SERVER_ERROR, ee.getCode(), ee.getMsg());
    }


    @ExceptionHandler(MethodArgumentNotValidException.class)
    public void handleArgumentNotValidException(HttpServletResponse response, MethodArgumentNotValidException e) throws TransactionException {
        ResponseUtils.sendFail(response, HttpServletResponse.SC_OK, -1, e.getBindingResult().getFieldError().getDefaultMessage());
    }
}

```