- [Spring Boot](https://spring.io/projects/spring-boot) 是什么?
	- Spring Boot 可以轻松创建独立的、生产级的基于 Spring 的应用程序
	- Spring Boot 直接嵌入 Tomcat、Jetty 或 Undertow
		- 可以"直接运行" Spring Boot 应用程序
## 一、SpringBoot 快速入门
1. 新建项目, 创建 Maven 项目 
2. 在 `pom.xml` 中引入<span style="background:#fff88f">SpringBoot 父工程</span>和<span style="background:#fff88f">web 项目场景启动器</span>
>[!note]- `pom.xml` 代码:
>```xml
>  <parent>
>     <artifactId>spring-boot-starter-parent</artifactId>
>     <groupId>org.springframework.boot</groupId>
>     <version>2.5.3</version>
> </parent>
> 
> <dependencies>
>     <dependency>
>       <groupId>org.springframework.boot</groupId>
>       <artifactId>spring-boot-starter-web</artifactId>
>     </dependency>
> </dependencies>
> ```
3. SpringBoot 于 Maven 中的依赖关系
>[!note]- `spring-boot-starter-web` web 项目场景启动器 maven 依赖示意图
> ![[SpringBoot.png]]
4. 编写 `Springboot` 启动类 `MainApp.java`
	```java
	// @SpringBootApplication: 标识为Springboot项目
	@SpringBootApplication
	public class MainApp {
	  public static void main(String[] args) {
	    // 启动SpringBoot项目
	    SpringApplication.run(MainApp.class, args);
	  }
	}
	```
5. 编写 `Controller` 控制层 
	```java
	@Controller
	public class HelloController {
	  @RequestMapping("/hello") // 标识请求url
	  @ResponseBody // 指定返回的数据格式是json 
	  public String hello() {
	    return "Hello! SpringBoot";
	  }
	}
	```

### Spring、MVC、Boot 的关系
- 大致关系: `SpringBoot` > `SpringMVC` > `Spring` 
	- `MVC`: 
		- 只是 Spring 处理 WEB 层请求的一个模块/组件
		- Spring MVC 的基石是 Servlet
	- `Spring`: 
		- 核心是 IOC 和 AOP
			- IOC 提供了依赖注入的容器
			- AOP 解决了面向切面编程
	- `SpringBoot` 
		- 为了简化开发, 推出的封神框架
			- 约定优于配置 (COC)，简化了 Spring 项目的配置流程
			- SpringBoot 包含很多组件/框架
				- Spring 就是最核心的内容之一
				- 也包含 Spring MVC

### 约定优于配置
- 约定编程: 
	- 约定优于配置 (Convention over Configuration | COC)
		- 是一种软件设计规范
	- 本质:
		- 对系统、类库或框架中一些东西假定一个大众化合理的默认值 (缺省值) 
- 简单理解: 
	- <u style="color:#2DC26B">期待的配置</u> 与 <u style="color:#2DC26B">约定的配置一致</u>
		- 那么就可以不做任何配置
	- 约定 <u style="color:#ff0000">不符合</u> 期待
		- 才需要对约定进行替换配置
- 约定其实就是一种规范，遵循了规范，那么就存在通用性
	- 存在通用性，那么事情就会变得相对简单
	- 程序员之间的沟通成本会降低，工作效率会提升，合作也会变得更加简单
## 二、依赖管理和自动配置
### 2.1 依赖管理
- `spring-boot-starter-parent` 还有父项目
	- 声明了开发中常用的依赖的版本号
- 并且进行自动版本仲裁
	- 如果程序员没有指定某个依赖 jar 的版本
		- 则以父项目指定的版本为准

![[Pasted image 20230412145418.png|400]]
![[Pasted image 20230412145423.png|400]]

#### 修改自动仲裁/默认版本号
1. 方法一: 显示导入依赖, 并明确指定 `<version>`
	- `dependencies` 节点下再新增依赖并指定版本号
	![[Pasted image 20230412145725.png]]
2. 方法二: 在 `pom.xml` 文件中, 新增 `<properties>` 并制定依赖的 key
	1. `dependencies` 节点下新增依赖
	2. 增加 `properties` 节点, 节点下指定依赖的 key
		![[Pasted image 20230412150525.png]]

#### starter 场景启动器
- 开发中我们引入了相关场景的 starter
	- 这个场景中所有的相关依赖都引入进来了
		 ![[Pasted image 20230412152000.png|400]]
	- 所有场景启动器最基本的依赖就是 Spring-boot-starter 
		- 这个依赖也就是 SpringBoot 自动配置的核心依赖
- [官方场景](https://docs.spring.io/spring-boot/docs/current/reference/html/using.html#using.build-systems.starters) 
- 第三方 starter
	- 第三方启动程序通常以项目名称开头

### 2.2 自动配置 
SpringBoot 中，存在自动配置机制，提高开发效率, 不用再像 SSM 整合写一大堆配置了.
1. 自动配置 Tomcat
2. 自动配置 SpringMVC
3. 自动配置 Web 常用功能: 比如字符过滤器...

可以通过获取 ioc 容器，查看容器创建的组件来验证
![[Pasted image 20230412155031.png]]

#### 默认扫描包结构
![[Pasted image 20230412163441.png]]
#### 修改默认扫描包结构
![[Pasted image 20230412170012.png]]
#### 修改默认约定
`resources/application.properties`
```properties
# 默认 server.port=8080
server.port=8008
# 比如: 默认 spring.servlet.multipart.max-file-size=1MB 
# 该属性可以指定 springboot 上传文件大小的限制 
# 默认配置最终都是映射到某个类上 , 比如这里配置会映射到 MultipartProperties 
# 把光标放在该属性，ctrl+b 就可以定位该配置映射到的类
spring.servlet.multipart.max-file-size=10MB
```
##### 配置大全
[[SpringBoot 默认约定#所有配置列表]]
##### 常用配置
[[SpringBoot 默认约定#常用配置]]
##### 自定义配置
`application.properties`
```properties
my.website=https://www.baidu.com
```
`具体使用: 某个bean`
```java
@Value("${my.website}") 
private String bdUrl;
```
#### 配置读取配置文件
SpringBoot 通过 `ConfigFileApplicationListener.java` 读取 `application.properites` 配置文件
![[Pasted image 20230417155629.png]]
#### 自动配置
- 自动配置 —— 遵守按需加载原则
	- 引入了哪个场景 starter 就会加载该场景关联的 jar 包
	- 没有引入的 starter 则不会加载其关联 jar

![[Pasted image 20230417160020.png]]

SpringBoot 所有的自动配置功能都在 `Spring-boot-autoconfigure` 包里面 
![[Pasted image 20230417160224.png]]

在 SpringBoot 的自动配置包, <font color="#ff0000">一般是</font> `XxxAutoConfiguration.java` 对应 `XxxxProperties.java` 
![[Pasted image 20230417160629.png]]
![[bean、java、配置文件的关系.excalidraw|800]]

>[!cite]- 如: `MultipartProperties`, `MultipartAutoConfiguration` 和 `application. properties` 
![[Pasted image 20230417194309.png]]
![[Pasted image 20230417194313.png]]

## 三、容器功能
### 3.1 Spring 注入组件的注解
`@Component`、`@Controller`、 `@Service`、`@Repository`
说明: 这些在 Spring 中的传统注解仍然有效，通过这些注解可以给容器注入组件

```xml
  <bean id="monster03" class="com.fishx.springboot.entity.Monster">
    <property name="name" value="牛魔王~"/>
    <property name="age" value="5000"/>
    <property name="skill" value="芭蕉扇~"/>
    <property name="id" value="1000"/>
  </bean>
```

```java
ApplicationContext context = new ClassPathXmlApplicationContext("springType-bean.xml");
System.out.println("Monster--" + context.getBean("monster03"));
```

### 3.2 @Configuration 注入
使用 SpringBoot 的@Configuration 添加/注入组件
>[!warning] 使用 lombok 的 @Date 自动封装, 会自动重写 hashcode 方法!
```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class Monster {
  private Integer id;

  private String name;

  private Integer age;

  private String skill;
}
```

`config/BeanConfig.java`
```java
@Configuration // 标识为一个配置类, 等价于 配置文件
public class BeanConfig {
  /*
   1. @Bean 给容器添加组件,就是Monster Bean
   2. monster01() 默认方法名monster01 作为 Bean的name/id
   3. 返回值 Monster 注入的类型
   4. new Monster(200, "牛魔王", 21, "牛魔群") 注入容器的详细信息
   5.  @Bean(name = "monster_nmw")在配置/注入Bean同时指定Bean的name
   6. 默认为单列, @Scope("prototype")修改为多例
  */
  @Bean
  @Scope("prototype")
  public Monster monster01() {
    return new Monster(200, "牛魔王", 21, "牛魔群");
  }
}
```
`MainApp.java`
```java
// @SpringBootApplication: 标识为Springboot项目
// 属性scanBasePackages[]修改包类名,只有一个可以省略{}
@SpringBootApplication
public class MainApp {
  public static void main(String[] args) {
    // 启动SpringBoot项目
    ConfigurableApplicationContext ioc
        = SpringApplication.run(MainApp.class, args);

    // SpringBoot使用 @Configuration 配置/注入 Bean
    Monster monsterNmw1 = ioc.getBean("monster01", Monster.class);
    Monster monsterNmw2 = ioc.getBean("monster01", Monster.class);
    System.out.println(monsterNmw1.hashCode() + "\t" + monsterNmw2.hashCode()
        + "\n" + monsterNmw1.equals(monsterNmw2));
    System.out.println(ioc.getBean(BeanConfig.class));

  }
}
```
>[!note]- 通过 Debug 来查看 Ioc 容器是否存在 `monster01` Bean 实例 
> - beanDefinitionMap, 只是存放了 bean 定义信息
> 	 ![[Pasted image 20230418102502.png]]
> 	 ![[Pasted image 20230418102610.png]]
> 	- 真正存放 Bean 实例的在 singleonObjectis 的 Map 中
> 		- 对于非单例，是每次动态反射生成的实例
#### Full 模式和 Lite 模式
 SpringBoot 2 新增特性： proxyBeanMethods 指定 Full 模式和 Lite 模式
```java
/**
 * proxyBeanMethods：代理 bean 的方法
 *    (1) Full(proxyBeanMethods = true)
 *      【保证每个 @Bean 方法被调用多少次返回的组件都是单例的, 是代理方式】
 *    (2) Lite(proxyBeanMethods = false)
 *      【每个 @Bean 方法被调用多少次返回的组件都是新创建的, 是非代理方式】
 *    (3) 特别说明 : proxyBeanMethods 是在调用 @Bean 方法才生效
 *       -> 因此,需要先获取 BeanConfig 组件,再调用方法
 *       -> 而不是直接通过 SpringBoot 主程序得到的容器来获取 bean
 *       -> 注意观察直接通过 ioc.getBean() 获取 Bean, proxyBeanMethods 值并没有生效
 *    (4) 如何选择:
 *       -> 组件依赖必须使用 Full 模式默认
 *       -> 如果不需要组件依赖使用 Lite 模式
 *    (5) Lite 模式 也称为轻量级模式，因为不检测依赖关系，运行速度快
 */
@Configuration(proxyBeanMethods = false) // 标识为一个配置类, 等价于 配置文件
public class BeanConfig {
  @Bean
  @Scope("prototype")
  public Monster monster01() {
	return new Monster(200, "牛魔王", 21, "牛魔群");
  }
}
```
#### 注意事项和细节
1. 配置类本身也是组件，因此也可以获取
	```java
	// 配置类本身也是组件， 因此也可以获取
    System.out.println(ioc.getBean(BeanConfig.class));
	```
2. 配置类可以有多个, 就和 Spring 可以有多个 ioc 配置文件是一个道理
### 3.3 @Import 注入组件
`ImportConfig.java`
```java
/*
  value = {} value可默认省略
  1. @Import 源码: (有一个Class[]可以注入指定类型的Bean)
    public @interface Import {
      Class<?>[] value();
    }
  2. 通过@Import注解导入的Bean,默认组件名/id为全类名
*/
@Import({Dog.class, Cat.class})
@Configuration
public class ImportConfig {
}
```
`MainApp.java`
```java
@SpringBootApplication
public class MainApp {
  public static void main(String[] args) {
    // 启动SpringBoot项目
    ConfigurableApplicationContext ioc
        = SpringApplication.run(MainApp.class, args);

    // @Import
    Dog dog = ioc.getBean(Dog.class);
    Cat cat = ioc.getBean(Cat.class);
    System.out.println("dog —— " + dog);
    System.out.println("cat —— " + cat);
  }
}
```

### 3.4 @Conditional
- 条件装配：
	- 满足 Conditional 指定的条件，则进行组件注入
- @Conditional 是一个根注解
	- 下面有很多扩展注解
	
	| @Condition 扩展注解                  | 作用 (判断是否满足当前指定文件)              |
	|:-------------------------------:|:-----------------------------:|
	| @ConditionalOnJava              | 系统 Java 版本是否符合要求                |
	| @ConditionalOnBean              | 容器中存在指定 Bean                   |
	| @ConditionalOnMissingBean       | 容器中不存在指定 Bean                  |
	| @ConditionalOnExpression        | 满足 SpEL 表达式指定                   |
	| @ConditionalOnClass             | 系统中有指定的类                      |
	| @ConditionalOnMissingClass      | 系统中没有指定的类                     |
	| @ConditionalOnSingleCandidate   | 容器中只有一个指定的 Bean, 或者此 Bean 是首选 Bean |
	| @ConditionalOnProperty          | 系统中指定的属性是否有指定的值               |
	| @ConditionalOnResource          | 类路径下是否存在指定的值                  |
	| @ConditionalOnWebApplication    | 当前是 web 环境                      |
	| @ConditionalOnNotWebApplication | 当前不是 web 环境                     |
	| @ConditionalOnJndi              | JNDI 存在指定项                     |  

```java
@Repository("beanA")
public class BeanA {
}
```

```java
@Configuration // 标识为一个配置类, 等价于 配置文件
// 标识配置类,表示对该配置类的所有要注入的组件,都进行条件约束
@ConditionalOnBean(name = "beanA")
public class BeanConfig {
  // @Bean(name = "monster_nmw")
  @Bean
  public Monster monster01() {
    return new Monster(200, "牛魔王", 21, "牛魔群");
  }

  @Bean
  /*
   @ConditionalOnBean(name = "monster_nmw"):
     1. 当容器中有一个Bean,名为monster_nmw,就注入dog01这个Dog Bean
     2. 如果没有monster_nmw Bean就不注入dog01这个Dog Bean
     3. 只会 限制name 存在,不会检测 限制name 的类型
  */
  // 当容器中没有一个Bean,名为monster_nmw,就注入此JavaBean
  @ConditionalOnMissingBean(name = "monster_nmw")
  public Dog dog01() {
    return new Dog("小白");
  }
}
```
### 3.5 @ImportResource
- 作用：
	- 原生配置文件引入, 也就是可以直接导入 Spring 传统的 `beans.xml`
	- 可以认为是 SpringBoot 对 Spring 容器文件的兼容.
	- 也就是说可以通过 SpringBoot 容器去获取

```java
@Configuration
// 导入 springType-bean.xml 就可以获取注入的Bean
// String[] locations() 单个可省略{},多个用逗号隔开{bean1.xml,bean2.xml}
@ImportResource(locations = "classpath:springType-bean.xml")
public class ImportResourceConfig {
}
```

```java
@SpringBootApplication
public class MainApp {
  public static void main(String[] args) {
    // 启动SpringBoot项目
    ConfigurableApplicationContext ioc = SpringApplication.run(MainApp.class, args);
    System.out.println(ioc.getBean("monster03"));
  }
}
```

### 3.6 配置绑定
- 一句话：
	- 使用 Java 读取到 SpringBoot 核心配置文件 `application.properties` 的内容，并且把它封装到 JavaBean 中
- 需求: 
	- 将 `application.properties` 指定的 k-v 和 JavaBean 绑定

`application.properties`
```properties
# 自定义配置属性
my.website=http://www.baidu.com

# Set Furn's properties k-v
# prefix used to differentiate
furn01.id=100
furn01.name=TV
furn01.price=1009.1
```

>[!note]- 第一种配置方式:
> `entity`
> ```java
> @Data
> @Component
> @ConfigurationProperties(prefix = "furn01")
> public class Furn {
>   private Integer id;
>   private String name;
>   private Double price;
> }
> ```

>[!note]- 第二种配置方式: 
> `entity`
> ```java
> @Data
> @ConfigurationProperties(prefix = "furn01")
> public class Furn {
>   private Integer id;
>   private String name;
>   private Double price;
> }
> ```
> `config`
> ```java
> @Import({Dog.class, Cat.class})
> @Configuration
> /*
> @EnableConfigurationProperties(Furn.class)解读: 
>  1、开启 Furn 配置绑定功能 
>  2、把 Furn 组件自动注册到容器中
> */
> @EnableConfigurationProperties({Furn.class})
> public class ImportConfig {
> }
> ```

测试: `controller`
```java
@Controller
public class HelloController {
  // 从application.properties配置文件中获取属性
  @Value("${my.website}")
  private String website;

  @Resource // 标识注入
  private Furn furn;

  @RequestMapping("/hello")
  @ResponseBody
  public Furn hello() {
    System.out.println("website: " + website);
    return furn;
  }
}
```
- 注意事项和细节
	1. 如果 `application.properties` 有中文, 需要转成 unicode 编码写入, 否则出现乱码
	2. 使用 `@ConfigurationProperties(prefix = "furn01")` 
		- 会提示如下信息, 但是不会影响使用
			[[Pasted image 20230419153653.png|使用 @ConfigurationProperties IDEA错误提示]]
		- 解决: 在 `pom.xml` 增加依赖
			```xml
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-configuration-processor</artifactId>
				<optional>true</optional>
			</dependency>
			```
## 四、分析 SpringBoot 底层机制
Tomcat 启动分析 + Spring 容器初始化 + Tomcat 如何关联 Spring 容器 
### 4.1 搭建 SpringBoot 底层机制开发环境
1. 创建 Maven 项目 hsp-springboot
2. 导入 SpringBoot 父工程和 web 场景启动器 
3. 编写 `Springboot` 启动类 `MainApp.java`
### 4.2 分析@Configuration 和 @Bean 机制
1. 编写实体层 `Dog`
2. 编写 `config` 配置并标识 `@Configuration` 
	- 在该类中编写返回 `Dog` 实体公开的方法并标识 `@Bean` 
3. 底层机制分析: 仍然 Spring 容器机制 (IO/文件扫描+注解+反射+集合+映射) 

### 思考: SpringBoot 是怎么启动 Tomcat，并可以支持访问@Controller
1. 创建 `controller` 层, 编写 `HiController.java`
2. 标识 `@RestController` (`@Controller` + `@ResponseBody`) 

>[!note]- DeBug: SpringBoot 是怎么启动 Tomcat
> - Step 1: `SpringApplication.java`
> 	![[Pasted image 20230420145712.png]]
> - Step 2: `SpringApplication.java` 创建和返回 ConfigurableApplicationContext
> 	![[Pasted image 20230420145930.png]]
> - Step 3: `SpringApplication.java` 
> 	![[Pasted image 20230420152446.png]]
> 	![[Pasted image 20230420150831.png]]
> - Step 4: `SpringApplication.java` 默认 SERVLET
> 	![[Pasted image 20230420151901.png]]
> - Step 5: `ApplicationContextFactory.java` 创建相应的对象
> 	![[Pasted image 20230420154034.png]]
> - Step 6: 步出至 `SpringApplication.java`,观察第二个关注点 `refreshContext(context);`
> 	- 此时评估 `context`,可知 SpringBoot 在此刻中注入了最基础的组件, 核心就在 `refreshContext`
> 		![[Pasted image 20230420154426.png|800]]
> 	- 步入 `refreshContext`
> 		![[Pasted image 20230420154549.png]]
> 	- 步入 `refresh(context);` -> `applicationContext.refresh();` 继续步入 `ServletWebServerApplicationContext.java`
> 		![[Pasted image 20230420154924.png]]
> 	- 步入 `super.refresh();` -> `AbstractApplicationContext.java` 关注 `refresh()` 里的 `onRefresh();`
> 		![[Pasted image 20230420155423.png]]
> 	- 步入后又回到 `ServletWebServerApplicationContext.java` 的 `onRefresh()`
> 		![[Pasted image 20230420155820.png]]
> 	- 步入
> 		![[Pasted image 20230420161526.png]]
> 		![[Pasted image 20230420163122.png]]
> 		![[Pasted image 20230420163456.png]]
> 		![[Pasted image 20230420163654.png]]
> 		![[Pasted image 20230420163752.png]]
> 		组件注入完成, Tomcat 容器启动成功
> 		![[Pasted image 20230420163923.png]]

## 五、Lombok
- Lombok 作用
	- 简化 JavaBean 开发, 可以使用 Lombok 的注解让代码更加简洁
- Lombok 常用注解

	|         注解          |    注解位置     |                                      作用                                       |
	|:---------------------:|:---------------:|:-------------------------------------------------------------------------------:|
	|        `@Data`        |   注解在类上    | 提供类所有属性的 Getter、Setter, 还有 equals、canEqual、hashCode、toString 方法 |
	|       `@Setter`       | 注解在属性/类上 |                            为属性/类提供 Setter 方法                            |
	|       `@Getter`       | 注解在属性/类上 |                            为属性/类提供 Getter 方法                            |
	|       `@Log4j`        |   注解在类上    |                  为类提供一个属性名为 log 的 log 4 j 日志对象                   |
	| `@NoArgsConstructor`  |   注解在类上    |                              为类提供一个无参构造                               |
	| `@AllArgsConstructor` |   注解在类上    |                             为类提供一个全参构造器                              |
	|      `@Cleanup`       |                 |                                   可以关闭流                                    |
	|      `@Builder`       |   注解在类上    |                              给该类加个构造者模式                               |
	|    `@Synchronized`    |                 |                                   加个同步锁                                    |
	|    `@SneakyThrows`    |                 |                           等同于 `try/catch` 捕获异常                           |
	|      `@NonNull`       |  注解在参数上   |                         当参数为 null 时抛出空指针异常                          |
	|       `@Value`        | 和 `@Data` 类似 |  但它会把所有的成员变量默认定义为 `private final` 修饰, 并不会生成 Setter 方法  |
- `SpringBoot` 使用 Lombok
	- Springboot 父工程指定了 Lombok 版本
	- 要使用需要在 `pom.xml` 显示声明
		```xml
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
		</dependency>
		```

![[Pasted image 20230420195348.png|650]]

演示使用 `@Slf4j`
```java
@Slf4j
@Controller
public class HelloController {
  // 从application.properties配置文件中获取属性
  @Value("${my.website}")
  private String website;

  @Resource // 标识注入
  private Furn furn;

  @RequestMapping("/hello")
  @ResponseBody
  public Furn hello() {
    // 使用slf4j日志输出:
    log.error("普通输出: website: " + website);
    log.info("占位符方式输出: furn={}", furn);
    return furn;
  }
}

```
## 六、yaml
- YAML 是"YAML Ain't a Markup Language" (YAML 不是一种标记语言) 的递归缩写
	- "Yet Another Markup Language" (仍是一种标记语言)
	- 是为了强调这种语言以数据做为中心，而不是以标记语言为重点，而用反向缩略语重命名
- [yaml for Java](https://www.cnblogs.com/strongmore/p/14219180.html)

### 6.1 yaml 基本语法
1. 形式为 `key: value; ` <font color="#ff0000">注意: 后面有空格</font>
2. 区分大小写
3. 使用缩进表示层级关系
4. 缩进不允许使用 tab，只允许空格 (有些地方也识别 tab ,<span style="background:#fff88f"> 推荐使用空格 </span>)
5. 缩进的空格数不重要，只要相同层级的元素左对齐即可
6. 字符串无需加引号
7. yaml 中, 注释使用 `#`

### 6.2 数据类型
#### 字面量
1. 单个的、不可再分的值. 如 date、boolean、string、number、null
2. 保存形式为 key: value
	![[Pasted image 20230420221313.png]]

#### 对象
- 键值对的集合, 比如 map、hash、set、object
- 语法: 
	```yml
	# 行内写法
	k: {k1:v1,k2:v2,k3:v3}
	
	# 或换行形式
	k:
	  k1: v1
	  k2: v2 
	  k3: v3
	monster:
	  id: 100
	  name: 牛魔王
	```
	
	![[Pasted image 20230420222138.png]]
#### 数组
- 一组按次序排列的值, 比如 array、list、queue
- 语法: 
```yaml
# 行内写法： 
k: [v1,v2,v3]
hobby: [打篮球, 打乒乓球, 踢足球]

# 或者换行格式
k:
  - v1
  - v2
  - v3 

hobby:
  - 打篮球
  - 打乒乓球
  - 踢足球
```
![[Pasted image 20230420223919.png]]

### 6.3 yaml 应用实例
需求: 使用 yaml 配置文件和 JavaBean 进行数据绑定, 体会 yaml 使用方式
1. 配置环境
	1. 添加父依赖
	2. 添加 web 场景启动器
	3. 添加 lombok 和 yaml 
	4. 添加 `spring-boot-configuration-processor`
2. 创建 `entity` 层添加 `Car`、`Monster` 两个对象
	1. Car 代码:
		```java
		@Data
		public class Car {
		  private String name;
		
		  private Double price;
		}
		```
	2. Monster 代码:
		```java
		@ConfigurationProperties(prefix = "monster")
		@Component
		@Data
		public class Monster {
		  private Integer id;
		  private String name;
		  private Integer age;
		  private Boolean isMarried;
		  private Date birth;
		  private Car car;
		  private String[] skill;
		  private List<String> hobby;
		  private Map<String, Object> wife;
		  private Set<Double> salaries;
		  private Map<String, List<Car>> cars;
		}
		```
3. 创建 `controller` 层添加 `HiController.java`
	1. 给类标识 `@RestController`
	2. 给成员变量 `monster` 标识 `@Resource`, 依赖注入 `Monster`
	3. 新增方法并返回 `Monster`, 给方法标识 `@RequestMapping("/monster")` 映射路径 
4. 新增 `SpringBoot` 启动入口
	1. 给类标识 `@SpringBootApplication`
	2. 增加 `mian()`,调用 `SpringApplication` 静态方法 `run()`
5. 在 `resources` 资源文件增加 `application.yml` 配置文件, 给 `Monster` 对象赋值
	```yml
	monster:
	  id: 100
	  name: 牛魔王
	  age: 500
	  isMarried: true
	  birth: 2000/01/02
	  # 对象形式(行内) car: {name: 宝马,price: 210000.43}
	  car:
	    name: 宝马
	    price: 210000.43
	  # 数组形式(行内) skill: [ 排山倒海,七十二变,救命毫毛 ]
	  skill:
	    - 排山倒海
	    - 七十二变
	    - 救命毫毛
	  hobby: [ 喝酒,吃桃 ]
	  # map形式类似于对象(行内)
	  # wife: { no1: 青面狐狸,no2: 铁扇公主 }
	  wife:
	    no1: 青面狐狸
	    no2: 铁扇公主
	  # set类似于数组 salaries: [ 10000,21000 ]
	  salaries:
	    - 10000
	    - 21652
	  cars: # Map<String,List<Car>>
	    grade01:
	      - { name: 宝马, price: 210000.43 }
	      - { name: 奔驰, price: 200000.31 }
	    grade02:  [ { name: 马自达, price: 19000.43 }]
	```
### 6.4 yaml 使用细节
1. 如果 `application.properties` 和 `application.yml` 有相同的前缀值绑定
	- `application.properties` 优先级高
	- 开发时，应当避免
2. 字符串无需加引号
3. 解决 yaml 配置文件，不提示字段信息问题
	- 在 `pom.xml` 加入 `spring-boot-configuration-processor` 依赖
	- 输入你在 Bean 配置的 prefix 名字就会提示.

## 七、WEB 开发-静态资源访问
1. 只要静态资源放在<font color="#ff0000">类路径</font>下, 就可以被直接访问
	- [[Pasted image 20230421084758.png|对应Java文件: WebProperties]]
2. 常见静态资源：JS、CSS 、图片、字体文件 (Fonts)等
3. 访问方式：
	- 默认: 项目根路径/ + 静态资源名
	- [[Pasted image 20230421085403.png|由WebMvcProperties管理静态资源根路径]]

![[Pasted image 20230421091413.png]] 

- `application.yml` 配置修改默认约定
	- 修改静态资源根路径
		- [[Pasted image 20230421093510.png|修改路径之后,访问静态资源]]
			```yml
			spring:
			  mvc:
			    static-path-pattern: /fishx/**
			```
	- 修改静态资源文件路径 (自定义静态资源目录)
		- [[Pasted image 20230421095448.png|自定义静态资源目录-文件列表]]
			```yml
			spring:
			  web:
			    resources:
			      static-locations:
			        - classpath:/META-INF/resources/
			        - classpath:/resources/
			        - classpath:/static/
			        - classpath:/public/
			        - classpath:/fishx/
			```

## 八、Rest 风格请求处理
- Rest 风格支持
	- 使用 HTTP 请求方式动词来表示对资源的操作
	- REST 风格 `put` 和 `patch` 的区别?
		- put 是整体更新, patch 是局部更新
   - REST 风格的 `put` 和 `post` 的区别
	   - put 是幂等的 (一个操作, 无论执行多少次, 结果都是一样的, 那么这个操作就是幂等的), post 不是幂等的
- 举例说明：`/monster`
	- GET-获取怪物
	- DELETE-删除怪物
	- PUT-修改怪物
	- POST-保存妖怪
- PUT、DELETE 等请求更加安全
	- PUT、DELETE 等请求不会被浏览器缓存
	- 但浏览器只支持 GET 和 POST 请求
		- 需要 `HiddenHttpMethodFilter` 将 POST 请求转换为 PUT、DELETE 等请求
		- `application.yml` 配置
			```yml
			spring:
			  webflux:
			    hiddenmethod:
				# 启用了 HiddenHttpMethodFilter
			      filter:
			        enabled: true # 开启页面表单的 Rest 功能
			```
	- axios 支持 Rest url 风格
		- 不需要再添加 `HiddenHttpMethodFilter`
			- axios 会自动添加请求头 X-HTTP-Method-Override
		- 浏览器表单提交需要添加 `HiddenHttpMethodFilter`

```html
<h1>测试 rest 风格的 url, 来完成请求.</h1>
<form action="/monster" method="post">
  u: <input type="text" name="name"><br/>
  <!--如果要测试 delete, put 就打开下面的注释-->
  <input type="hidden" name="_method" value="put">
  <input type="submit" value="点击提交"/>
</form>
```

```properties
spring.mvc.hiddenmethod.filter.enabled=true
```

```java
// @RequestMapping(value = "/monster",method = RequestMethod.PUT)
@PutMapping("/monster")
public String putMonster() {
	return "PUT-修改妖怪";
}

// @RequestMapping(value = "/monster",method = RequestMethod.DELETE)
@DeleteMapping("/monster")
public String removeMonster() {
	return "DELETE-删除妖怪";
}
```
## 九、接收参数相关注解
SpringBoot 接收客户端提交数据/参数会使用到相关注解

|      参数注解       |         作用          |
|:-------------------:|:---------------------:|
|   `@PathVariable`   |       路径变量        |
|  `@RequestHeader`   |    获取Http请求头     |
|   `@RequestParam`   |     获取请求参数      |
|   `@CookieValue`    |     获取cookie值      |
|   `@RequestBody`    | 获取 request 请求信息 |
| `@RequestAttribute` |  获取 request 域属性  |

>[!note]- `@PathVariable` 路径变量
> ![[Pasted image 20230421212331.png]]

>[!note]- `@RequestHeader` 获取 Http 请求头
> ![[Pasted image 20230421214026.png]]

>[!note]- `@RequestParam` 获取请求参数
> ![[Pasted image 20230422145549.png]]

>[!note]- `@CookieValue` 获取 cookie 值
> ![[Pasted image 20230422150949.png]]

>[!note]- `@RequestBody` 获取 request 请求对象
> ![[Pasted image 20230422212521.png]]

> [!note]- `@RequestAttribute` 获取 request 域属性
> ![[Pasted image 20230423083806.png]]
### 9.1 复杂参数
1. 在开发中，SpringBoot 在响应客户端请求时，也支持复杂参数
2. Map、Model 数据会被放在 request 域，底层 `request.setAttribute()`
	![[Pasted image 20230425185251.png]]
3. RedirectAttributes 重定向携带数据

### 9.2 自定义对象参数-自动封装
1. 在开发中，SpringBoot 在响应客户端请求时，也支持自定义对象参数
2. 完成自动类型转换与格式化
3. 支持级联封装

- 自定义对象参数-应用实例
	- 自定义对象参数使用, 完成自动封装、类型转换
	- 页面代码: 
		```html
		<h1>添加角色-坐骑[测试封装 POJO；]</h1>
		<form action="/monster/save" method="post">
		  编号： <input name="id" value="100"><br/>
		  姓名： <input name="name" value="牛魔王"/> <br/>
		  年龄： <input name="age" value="120"/> <br/>
		  婚否： <input name="isMarried" value="true"/> <br/>
		  生日： <input name="birth" value="2000/11/11"/> <br/>
		  坐骑：<input name="mount.name" value="法拉利"/><br/>
		  价格：<input name="mount.price" value="99999.9"/>
		  <input type="submit" value="保存">
		</form>
		```
	- 实体层 `entity` 两对象 `Role`、`Mount`
		```java
		@Data
		public class Mount {
		  private String name;
		  private Double price;
		}
		```
		
		```java
		@Data
		public class Role {
		  private Integer id;
		  private String name;
		  private Integer age;
		  private boolean isMarried;
		  private Date birthday;
		  private Mount mount;
		}
		```
	- 控制层 `controller`
		```java
		@PostMapping("/monster/save")
		@ResponseBody
		public String saveMonster(Role role) {
		  System.out.println("role: " + role);
		  return "success";
		}
		```

- 底层实现: 转化器

## 十、自定义转换器
SpringBoot 在响应客户端请求时，将提交的数据封装成对象时，使用了内置的转换器
![[Pasted image 20230426201704.png]]
>[!note]- 自定义转换器-应用实例
> ![[Pasted image 20230426202026.png]]
> 添加 `WebConfig` 配置类, 使用 `Lite模式(直接返回新实例对象)`
> ```java
> @Configuration(proxyBeanMethods = false)
> public class WebConfig {
>   @Bean
>   public WebMvcConfigurer webMvcConfigurer() {
>     return new WebMvcConfigurer() {
>       @Override
>       public void addFormatters(FormatterRegistry registry) {
>         // 自定义转化
>         Converter<String, Mount> converter = new Converter<String, Mount>() {
>           @Override
>           public Mount convert(String source) {
>             if (!ObjectUtils.isEmpty(source)) {
>               String[] split = source.split(",");
>               return new Mount(split[0], Double.parseDouble(split[1]));
>             }
>             return null;
>           }
>         };
>         // 可以添加多个转换器 converters, key 是以 (原类型->目标类型)
>         // 例如, 该转换器的 key:
>         // "java.lang.String -> com.fishx.springboot.entity.Mount"
>         registry.addConverter(converter);
>       }
>     };
>   }
> }
> ```
## 十一、内容协商
> [!example]- 处理 JSON: 返回 JSON 格式数据
> ```java
> @Controller
> public class ResponseController {
>   @GetMapping("/get/monster")
>   @ResponseBody
>   public Monster getMonster() {
>     Monster monster = new Monster();
>     monster.setId(1);
>     monster.setName("牛魔王");
>     monster.setSkill("芭蕉扇");
>     monster.setAge(500);
>     return monster;
>   }
> }
> ```
> 底层机制：返回值处理器的消息转换器进行处理
> ![[Pasted image 20230507112213.png]]

- 内容协商
	- 根据客户端接收能力不同，SpringBoot 返回不同媒体类型的数据

![[Pasted image 20230507113211.png]]

```xml
<!-- 引入支持返回 xml 数据格式 -->
<dependency> 
   <groupId>com.fasterxml.jackson.dataformat</groupId>
   <artifactId>jackson-dataformat-xml</artifactId> 
</dependency>
```

此时 Springboot 具备返回 xml 数据格式能力

![[Pasted image 20230507113606.png]]

- 注意事项和使用细节
	- Postman 可以通过修改 Accept 的值，来返回不同的数据格式
	- SpringBoot 开启支持基于请求参数的内容协商功能 
		- 修改 `application.yml`, 开启基于请求参数的内容协商功能
			```yaml
			spring:
			  mvc:
			    hiddenmethod:
			      filter:
			        enabled: true # 开启表单REST功能
			    contentnegotiation:
			      favor-parameter: true # 开启基于请求参数内容协商功能
			      parameter-name: fishx # 指定一个内容协商的参数名,默认‘format’
			  web:
			    resources:
			      static-locations: [ classpath:/static/,classpath:/public/ ]
			```
		- 请求 `url?format=数据格式`
			![[Pasted image 20230507115251.png]]
	- 参数 `format` 是规定好的 (<font color="#f79646">可以指定修改</font>)
		- 在开启请求参数的内容协商功能后
			- SpringBoot 底层 `ParameterContentNegotiationStrategy` 会通过 format 来接收参数
			- 然后返回对应的媒体类型/数据格式
			- 当然 format=xx 这个 xx 媒体类型/数据格式是 SpringBoot 可以处理的才行，不能乱写
		![[Pasted image 20230507115734.png]]

## 十二、Thymeleaf
- Thymeleaf 是什么?
	1. Thymeleaf 是一个跟 Velocity、FreeMarker 类似的模板引擎，可完全替代 JSP
	2. Thymeleaf 是一个 java 类库，他是一个 xml/xhtml/html5 的模板引擎
		- 可以作为 mvc 的 web 应用的 view 层
- Thymeleaf 的优点 
	1. 实现 JSTL、 OGNL 表达式效果，语法相似, java 程序员上手快
	2. Thymeleaf 模版页面无需服务器渲染，也可以被浏览器运行，页面简洁
	3. SpringBoot 支持 FreeMarker、Thymeleaf、veocity
- Thymeleaf 的缺点 
	- 并不是一个高性能的引擎，适用于单体应用
- 说明：如果要做一个高并发的应用, 选择前后端分离更好

### 12.1 Thymeleaf 机制说明
Thymeleaf 是服务器渲染技术, 页面数据是在服务端进行渲染的
因为使用了服务端渲染技术, 所以是前后端不分离

### 12.2 Thymeleaf 语法
#### 1. 表达式
| 表达式名字 |   语法   |                 用途                 |
|:----------:|:--------:|:------------------------------------:|
|  变量取值  | `${...}` |   获取请求域、session 域、对象等值   |
|  选择变量  | `*{...}` |           获取上下文对象值           |
|    消息    | `#{...}` |            获取国际化等值            |
|    链接    | `@{...}` |               生成链接               |
| 片段表达式 | `~{...}` | `jsp:include` 作用，引入公共页面片段 |

#### 2. 字面量
- 文本值: 'hsp edu' , 'hello' ,…
- 数字: 10 , 7 , 36.8 , …
- 布尔值: true , false 
- 空值: null 
- 变量： name，age，.... 变量不能有空格

#### 3. 文本操作
字符串拼接: + 

变量替换: |age= ${age}|

#### 4. 运算符
1. 数学运算运算符: + , - , * , / , %
2. 布尔运算运算符: and , or 一元运算: ! , not
3. 比较运算
	- 比较: > , < , >= , <= ( gt , lt , ge , le )
	- 等式: == , != ( eq , ne )
4. 条件运算
	- if-then: (if) ? (then) 
	- if-then-else: (if) ? (then) : (else)
	- Default: (value) ?: (defaultvalue)

#### 5. th 属性
![[Pasted image 20230507165849.png]]
![[Pasted image 20230507170553.png]]
### 12.3 使用 Thymeleaf 注意点
1. 使用 Thymeleaf 语法，首先要声明名称空间： `xmlns:th="http://www.thymeleaf.org"`
2. 使用场景: 
	1. 设置文本内容 `th:text`
	2. 设置 input 的值 `th:value`
	3. 循环输出 `th:each`
	4. 条件判断 `th:if`
	5. 插入代码块 `th:insert`
	6. 定义代码块 `th:fragment`
	7. 声明变量 `th:object`
3. 变量表达式中提供了很多的内置方法，该内置方法是用 `#开头` ，请不要与 `#{}` 消息表达式弄混

## 十三、拦截器
在 Spring Boot 项目中，`拦截器(HandlerInterceptor)` 是开发中常用手段，要来做登陆验证、性能检查、日志记录等 

> [!summary] 实现步骤:
> 1. 编写一个拦截器实现 HandlerInterceptor 接口 
> 2. 拦截器注册到配置类中 (实现 WebMvcConfigurer 的 addInterceptors) 
> 3. 指定拦截规则

编写拦截器
```java
@Slf4j
public class LoginInterceptor implements HandlerInterceptor {
  @Override
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    String uri = request.getRequestURI();
    log.info("preHandle拦截到的请求uri = {}", uri);
    // 进行登录校验
    HttpSession session = request.getSession();
    Object admin = session.getAttribute("admin");
    if (admin != null) return true;
    // 校验失败拦截,返回至登录页
    request.setAttribute("msg", "请先登录~");
    request.getRequestDispatcher("/").forward(request, response);
    return false;
  }

  @Override
  public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
    log.info("postHandle() ...");
  }

  @Override
  public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    log.info("afterCompletion() ...");
  }
}
```
> [!note]- 注册拦截器并指定规则
> 方案一:
> ```java
> @Configuration
> public class WebConfig implements WebMvcConfigurer {
>   @Override
>   public void addInterceptors(InterceptorRegistry registry) {
>     // 注册自定义拦截器,并指定拦截规则
>     registry.addInterceptor(new LoginInterceptor())
>         .addPathPatterns("/**") // 拦截所有路径
>         .excludePathPatterns("/","/login","/images/**"); // 指定放行路径
>   }
> }
> ```
> 方案二: 
> ```java
> @Configuration
> public class config {
>   @Bean
>   public WebMvcConfigurer webMvcConfigurer() {
>     return new WebMvcConfigurer() {
>       @Override
>       public void addInterceptors(InterceptorRegistry registry) {
>         // 注册拦截器
>         registry.addInterceptor(new LoginInterceptor())
>             .addPathPatterns("/**")
>             .excludePathPatterns("/", "/login", "/images/**");
>       }
>     };
>   }
> }
> ```

## 十四、文件上传
![[Pasted image 20230508133352.png]]

```java
@Slf4j
@Controller
public class UploadController {
  // 处理请求转发-文件上传页
  @GetMapping("/upload.html")
  public String uploadPage() {
    // 视图解析转发至 templates/upload.html
    return "upload";
  }

  // 处理用户注册请求(处理文件上传)
  @PostMapping("/upload")
  @ResponseBody
  public String upload(
      @RequestParam("name") String name,
      @RequestParam("email") String email,
      @RequestParam("age") Integer age,
      @RequestParam("job") String job,
      @RequestParam("header") MultipartFile header,
      @RequestParam("photos") MultipartFile[] photos
  ) throws IOException {
    // 动态创建目录
    String path = ResourceUtils.getURL("classpath:").getPath();
    File file = new File(path + "static/images/upload/");
    if (!file.exists()) {
      log.info("目录不存在,创建目录{}", file.mkdirs() ? "成功" : "失败");
    }
    // 文件上传
    if (!header.isEmpty()) {
      String originalFilename = header.getOriginalFilename();
      header.transferTo(new File(file.getAbsolutePath() + "/" + originalFilename));
    }
    for (MultipartFile photo : photos) {
      if (!photo.isEmpty()) {
        String originalFilename = photo.getOriginalFilename();
        photo.transferTo(new File(file.getAbsolutePath() + "/" + originalFilename));
      }
    }
    return "上传成功!";
  }
}
```

## 十五、异常处理
1. 默认情况下，Spring Boot 提供/error 处理所有错误的映射
2. 对于机器客户端，它将生成 JSON 响应，其中包含错误，HTTP 状态和异常消息的详细信息。
	- 对于浏览器客户端，响应一个"whitelabel" 错误视图，以 HTML 格式呈现相同的数据

> [!debug] 异常处理机制:
> 找到 `DefaultErrorViewResolver` 类的 `resolve()`,断点
> 1. 先根据错误状态码, 找具体 `错误状态码.html`
> 2. 没有, 客户端错误就找 `4xx.html`, 服务端错误就找 `5xx.html`
> 3. 没有就默认错误页返回

### 15.1 自定义异常
![[Pasted image 20230508144410.png]]
定义错误的`Controller`
![[Pasted image 20230508144313.png]]

### 15.2 全局异常
1. `@ControllerAdvice` `@ExceptionHandler` 处理全局异常
2. 底层是 `ExceptionHandlerExceptionResolver` 支持的

当发生 ArithmeticException... 异常时, 不使用默认异常机制匹配的 `xxx.html` 
而是显示全局异常机制指定的错误页面
```java
@Slf4j
@ControllerAdvice
public class GlobalException {
  // 处理异常类型
  @ExceptionHandler({ArithmeticException.class})
  public String handleArithmetic(Exception e, Model model) {
    log.info("异常信息{}", e.getMessage());
    // 将异常信息放入model对象,将在页面展示错误信息
    model.addAttribute("msg", e.getMessage());
    return "/error/global"; // 错误跳转至 templates/error/global.html
  }
}
```
![[Pasted image 20230514124232.png]]
### 15.3 自定义异常
如果 Spring Boot 提供的异常不能满足开发需求
程序员也可以自定义异常 `@ResponseStatus` + `自定义异常` 
底层是 `ResponseStatusExceptionResolver`,调用 `response.sendError(statusCode, resolvedReason)`  
```java
// 当发生AccessException异常,我们通过http协议返回的状态码
// 这个状态码和自定义异常的对应关系是由程序员来决定的
@ResponseStatus(HttpStatus.FORBIDDEN)
public class AccessException extends RuntimeException{
  public AccessException(String message) {
    super(message);
  }

  public AccessException() {
  }
}
```
`controller` 模拟 `AccessException` 发生
```java
@GetMapping("/err3")
public String err3(String name) {
  if (!"tom".equals(name)) {
    throw new AccessException();
  }
  return "redirect:/manage.html";
}
```
当抛出自定义异常后，仍然会根据状态码，去匹配使用 `x.html` 显示

## 十六、web 三大组件注入 
注入 Servlet、Filter、Listener
Spring-Boot 支持将 Servlet、Filter、Listener 注入 Spring 容器, 成为 Spring bean

### 16.1 `@WebServlet` 注入 Servlet
原生 `Servlet`
```java
/*
 注入的原生Servlet不会被SpringBoot拦截器拦截
 对于开发的原生servlet需要指定@ServletComponentScan
 需要扫描的原生Servlet所在包,才会注入到容器中
*/
@WebServlet(name = "MyServlet",value = {"/s01","/s02"})
public class MyServlet extends HttpServlet {
  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp)
   throws ServletException, IOException {
    resp.getWriter().write("Hello World!");
  }
}
```
指定`@ServletComponentScan`
```java
@ServletComponentScan(basePackages = "com.fishx")
@SpringBootApplication
public class Application {
  public static void main(String[] args) {
    ConfigurableApplicationContext ioc
        = SpringApplication.run(Application.class, args);
    System.out.println("启动成功...");
  }
}
```

### 16.2 `@WebFilter` 注入 Filter
```java
/*
  @WebFilter表示Filter是一个过滤器并注入容器
  当请求 /css/目录资源 或者 /images/ 目录下资源时,会经过该Filter
  放行后, 再经过拦截器,拦截器根据拦截规则处理
  大致流程: 浏览器 -> Tomcat -> Filter -> Interceptor -> 资源
*/
@Slf4j
@WebFilter(urlPatterns = {"/css/*,/images/*"})
public class MyFilter implements Filter {
  @Override
  public void init(FilterConfig filterConfig) throws ServletException {
    log.info("Filter init()...");
  }

  @Override
  public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
   throws IOException, ServletException {
    HttpServletRequest httpServletRequest = (HttpServletRequest) request;
    log.info("过滤器处理的uri={}", httpServletRequest.getRequestURI());
    log.info("Filter doFilter()...");
    chain.doFilter(request, response);
  }

  @Override
  public void destroy() {
    log.info("Filter destroy()...");
  }
}
```
在 servlet 匹配全部是 `/*`, 在 Spring-Boot 是 `/**` 
### 16.3 `@WebListener` 注入 Listener
```java
@Slf4j
@WebListener
public class MyListener implements ServletContextListener {
  @Override
  public void contextInitialized(ServletContextEvent sce) {
    // 可以在此加入项目初始化的相关业务代码
    log.info("Listener contextInitialized() 项目初始化...");
  }

  @Override
  public void contextDestroyed(ServletContextEvent sce) {
    // 项目销毁时的相关业务代码
    log.info("Listener contextDestroyed() 项目销毁...");
  }
}
```
模拟项目 over 
```java
@ServletComponentScan(basePackages = "com.fishx")
@SpringBootApplication
public class Application {
  public static void main(String[] args) {
    ConfigurableApplicationContext ioc
        = SpringApplication.run(Application.class, args);
    ioc.stop(); // 停止容器
    System.out.println("启动成功...");
  }
}
```

### 16.4 使用 RegistrationBean 方式注入
去掉 `@WebServlet`、`@WebFilter`、`@WebListener`
```java
// 标识为配置类,默认true为单列模式返回Bean
@Configuration
// 以使用RegistrationBean方式
public class RegistrationConfig {
  @Bean
  // 注入Servlet
  public ServletRegistrationBean<MyServlet> myServlet() {
    // 实例化原生Servlet对象
    MyServlet myServlet = new MyServlet();
    // 将 MyServlet 对象关联到 ServletRegistrationBean 对象
    return new ServletRegistrationBean<>(myServlet, "/s01", "/s02");
  }

  @Bean
  // 注入Filter
  public FilterRegistrationBean<MyFilter> myFilter() {
    MyFilter myFilter = new MyFilter();
    FilterRegistrationBean<MyFilter> filterRegistrationBean = new FilterRegistrationBean<>(myFilter);
    // 设置filter的url-parent
    filterRegistrationBean.setUrlPatterns(Arrays.asList("/css/*","/images/*"));
    return filterRegistrationBean;
  }

  @Bean
  // 注入Listener
  public ServletListenerRegistrationBean<MyListener> myListener() {
    MyListener myListener = new MyListener();
    return new ServletListenerRegistrationBean<>(myListener);
  }
}
```

### 请求 servlet 时, why 不会到达拦截器
请求 Servlet 时, 不会到达 DispatherServlet, 因此也不会达到拦截器
- 原因分析:
	- 注入的 Servlet 会存在于 Spring 容器
	- DispatherServlet 也存在 Spring 容器
	- 二者都是同级关系, Tomcat 对 Servlet url 匹配的原则: 
		- 多个 Servlet 都能处理同一层路径, 精准优先原则/最长前缀匹配原则

![[Pasted image 20230515085131.png]]

> [!note]- 源码分析:
> 1. DispatcherServletAutoConfiguration 完成对 DispatcherServlet 自动配置
> 	![[Pasted image 20230515092842.png]]
> 2. 使用 RegisterationBean 机制注入
> 	![[Pasted image 20230515092940.png]]
> 3. 对 `webMvcProperties.getServlet().getPath()` 求值
> 	![[Pasted image 20230515093131.png]]

## 十七、内置 Tomcat 配置和切换
1. SpringBoot 支持的 webServer: `Tomcat` `Jetty` `Undertow`
![[Pasted image 20230515093347.png]]
![[Pasted image 20230515093352.png]]
2. SpringBoot 应用启动是 Web 应用时。Web 场景包-导入 tomcat
3. 支持对 Tomcat (也可以是 Jetty 、Undertow)的配置和切换

### 17.1 内置 Tomcat 的配置
#### 通过 `application.yml` 完成配置
```yaml
server: # 端口
  port: 9999
  tomcat:
	threads:
	# 最大工作线程数
	max: 10 
	# 最小工作线程数
	min-spare: 5 
	# tomcat 启动的线程数达到最大时，接受排队的请求个数，默认值为 100
	accept-count: 200 
	# 最大连接数
	max-connections: 2000 
	# 建立连接超时时间,单位毫秒
	connection-timeout: 10000
	# 还有其它就不一一列举
```
![[Pasted image 20230515094644.png]]

#### 通过类来配置 Tomcat
<span style="background:#fff88f"><u>说明: 配置文件可配置的更全</u></span>
![[Pasted image 20230515095924.png]]

### 17.2 切换 webServer 
在 `pom.xml` 文件中, 先排除 `Tomcat` 导入, 再导入其他 webServer
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <!--排除tomcat starter-->
  <exclusions>
	<exclusion>
	  <groupId>org.springframework.boot</groupId>
	  <artifactId>spring-boot-starter-tomcat</artifactId>
	</exclusion>
  </exclusions>
</dependency>
<!--引入undertow-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-undertow</artifactId>
</dependency>
```
当排除 Tomcat 依赖时, `import javax.servlet` 及子包都会爆红, 导入其他 webServer 之后就不会
因为 `javax.servlet` 是一套标准规范, 并非 Tomcat 独有, 如此可以无损替换

## 十八、数据库操作
### 18.1 JDBC + HikariDataSource
HikariDataSource : 目前市面上非常优秀的数据源, 是 springboot2 默认数据源
1. 引入开发数据库相关依赖
	```xml
	<!--进行数据库开发,引入data-jdbc starter-->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jdbc</artifactId>
    </dependency>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.32</version>
    </dependency>
    <dependency>
      <groupId>org.hibernate.validator</groupId>
      <artifactId>hibernate-validator</artifactId>
      <version>6.1.0.Final</version>
    </dependency>
	
	<!--开发Springboot 测试类,需要引入 teststart-->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
    </dependency>
	```
2. yml 配置文件, 配置数据库连接相关信息 
	```yaml
	spring:
	  datasource: # 配置数据源
	    # 说明: 如果你没有指定 useSSL=true,项目启动会爆红
	    url: jdbc:mysql://localhost:3306/furn_ssm?useSSL=true&useUnicode=true&characterEncoding=UTF-8
	    username: root
	    password: admin
	    driver-class-name: com.mysql.cj.jdbc.Driver
	```

3. 创建实体类 `Frun` 
	```java
	@Data
	public class Furn {
	  private Integer id;
	
	  @NotEmpty(message = "请输入家居名")
	  private String name;
	
	  @NotEmpty(message = "请输入生产厂商")
	  private String maker;
	
	  @NotNull(message = "请输入数字")
	  @Range(min = 0, message = "价格不能小于零")
	  private BigDecimal price;
	
	  @NotNull(message = "请输入数字")
	  @Range(min = 0, message = "销量不能小于零")
	  private Integer sales;
	
	  @NotNull(message = "请输入数字")
	  @Range(min = 0, message = "库存不能小于零")
	  private Integer stock;
	
	  private String imgPath = "assets/images/product-image/1.jpg";
	}
	```
4. 测试类测试
	```java
	@SpringBootTest
	public class ApplicationTests {
	  @Resource
	  private JdbcTemplate jdbcTemplate;
	
	  @Test
	  public void contextLoads() {
	    BeanPropertyRowMapper<Furn> rowMapper
	        = new BeanPropertyRowMapper<>(Furn.class);
	    List<Furn> furnList = jdbcTemplate.query("SELECT * FROM `furn`", rowMapper);
	    furnList.forEach(System.out::println);
	    System.out.println(jdbcTemplate.getDataSource().getClass());
	  }
	}
	```

### 18.2 整合 Druid 到 Spring-Boot
1. HiKariCP: 目前市面上非常优秀的数据源, 是 springboot 2 默认数据源
2. Druid： 性能优秀，Druid 提供性能卓越的连接池功能外，还集成了 SQL 监控，黑名单拦截等功能，强大的监控特性，通过 Druid 提供的监控功能，可以清楚知道连接池和 SQL 的工作情况，所以根据项目需要，我们也要掌握 Druid 和 SpringBoot 整合

- 整合 Druid 到 Spring-Boot 方式
	- 自定义方式
	- 引入 starter 方式
#### Durid 基本使用
- 将 Spring-Boot 的数据源切换成 Druid
	```xml
	<!-- 引入 druid 依赖-->
	<dependency>
	  <groupId>com.alibaba</groupId>
	  <artifactId>druid</artifactId>
	  <version>1.1.17</version>
	</dependency>
	```
- 创建 Druid 数据源配置类
	```java
	@Configuration
	public class DruidDataSourceConfig {
	  // 配置读取yml配置文件信息
	  @ConfigurationProperties("spring.datasource")
	  // 注入DruidDataSource
	  @Bean
	  public DataSource dataSource() {
		return new DruidDataSource();
	  }
	}
	```

>[!question] 当我们注入自己的 `DataSource`, 为什么 `HikariDataSource` 会失效?
>- 默认的数据源配置, 查看 `DataSourceAutoConfiguration` 类
>	- `@ConditionalOnMissingBean({ DataSource.class, XADataSource.class })`
>	- 通过 `@ConditionalOnMissingBean({ DataSource.class })` 判断容器是否有 DataSource Bean
>		- 有就不注入默认的 DataSource, 也就是 `HikariDataSource`

#### Durid 监控功能-SQL 监控
`DruidDataSourceConfig` 
[增加 Druid 监控功能](https://github.com/alibaba/druid/wiki/配置_StatViewServlet配置)
![[Pasted image 20230518175358.png]]
[打开监控统计功能 (配置 StatFilter)](https://github.com/alibaba/druid/wiki/配置_StatFilter)
![[Pasted image 20230518175511.png]]
```java
@Configuration
public class DruidDataSourceConfig {
  // 配置读取yml配置文件信息
  @ConfigurationProperties("spring.datasource")
  // 注入DruidDataSource
  @Bean
  public DataSource dataSource() throws SQLException {
    DruidDataSource dataSource = new DruidDataSource();
    // 配置_StatFilter
    dataSource.setFilters("stat");
    return dataSource;
  }

  // 配置Druid监控页功能
  @Bean
  public ServletRegistrationBean<StatViewServlet> statViewServlet() {
    StatViewServlet statViewServlet = new StatViewServlet();
    ServletRegistrationBean<StatViewServlet> registrationBean =
        new ServletRegistrationBean<>(statViewServlet, "/druid/*");
    registrationBean.addInitParameter("loginUsername", "fishx");
    registrationBean.addInitParameter("loginPassword", "admin");
    return registrationBean;
  }
}
```
创建 `DruidSqlController.java`，模拟操作 DB 的请求
```java
@Controller
public class DruidSqlController {
  @Resource
  public JdbcTemplate jdbcTemplate;

  @ResponseBody
  @GetMapping("/list")
  public List<Furn> getList() {
    BeanPropertyRowMapper<Furn> rowMapper =
        new BeanPropertyRowMapper<>(Furn.class);
    return jdbcTemplate.query("SELECT * FROM furn", rowMapper);
  }
}
```

访问 `http://localhost:10000/druid/index.html`
![[Pasted image 20230518181837.png]]
[Druid连接池介绍-区间分布](https://github.com/alibaba/druid/wiki/Druid%E8%BF%9E%E6%8E%A5%E6%B1%A0%E4%BB%8B%E7%BB%8D#37-%E5%8C%BA%E9%97%B4%E5%88%86%E5%B8%83)

#### Durid 监控功能-Web 关联监控
[Web关联监控配置](https://github.com/alibaba/druid/wiki/配置_配置WebStatFilter)
```java
@Configuration
public class DruidDataSourceConfig {
  // 配置读取yml配置文件信息
  @ConfigurationProperties("spring.datasource")
  // 注入DruidDataSource
  @Bean
  public DataSource dataSource() throws SQLException {
    DruidDataSource dataSource = new DruidDataSource();
    dataSource.setFilters("stat");
    return dataSource;
  }

  // 配置Druid监控页功能
  @Bean
  public ServletRegistrationBean<StatViewServlet> statViewServlet() {
    StatViewServlet statViewServlet = new StatViewServlet();
    ServletRegistrationBean<StatViewServlet> registrationBean =
        new ServletRegistrationBean<>(statViewServlet, "/druid/*");
    registrationBean.addInitParameter("loginUsername", "fishx");
    registrationBean.addInitParameter("loginPassword", "admin");
    return registrationBean;
  }

  // 配置WebStatFilter,用于采集web-jdbc关联的监控数据
  @Bean
  public FilterRegistrationBean<Filter> webStatFilter() {
    // 创建 WebStatFilter
    WebStatFilter webStatFilter = new WebStatFilter();
    FilterRegistrationBean<Filter> filterRegistration
        = new FilterRegistrationBean<>(webStatFilter);
    // 默认对所以的url请求进行监听
    filterRegistration.setUrlPatterns(Arrays.asList("/*"));
    // 排除指定的url
    filterRegistration.addInitParameter("exclusions",
        "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*");
    return filterRegistration;
  }
}
```
#### Durid 监控功能-SQL 防火墙
[配置防御 SQL 注入攻击](https://github.com/alibaba/druid/wiki/配置-wallfilter)

```java
@Configuration
public class DruidDataSourceConfig {
  // 配置读取yml配置文件信息
  @ConfigurationProperties("spring.datasource")
  // 注入DruidDataSource
  @Bean
  public DataSource dataSource() throws SQLException {
    DruidDataSource dataSource = new DruidDataSource();
    // 加入Druid监控功能, 加入sql防火墙监控
    dataSource.setFilters("stat,wall");
    return dataSource;
  }

  // 配置Druid监控页功能
  @Bean
  public ServletRegistrationBean<StatViewServlet> statViewServlet() {
    StatViewServlet statViewServlet = new StatViewServlet();
    ServletRegistrationBean<StatViewServlet> registrationBean =
        new ServletRegistrationBean<>(statViewServlet, "/druid/*");
    registrationBean.addInitParameter("loginUsername", "fishx");
    registrationBean.addInitParameter("loginPassword", "admin");
    return registrationBean;
  }

  // 配置WebStatFilter,用于采集web-jdbc关联的监控数据
  @Bean
  public FilterRegistrationBean<Filter> webStatFilter() {
    // 创建 WebStatFilter
    WebStatFilter webStatFilter = new WebStatFilter();
    FilterRegistrationBean<Filter> filterRegistration
        = new FilterRegistrationBean<>(webStatFilter);
    // 默认对所以的url请求进行监听
    filterRegistration.setUrlPatterns(Arrays.asList("/*"));
    // 排除指定的url
    filterRegistration.addInitParameter("exclusions",
        "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*");
    return filterRegistration;
  }
}
```
#### Durid 监控功能-Session 监控
[使用Druid的内置监控页面](https://github.com/alibaba/druid/wiki/配置_StatViewServlet配置)
```java
@Configuration
public class DruidDataSourceConfig {
  // 配置Druid监控页功能
  @Bean
  public ServletRegistrationBean<StatViewServlet> statViewServlet() {
    // 创建StatViewServlet
    StatViewServlet statViewServlet = new StatViewServlet();
    // 注册StatViewServlet,并指定url
    ServletRegistrationBean<StatViewServlet> registrationBean =
        new ServletRegistrationBean<>(statViewServlet, "/druid/*");
    // 配置监控页面访问密码
    registrationBean.addInitParameter("loginUsername", "fishx");
    registrationBean.addInitParameter("loginPassword", "admin");
    return registrationBean;
  }
}
```
Session 监控无需额外配置
### 18.3 Druid Spring Boot Starter
`pom.xml` 引入 Druid 场景启动器
```xml
<!--引入 druid start-->
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid-spring-boot-starter</artifactId>
  <version>1.1.17</version>
</dependency>
```
`application.yaml` 配置信息
```yml
spring:
  datasource: # 配置数据源
    url: jdbc:mysql://localhost:3306/furn_ssm?useUnicode=true&characterEncoding=UTF-8
    username: root
    password: admin
    driver-class-name: com.mysql.cj.jdbc.Driver
    #配置druid和监控功能
    druid:
      stat-view-servlet:
        enabled: true
        login-username: fishx
        login-password: admin
        reset-enable: false
      web-stat-filter: #配置web监控
        enabled: true
        url-pattern: /*
        exclusions: '*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*'
      filter:
        stat: #sql监控
          slow-sql-millis: 1000
          log-slow-sql: true
          enabled: true
        wall: #配置sql防火墙
          enabled: true
          config:
            drop-table-allow: false #不允许删表操作
            select-all-column-allow: false #不允许SELECT*
```

## 十九、Spring Boot 整合 MyBatis
>[!note]- 创建并启动 maven 项目 `springboot-mybatis`
> `pom.xml` 导入依赖
> ```xml
> <!--先引入springboot父工程-->
> <parent>
> 	<artifactId>spring-boot-starter-parent</artifactId>
> 	<groupId>org.springframework.boot</groupId>
> 	<version>2.5.3</version>
> </parent>
> 
> <dependencies>
> 	<!--web场景启动器-->
> 	<dependency>
> 	  <groupId>org.springframework.boot</groupId>
> 	  <artifactId>spring-boot-starter-web</artifactId>
> 	</dependency>
> 	<!--引入mybatis-->
> 	<dependency>
> 	  <groupId>org.mybatis.spring.boot</groupId>
> 	  <artifactId>mybatis-spring-boot-starter</artifactId>
> 	  <version>2.2.2</version>
> 	</dependency>
> 	<!--引入mysql驱动-->
> 	<dependency>
> 	  <groupId>mysql</groupId>
> 	  <artifactId>mysql-connector-java</artifactId>
> 	</dependency>
> 	<!--引入配置处理器,不需要再额外下载插件!-->
> 	<dependency>
> 	  <groupId>org.springframework.boot</groupId>
> 	  <artifactId>spring-boot-configuration-processor</artifactId>
> 	  <optional>true</optional>
> 	</dependency>
> 	<!--引入lombok-->
> 	<dependency>
> 	  <groupId>org.projectlombok</groupId>
> 	  <artifactId>lombok</artifactId>
> 	</dependency>
> 	<!--Springboot 测试类,引入 test-start-->
> 	<dependency>
> 	  <groupId>org.springframework.boot</groupId>
> 	  <artifactId>spring-boot-starter-test</artifactId>
> 	</dependency>
> 	<!-- 引入 druid 依赖-->
>     <dependency>
>       <groupId>com.alibaba</groupId>
>       <artifactId>druid</artifactId>
>       <version>1.1.17</version>
>     </dependency>
> </dependencies>
> ```
> `application.yml` 配置端口、数据库连接字符
> ```yaml
> server:
>   port: 8008
> spring:
>   datasource:
>     driver-class-name: com.mysql.cj.jdbc.Driver
>     url: jdbc:mysql://localhost:3306/furn_ssm?useSSL=true&useUnicode=true&characterEncoding=UTF-8
>     username: root
>     password: admin
> ```
> `DruidDataSourceConfig.java` 配置数据源
> ```java
> @Configuration
> public class DruidDataSourceConfig {
>   @ConfigurationProperties("spring.datasource")
>   @Bean
>   public DataSource dataSource() throws SQLException {
>     DruidDataSource dataSource = new DruidDataSource();
>     // dataSource.setFilters("stat,wall");
>     return dataSource;
>   }
> }
> ```
> 项目启动
> ```java
> @SpringBootApplication
> public class Application {
>   public static void main(String[] args) {
>     SpringApplication.run(Application.class, args);
>   }
> }
> ```
> 测试类
> ```java
> @SpringBootTest
> public class ApplicationTest {
>   @Resource
>   private JdbcTemplate jdbcTemplate;
> 
>   @Test
>   public void t1() {
>     System.out.println(jdbcTemplate.getDataSource().getClass());
>   }
> }
> ```

> [!note]- 整合并使用 Mybatis
> 1. 创建 `MonsterMapper` 接口, 标识 `@Mapper` 注入 `Mapper` 接口 
> 	```java
> 	// 在Mapper接口使用@Mapper就会扫描,并将Mapper接口对象注入
> 	@Mapper
> 	public interface MonsterMapper {
> 	  // 根据id返回Monster对象
> 	  Monster getMonsterById(Integer id);
> 	}
> 	```
> 2. 创建 `mapper/MonsterMapper.xml`
> 	```xml
> 	<?xml version="1.0" encoding="UTF-8" ?>
> 	<!DOCTYPE mapper
> 	    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
> 	    "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
> 	<mapper namespace="com.fishx.springboot.mybatis.mapper.MonsterMapper">
> 	  <!--配置getMonsterById-->
> 	  <select id="getMonsterById" resultType="Monster">
> 	    SELECT * FROM `monster` WHERE id=#{id}
> 	  </select>
> 	</mapper>
> 	```
> 3. `appliaction.yaml` 添加配置 `mybatis` 信息 
> 	```yml
> 	mybatis:
> 	  mapper-locations: classpath:mapper/*.xml
> 	  # 1.可以通过 config-location 指定mybatis-config.xml,以传统的方式来配置mybatis
> 	  # 2.也可以直接在application.yaml里配置
> 	  # 配置别名
> 	  type-aliases-package: com.fishx.springboot.mybatis.entity
> 	  configuration:
> 	    # 配置Mybatis标准日志输出
> 	    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
> 	```
> 4. 使用
> 	```java
> 	@SpringBootTest
> 	public class ApplicationTest {
> 	  @Resource
> 	  private MonsterMapper monsterMapper;
> 	
> 	  @Test
> 	  public void getMonsterById() {
> 	    Monster monster = monsterMapper.getMonsterById(1);
> 	    System.out.println("monster--" + monster);
> 	  }
> 	}
> 	```

> [!note]- 三层架构使用 `mybatis` 
> - `dao/mapper`
> 	```java
> 	@Mapper
> 	public interface MonsterMapper {
> 	  // 根据id返回Monster对象
> 	  Monster getMonsterById(Integer id);
> 	}
> 	```
> - `service`
> 	```java
> 	public interface MonsterService {
> 	  // 根据id返回Monster对象
> 	  Monster getMonsterById(Integer id);
> 	}
> 	```
> 	
> 	```java
> 	@Service
> 	public class MonsterServiceImpl implements MonsterService {
> 	  @Resource
> 	  private MonsterMapper monsterMapper;
> 	
> 	  @Override
> 	  public Monster getMonsterById(Integer id) {
> 	    return monsterMapper.getMonsterById(id);
> 	  }
> 	}
> 	```
> - `controller`
> 	```java
> 	@RestController
> 	public class MonsterController {
> 	  @Resource
> 	  private MonsterService monsterService;
> 	
> 	  @GetMapping("/monster")
> 	  public Monster getMonsterById(@RequestParam("id") Integer id) {
> 	    return monsterService.getMonsterById(id);
> 	  }
> 	}
> 	```

## 二十、Spring Boot 整合 MyBatisPlus 
Spring Boot 整合 [MyBatis-Plus](https://www.baomidou.com)
1. MyBatis-Plus （简称 MP）是一个 MyBatis 的增强工具
	- 在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。
2. 强大的 CRUD 操作：
	- 内置通用 Mapper、通用 Service
	- <font color="#ff0000">通过少量配置即可实现单表大部分 CRUD 操作</font>，更有强大的条件构造器，满足各类使用需求

>[!note]- 创建并启动 maven 项目 `springboot-mybatis-plus`
> `pom.xml` 添加依赖
> ```xml
> <!--先引入springboot父工程-->
> <parent>
> 	<artifactId>spring-boot-starter-parent</artifactId>
> 	<groupId>org.springframework.boot</groupId>
> 	<version>2.5.3</version>
> </parent>
> <dependencies>
> 	<!--web场景启动器-->
> 	<dependency>
> 	  <groupId>org.springframework.boot</groupId>
> 	  <artifactId>spring-boot-starter-web</artifactId>
> 	</dependency>
> 	<!--引入mybatis-plus-starter-->
> 	<dependency>
> 	  <groupId>com.baomidou</groupId>
> 	  <artifactId>mybatis-plus-boot-starter</artifactId>
> 	  <version>3.4.3.4</version>
> 	</dependency>
> 	<!--引入mysql驱动-->
> 	<dependency>
> 	  <groupId>mysql</groupId>
> 	  <artifactId>mysql-connector-java</artifactId>
> 	</dependency>
> 	<!--引入配置处理器-->
> 	<dependency>
> 	  <groupId>org.springframework.boot</groupId>
> 	  <artifactId>spring-boot-configuration-processor</artifactId>
> 	  <optional>true</optional>
> 	</dependency>
> 	<!--引入lombok-->
> 	<dependency>
> 	  <groupId>org.projectlombok</groupId>
> 	  <artifactId>lombok</artifactId>
> 	</dependency>
> 	<!--Springboot 测试类,引入 test-start-->
> 	<dependency>
> 	  <groupId>org.springframework.boot</groupId>
> 	  <artifactId>spring-boot-starter-test</artifactId>
> 	</dependency>
> 	<!-- 引入 druid 依赖-->
> 	<dependency>
> 	  <groupId>com.alibaba</groupId>
> 	  <artifactId>druid</artifactId>
> 	  <version>1.1.17</version>
> 	</dependency>
> </dependencies>
> ```
> `application.yml` 修改端口、数据源连接配置信息、mybatis-plus 日志输出
> ```yml
> server:
>   port: 8008
> spring:
>   datasource:
>     username: root
>     password: admin
>     driver-class-name: com.mysql.cj.jdbc.Driver
>     url: jdbc:mysql://localhost:3306/spring?useSSL=true&useUnicode=true&characterEncoding=UTF-8
> mybatis-plus:
>   configuration:
>     log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
> ```
> 添加配置类, 创建并注入 Druid 数据源
> ```java
> @Configuration
> public class DruidDataSourceConfig {
>   @ConfigurationProperties("spring.datasource")
>   @Bean
>   public DataSource dataSource() {
>     return new DruidDataSource();
>   }
> }
> ```
> SpringBoot 启动类
> ```java
> @SpringBootApplication
> public class Application {
>   public static void main(String[] args) {
>     SpringApplication.run(Application.class, args);
>   }
> }
> ```

> [!note]- 三层架构-使用 `mybatis-plus`
> - `entity层`
> 	```java
> 	@Data
> 	@NoArgsConstructor
> 	@AllArgsConstructor
> 	public class Monster {
> 	  private Integer id;
> 	  private Integer age;
> 	  @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
> 	  private Date birthday;
> 	  private String email;
> 	  private String gender;
> 	  private String name;
> 	  private Double salary;
> 	}
> 	```
> - `dao层(mapper)`
> 	```java
> 	// BaseMapper已经默认提供了很多crud方法
> 	// 如果BaseMapper提供的方法不满足业务需求
> 	//    可以再开发新的方法,并在MonsterMapper.xml进行配置
> 	@Mapper
> 	public interface MonsterMapper extends BaseMapper`<Monster>` {
> 	  // 自定义方法
> 	}
> 	```
> - `service层`
> 	- `service 接口`
> 		```java
> 		// 1.传统方式 在接口中定义/声明方法,然后在实现类中进行实现
> 		// 2. 在mybatis-plus中,我们可以继承父接口 IService
> 		// 3. 这个IService接口声明很多方法
> 		public interface MonsterService extends IService`<Monster>` {
> 		  // 默认方法不能满足需求 ->  自定义方法
> 		}
> 		```
> 	- `serviceImpl 实现类`
> 		```java
> 		/*
> 		 (1). 传统方式: 在实现类中直接进行 implements MonsterService
> 		 (2). mybatis-plus中,需要继承 ServiceImpl,并实现相应的接口
> 		
> 		 ## 目标: MonsterServiceImpl 需要实现 MonsterService接口
> 		 ∵ MonsterService接口 继承 IService接口
> 		 ∴ 它的实现类 MonsterServiceImpl 就需要实现 IService接口声明的方法
> 		 ∵ ServiceImpl 实现了 IService接口,实现了 IService接口声明的方法
> 		 ∴ MonsterServiceImpl 继承了 ServiceImpl类,就拥有该类的所有方法
> 		 => MonsterServiceImpl 实现了 MonsterService接口抽象声明的方法
> 		
> 		 如果 IService接口中方法不满足需求, MonsterService接口自定义了方法
> 		 那么 MonsterServiceImpl 需要实现此方法
> 		*/
> 		@Service
> 		public class MonsterServiceImpl
> 		    extends ServiceImpl<MonsterMapper, Monster>
> 		    implements MonsterService {
> 		}
> 		```
> - `servlet层 (controller)`
> 	```java
> 	@RestController
> 	public class MonsterController {
> 	  @Resource
> 	  private MonsterService monsterService;
> 	
> 	  @GetMapping("/monster") // @RequestParam Integer id
> 	  public Monster getMonster(@RequestParam("id") Integer id) {
> 	    return monsterService.getById(id);
> 	  }
> 	}
> 	```

指定扫描所有 `Mapper` 接口
```java
@MapperScan(basePackages = {"com.fishx.mybatisplus.mapper"})
@SpringBootApplication
public class Application {
  public static void main(String[] args) {
    SpringApplication.run(Application.class, args);
  }
}
```

>[!help]- 整合 MyBatis-Plus 注意事项和细节
> 1. `@TableName` 作用
> ![[Pasted image 20230520113315.png]]
> 如果这个类名 `Monster` 和表名不一致，不能映射上，则可以通过 `@TableName` 指定
> 2. MyBatis-Plus starter 到底引入了哪些依赖?
> 	![[Pasted image 20230520113414.png]]
> 3. 快速开发推荐安装 `MyBatisX` 插件


## 二十一、SpringBoot 其他细节
### 21.1 SpringBoot 配置文件明文加密
1. 导入 jar 包
	```xml
	<!--配置文件加密-->
	<dependency>
	  <groupId>com.github.ulisesbocchio</groupId>
	  <artifactId>jasypt-spring-boot-starter</artifactId>
	  <version>2.1.0</version>
	</dependency>
	<dependency>
	  <groupId>com.github.ulisesbocchio</groupId>
	  <artifactId>jasypt-spring-boot</artifactId>
	  <version>2.1.0</version>
	</dependency>
	<dependency>
	  <groupId>org.jasypt</groupId>
	  <artifactId>jasypt</artifactId>
	  <version>1.9.3</version>
	</dependency>
	```
2. 增加解密密钥
	- `application.yml` 配置文件中添加
		```yml
		jasypt:
		  encryptor:
		    password: EbfYkitulv73I2
		```
	- 在 JVM 启动参数中设置 (建议)
		```shell
		-Djasypt.encryptor.password=EbfYkitulv73I2
		```
		[[Pasted image 20230713091233.png]]
		[[Pasted image 20230713091438.png]]
3. 编写测试用例, 获得加密码
	```java
	@SpringBootTest
	public class JTest {
	  @Resource
	  StringEncryptor encryptor;
	
	  @Test
	  public void getPass() {
	    String url = encryptor.encrypt("jdbc:mysql://localhost:3306/furn_ssm?useSSL=true&useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8");
	    String name = encryptor.encrypt("root");
	    String password = encryptor.encrypt("admin");
	    System.out.println(url);
	    System.out.println(name);
	    System.out.println(password);
	  }
	}
	```
4. 加密码替换明文内容

### 21.2 SpringBoot 跨域
```java
@Configuration
public class webConfig {
  @Bean
  public WebMvcConfigurer webMvcConfigurer() {
    return new WebMvcConfigurer() {
      @Override
      public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginInterceptor())
            .addPathPatterns("/**")
            .excludePathPatterns("/", "/login", "/images/**");
      }

      @Override
      public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
            .allowedOriginPatterns("*")
            //允许方法
            .allowedMethods("GET", "POST", "DELETE", "PUT", "OPTIONS")
            // 允许发送Cookie
            .allowCredentials(true).maxAge(3600);
      }
    };
  }
}
```
### 21.3 SpringBoot 异常处理
```java
@Slf4j
@RestControllerAdvice(basePackages = "com.fishx.www.controller")
public class GlobalException {
  // 处理异常类型
  @ExceptionHandler(value = MethodArgumentNotValidException.class)
  public Result<?> handleValidException(MethodArgumentNotValidException e) {
    log.error("数据校验出现问题：{},异常类型：{}", e.getMessage(), e.getClass());
    BindingResult bindingResult = e.getBindingResult();
    Map<String, String> map = new HashMap<>();
    bindingResult.getFieldErrors().forEach((item) -> map.put(item.getDefaultMessage(), item.getField()));
    return Result.fail("-1000", "参数校验失败", map);
  }

  // 其余异常
  @ExceptionHandler(value = Throwable.class)
  public Result<?> handleValidException(Exception e) {
    log.error("系统未知异常：{},异常类型：{}", e.getMessage(), e.getClass());
    return Result.fail("-1001", "系统未知异常");
  }
}
```