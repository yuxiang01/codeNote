[[Spring/Spring| spring 学习初体验]]

## 1. 创建JavaBean(entity)对象
```java
public class Monster {  
  private Integer monsterId;  
  private String name;  
  private String skill;  
  
  public Monster(Integer monsterId, String name, String skill) {  
    this.monsterId = monsterId;  
    this.name = name;  
    this.skill = skill;  
  }  
  
  public Monster() {  
  }  
  public Integer getMonsterId() {  
    return monsterId;  
  }  
  
  public void setMonsterId(Integer monsterId) {  
    this.monsterId = monsterId;  
  }  
  
  public String getName() {  
    return name;  
  }  
  
  public void setName(String name) {  
    this.name = name;  
  }  
  
  public String getSkill() {  
    return skill;  
  }  
  
  public void setSkill(String skill) {  
    this.skill = skill;  
  }  
  
  @Override  
  public String toString() {  
    return "Monster{" +  
        "monsterId=" + monsterId +  
        ", name='" + name + '\'' +  
        ", skill='" + skill + '\'' +  
        '}';  
  }  
}
```

## 2. 以xml方式配置JavaBean对象
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
  <!--  
  1.配置monster对象/javabean  
  2.在beans中可以配置多个bean  
  3.bean表示就是一个Java对象  
  4.class属性是用于指定类的全路径 -> Spring底层使用反射创建  
  5.id属性表示该Java对象在Spring容器中的id，通过id可以获取到对象  
  -->  
  <bean class="spring.bean.Monster" id="monster1">  
    <property name="monsterId" value="100"/>  
    <property name="name" value="牛魔王"/>  
    <property name="skill" value="芭蕉扇"/>  
  </bean>  
</beans>
```

## 3. 编写测试类,容器中读取对象
```java
public class SpringBeanTest {  
  
  @Test  
  public void getMonster() {  
    // 1. 创建容器 ApplicationContext    
    // 2. 该容器和配置文件关联  
    // 为什么能读取到xml配置文件？=》 类加载路径  
    ApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");  
    // 3. 通过getBean获取对应的对象,默认返回的是Object,但是运行类型是Monster  
    Object monster1 = ioc.getBean("monster1");  
    // 4. 还可以在getBean获取时,直接指定Class类型  
    Monster monster2 = ioc.getBean("monster1", Monster.class);  
    // 5. 输出  
    System.out.println("monster1 = " + monster1 + ", 运行类型 = " + monster1.getClass());  
    System.out.println("monster2.Name = " + monster2.getName() + ", 运行类型 = " + monster2.getClass());  
  }  
  
  @Test  
  public void getClassPath() {  
    File file = new File(this.getClass().getResource("/").getPath());  
    System.out.println("file = " + file);  
  }  
}
```

## 4. 手动实现 Spring-IOC 基于 XML 配置
[[Spring/Spring#1. 简单 Spring 基于 XML 配置程序| 简单Spring基于XML配置程序]]

```java
// 用于实现Spring的简单容器机制  
public class fishxApplicationContext {  
  private ConcurrentHashMap<String, Object> singletonObjects = new ConcurrentHashMap<>();  
  
  // 构造器，接收一个容器的配置文件【比如beans.xml】  
  public fishxApplicationContext(String iocBeanXmlFile) throws Exception {  
    // 1. 得到类加载路径  
    String path = this.getClass().getResource("/").getPath();  
    // 2. 获取解析器  
    SAXReader saxReader = new SAXReader();  
    // 3. 指定具体解析的XML  
    Document document = saxReader.read(new File(path + iocBeanXmlFile));  
    // 4. 获取根节点  
    Element rootElement = document.getRootElement();  
    // 5. 通过根元素获取第一个子元素  
    Element bean = (Element) rootElement.elements().get(0);  
    // 6. 获取第一个子元素的相关属性  
    String id = bean.attributeValue("id");  
    String classFullPath = bean.attributeValue("class");  
    // 7. 通过第一个子元素获取它的相关信息 => 保存至 beanDefinitionMap 中  
    List<Element> properties = bean.elements("property");  
    int monsterId = Integer.parseInt(properties.get(0).attributeValue("value"));  
    String name = properties.get(1).attributeValue("value");  
    String skill = properties.get(2).attributeValue("value");  
    // 8. 使用反射创建对象  
    Class<?> aClass = Class.forName(classFullPath);  
    Monster monster = (Monster) aClass.newInstance();  
    monster.setMonsterId(monsterId);  
    monster.setName(name);  
    monster.setSkill(skill);  
    singletonObjects.put(id, monster);  
  }  
  
  public Object getBean(String id) {  
    return singletonObjects.get(id);  
  }  
}
```

## 5. 手动实现 Spring-IOC 基于注解配置 
[[Spring/Spring#2. 简单的 Spring 基于注解配置的程序| 简单Spring基于注解配置程序]]

### 第一阶段: 搭建基本结构并获取扫描包
#### 1. 自定义注解
```java
/**  
 * 1.@Target(ElementType.TYPE)指定该类注解可以修饰Type程序元素  
 * 2.@Retention(RetentionPolicy.RUNTIME) 指定该类注解作用范围  
 * 3.String value() default "";表示可以接收value  
 */
@Target(ElementType.TYPE)  
@Retention(RetentionPolicy.RUNTIME)  
public @interface ComponentScan {  
  String value() default "";  
}
```

#### 2. 自定义配置类 (类似于 xml 配置文件, 指明扫描包)
```java
@ComponentScan(value = "spring.component")  
public class Fishx_SpringConfig { }
```

#### 3. 通过传入的 class 获取注解里的 value 去获取扫描包
```java
// 此类的作用类似Spring-IOC容器  
public class Fishx_ApplicationContext {  
  private final ConcurrentHashMap<String, Object> singletonObjects = new ConcurrentHashMap<>();  
  private Class configClass;  
  
  public Fishx_ApplicationContext(Class configClass) {  
    this.configClass = configClass;  
    // 拿到class之后通过反射获取注解 => 获取到将要扫描的包  
    ComponentScan componentScan = (ComponentScan) this.configClass.getDeclaredAnnotation(ComponentScan.class);  
    String path = componentScan.value();  
    System.out.println("获取到将要扫描的包 = " + path);  
  }  
}
```


### 第二阶段: 获取扫描包下所有的 `.class` 文件
```java
// 此类的作用类似Spring-IOC容器  
public class Fishx_ApplicationContext {  
  private final ConcurrentHashMap<String, Object> singletonObjects = new ConcurrentHashMap<>();  
  private Class configClass;  
  
  public Fishx_ApplicationContext(Class configClass) {  
    this.configClass = configClass;  
    // 拿到class之后通过反射获取注解 => 获取到将要扫描的包  
    ComponentScan componentScan = (ComponentScan) this.configClass.getDeclaredAnnotation(ComponentScan.class);  
    String path = componentScan.value();  
    System.out.println("获取到将要扫描的包 = " + path);  
  
    // 通过(类的加载器、IO、注解解析)得到包下的加载资源(.class)  
    path = path.replace(".", "/");  
    // 1. 获取类的加载器  
    ClassLoader loader = Fishx_ApplicationContext.class.getClassLoader();  
    URL resource = loader.getResource(path);  
    System.out.println("resource = " + resource);  
    // 2. 通过类加载器和IO获取.class文件  
    File file = new File(resource.getFile());  
    if (file.isDirectory()){  
      File[] files = file.listFiles();  
      for (File f : files) {  
        System.out.println("- - - - - - - - - - - - -");  
        System.out.println(f.getAbsolutePath());  
      }  
    }  
  }
}
```
### 第三阶段: 获取全类名、反射实例对象、放入容器
```java
 public Fishx_ApplicationContext(Class configClass) {  
    this.configClass = configClass;  
    // 拿到class之后通过反射获取注解 => 获取到将要扫描的包  
    ComponentScan componentScan = (ComponentScan) this.configClass.getDeclaredAnnotation(ComponentScan.class);  
    String path = componentScan.value();  
    System.out.println("获取到将要扫描的包 = " + path);  
  
    // 通过(类的加载器、IO、注解解析)得到包下的加载资源(.class)  
    // 1. 获取类的加载器  
    ClassLoader loader = Fishx_ApplicationContext.class.getClassLoader();  
    URL resource = loader.getResource(path.replace(".", "/"));  
    System.out.println("resource = " + resource);  
    // 2. 通过类加载器和IO获取.class文件  
    File file = new File(resource.getFile());  
    if (file.isDirectory()) {  
      File[] files = file.listFiles();  
      for (File f : files) {  
        String absolutePath = f.getAbsolutePath();  
        System.out.println(absolutePath);  
        // 只处理.class文件  
        if (absolutePath.endsWith(".class")) {  
          // 1.获取到类名  
          String className = absolutePath.substring(absolutePath.lastIndexOf("/") + 1,  
              absolutePath.lastIndexOf(".class"));  
          System.out.println("获取到类名 = " + className);  
          // 2. 获取类的完整路径(全类名) spring.component.UserService  
          String classFullPath = path + "." + className;  
          System.out.println("classFullPath = " + classFullPath);  
          // 3. 判断该类需要不要注入容器（通过查询是否有@注解）  
          try {  
            // 此时就得到了该类的Class对象，再通过反射判断有无@注解  
            // Class.forName() 与 loader.loadClass() 都能得到Class对象  
            // 主要区别在与Class.forName() 会调用该类的静态方法  
            Class<?> clazz = loader.loadClass(classFullPath);  
            if (clazz.isAnnotationPresent(Component.class) ||  
                clazz.isAnnotationPresent(Controller.class) ||  
                clazz.isAnnotationPresent(Service.class) ||  
                clazz.isAnnotationPresent(Repository.class)) {  
              if (clazz.isAnnotationPresent(Controller.class)){  
                Controller controller = clazz.getAnnotation(Controller.class);  
                String id = controller.value();  
                if (!"".equals(id)) {  
                  className = id;  
                }  
              }              
              // 此时说明要反射生成对象注入容器中,将作为key的首字母转小写  
              Object newInstance = Class.forName(classFullPath).newInstance();  
              singletonObjects.put(StringUtils.uncapitalize(className), newInstance);  
            }  
          } catch (ClassNotFoundException | InstantiationException | IllegalAccessException e) {  
            e.printStackTrace();  
          }  
        }
      }    
    }  
  }  
  public Object getBean(String key) {  
    return singletonObjects.get(key);  
  }  
}
```

## 6. 基于动态代理的方式 (内置 aop 实现)

```java
public interface SmartAnimal {  
  float getSum(float i, float j);  
  
  float getSub(float i, float j);  
}
```

```java
public class SmartDog implements SmartAnimal {  
  @Override  
  public float getSum(float i, float j) {  
    float result = i + j;  
    System.out.println("方法内部打印：result=" + result);  
    return result;  
  }  
  
  @Override  
  public float getSub(float i, float j) {  
    float result = i - j;  
    System.out.println("方法内部打印：result=" + result);  
    return result;  
  }  
}
```

```java
public class MyProxyProvider {  
  private SmartAnimal target_obj;  
  
  public MyProxyProvider(SmartAnimal target_obj) {  
    this.target_obj = target_obj;  
  }  
  
  public SmartAnimal getProxy() {  
    // 1. 获取类加载器  
    ClassLoader classLoader = target_obj.getClass().getClassLoader();  
    // 2. 获得接口信息  
    Class<?>[] interfaces = target_obj.getClass().getInterfaces();  
    // 3. 创建调用处理器  
    InvocationHandler invocationHandler = new InvocationHandler() {  
      @Override  
      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  
        Object result = null;  
        String name = method.getName();  
        try {  
          // 从AOP角度来看,就是一个横切面关注点: 前置通知  
          System.out.println("前置通知: 日志--方法名--" + name + "方法开始--参数：" + Arrays.asList(args));  
          result = method.invoke(target_obj, args);  
          // 横切面关注点: 后置通知  
          System.out.println("后置通知: 日志--方法名--" + name + " 方法结束--结果：result=" + result);  
        } catch (Exception e) {  
          // 横切面关注点: 异常通知  
          System.out.println("异常通知: 日志--方法名--" + name + "--异常类型--" + e.getClass().getName());  
        } finally {  
          // 横切面关注点: 最终通知  
          System.out.println("最终通知: 日志--方法名：" + name + "--方法最终结束");  
        }  
        return result;  
      }  
    };  
    return (SmartAnimal) Proxy.newProxyInstance(classLoader, interfaces, invocationHandler);  
  }  
}
```

```java
SmartDog smartDog = new SmartDog();  
SmartAnimal proxy = new MyProxyProvider(smartDog).getProxy();  
proxy.getSub(10.78f, 89.7f);
proxy.getSum(10.78f, 89.7f);
```

## 7. 基于动态代理的方式 (Spring 框架)

接口、实体类都与上一个一致, MyProxyProvider 由 SmartAnimalAspect 接管, 然后使用 xml 配置 AOP

```java
@Aspect // 表示是一个切面类「底层切面编程支撑」(动态代理+反射+动态绑定)  
@Component // 会注入到容器中  
public class SmartAnimalAspect {  
  
  /**  
   * You want to use the f1() as a pre notification   
   * 1. @Before 表示前置通知: 在目标对象执行方法之前调用  
   * 2. value 表示切入哪个对象哪个类的哪个方法「形式: 访问修饰符 返回类型 全类名.方法名(形参列表)」  
   * 3. 在底层执行时,会给该切入方法传入joinPoint对象,然后可以通过joinPoint获取一些信息  
   * 4. 切面编程 => joinPoint => 切面方法  
   *  
   * @param joinPoint 连接点对象  
   */  
  @Before(value = "execution(public float spring.AOP.aspectj.SmartDog.getSum(float, float))")  
  public static void showBeginLog(JoinPoint joinPoint) {  
    // 通过连接点对象JoinPoint 获取方法签名  
    Signature signature = joinPoint.getSignature();  
    System.out.println("前置通知: 日志--方法名--" + signature.getName()  
        + "方法开始--参数：" + Arrays.asList(joinPoint.getArgs()));  
  }  
  
  @AfterReturning(value = "execution(public float spring.AOP.aspectj.SmartDog.getSum(float,float))")  
  public void showSuccessEndLog(JoinPoint joinPoint) {  
    Signature signature = joinPoint.getSignature();  
    System.out.println("返回通知: 日志--方法名--" + signature.getName()  
        + " 方法结束--结果：result=" + joinPoint);  
  }  
  
  @AfterThrowing(value = "execution(public float spring.AOP.aspectj.SmartDog.getSum(float,float))")  
  public void showExceptionLog(JoinPoint joinPoint) {  
    System.out.println("异常通知: 日志--方法名--" + joinPoint.getSignature().getName()  
        + "--异常类型--e.getClass().getName()");  
  }  
  
  @After(value = "execution(public float spring.AOP.aspectj.SmartDog.getSum(float,float))")  
  public void showFinallyEndLog(JoinPoint joinPoint) {  
    System.out.println("最终通知: 日志--方法名：" + joinPoint.getSignature().getName()  
        + " --方法最终结束");  
  }  
}
```

```xml
<context:component-scan base-package="spring.AOP.aspectj"/>  
<!--开启基于注解的Aop功能-->  
<aop:aspectj-autoproxy/>
```

## 8.手动实现 Spring 底层机制
【初始化 IOC 容器+依赖注入+BeanPostProcessor 机制+AOP】
### Spring 整体架构分析
![[Pasted image 20230112140103.png]]
![[Spring 整体架构分析.excalidraw|1000]]

### 第一阶段: 编写自己 Spring 容器，实现扫描包，得到 bean 的 class 对象 
- 搭建整体框架
	- Annotation
		- 自定义三个注解 `Component`、`ComponentScan`、`Scope`
	- Component
		- 两个有注解 `MonsterDao`、`MonsterService`、一个无注解 `Car` 测试
	- IOC
		- `ApplicationContext` 模拟 IOC 容器
		- `BeanDefinition` 用来封装记录 Bean 信息
		- `SpringConfig` Spring 配置类似与 XML 配置扫描包信息

![[Pasted image 20230112153736.png]]

```java
public class ApplicationContext {  
  private Class configClass;  
  // 定义属性BeanDefinitionMap => 存放BeanDefinition对象  
  private ConcurrentHashMap<String, BeanDefinition> beanDefinitionMap = new ConcurrentHashMap<>();  
  // 定义属性SingletonObjects单例池 => 存放单例对象  
  private ConcurrentHashMap<String, Object> singletonObjects = new ConcurrentHashMap<>();  
  
  public ApplicationContext(Class configClass) {  
    this.configClass = configClass;  
    ComponentScan componentScan = (ComponentScan) this.configClass.getDeclaredAnnotation(ComponentScan.class);  
    // 获取注解里的path  
    String path = componentScan.value();  
    // 通过,类加载器 => 资源路径 => 读取文件的绝对路径(File/IO) =>  
    // 全类名 => Class对象 => 判断是否需要加入容器 => 反射动态创建  
    ClassLoader loader = ApplicationContext.class.getClassLoader();  
    URL resource = loader.getResource(path.replace(".", "/"));  
    System.out.println("resource = " + resource);  
    File file = new File(resource.getFile());  
    if (file.isDirectory()) {  
      File[] files = file.listFiles();  
      for (File f : files) {  
        String absolutePath = f.getAbsolutePath();  
        if (absolutePath.endsWith(".class")) {  
          System.out.println("- - - - - - - - - - - - -");  
          System.out.println("absolutePath: " + absolutePath);  
          String className = absolutePath.substring(absolutePath.lastIndexOf("/") + 1,  
              absolutePath.indexOf(".class"));  
          System.out.println("className: " + className);  
          String classFullPath = path + "." + className;  
          System.out.println("classFullPath: " + classFullPath);  
          try {  
            // 判断该类需要不要注入容器（通过查询是否有@Component注解）  
            Class<?> clazz = loader.loadClass(classFullPath);  
            if (clazz.isAnnotationPresent(Component.class)) {  
              System.out.println("是一个Bean = " + clazz);
              // TODO: 将 bean 信息封装到 BeanDefinition 对象
            } else {  
              System.out.println("> 不是一个Bean,不会加入容器中");  
            }  
          } catch (ClassNotFoundException e) {  
            e.printStackTrace();  
          }  
        }      
	  }    
    }  
  }
}
```
### 第二阶段: 扫描将 bean 信息封装到 BeanDefinition 对象，并放入到 Map
![[Pasted image 20230112162907.png]]

- BeanDefinition 用来封装记录 Bean 信息:  
  1. scope 以及属性值[单例与否]  
  2. Bean 对应的 Class 对象, 反射可以生成对应对象 
```java
public class BeanDefinition {  
  private String scope = "singleton";  
  private Class clazz;  
  
  // ...Getter/Setter方法封装、重写toString()
}
```

之前用 StringUtils 工具类是 Spring 框架里的, 不希望引入 Spring 框架, 就引入 commons-lang.

```java
public class ApplicationContext {
	// 先得到beanName[指定 => 指定, 默认 => 类名,首字母小写]  
	Component component = clazz.getDeclaredAnnotation(Component.class);  
	String value = component.value();  
	if (!"".equals(value)) {  
	  className = value;  
	} else {  
	  className = StringUtils.uncapitalize(className);  
	}  
	BeanDefinition beanDefinition = new BeanDefinition();  
	// 再将bean信息封装到BeanDefinition对象 => put添加Map  
	beanDefinition.setClazz(clazz);  
	// 获取Scope信息  
	if (clazz.isAnnotationPresent(Scope.class)) {  
	  Scope scope = clazz.getDeclaredAnnotation(Scope.class);  
	  beanDefinition.setScope(scope.value());  
	}
	beanDefinitionMap.put(className, beanDefinition);
}
```

### 第三阶段: 初始化 bean 单例池, 并完成 getBean 方法, createBean 方法
#### createBean 方法
```java
private Object createBean(BeanDefinition beanDefinition) {  
  // 得到Bean对象  
  Class clazz = beanDefinition.getClazz();  
  // 使用反射得到实例  
  try {  
    Object instance = clazz.getDeclaredConstructor().newInstance();  
    // TODO: 添加依赖注入的业务逻辑  
    return instance;  
  } catch (InstantiationException | IllegalAccessException | InvocationTargetException | NoSuchMethodException e) {  
    e.printStackTrace();  
  }  
  return null;  
}
```
#### 初始化 bean 单例池
```java
public ApplicationContext(Class configClass) {  
  this.configClass = configClass;  
  beanDefinitionByScan();  
  initSingleton();  
}
```

```java

// 通过beanDefinitionMap,初始化singletonObjects单例池:  
private void initSingleton() {  
  // 1. 封装成方法  
  Enumeration<String> keys = beanDefinitionMap.keys();  
  while (keys.hasMoreElements()) {  
    // 2. 得到beanName -> beanDefinition对象 -> 判断是否为单例  
    String beanName = keys.nextElement();  
    BeanDefinition beanDefinition = beanDefinitionMap.get(beanName);  
    if ("singleton".equalsIgnoreCase(beanDefinition.getScope())) {  
      // 3. 是单例 -> 将bean实例放入单例池中  
      Object bean = createBean(beanDefinition);  
      singletonObjects.put(beanName, bean);  
    }  
  }  
}
```

#### getBean 方法
```java
public Object getBean(String name) {  
  if (beanDefinitionMap.containsKey(name)) {  
    BeanDefinition beanDefinition = beanDefinitionMap.get(name);  
    // 得到scope -> 是否为单例  
    if ("singleton".equalsIgnoreCase(beanDefinition.getScope())) {  
      // 是单例 -> 从单例池中获取返回对象  
      return singletonObjects.get(name);  
    } else {  
      // 不是单例 -> 返回prototype  
      return createBean(beanDefinition);  
    }  
  } else {  
    throw new NullPointerException("没有此Bean");  
  }  
}
```
### 第四阶段: 完成依赖注入
#### 在 createBean 方法中完成依赖注入功能的完善
```java
// 添加依赖注入的业务逻辑:  
// 遍历当前要创建的对象的所有字段  
for (Field declaredField : clazz.getDeclaredFields()) {  
  // 通过反射获取所有声明的属性(属性),再去判断这些字段上有无@AutoWrite  
  if (declaredField.isAnnotationPresent(AutoWrite.class)) {  
    // 有@AutoWrite -> 进行依赖注入(先获取这些字段的名字 -> 获取Bean)  
    Object bean = getBean(declaredField.getName());  
    // 利用反射,给有@AutoWrite的字段进行赋值操作,组装完成  
    declaredField.setAccessible(true); // 私有属性 -> 爆破访问  
    declaredField.set(instance, bean);  
  }
  // TODO:添加后置处理器  
}
```
### 第五阶段:  bean 后置处理器实现
> 根据该类是否实现了某个接口, 来判断是否要执行某个方法[实际上就是接口编程]
- processor
	- `interface initialzingBean` 完成 `init()` 方法
	- `interface BeanPostProcessor` 后置处理器的具体接口默认完成 `before、after` 方法
- component
	- `class PostProcessor` 实现接口 `BeanPostProcessor`, 实现方法

#### 在 beanDefinitionByScan 方法
```java
// 判断该类需要不要注入容器（通过查询是否有@Component注解）之后
// 判断当前的这个clazz有没有实现BeanPostProcessor  
// clazz不是一个实例对象,而是一个类对象/clazz，使用isAssignableFrom  
if (BeanPostProcessor.class.isAssignableFrom(clazz)) {  
  BeanPostProcessor instance = (BeanPostProcessor) clazz.newInstance();  
  beanPostProcessorList.add(instance);  
  continue;  
}
```

#### createBean 方法中完善后置处理器
```Java
// init()之前,调用后置处理before  
for (BeanPostProcessor beanPostProcessor : beanPostProcessorList) {  
  // 在后置处理器的before方法，可以对容器的bean实例进行处理  
  // 然后返回处理后的bean实例，相当于做一个后置处理  
  instance = beanPostProcessor.postProcessBeforeInitialization(instance, beanName);  
}  
if (instance instanceof InitialzingBean) {  
  try {
    // 调用init()方法
    ((InitialzingBean) instance).afterPropertiesSet();  
  } catch (Exception e) {  
    e.printStackTrace();  
  }  
}  
// init()之后,调用后置处理after  
for (BeanPostProcessor beanPostProcessor : beanPostProcessorList) {  
  // 在后置处理器的after方法，可以对容器的bean实例进行处理  
  // 然后返回处理后的bean实例，相当于做一个后置处理  
  Object current = beanPostProcessor.postProcessAfterInitialization(instance, beanName);  
  if (current != null) {  
    instance = current;  
  }  
}  
System.out.println("- - - - - - - - - - -");  
return instance;
```

### 第六阶段: AOP 机制实现
- component
	- `SmartAnimalAspect`
	- `SmartAnimalcule`
	- `SmartDog implements SmartAnimalcule`

在 `PostProcessor` 的 `after()` 实现 AOP, 返回代理对象, 即对 Bean 进行包装 
```java
if ("smartDog".equals(beanName)) {  
  return Proxy.newProxyInstance(  
      PostProcessor.class.getClassLoader(), bean.getClass().getInterfaces(),  
      new InvocationHandler() {  
        @Override  
        public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  
          System.out.println("method = " + method);  
          Object result = null;  
          // 假如进行前置通知处理的方法是getSum  
          if ("getSum".equals(method.getName())) {  
            SmartAnimalAspect.showBeginLog();  
            result = method.invoke(bean, args);  
            SmartAnimalAspect.showSuccessEndLog(); // 返回通知  
          } else {  
            result = method.invoke(bean, args);  
          }  
          return result;  
        }  
    }  
  );  
}
```