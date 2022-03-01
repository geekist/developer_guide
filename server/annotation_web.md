

**5 种常见的请求类型:**

- **GET** ：请求从服务器获取特定资源。举个例子：`GET /users`（获取所有学生）

- **POST** ：在服务器上创建一个新的资源。举个例子：`POST /users`（创建学生）

- **PUT** ：更新服务器上的资源（客户端提供更新后的整个资源）。举个例子：`PUT /users/12`（更新编号为 12 的学生）

- **DELETE** ：从服务器删除特定的资源。举个例子：`DELETE /users/12`（删除编号为 12 的学生）

- **PATCH** ：更新服务器上的资源（客户端提供更改的属性，可以看做作是部分更新），使用的比较少，这里就不举例子了。


## @Controller

此注解使用在class上声明此类是一个Spring controller，是@Component注解的一种具体形式。

## @RequestMapping

此注解可以用在class和method上，用来映射web请求到某一个handler类或者handler方法上。当此注解用在Class上时，就创造了一个基础url，其所有的方法上的@RequestMapping都是在此url之上的。可以使用其method属性来限制请求匹配的http method。

```
@Controller
@RequestMapping("/users")
public class UserController {

@RequestMapping(method = RequestMethod.GET)
public String getUserList() {
    return "users";
}
}
```
此外，Spring4.3之后引入了一系列@RequestMapping的变种。如下：

@GetMapping
@PostMapping
@PutMapping
@PatchMapping
@DeleteMapping

分别对应了相应method的RequestMapping配置。

## CookieValue

此注解用在@RequestMapping声明的方法的参数上，可以把HTTP cookie中相应名称的cookie绑定上去。

```
@ReuestMapping("/cookieValue")
      public void getCookieValue(@CookieValue("JSESSIONID") String cookie){
}
cookie即http请求中name为JSESSIONID的cookie值。
```

## @CrossOrigin

此注解用在class和method上用来支持跨域请求，是Spring 4.2后引入的。

```
@CrossOrigin(maxAge = 3600)
@RestController
@RequestMapping("/users")
public class AccountController {
    @CrossOrigin(origins = "http://xx.com")
    @RequestMapping("/login")
    public Result userLogin() {
        // ...
    }
}
```
## @ExceptionHandler

此注解使用在方法级别，声明对Exception的处理逻辑。可以指定目标Exception。

## @InitBinder

此注解使用在方法上，声明对WebDataBinder的初始化(绑定请求参数到JavaBean上的DataBinder)。在controller上使用此注解可以自定义请求参数的绑定。

## @MatrixVariable

此注解使用在请求handler方法的参数上，Spring可以注入matrix url中相关的值。这里的矩阵变量可以出现在url中的任何地方，变量之间用;分隔。如下：

```
// GET /pets/42;q=11;r=22
@RequestMapping(value = "/pets/{petId}")
public void findPet(@PathVariable String petId, @MatrixVariable int q) {
    // petId == 42
    // q == 11
}
```
需要注意的是默认Spring mvc是不支持矩阵变量的，需要开启。

<mvc:annotation-driven enable-matrix-variables="true" />
注解配置则需要如下开启：
```
@Configuration
public class WebConfig extends WebMvcConfigurerAdapter {
 
    @Override
    public void configurePathMatch(PathMatchConfigurer configurer) {
        UrlPathHelper urlPathHelper = new UrlPathHelper();
        urlPathHelper.setRemoveSemicolonContent(false);
        configurer.setUrlPathHelper(urlPathHelper);
    }
}
```

## @PathVariable

此注解使用在请求handler方法的参数上。@RequestMapping可以定义动态路径，如:
```
@RequestMapping("/users/{uid}")
可以使用@PathVariable将路径中的参数绑定到请求方法参数上。

@RequestMapping("/users/{uid}")
public String execute(@PathVariable("uid") String uid){
}
```

## @RequestAttribute

此注解用在请求handler方法的参数上，用于将web请求中的属性(request attributes，是服务器放入的属性值)绑定到方法参数上。

## @RequestBody

此注解用在请求handler方法的参数上，用于将http请求的Body映射绑定到此参数上。HttpMessageConverter负责将对象转换为http请求。

## @RequestHeader

此注解用在请求handler方法的参数上，用于将http请求头部的值绑定到参数上。

## @RequestParam

此注解用在请求handler方法的参数上，用于将http请求参数的值绑定到参数上。

## @RequestPart

此注解用在请求handler方法的参数上，用于将文件之类的multipart绑定到参数上。

## @ResponseBody

此注解用在请求handler方法上。和@RequestBody作用类似，用于将方法的返回对象直接输出到http响应中。

## @ResponseStatus

此注解用于方法和exception类上，声明此方法或者异常类返回的http状态码。可以在Controller上使用此注解，这样所有的@RequestMapping都会继承。

## @ControllerAdvice

此注解用于class上。前面说过可以对每一个controller声明一个ExceptionMethod。这里可以使用@ControllerAdvice来声明一个类来统一对所有@RequestMapping方法来做@ExceptionHandler、@InitBinder以及@ModelAttribute处理。

## @RestController

此注解用于class上，声明此controller返回的不是一个视图而是一个领域对象。其同时引入了@Controller和@ResponseBody两个注解。

## @RestControllerAdvice

此注解用于class上，同时引入了@ControllerAdvice和@ResponseBody两个注解。

## @SessionAttribute

此注解用于方法的参数上，用于将session中的属性绑定到参数。

## @SessionAttributes

此注解用于type级别，用于将JavaBean对象存储到session中。一般和@ModelAttribute注解一起使用。如下：

```
@ModelAttribute("user")

public PUser getUser() {}

// controller和上面的代码在同一controller中
@Controller
@SeesionAttributes(value = "user", types = {
    User.class
})

public class UserController {}
```

#### 3.1. GET 请求

`@GetMapping("users")` 等价于 `@RequestMapping(value="/users",method=RequestMethod.GET)`

```java
@GetMapping("/users")
public ResponseEntity<List<User>> getAllUsers() {
 return userRepository.findAll();
}
```

#### 3.2. POST 请求

`@PostMapping("users")` 等价于`@RequestMapping(value="/users",method=RequestMethod.POST)`

关于`@RequestBody`注解的使用，在下面的“前后端传值”这块会讲到。

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@Valid @RequestBody UserCreateRequest userCreateRequest) {
 return userRespository.save(userCreateRequest);
}
```

#### 3.3. PUT 请求

`@PutMapping("/users/{userId}")` 等价于`@RequestMapping(value="/users/{userId}",method=RequestMethod.PUT)`

```java
@PutMapping("/users/{userId}")
public ResponseEntity<User> updateUser(@PathVariable(value = "userId") Long userId,
  @Valid @RequestBody UserUpdateRequest userUpdateRequest) {
  ......
}
```

#### 3.4. **DELETE 请求**

`@DeleteMapping("/users/{userId}")`等价于`@RequestMapping(value="/users/{userId}",method=RequestMethod.DELETE)`

```java
@DeleteMapping("/users/{userId}")
public ResponseEntity deleteUser(@PathVariable(value = "userId") Long userId){
  ......
}
```

#### 3.5. **PATCH 请求**

一般实际项目中，我们都是 PUT 不够用了之后才用 PATCH 请求去更新数据。

```java
  @PatchMapping("/profile")
  public ResponseEntity updateStudent(@RequestBody StudentUpdateRequest studentUpdateRequest) {
        studentRepository.updateDetail(studentUpdateRequest);
        return ResponseEntity.ok().build();
    }
```

### 4. 前后端传值

**掌握前后端传值的正确姿势，是你开始 CRUD 的第一步！**

#### 4.1. `@PathVariable` 和 `@RequestParam`

`@PathVariable`用于获取路径参数，`@RequestParam`用于获取查询参数。

举个简单的例子：

```java

@GetMapping("/klasses/{klassId}/teachers")
public List<Teacher> getKlassRelatedTeachers(
         @PathVariable("klassId") Long klassId,
         @RequestParam(value = "type", required = false) String type ) {
...
}
```

如果我们请求的 url 是：`/klasses/123456/teachers?type=web`

那么我们服务获取到的数据就是：`klassId=123456,type=web`。

#### 4.2. `@RequestBody`

用于读取 Request 请求（可能是 POST,PUT,DELETE,GET 请求）的 body 部分并且**Content-Type 为 application/json** 格式的数据，接收到数据之后会自动将数据绑定到 Java 对象上去。系统会使用`HttpMessageConverter`或者自定义的`HttpMessageConverter`将请求的 body 中的 json 字符串转换为 java 对象。

我用一个简单的例子来给演示一下基本使用！

我们有一个注册的接口：

```java
@PostMapping("/sign-up")
public ResponseEntity signUp(@RequestBody @Valid UserRegisterRequest userRegisterRequest) {
  userService.save(userRegisterRequest);
  return ResponseEntity.ok().build();
}
```

`UserRegisterRequest`对象：

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class UserRegisterRequest {
    @NotBlank
    private String userName;
    @NotBlank
    private String password;
    @NotBlank
    private String fullName;
}
```

我们发送 post 请求到这个接口，并且 body 携带 JSON 数据：

```json
{"userName":"coder","fullName":"shuangkou","password":"123456"}
```

这样我们的后端就可以直接把 json 格式的数据映射到我们的 `UserRegisterRequest` 类上。

![](images/spring-annotations/@RequestBody.png)

👉 需要注意的是：**一个请求方法只可以有一个`@RequestBody`，但是可以有多个`@RequestParam`和`@PathVariable`**。 如果你的方法必须要用两个 `@RequestBody`来接受数据的话，大概率是你的数据库设计或者系统设计出问题了！