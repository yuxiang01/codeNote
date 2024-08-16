# 1. SpringMVC-基本介绍
1. SpringMVC 是 WEB 层框架 (SpringMVC 接管了 Web 层组件, 同时支持 MVC 的开发模式/开发架构)
2. SpringMVC 通过注解，让 POJO 成为控制器，不需要继承类或者实现接口
3. SpringMVC 采用低耦合的组件设计方式，具有更好扩展和灵活性.
4. 支持 REST 格式的 URL 请求
5. SpringMVC 是基于 Spring 的, 也就是 SpringMVC 是在 Spring 基础上的

## 1.1 梳理 Spring 三大框架的关系
1. `SpringMVC` 只是 `Spring` 处理 WEB 层请求的一个模块/组件
	- `SpringMVC` 的基石是 `Servlet`
2. `SpringBoot`是为了简化开发者的使用, 推出的封神框架 (约定优于配置，简化了 `Spring` 的配置流程)
	- `SpringBoot` 包含很多组件/框架，`Spring` 就是最核心的内容之一，也包含 `SpringMVC`
3. 关系大概是: `SpringBoot` > `Spring` > `SpringMVC` 
![[梳理Spring SpringMVC SpringBoot的关系.excalidraw|800]]

## 1.2 快速入门
1. 创建 web 工程并配置 Tomcat 
2. 导入 SpringMVC 开发需要的 jar 包
3. 创建 spring 的容器文件 `dispatcherServlet-servlet.xml`
	```xml
	<!--配置要自动扫描的包-->  
	<context:component-scan base-package="controller"/>  
	<!--配置视图解析器-->  
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">  
	  <!--配置属性prefix和suffix-->  
	  <property name="prefix" value="/WEB-INF/page/"/>  
	  <property name="suffix" value=".jsp"/>  
	</bean>
	```
4. 配置 `WEB-INF/web.xml`
	```xml
	<!--配置中央控制器/分发控制器-->  
	<servlet>  
	  <servlet-name>dispatcherServlet</servlet-name>  
	  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
	  <!--  
	  配置属性contextConfigLocation,指定DispatcherServlet去操作Spring的配置文件
	  不显示配置就会按照默认配置查找Spring的配置文件,
	  默认配置规则: 
		  1.Spring的配置文件与该配置文件放同一文件夹
		  2.默认查找名字为“${servletName}-servlet.xml”的Spring配置文件
		  3.如果没有配置又没有找到该文件,就会报错
	  <init-param>  
	    <param-name>contextConfigLocation</param-name>    
	    <param-value>classpath:dispatcherServlet-servlet.xml</param-value>  
	  </init-param>  
	  -->  
	  <!--在web项目启动时,就自动加载DispatcherServlet-->  
	  <load-on-startup>1</load-on-startup>  
	</servlet>  
	<servlet-mapping>  
	  <servlet-name>dispatcherServlet</servlet-name>  
	  <url-pattern>/</url-pattern>  
	</servlet-mapping>
	```
5. 创建 `controller/UserServlet.java`
	```java
	@Controller  
	public class UserServlet {  
	  // 编写方法,响应用户请求  
	  @RequestMapping(value = "/login")  
	  public String login() {  
	    System.out.println("login ok...");  
	    return "login_ok"; // 返回结果给视图解析器,ta会根据配置决定跳转页面  
	  }  
	}
	```

## 1.3 注意事项和细节说明
1. UserServlet 需要注解成@Controller, 我们称为一个 Handler 处理器
2. UserServlet 指定 url 时, 可以省略 value, 如 `@RequestMapping("/login")`  
3. 关于 SpringMVC 的 DispatcherServlet 的配置文件
	- 如果不在 `web.xml` 指定 `applicationContext-mvc.xml`
		- 默认在 `/WEB-INF/${servletName}-servlet.xml` 找这个配置文件
		- 没有配置、又没有默认找到文件, 就会报错

# 2. SpingMVC 执行流程 
![[Pasted image 20230115111901.png]]

# 3. `@RequestMapping`  
- `@RequestMapping` <font color="#2DC26B">注解</font>可以<font color="#2DC26B">指定</font>控制器/处理器的某个<font color="#2DC26B">方法的请求的 url</font>
 - 基础用法: `RequestMapping(value = "/login")`
 - SpringMVC 控制器默认支持 GET 和 POST 两种方式
	 - 当明确指定了 method , 则需要按指定方式请求, 否则会报 405
- `@RequestMapping` 可指定 params 和 headers 支持简单表达式 
	- param: 表示请求必须包含名为 param 的请求参数
		- `params = {"username=fishx", "password=123456"}`
			- 参数 username 和 password 必须与值相等
		- `params = {"username=fishx", "password"}`
			- 必须包含参数 username 和 password 且 username 必须与值相等
		- `params = {"username=fishx", "password!=123456"}`
			- 必须包含参数 username 和 password 而 password 不等于值, 且 username 必须与值相等
- `@RequestMapping` 支持 Ant 风格资源地址
	- `?` 匹配文件名中的一个字符
		- `/user/createUser??`: 匹配 `/user/createUseraa、/user/createUserbb` 等 URL 
	- `*` 匹配文件名中的任意字符
		- `/user/*/createUser`: 匹配 `/user/aaa/createUser、/user/bbb/createUser` 等 URL
	- `**` 匹配多层路径
		- `/user/**/createUser`: 匹配 `/user/createUser、/user/aaa/bbb/createUser` 等 URL 
- `@RequestMapping` 还可以配合 `@PathVariable` 映射 URL 绑定的占位符
	```java
	@GetMapping("/reg/{username}/{userid}")  
	public String register(@PathVariable String userid, @PathVariable("username") String username) {  
	  System.out.println("接收到参数--" + "username= " + username + "--" + "userId= " + userid);  
	  return "login_ok";  
	}
	```
- `@RequestMapping(value = "/user")` 给类添加, 就是 `前缀`

# 4. SpringMVC 映射请求数据
- 获取参数值
	```java
	/*
	 * @RequestParam(value="name", required=false)
	 * 1. @RequestParam: 表示说明一个接受到的参数
	 * 2. value="name": 接收的参数名是 name
	 * 3. required=false : 表示该参数可以有,也可以没有,如果required=true,表示必须传递该参数.
	 *   默认是 required=true
	 */
	@RequestMapping(value = "/vote01") 
	public String test01(@RequestParam(value = "name", required = false) String username) {
		System.out.println("得到的 username= " + username); 
		//返回到一个结果 
		return "success";
	}
	```
- 获取 http 请求消息头
	- 形参列表 `@RequestHeader("Host") String host`
- 获取 javabean 形式的数据
	```java
	public class Master {  
	  private Integer id;  
	  private String name;  
	  private Pet pet; // 级联属性
	  // Getter、Setter、toString()...
	}
	```
	
	```html
	<!--  
	1. 这里的字段名称和对象的属性名保持一致,级联添加属性也是一样保持名字对应关系  
	2. 如果只是添加主人信息，则去掉宠物号和宠物名输入框即可,pet 为 null
	-->  
	<form action="master/v1" method="post">  
	  主人号:<input type="text" name="id"><br>  
	  主人名:<input type="text" name="name"><br>  
	  宠物号:<input type="text" name="pet.id"><br>  
	  宠物名:<input type="text" name="pet.name"><br>  
	  <input type="submit" value="添加主人和宠物">  
	</form>
	```
	
	```java
	@RequestMapping("/v1")  
	public String test01(Master master) {  
	  System.out.println("master = " + master);  
	  return "login_ok";  
	}
	```
- 获取 servlet api
	- 使用 servlet api , 需要引入 `servlet-api.jar` 
	![[Pasted image 20230116102731.png]]

# 6. 模型数据处理
## 6.1 数据放入 request 域
### 方式一: 通过 HttpServletRequest
通过 HttpServletRequest 放入 request 域
1. SpringMVC 会自动把获取的 model 模型, 放入 request 域中 `("master", master)` 
2. 也可以手动将 master 放入 request `通过HttpServletRequest` 
3. SpringMVC 默认存放对象到 request 域中的属性名是类名/类型名 (首字母小写)

### 方式二: 放入 Map<String,Object> 
通过请求的方法参数 `Map<String,Object>` 放入 request 域 
```java
@RequestMapping("/v2")  
public String test02(Master master, Map<String, Object> map) {  
  // 通过Map集合,添加属性到request中  
  // 原理分析: SpringMVC会遍历Map集合,然后将Map的k-v,存放到request域  
  map.put("address", "上海");  
  System.out.println("master = " + master); 
  map.put("master",null); // 替换了Master,因此request域中的master也是null
  return "v1_ok";  
}
```

### 方式三: 返回 ModelAndView 对象 
通过返回 ModelAndView 对象实现 request 域数据
```java
@RequestMapping("/v3")  
public ModelAndView test03(Master master) {  
  ModelAndView modelAndView = new ModelAndView();  
  modelAndView.addObject("address", "天津");  
  System.out.println("master = " + master);  
  modelAndView.setViewName("v1_ok"); // 指定跳转视图名称  
  return modelAndView;  
}
```

## 6.2 数据放入 session 域
```java
@RequestMapping("/v4")  
public String test04(Master master, HttpSession session) {  
  session.setAttribute("master", master);  
  session.setAttribute("address", "重庆");  
  System.out.println("master = " + master);  
  return "v1_ok";  
}
```

## 6.3 @ModelAttribute 实现 prepare 方法
开发中，有时需要使用某个前置方法 (比如 prepareXxx (), 方法名由程序员定) 给目标方法准备一个模型对象
1. @ModelAttribute 注解可以实现这样的需求
2. 在某个方法上，增加了@ModelAttribute 注解后
3. 那么在调用该 Handler 的任何一个方法时，都会先调用这个方法
```java
/** 
 * 1. 当在某个方法上，增加了@ModelAttribute 注解 
 * 2. 那么在调用该 Handler 的任何一个方法时，都会先调用这个方法 
*/ 
@ModelAttribute 
public void prepareModel() { 
  System.out.println("prepareModel()-----完成准备工作-----");
}																						
```

- `@ModelAttribute` 最佳实践
	- 修改用户信息（就是经典的使用这种机制的应用）
		1. 在修改前，在前置方法中从数据库查出这个用户
		2. 在修改方法 (目标方法) 中，可以使用前置方法从数据库查询的用户
		3. 如果表单中对用户的某个属性修改了，则以新的数据为准，如果没有修改，则以数据库的信息为准，比如，用户的某个属性不能修改，就保持原来的值

# 7. 视图和视图解析器 
1. 在 springMVC 中的目标方法最终返回都是一个视图 (有各种视图).
2. 返回的视图都会由一个视图解析器来处理 (视图解析器有很多种)

## 7.1 自定义视图
在默认情况下，我们都是返回默认的视图, 然后这个返回的视图交由 SpringMVC 的 `InternalResourceViewResolver` 视图处理器来处理的 
1. 创建自定义视图类
	- 创建一个 View 的 bean, 同时继承自 `AbstractView`, 并实现 `renderMergedOutputModel` 方法 
	- 添加注解, 加入到 IOC 容器中
2. 配置 `dispatcherServlet-servlet.xml`, 增加自定义视图解析器
	```xml
	<!--  
	自定义视图解析器  
	BeanNameViewResolver可以去解析自定义视图  
	属性order表示解析器要执行的顺序(值越小,越先执行)  
	-->  
	<bean class="org.springframework.web.servlet.view.BeanNameViewResolver">  
	  <property name="order" value="99"/>  
	</bean>
	```
3. 自定义视图-工作流程
	1. SpringMVC 调用目标方法, 返回自定义 View 在 IOC 容器中的 id
	2. SpringMVC 调用 BeanNameViewResolver 解析视图
		- 从 IOC 容器中获取返回 id 值对应的 bean, 即自定义的 View 的对象
	3. SpringMVC 调用自定义视图的 renderMergedOutputModel 方法渲染视图
	4. 如果在 SpringMVC 调用目标方法, 返回自定义 View 在 IOC 容器中的 id 不存在, 则仍然按照默认的视图处理器机制处理.

## 7.2 目标方法直接指定转发或重定向
1. 默认返回的方式是请求转发，然后用视图处理器进行处理
	![[Pasted image 20230116153620.png]]
2. 也可以在目标方法直接指定重定向或转发的 url 地址
3. 如果指定重定向，不能定向到 /WEB-INF 目录中
	```java
	public String order() {
		System.out.println("=======order()=====");
		// 这里直接指定 转发到哪个页面 
		return "forward:/WEB-INF/pages/my_view.jsp"; 
		// 重定向, 如果是重定向，就不能重定向到 /WEB-INF 目录中 
		return "redirect:/login.jsp";
	}
	```

# 8. 数据格式化

Spring MVC 上下文中内建了很多转换器，可完成大多数 Java 类型的转换工作。

## 8.1 基本数据类型和字符串自动完成转换

基本数据类型和字符串自动完成转换

## 8.2 特殊数据类型和字符串间的转换
1. 特殊数据类型和字符串之间的转换使用注解 (比如日期，规定格式的小数比如货币形式等)
2. 对于日期和货币可以使用 @DateTimeFormat 和 @NumberFormat 注解. 把这两个注解标记在字段上即可.
	```java
	@DateTimeFormat(pattern = "yyyy-MM-dd")  
	private Date birthday;  
	@NumberFormat(pattern = "###,###.##")  
	private float salary;
	```

# 9. 验证以及国际化

1. 对输入的数据 (比如表单数据)，进行必要的验证，并给出相应的提示信息。
2. 对于验证表单数据，SpringMVC 提供了很多实用的注解, 这些注解由 `JSR 303` 验证框架提供.
	- JSR 303 是 Java 为 Bean 数据合法性校验提供的标准框架，它已经包含在 JavaEE 中
	- 通过在 Bean 属性上标注类似于 @NotNull、@Max 等标准的注解指定校验规则，并通过标准的验证接口对 Bean 进行验证
	![[Pasted image 20230122165614.png]]
3. 注解联合使用
	- @NotEmpty `Asserts that the annotated string, collection, map or array is not null or empty.`
	- @NotNull `The annotated element must not be {@code null}.`
	- age 是 Integer 包装类不能使用 `@NotEmpty`
		```java
		@Range(max = 100, min = 1)  
		@NotNull  
		private Integer age;
		```

jsp 页面代码
```html
<form:form action="save" method="POST" modelAttribute="monster">  
  妖怪名字: <form:input path="name"/> <form:errors path="name"/><br><br>  
  妖怪年龄~:<form:input path="age"/> <form:errors path="age"/><br><br>  
  电子邮件: <form:input path="email"/> <form:errors path="email"/><br><br>  
  妖怪生日: <form:input path="birthday"/> <form:errors path="birthday"/>要求以"9999-11-11"的形式<br><br>  
  妖怪工资: <form:input path="salary"/> <form:errors path="salary"/>要求以"123,890.12"的形式<br><br>  
  <input type="submit" value="添加妖怪"/>  
</form:form>
```

- 细节说明和注意事项
	- 在需要验证的 Javabean/POJO 的字段上加上相应的验证注解
	- 目标方法上, 在 JavaBean/POJO 类型的参数前, 添加 @Valid 注解. 告知 SpringMVC 该 bean 是需要验证的
		```java
	   public String save(@Valid Monster monster, Errors errors, Map<String, Object> map)
	   ```
	- 在 @Valid 注解之后, 添加一个 Errors 或 BindingResult 类型的参数, 可以获取到验证的错误信息
	- 需要使用 `<form:errors path="email"></form:errors>` 标签来显示错误消息, 这个标签，需要写在 `<form:form>` 标签内生效. 
	- 错误消息的国际化文件 `i18n.properties` , 中文需要是 Unicode 编码，使用工具转码.
		```json
		NotEmpty.monster.name=\u7528\u6237\u540d\u4e0d\u80fd\u4e3a\u7a7a
		typeMismatch.monster.birthday=\u751f\u65e5\u683c\u5f0f\u4e0d\u6b63\u786e  
		typeMismatch.monster.salary=\u85aa\u6c34\u683c\u5f0f\u4e0d\u6b63\u786e
	   ```

## 取消某个属性的绑定
```java
//取消绑定 monster 的 name 表单提交的值给 monster.name 属性
@InitBinder 
public void initBinder(WebDataBinder dataBinder) {
	/*
	  1. setDisallowedFields() 是可变形参，可以指定多个字段 
	  2. 当将一个字段/属性，设置为 disallowed,就不在接收表单提交的值 
	  3. 那么这个字段/属性的值，就是该对象默认的值 (具体看程序员定义时指定)
	*/
	dataBinder.setDisallowedFields("name");
}
```

![[Pasted image 20230123200839.png]]

# 10. 中文乱码处理
## 10.1 自定义中文乱码过滤器
写一个类, 去实现 `Filter接口`, 完成 xml 配置, 实现一个过滤器
```java
servletRequest.setCharacterEncoding("utf-8");
```

## 10.2 Spring 提供的过滤器处理中文 
```xml
<filter>  
  <filter-name>characterEncodingFilter</filter-name>  
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>  
  <init-param>  
    <param-name>encoding</param-name>  
    <param-value>UTF-8</param-value>  
  </init-param>  
</filter>  
<filter-mapping>  
  <filter-name>characterEncodingFilter</filter-name>  
  <url-pattern>/*</url-pattern>  
</filter-mapping>
```

# 11. 处理 json 
处理 json 和 `HttpMessageConverter<T>` 
## 11.1 处理 JSON `@ResponseBody`  

```java
@RequestMapping(value = "/dog")  
//指定返回的数据格式是json 
@ResponseBody
public Dog getJson() {  
  return new Dog("jack", "beijing");  
}
```

## 11.2 处理 JSON `@RequestBody`  
```java
@RequestMapping(value = "/user")  
@ResponseBody  
// 形参user指定了@RequestBody,SpringMVC就会将提交的Json填充个指定的JavaBean  
public User getUser(@RequestBody User user) {  
  System.out.println(user);  
  // 然后又将user对象以Json格式返回(需要标注@ResponseBody)  
  return user;  
}
```
- 注意事项和细节
	- 目标方法正常返回 JSON 需要的数据, 可以是一个对象, 也可以是一个集合
	- @ResponseBody 可以直接写在 controller 上，这样对所有方法生效
		- @Controller 和 @ResponseBody 可以直接写成 @RestController 
 
### 使用 api 请求工具
* 请求头 `Header` 添加参数 `Content-Type: application/json`
	![[Pasted image 20230412142519.png]]
* 请求体 `Body` 选择 `Row` 然后填写要携带的 Json 
	![[Pasted image 20230412142631.png]]

## 11.3 `HttpMessageConverter<T>` 

SpringMVC 处理 JSON-底层实现是依靠 `HttpMessageConverter<T>` 来进行转换的

![[Pasted image 20230124130238.png]]

* 使用 `HttpMessageConverter<T>` 将请求信息转化并绑定到处理方法的入参中, 或将响应结果转为对应类型的响应信息, Spring 提供了两种途径: 
	* 使用 `@RequestBody` `@ResponseBody` 对目标方法进行标注
	* 使用 `HttpEntity<T>` `@ResponseEntity<T>` 作为目标方法的入参或返回值

- 当控制器处理方法使用到 `@RequestBody` `@ResponseBody` `HttpEntity<T>` `ResponseEntity<T>` 时, 
	- Spring 首先根据请求头或响应头的 Accept 属性选择匹配的 HttpMessageConverter
		- 进而根据参数类型或泛型类型的过滤得到匹配的 HttpMessageConverter
		- 若找不到可用的 HttpMessageConverter 将报错

# 12. 文件下载和上传 
## 12.1 文件下载 `ResponseEntity`
```java
@RequestMapping("/down")  
public ResponseEntity<byte[]> downFile(HttpSession session) throws IOException {  
  /*  
   构建RequestMapping对象:  
   public ResponseEntity(     
     @Nullable T body, --下载的文件数据  
     @Nullable MultiValueMap<String, String> headers, --得到http响应头Headers  
     HttpStatus status --http响应状态  
   ) {}  
   */  
  // 1. 先获取到下载文件的inputStream  
  InputStream inputStream = session.getServletContext().getResourceAsStream("/img/img.png");  
  // 2. 开辟一个存放文件的byte[],这里使用byte[]存放2进制数据  
  byte[] bytes = new byte[inputStream.available()];  
  // 3. 将要下载的文件存入byte[]  
  System.out.println(inputStream.read(bytes));  
  // 4. http响应状态  
  HttpStatus httpStatus = HttpStatus.OK;  
  // 5. http响应头Headers  
  HttpHeaders headers = new HttpHeaders();  
  headers.add("Content-Disposition","attachment;filename=img.png");  
  return new ResponseEntity<byte[]>(bytes,headers,httpStatus);  
}
```

## 12.2 文件上传
1. SpringMVC 为文件上传提供了直接的支持，这种支持是通过即插即用的 MultipartResolver 实现的
	- Spring 用 `Jakarta Commons FileUpload技术` 
		- 实现了一个 MultipartResolver 的实现类：CommonsMultipartResovler
2. SpringMVC 上下文中默认没有装配 MultipartResovler
	- 因此默认情况下不能处理文件的上传工作，如果想使用 Spring 的文件上传功能
		- 需现在上下文中配置 MultipartResolver
		 ![[Pasted image 20230124152640.png]]


```java
@RequestMapping("upload")  
public String fileUpload(@RequestParam MultipartFile file, HttpServletRequest request) throws IOException {  
  // 获取文件名  
  String filename = file.getOriginalFilename();  
  System.out.println("文件名为" + filename);  
  // 确认保存路径[全路径]  
  String realPath = request.getServletContext().getRealPath("/img/" + filename);  
  System.out.println("全路径: " + realPath);  
  // 创建File文件对象  
  File saveToFile= new File(realPath);  
  // 将上传的文件转存至saveToFile  
  file.transferTo(saveToFile);  
  return "/login_ok";  
}
```

# 13. 拦截器
- 拦截器是一种在 Java 中用于在请求和响应处理之间添加额外处理的机制
	- 它们可用于实现认证、授权、日志记录等功能。
	- 拦截器可以在请求到达目标之前和响应返回客户端之后执行操作。
- 过滤器和拦截器都是在请求和响应处理之间添加额外处理的机制，但它们有一些重要的区别:
	- 过滤器是 Java Servlet 规范中定义的一种特殊类型，用于对请求和响应进行过滤。
		- 过滤器可以访问请求和响应的数据，并且可以对它们进行修改。
		- 过滤器通常用于在请求和响应之间进行安全检查，例如认证和授权
	- 拦截器是 JavaEE 规范中定义的一种特殊类型，用于拦截请求和响应。
		- 拦截器可以在请求到达目标之前和响应返回客户端之后执行操作。
		- 拦截器则可以用于实现更复杂的处理，例如性能监视和日志记录。
	- 总而言之, 拦截器更加强大和灵活，能够在请求进入方法前和离开方法后进行操作，而过滤器只能在请求和响应之间进行操作。

## 13.1 自定义拦截器及执行流程分析图 
![[Pasted image 20230126102924.png]]

- 自定义拦截器执行流程说明
	- 如果 preHandle 方法返回 false, 则不再执行目标方法, 可以在此指定返回页面
	- postHandle 在目标方法被执行后执行 . 可以在方法中访问到目标方法返回的 ModelAndView 对象
	- 若 preHandle 返回 true, 则 afterCompletion 方法在渲染视图之后被执行.
	- 若 preHandle 返回 false, 则 afterCompletion 方法不会被调用
	- 在配置拦截器时，可以指定该拦截器对哪些请求生效，哪些请求不生效

```java
@Component  
public class MyInterceptor01 implements HandlerInterceptor {  
  @Override  
  // 在业务处理器处理请求之前被调用,在该方法中对用户请求 request 进行处理。  
  public boolean preHandle(...) throws Exception {  
    ...  
    return boolean;  
  }  
  
  @Override  
  // 方法在目标方法处理完请求后执行  
  public void postHandle(...) throws Exception {  
    ... 
  }  
  
  @Override  
  // 方法在完全处理完请求后被调用,可以在该方法中进行一些资源清理的操作。  
  public void afterCompletion(...) throws Exception {  
    ...
  }  
}
```
## 13.2 多个拦截器及执行流程分析图
![[Pasted image 20230126213625.png|800]]
![[多个拦截器执行流程.excalidraw|800]]

## 13.3 注意事项和细节
1. 默认配置是都所有的目标方法都进行拦截, 也可以指定拦截目标方法
	```xml
	<mvc:interceptor> 
		<mvc:mapping path="/hi"/> 
		<ref bean="myInterceptor01"/> 
	</mvc:interceptor>
	```
2. mvc: mapping 支持通配符, 同时指定不对哪些目标方法进行拦截
	```xml
	<mvc:interceptor> 
	  <mvc:mapping path="/h*"/> 
	  <mvc:exclude-mapping path="/hello"/>
	  <ref bean="myInterceptor01"/>
	</mvc:interceptor>
	```
3. 拦截器需要配置才生效，不配置是不生效的.
4. 如果 `preHandler()` 方法返回了 `false`, 就不会执行目标方法 (前提是你的目标方法被拦截了), 程序员可以在这里根据业务需要指定跳转页面.
5. 如果第 1 个拦截器的 `preHandle()` 返回 false, 后面都不在执行 
6. 如果第 2 个拦截器的 `preHandle()` 返回 false, 就直接执行第 1 个拦截器的 afterCompletion () 方法, 如果拦截器更多，规则类似 
7. 拦截器简单实例
	```java
	@Override  
	// 在业务处理器处理请求之前被调用,在该方法中对用户请求 request 进行处理。  
	public boolean preHandle(  
	    HttpServletRequest request,  
	    HttpServletResponse response,  
	    Object handler  
	) throws Exception {  
	  System.out.println("MyInterceptor01 preHandle()...");  
	  String keyword = request.getParameter("keyword");  
	  if ("郭嘉".equals(keyword)) {  
	    request.getRequestDispatcher("fail.jsp")  
	        .forward(request,response);  
	    return false;  
	  }  
	  return true;  
	}
	```

# 14. 异常处理

1. SpringMVC 通过 HandlerExceptionResolver 绑定以及目标方法执行时发生的异常。
	- 处理程序的异常, 包括 Handler 映射、数据
2. 主要处理 Handler 中用 @ExceptionHandler 注解定义的方法。
3. SpringMVC 是通过 ExceptionHandlerMethodResolver 处理异常
	- 先到本类查找, 如果局部若找不到 @ExceptionHandler 注解的话 
		- 就会去找 @ControllerAdvice 类的 @ExceptionHandler 注解方法 
			- 再找 `SimpleMappingExceptionResolver` 
				- 依然没有找到, 就由 Tomcat 默认处理
4. 异常处理的优先级:
	- 局部异常 > 全局异常 > SimpleMappingExceptionResolver > tomcat 默认机制

## 14.1 局部异常
在本类 Handler 中添加局部异常
```java
@ExceptionHandler({ArithmeticException.class, NullPointerException.class})  
public String localAnomaly(Exception ex, HttpServletRequest request) {  
  System.out.println("异常信息：" + ex.getMessage());  
  request.setAttribute("reason",ex.getMessage());  
  return "error_1";  
}
```

## 14.2 全局异常
新建一个类, 给类添加 `@ControllerAdvice` 注解
```java
@ExceptionHandler({NumberFormatException.class, ClassCastException.class})  
public String globalException(Exception ex, HttpServletRequest request) {  
  System.out.println("全局异常：" + ex.getClass().getSimpleName()+ " , " + ex.getMessage());  
  request.setAttribute("reason", ex.getMessage());  
  return "error_1";  
}
```

## 14.3 自定义异常
```java
@ResponseStatus(reason = "年龄合理范围为1-120之间", value = HttpStatus.BAD_REQUEST)  
public class AgeException extends RuntimeException {  
  public AgeException() {}  
  
  public AgeException(String message) {  
    super(message);  
  }  
}
```

可以让全局异常、局部异常处理

## 14.4 SimpleMappingExceptionResolver
1. 如果希望对所有异常进行统一处理，可以使用 SimpleMappingExceptionResolver
2. 它将异常类名映射为视图名，即发生异常时使用对应的视图报告异常
3. 需要在 ioc 容器中配置

统一未归类异常
```xml
<!--配置一个统一异常处理-->  
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">  
  <property name="exceptionMappings">  
    <props>  
      <prop key="java.lang.Exception">error</prop>  
    </props>  
  </property>  
</bean>
```

```xml
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
	<head>  
	  <title>未知异常信息</title>  
	</head>  
	<body>  
	  <h1>朋友，系统发生了未知异常，请上报...</h1>  
	</body>  
</html>
```