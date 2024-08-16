轻量级容器框架

---
# 1. 基本介绍
![[Pasted image 20221226200520.png]]

1. Spring 核心学习内容IOC、AOP, jdbcTemplate,声明式事务
2. IOC: 控制反转,可以管理java对象
3. AOP: 切面编程
4. JDBCTemplate: 是 spring 提供一套访问数据库的技术,应用性强,相对好理解
5. 声明式事务: 基于 ioc/aop 实现事务管理

## 1. Spring 几个重要概念
1. Spring 可以整合其他的框架(老韩解读: Spring 是管理框架的框架)
2. Spring 有两个核心的概念: IOC 和 AOP
3. IOC [Inversion Of Control 反转控制]
	![[ioc开发模式与传统方式对比.excalidraw | 900]]
4. DI—Dependency Injection 依赖注入，可以理解成是 IOC 的另外叫法.
5. Spring 最大的价值，通过配置，
	- 给程序提供需要使用的对象
		- Servlet (Action/Controller)
		- Service
		- Dao (JavaBean/entity)
	- 这个是核心价值所在，也是 ioc 的具体体现, 实现解耦.

## 2. Spring 快速入门
### 1. 需求说明
通过 Spring 的方式[配置文件]，获取 JavaBean: Monster 的对象，并给该的对象属性赋值，输出该对象信息.
### 2. 代码实现
[[spring Initial experience]]
### 3. 查看spring 容器结构
> 先配置debugger

![[Pasted image 20221230113410.png]]

> 剖析 ioc 容器

#### 1. debugger 方式

![[Pasted image 20221230113710.png]]
![[Pasted image 20221230113754.png]]
![[Pasted image 20221230113910.png]]
![[Pasted image 20221230113917.png]]
![[Pasted image 20221230113922.png]]
![[Pasted image 20221230113936.png]]

#### 2. 图解方式

![[Pasted image 20230102084746.png]]

# 2. Spring 管理 Bean-IOC
![[Pasted image 20230102114052.png]]
## 1. Bean 管理包括两方面
### 1. 创建 bean 对象
### 2. 给 bean 注入属性
## 2. Bean 配置方式
### 1. 基于 xml 文件配置方式
#### 0. 通过属性 id 获取 bean
```java
// 1. 创建容器 ApplicationContext ioc
ApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");
// 2. 通过属性id获取时,直接指定Class类型  
Monster monster2 = ioc.getBean("monster1", Monster.class);
```
#### 1. 通过类型来获取 bean
```java
// 创建容器 ApplicationContext ioc ...
// 通过类型来获取 bean
Monster bean = ioc.getBean(Monster.class);
```
##### 细节说明
1. 按类型来获取 bean, 要求 ioc 容器中的同一个类的 **bean 只能有一个**, 否则会抛出异常 `NoUniqueBeanDefinitionException`
2. 应用场景: 比如在一个线程中只需要一个对象实例 (单例) 的情况
3. 在容器配置文件 (比如 beans. xml) 中给属性赋值, 底层是通过 Setter 方法完成的
	- JSP 页面中 EL 标签 `${对象.属性}`, 底层调用的是 Getter 方法

#### 2. 通过构造器配置 bean
```xml
<bean class="spring.bean.Monster" id="monster3">  
  <constructor-arg name="monsterId" value="300" />  
  <constructor-arg name="name" value="孙悟空" />  
  <constructor-arg name="skill" value="七十二变" />  
</bean>
```
指定使用全参构造函数
```java
// 创建容器 ApplicationContext ioc ...
// 通过构造器配置 bean
Monster monster3 = ioc.getBean("monster3", Monster.class);
```
##### 细节说明
- constructor-arg 标签可以指定使用构造器的参数
	- index: 表示构造器的第几个参数, 从 0 开始计算;
	- name: 构造器参数名; 
	- type: 构造器类型, 重载机制不会允许同参不同名情况出现

#### 3. 通过 p 名称空间配置 bean
在 spring 的 ioc 容器, 可以通过 p 名称空间 (**引入**) 来配置 bean 对象
```xml
<bean class="spring.bean.Monster" id="monster3"  
  p:monsterId="300" p:name="孙悟空" p:skill="七十二变"/>
```
其他不变

#### 4. 引用/注入其它 bean 对象
在 spring 的 ioc 容器, 可以通过 ref 来实现 bean 对象的**相互引用**

![[Pasted image 20230102173335.png]]

```xml
<!--配置MemberDaoImpl对象-->  
<bean class="spring.Dao.MemberDaoImpl" id="memberDao"/>  
<!--配置MemberService对象,并处理联系-->  
<bean class="spring.Service.MemberService" id="memberService">  
  <!--ref引用的对象是id=memberDao,体现了依赖注入引用关系-->  
  <property name="memberDao" ref="memberDao"/>  
</bean>
```

```java
// 创建容器 ApplicationContext ioc ...
ApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");  
// 获取Service
MemberService memberService = ioc.getBean("memberService", MemberService.class);  
// Service调用方法
memberService.add();
```

##### 细节说明
在 Spring 容器中, 是作为一个整体来执行的 (`即引用到 bean 对象, 对配置的顺序没有要求`)

#### 5. 引用/注入内部 bean 对象
在 spring 的 ioc 容器, 可以直接配置内部 bean 对象
```xml
<bean class="spring.Service.MemberService" id="memberService">  
  <property name="memberDao">  
    <bean class="spring.Dao.MemberDaoImpl"/>  
  </property>  
</bean>
```

#### 6. 引用/注入集合/数组类型
在 Spring 的 ioc 容器, 给 bean 对象的集合/数组类型属性赋值
```xml
<bean class="spring.bean.Master" id="master">  
  <property name="name" value="太上老君"/>  
  <!--List集合-->  
  <property name="masterList">  
    <list>  
      <ref bean="monster1"/>  
      <ref bean="monster2"/>  
      <ref bean="monster3"/>  
    </list>  
  </property>  
  <!--Map集合-->  
  <property name="monsterMap">  
    <map>  
      <entry value-ref="monster3" key="monster001"/>  
      <entry>  
        <key>  
          <value>monster002</value>  
        </key>  
        <ref bean="monster2"/>  
      </entry>  
      <entry value-ref="monster1" key="monster003"/>  
    </map>  
  </property>  
  <!--Set集合-->  
  <property name="monsterSet">  
    <set>  
      <ref bean="monster2"/>  
      <ref bean="monster1"/>  
      <ref bean="monster3"/>  
    </set>  
  </property>  
  <!--Array-->  
  <property name="monsterName">  
    <array>  
      <value>小妖怪</value>  
      <value>大妖怪</value>  
      <value>老妖怪</value>  
    </array>  
  </property>  
  <!--Properties集合-->  
  <property name="pros">  
    <props>  
      <prop key="username">root</prop>  
      <prop key="password">admin</prop>  
      <prop key="url">127.0.0.1</prop>  
    </props>  
  </property>  
</bean>
``` 
##### 使用细节
1. 主要掌握 List/Map/Properties 三种集合的使用.
2. Properties 集合的特点
	- Properties 是 Hashtable 的子类
	- key 是 string 而 value 也是 string, 是 key-value 的形式

#### 7. 通过 util 名称空间创建 list
spring 的 ioc 容器, 可以通过 util 名称空间创建 liast 集合
```xml
<!--定义一个util：list 并且指定id，可以达到数据复用-->  
<util:list id="myBookList">  
  <value>三国演义</value>  
  <value>红楼梦</value>  
  <value>水浒传</value>  
</util:list>  
  
<bean class="spring.bean.BookStore" id="bookStore">  
  <property name="bookList" ref="myBookList"/>  
</bean>
```

#### 8. 级联属性赋值
spring 的 ioc 容器, 可以直接给对象属性的属性赋值，即级联属性赋值
```xml
<!--配置级联属性-->  
<bean class="spring.bean.Dept" id="dept"/>  
<bean class="spring.bean.Emp" id="emp">  
  <property name="name" value="jack"/>  
  <property name="dept" ref="dept"/>  
  <!--希望给dept的name属性指定值[级联属性赋值]-->  
  <property name="dept.name" value="软件开发部门"/>  
</bean>
```

#### 9. 通过静态工厂获取对象
```xml
<!--  
配置monster对象,通过静态工厂获取  
1. 通过静态工厂获取/配置bean  
2. class 是静态工厂类的全路径  
3. factory-method 表示指定静态工厂类的哪个方法返回对象  
4. constructor-arg指定获取要返回静态工厂的哪个对象  
-->  
<bean id="my_monster01" class="spring.Factory.MyStaticFactory" factory-method="getMonster">  
  <constructor-arg value="monster01"/>  
</bean>
```

```java
// 创建容器 ApplicationContext ioc ...
Monster myMonster01 = ioc.getBean("my_monster01", Monster.class);
```

#### 10. 通过实例工厂获取对象
```xml
<!--配置实例工厂对象-->  
<bean class="spring.Factory.MyInstanceFactory" id="instanceFactory"/>  
<!--
	配置monster对象,通过实例工厂,
	factory-bean指定工厂对象,
	factory-method指定工厂对象的具体方法返回bean
-->  
<bean id="my_monster02" factory-bean="instanceFactory" factory-method="getMonster">  
  <constructor-arg value="monster03"/>  
</bean>
```

#### 11. 通过 FactoryBean 获取对象 (重点)
`public class MyFactoryBean implements FactoryBean<Monster>{}`

```xml
<!--  
	配置monster对象,通过FactoryBean获取  
	1. class 指定使用FactoryBean  
	2. key 表示 MyFactoryBean 属性key  
	3. value 就是要获取的对象对应的key  
-->  
<bean id="my_monster03" class="spring.Factory.MyFactoryBean">  
  <property name="key" value="monster03"/>  
</bean>
```

#### 12. bean 配置信息重用 (继承)
在 spring 的 ioc 容器, 提供了一种继承的方式来实现 bean 配置信息的重用
```xml
<bean class="spring.bean.Monster" id="monster4" parent="monster1"/>
```

#### 13. 配置 bean 的后置处理器

1. 在 spring 的 ioc 容器, 可以配置 bean 的后置处理器
2. 该处理器/对象会在 **<u>bean 初始化方法</u>** 调用前和初始化方法调用后被调用
3. 程序员可以在后置处理器中编写自己的代码

```java
public class MyBeanPostProcessor implements BeanPostProcessor {  
  /**  
   * Bean初始化前的处理  
   * 何时被调用: 在Bean的init()之前调用  
   * @param bean     在IOC容器中创建/配置的Bean  
   * @param beanName 在IOC容器中创建/配置Bean的id  
   * @return Object 对传入的Bean处理  
   * @throws BeansException Bean异常  
   */  
  @Override  
  public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {  
    System.out.println("postProcessBeforeInitialization()... bean = " + bean + " beanName = " + beanName);  
    if (bean instanceof Cat) {  
      ((Cat) bean).setName("小猫咪");  
    }  
    return bean;  
  }  
  
  /**  
   * Bean初始化后处理  
   * 何时被调用: 在Bean的init()调用之后  
   * @param bean     在IOC容器中创建/配置的Bean  
   * @param beanName 在IOC容器中创建/配置Bean的id  
   * @return Object 对传入的Bean处理  
   * @throws BeansException Bean异常  
   */  
  @Override  
  public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {  
    System.out.println("postProcessAfterInitialization()... bean = " + bean + " beanName = " + beanName);  
    return bean;  
  }  
}
```

```xml
<!--配置Cat对象-->  
<bean class="spring.bean.Cat" id="cat" init-method="init" destroy-method="destroy">  
  <property name="id" value="101"/>  
  <property name="name" value="小狸猫"/>  
</bean>  
<bean class="spring.bean.Cat" id="cat2" init-method="init" destroy-method="init">  
  <property name="id" value="102"/>  
  <property name="name" value="小花猫"/>  
</bean>  
<!--配置后置处理器-->  
<bean class="spring.bean.MyBeanPostProcessor" id="beanPostProcessor"/>
```

- 说明
	1. 使用 AOP (反射+动态代理+IO+容器+注解)
	2. 可以对 IOC 容器中所有的对象进行统一处理 (如日志处理、权限处理、安全校验、事务管理)

#### 14. 通过属性文件给 bean 注入值
在 spring 的 ioc 容器, 通过属性文件给 bean 注入值
```xml
<!--配置Monster,${key}-->  
<bean class="spring.bean.Cat" id="cat3">  
  <property name="id" value="${id}"/>  
  <property name="name" value="${name}"/>  
</bean>  
<!--指定文件属性,location指定文件位置,需要带上classpath:-->  
<context:property-placeholder location="classpath:my.properties"/>
```

#### 15. 基于 XML 的 bean 的自动装配
在 spring 的 ioc 容器, 可以实现自动装配 bean
![[Pasted image 20230103230232.png]]
```xml  
<!--配置OrderDao对象-->  
<bean class="spring.Dao.OrderDao" id="orderDao"/>  
<!--  
配置OrderService,使用自动装配  
1.autowire="byType" 表示在创建OrderService时,通过类型方式给对象属性自动完成赋值/引用
2.autowire="byName" 表示通过名字完成自动装配,执行如下:
	1). 先看 OrderService 属性: private OrderDao orderDao;
	2). 再根据这个属性的setXxx()方法的Xxx来找对象的id
	3). public void setOrderDao2() 就会找id=orderDao2对象来进行自动装配
	4). 如果没有找到<bean id="orderDao2".../>就装配失败 
-->  
<bean class="spring.Service.OrderService" id="orderService" autowire="byType"/>  
<!--配置Controller-->  
<bean class="spring.Controller.OrderServlet" id="orderServlet" autowire="byType"/>
```

#### 16. spring el 表达式
```xml
<bean id="spELBean" class="spring.bean.SpELBean">  
  <!-- sp el 给字面量 -->  
  <property name="name" value="#{'fishx'}"/>  
  <!-- sp el 引用其它bean -->  
  <property name="monster" value="#{monster}"/>  
  <!-- sp el 引用其它 bean 的属性值 -->  
  <property name="monsterName" value="#{monster.name}"/>  
  <!-- sp el 调用普通方法赋值 -->  
  <property name="crySound" value="#{spELBean.cry('小猫喵喵叫...')}"/>  
  <!-- sp el 调用静态方法 赋值 -->  
  <property name="bookName" value="#{T(spring.bean.SpELBean).read('天龙八部')}"/>  
  <!-- sp el 通过运算赋值 -->  
  <property name="result" value="#{89*1.1}"/>  
</bean>
```
### 2. 基于注解方式
#### 1. 基本使用
基于注解的方式配置 bean, 主要是项目开发中的组件，比如 Controller、Service、和 Dao.

|    注解     |                   解释                   |      适用场景       |      |
|:-----------:|:----------------------------------------:|:-------------------:|:----:|
| @Component  |       表示当前注解标识的是一个组件       |                     | 组件 |
| @Controller |      表示当前注解标识的是一个控制器      |  通常用于 Servlet   |  控制器 |
|  @Service   | 表示当前注解标识的是一个处理业务逻辑的类 | 通常用于 Service 类 |  业务逻辑层  |
| @Repository |   表示当前注解标识的是一个持久化层的类   |   通常用于 Dao 类   |  实体层  |

1. 给类添上注解
```java
@Repository  
public class UserDao { }

@Service  
public class UserService { }

@Controller  
public class UserServlet { }

// 标识该类是一个组件,是一个通用的注解  
@Component  
public class MyComponent { }
```

2. 配置 beans. xml
```xml
<!--  
配置容器要扫描的包  
1. component-scan 指定包下的类进行扫描,并创建对象到容器  
2. base-package 指定要扫描的包  
3. 当Spring容器创建/初始化时,就会扫描指定包下所有的注解(@Controller...)类  
将其实例化,生成对象,放入到IOC容器  
-->  
<context:component-scan base-package="spring.component"/>
```

3. 从 Spring 的 IOC 容器中取出对象

#### 2. 注意事项和细节说明
1. 导入 `spring-aop-5.3.8.jar`
2. 必须在 Spring 配置文件中指定"自动扫描的包"，IOC 容器才能够检测到当前项目中哪些类被标识了注解，注意到导入 context 名称空间
	1. 可以使用通配符 * 来指定
	2. 同时还会扫描子包内容
3. Spring 的 IOC 容器只要检查到注解就会生成对象, 但这个注解含义 Spring 不会识别, 注解是给程序员辨别类的.
4. `<context:component-scan base-package="spring.component" resource-pattern="User*.class" />` 
	- `resource-pattern="User*.class"` : 表示只扫描满足要求的类
5. 排除注解类
```xml
<context:component-scan base-package="spring.component">  
  <!--  
  context:exclude-filter 指定要排除哪些类,  
  type 指定排除方式 annotation表示按照注解来排除,  
  expression 指定要排除的注解的全路径  
  -->  
  <context:exclude-filter type="annotation"  
                          expression="org.springframework.stereotype.Service"/>  
</context:component-scan>
```
6. 只扫描包含注解类
```xml
<!-- 不使用默认过滤/扫描机制 -->
<context:component-scan base-package="spring.component" use-default-filters="false">  
  <context:include-filter type="annotation"  
                          expression="org.springframework.stereotype.Service"/>  
</context:component-scan>
```
7. 默认情况：标记注解后，**<u>类名首字母小写作为 id 的值</u>**。也可以使用注解的 value 属性指定 id 值，并且 value 可以省略
```Java
// 1. 标记注解后,类名首字母小写作为id的值【默认】  
// 2. value 表示指定注解类的id,因此通过id获取也应该是value的值  
@Repository(value = "fishx")  
public class UserDao { }
```
8. 扩展: [彻底弄懂@Controller、@Service、@Component](https://zhuanlan.zhihu.com/p/454638478) 

#### 3. 自动装配
基于注解配置 Bean，也可实现自动装配，使用的注解是： `@AutoWired` 或者 `@Resource`
1. @AutoWired 的规则说明
	- 在 IOC 容器中查找待装配的组件的**类型**:
		- 如果有唯一的 bean 匹配
		  - 就使用该 bean 装配 
		- 如待装配的类型对应的 bean 在 IOC 容器中有多个
			- 则使用待装配的属性的属性名作为 id 值再进行查找, 找到就装配，找不到就抛异常
2. @Resource 的规则说明
	- @Resource有两个属性: 
		- name 属性解析为 bean 的名字
			- 如果使用 name 属性, 则使用 byName 的自动注入策略
		- type 属性则解析为 bean 的类型
			- 使用 type 属性时则使用 byType 自动注入策略
	- 没有指定 name 和 type 属性:
		- 先使用 byName 注入策略
		- 如果匹配不上, 再使用 byType 策略
		- 如果都不成功，就会报错

#### 4. 泛型依赖注入
Spring 容器对泛型自动装配
![[Pasted image 20230105180513.png]]

```java
public abstract class BaseDao<T> {  
  public abstract void save();  
}

@Repository  
public class BookDao extends BaseDao<Book> {  
  @Override  
  public void save() {  
    System.out.println("BookDao() save()...");  
  }  
}

public class BaseService<T> {  
  // 动态绑定,注入对应类  
  @Autowired  
  private BaseDao<T> baseDao;  
  
  public void save() {  
    baseDao.save();  
  }  
}

@Service  
public class BookService extends BaseService<Book> { }
```
### 3. 手动开发

#### 1. 简单 Spring 基于 XML 配置程序

1. 自己写一个简单的 Spring 容器, 通过读取 beans. xml，获取第 1 个 JavaBean: Monster 的对象，并给该的对象属性赋值，放入到容器中, 输出该对象信息.
2. 也就是说, 不使用 Spring 原生框架,**简单模拟实现**, 了解 Spring 容器的简单机制
3. 思路分析
	![[Pasted image 20230102103730.png]]

[[spring Initial experience#4. 手动实现 Spring-IOC 容器|手动实现基于xml的 Spring-IOC 容器]]

#### 2. 简单的 Spring 基于注解配置的程序
1. 自己写一个简单的 Spring 容器, 通过读取类的注解 (`@Component、@Controller、@Service、@Reponsitory`)，将对象注入到 IOC 容器
2. 不使用 Spring 原生框架，我们自己使用 `IO+Annotaion+反射+集合技术` 实现, 打通 Spring 注解方式开发的**技术痛点**
3. 思路分析
	![[Pasted image 20230105113511.png]]

[[spring Initial experience#5. 手动实现 Spring-IOC 基于注解配置|手动实现基于注解的 Spring-IOC 容器]]
## 3. bean 创建顺序
IOC 是作为整体执行的, 因此是先按顺序创建 Bean, 再处理 Bean 之间的关系
### 1. 默认情况
在 spring 的 ioc 容器, 默认是按照配置的顺序创建 bean 对象
### 2. 存在依赖关系
![[Pasted image 20230103174615.png]]
#### 实例 1. `depends-on`

```xml
<bean id="student01" class="com.hspedu.bean.Student" depends-on="department01"/>

<bean id="department01" class="com.hspedu.bean.Department" />
```

会先创建 `department01` 对象，再创建 ` student01` 对象. (无论先后顺序)

#### 实例 2. 引入/注入 Bean
`先默认` 按顺序创建 Bean, `再处理` Bean 之间的关系

## 4. bean 对象的单例和多例
在 spring 的 ioc 容器, 在默认是按照单例创建的，即配置一个 bean 对象后，ioc 容器只会创建一个 bean 实例。
```xml
<!--  
	1. 在默认情况下scope属性是singleton  
	2. 在IOC容器中，只有一个该Bean对象  
	3. 单例: 当执行getBean时,返回的是同一个对象  
	4. 当希望每次返回一个新的Bean对象,则可以改为prototype属性  
-->  
<bean id="car1" class="spring.bean.Cat" scope="prototype">  
  <property name="id" value="100"/>  
  <property name="name" value="小狸猫"/>  
</bean>
```
### 细节说明
1. 默认是单例 singleton, 在启动容器时, 默认就会创建, 并放入到 singletonObjects 集合
2. 当 `<bean scope="prototype">` 设置为多实例机制后, 该 bean 是在 `getBean()` 时才创建
3. 如果是单例 singleton, 同时希望在 `getBean()` 时才创建, 可以指定懒加载 `lazy-init="true"` (注意默认是 false)
4. 通常情况下, `lazy-init` 就使用默认值 `false`, 在开发看来, **<u>用空间换时间是值得的, 除非有特殊的要求</u>**.

## 5. bean 的生命周期
- bean 对象创建是由 JVM 完成的，然后执行如下方法
	1. 执行构造器
	2. 执行 set 相关方法
	3. 调用 bean 的初始化的方法（需要配置）
	4. 使用 bean
	5. 当容器关闭时候，调用 bean 的销毁方法（需要配置）

```xml
<bean id="car1" class="spring.bean.Cat" init-method="init" destroy-method="destroy">  
  <property name="id" value="100"/>  
  <property name="name" value="小狸猫"/>  
</bean>
```

```java
ApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");  
Cat cat = ioc.getBean("car1", Cat.class);  
System.out.println("使用Cat = " + cat);  
((ConfigurableApplicationContext) ioc).close();
```



# 3. AOP (切面编程)

[动画讲解SpringAOP](https://www.bilibili.com/video/BV16D4y157w6?vd_source=c694526ad958a6026a9455097a141295)

![[Pasted image 20230106124035.png]]

![[Pasted image 20230106125543.png]]

## 1. AOP 实现方式
### 1. 基于动态代理的方式[内置 aop 对象]
[[spring Initial experience#6. 基于动态代理的方式 (内置 aop 实现)| 基于原生Proxy对象的动态代理code]]
### 2. 使用 Spring 框架 aspectj 来实现
|      注解       |   名称   |
|:---------------:|:--------:|
|     @Before     | 前置通知 |
| @AfterReturning | 返回通知 |
| @AfterThrowing  | 异常通知 |
|     @After      | 后置通知 |
|     @Around     | 环绕通知 |
![[Pasted image 20230106155114.png]]

[[spring Initial experience#7. 基于动态代理的方式 (Spring 框架)|基于Spring框架的动态代理code]]

#### 细节说明
1. 切入表达式的更多配置，比如使用模糊配置
	- 匹配 `SmartDog类` 下所有 `形参列表为(float, float)` 的所有切面方法
```java
@Before(value="execution(public float spring.AOP.aspectj.SmartDog.*(float, float))")
public static void showBeginLog(JoinPoint joinPoint) { 
	// 通过连接点对象JoinPoint 获取方法签名 
	Signature signature = joinPoint.getSignature(); 
	System.out.println("前置通知: 日志--方法名--" + signature.getName());
}
```
2. 表示所有访问权限，所有包的下所有类的所有方法，都会被执行切面方法
	- 匹配 `所有包` 下 `所有类` 的 `所有方法`, 都执行这个切面方法
```java
@AfterReturning(value = "execution(* *.*(..))")  
public void showSuccessEndLog(JoinPoint joinPoint) {  
  Signature signature = joinPoint.getSignature();  
  System.out.println("返回通知: 日志--方法名--" + signature.getName());  
}
```
3. 当 spring 容器开启了 `基于注解的 AOP 功能` :
	- 获取注入的对象, 需要以接口的类型来获取
		- 获取注入的对象, 也可以通过 id 来获取, 但是也要转成接口类型
	- 因为你注入的对象. getClass () 已经是代理类型了
```xml
<!-- 开启基于注解的 AOP 功能 --> 
<aop:aspectj-autoproxy/>
```

## 2. AOP-切入表达式 
通过**表达式的方式**定位**一个或多个**具体的连接点.

切入点表达式的语法格式:
```Java
execution([权限修饰符][返回值类型][简单类名/全类名][方法名]([参数列表]))
```

### 常用的切入表达式
| 表达式 |      `execution(*com.spring.Camera.*(..))`       |
|:------:|:------------------------------------------------:|
| 含义⓵  |             接口/类中声明的所有方法.             |
| 含义⓶  |      第一个 `*` 代表任意修饰符及任意返回值       |
| 含义⓷  |             第二个 `*` 代表任意方法              |
| 含义⓸  |         “..”匹配任意数量、任意类型的参数         |
| 含义⓹  | 若目标类、接口与该切面类在同一个包中可以省略包名 |
 
| 表达式 | `execution(public *Camera.*(..))` |
|:------:|:---------------------------------:|
| 含义   |      接口/类的所有的公有方法      |

| 表达式 | `execution(public double Camera.*(..))` |
|:------:|:---------------------------------:|
| 含义   |      接口/类中返回 double 类型数值的方法      |

| 表达式 | `execution(public double Camera.*(double,..))` |
|:------:|:----------------------------------------------:|
| 含义⓵  |         第一个参数为 double 类型的方法         |
| 含义⓶  |       ".." 匹配任意数量、任意类型的参数        |

| 表达式 | `execution(public double Camera.*(double,double))` |
|:------:|:--------------------------------------------------:|
|  含义  |        参数类型为 double, double 类型的方法        |

| 表达式 | `execution(* *.add(int,..)) && execution (* *.sub (int,..)) ` |
|:------:|:-------------------------------------------------------------:|
| 含义⓵  |      任意类中第一个参数为 int 类型的 add 方法和 sub 方法      |
| 含义⓶  |    切入点表达式还可以通过 `&&`、`!!`、`!` 等操作符结合起来    |

### 注意事项和细节
1. 切入表达式也可以指向类的方法, 这时切入表达式会对该类/对象生效
2. 切入表达式也可以指向接口的方法, 这时切入表达式会对实现了接口的类/对象生效
3. 切入表达式也可以对没有实现接口的类，进行切入
4. [动态代理 jdk 的 Proxy 与 Spring 的 CGlib](https://www.cnblogs.com/threeAgePie/p/15832586.html)
	1. JDK 动态代理是面向接口的，只能增强实现类中接口中存在的方法。CGlib 是面向父类的，可以增强父类的所有方法
	2. JDK 得到的对象是 JDK 代理对象实例，而 CGlib 得到的对象是被代理对象的子类

### AOP-切入点表达式重用

为了统一管理切入点表达式，可以使用切入点表达式重用技术。

![[Pasted image 20230109175719.png]]


## 3. AOP-JoinPoint

JoinPoint 链接点

### JoinPoint 常用方法

```java
joinPoint.getSignature().getName(); // 获取目标方法名
joinPoint.getSignature().getDeclaringType().getSimpleName(); // 获取目标方法所属 类的简单类名 
joinPoint.getSignature().getDeclaringTypeName(); // 获取目标方法所属类的类名 
joinPoint.getSignature().getModifiers(); // 获取目标方法声明类型(public、private、 protected)
```

## 4. AOP-返回通知获取结果

```java
@AfterReturning(value = "execution(public float SmartDog.*(float, float)))", returning = "res")  
public void showSuccessEndLog(JoinPoint joinPoint, Object res) {  
  Signature signature = joinPoint.getSignature();  
  System.out.println("返回通知: 日志--方法名--" + signature.getName()  
      + " 方法结束--结果：result=" + res);  
}
```

## 5. AOP-异常通知中获取异常

```java
@AfterThrowing(value = "execution(public float SmartDog.getSum(float,float))", throwing = "throwable")  
public void showExceptionLog(JoinPoint joinPoint, Throwable throwable) {  
  System.out.println("异常通知: 日志--方法名--" + joinPoint.getSignature().getName()  
      + "--异常类型--" + throwable);  
}
```

## 6. AOP-环绕通知【了解】

![[Pasted image 20230109171739.png]]

```java
@Aspect  
@Component  
public class SmartAnimalAspect {  
  @Around(value = "execution(public int Camera.*(..))")  
  public Object showBeginLog(ProceedingJoinPoint joinPoint) {  
    Object result = null;  
    String methodName = joinPoint.getSignature().getName();  
    try {  
      // 1. 相当于[前置通知]切面方法  
      List<Object> args = Arrays.asList(joinPoint.getArgs());  
      System.out.println("AOP环绕通知--前置通知--" + methodName + "方法开始了--参数有：" + args);  
      // 在环绕通知中一定要调用 joinPoint.proceed() 来执行目标方法  
      result = joinPoint.proceed();  
      // 2. 相当于[返回通知]切面方法  
      System.out.println("AOP环绕通知--返回通知--" + methodName + "方法结束了--结果是：" + result);  
    } catch (Throwable e) {  
      // 3. 相当于[异常通知]切面  
      System.out.println("AOP环绕通知--异常通知--" + methodName + "方法抛出了--异常对象：" + e);  
    } finally {  
      // 4. 相当于[后置通知]切面方法  
      System.out.println("AOP环绕通知--后置通知--" + methodName + "方法最终结束了");  
    }  
    return result;  
  }  
}
```

## 7. AOP-切面优先级问题

如果同一个方法，有多个切面在同一个切入点切入，那么执行的优先级如何控制.

使用 `@order(value=n)` 来控制 n 值越小，优先级越高. 

如何理解输出的信息顺序? => 类似 Filter 的过滤链式调用机制

![[Pasted image 20230111172206.png]]

## 8. 基于 XML 配置-AOP
```xml
<!--配置一个Bean-切面类对象-->  
<bean class="spring.AOP.xml.SmartAnimalAspect" id="smartAnimalAspect1"/>  
<!--配置一个Bean-SmartDog对象-->  
<bean class="spring.AOP.xml.SmartDog" id="smartDog1"/>  
<!--配置切面类-->  
<aop:config>  
  <!--配置切入点-->  
  <aop:pointcut expression="execution(public float spring.AOP.xml.SmartDog.*(float,float))" id="myPoint"/>  
  <!--指定切面对象,配置切面的前置、返回、异常、最终通知-->  
  <aop:aspect ref="smartAnimalAspect1" order="1">  
    <aop:before method="showBeginLog" pointcut-ref="myPoint"/>  
    <aop:after-returning method="showSuccessEndLog" pointcut-ref="myPoint" returning="res"/>  
    <aop:after-throwing method="showExceptionLog" pointcut-ref="myPoint" throwing="throwable"/>  
    <aop:after method="showFinallyEndLog" pointcut-ref="myPoint"/>  
  </aop:aspect>  
</aop:config>
```

# 4. 手动实现 Spring 底层机制

初始化 IOC 容器 + 依赖注入 + BeanPostProcessor 机制 + AOP 

---
![[spring Initial experience#Spring 整体架构分析]]

## 类加载
Bootstrap 类加载器--------------对应路径 jre/lib 
Ext 类加载器--------------------对应路径 jre/lib/ext 
App 类加载器-------------------对应路径 classpath

# 5. JdbcTemplate
![[jdbcTemplate#jdbcTemplate 使用]]

# 6. 声明式事务 
## 1. 事务分类 
### 编程式事务
```java
Connection connection = JdbcUtils.getConnection(); 
try { 
	// 1. 先设置事务不要自动提交 
	connection.setAutoCommint(false); 
	// 2. 进行各种 crud 
	//多个表的修改，添加 ，删除 
	// 3. 提交 
	connection.commit(); 
} catch (Exception e) { 
	// 4. 回滚
	conection.rollback();
}
```
### 声明式事务
- 环境搭建
	- `tx`
		- `dao.GoodsDao`
		- `service.GoodsService`
	- `src/goods.properties`
	- `src/tx.xml`
- 声明式事务具体使用
	1. 标识需要使用事务控制的方法 `@Resource` 
		1. 使用 `@Transactional` 可以进行声明式事务控制 (即将标识的方法中的，对数据库的操作作为一个事务管理) 
		2. `@Transactional` 底层使用的仍然是 AOP 机制, 再使用动态代理对象来调用 buyGoods
		3. 在执行 `buyGoods()` 方法先调用事务管理器的 `doBegin()`, 调用 `buyGoods()`
			1. 如果执行没有发生异常，则调用事务管理器的 `doCommit()`
			2. 如果发生异常调用事务管理器的 `doRollback()`
	2. 配置事务管理对象, <span style="color:red">注意 JdbcTemplate 对象使用的 dataSource 和事务管理对象的要保持一致!</span> 
	3. 开启基于注解的声明式事务功能, <span style="color:red">注意包不要导错!</span>
- [[Declarative transaction]]

## 2. 事务的传播机制
### 事务传播的属性/种类一览图
![[Pasted image 20230113201112.png]]

### 事务传播的属性/种类机制分析
![[Pasted image 20230113201310.png]]
![[Pasted image 20230113201327.png]]

### 事务的传播机制的设置方法
![[Pasted image 20230113201730.png]]

### REQUIRES_NEW 和 REQUIRED 在处理事务的策略
- 处理事务的策略
	 - 如果设置为 `REQUIRES_NEW`:
		- `事务2` 如果出现错误, 不会影响到 `事务1`, 反之亦然
		- 即**它们的事务**是**独立的**
	- 如果设置为 `REQUIRED` (默认):
		- `事务1` 和 `事务2` 是一个整体
		- 只要有出现异常, 这两个事务都会被回滚

## 3. 事务的隔离级别
![[Pasted image 20230113212341.png]]

- 事务隔离级别说明
	- 默认的隔离级别, 就是 mysql 数据库默认的隔离级别一般为 `REPEATABLE_READ`
	- 查看数据库默认的隔离级别 `SELECT @@global.tx_isolation`

## 4. 事务的超时回滚

- 基本介绍
	1. 如果一个事务执行的时间超过某个时间限制,就让该事务回滚。
	2. 可以通过设置事务超时回滚来实现
- 基本语法
	![[Pasted image 20230114085543.png]]