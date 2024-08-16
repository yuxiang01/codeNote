- 标准的微服务SpringCloud 和 SpringCloud alibaba 出现原因和价值是什么?
	1. 微服务可以根据业务不同, 将一个大项目, 分解成不同的服务
		- 比如: 搜索服务、网关服务、配置服务、存储服务、发现服务等
	2. 各个服务通过分布式方式进行工作, 从而可以高效、快速、稳定的完成复杂的功能
	3. 大项目的某些模块 -> 抽出形成微服务 -> 配合分布式工作方式
		- 从而高效、快速、稳定的完成复杂业务

## 一、微服务
![[Pasted image 20230925200141.png]]
>[!note] “微服务” 一词源于 `Martin Fowler` 的名为 `Microserices` 的博文
> - 微服务是系统构架上的设计风格
> 	- 它的主旨是将<font color="#ff0000">一个原本独立的系统拆分成多个小型服务</font>
> 	- 这些小型服务都在各自独立的进程中运行, 服务之间通过基于 HTTP 的 RESTFUL API 进行通信协作
> - 被拆分成的每一个小型服务都围绕着系统中的某一项或一些耦合度较高的业务功能进行构建
> 	- 并且每个服务都维护着自身的数据存储、业务开发、自动化测试案例以及独立部署机制
> - 由于有轻量级的通信协作基础，所以这些微服务可以使用不同的语言来编写, 这里我们使用 java 

### 1.1 Spring Cloud 全面说明
1. SpringCloud 来源于 Spring, 是更高层次的、架构视角的综合性大型项目，目标旨在构建一套标准化的微服务解决方案，让架构师在使用微服务理念构建系统的时，面对各环节的问题都可以找到相应的组件来处理
2. Spring Cloud 是 Spring 社区为微服务架构提供的一个 "全家桶" 套餐。套餐中各个组件之间的配合，可以减少在组件的选型和整合上花费的精力，可以快速构建起基础的微服务架构系统，是微服务架构的最佳落地方案
3. Spirng Cloud 天然支持 Spring Boot (有版本对应要求)，使用门槛较低
4. 解决与分布式系统相关的复杂性 – 网络问题，延迟开销，带宽问题，安全问题
5. 处理服务发现的能力 – 服务发现允许集群中的进程和服务找到彼此并进行通信
6. 解决冗余问题 – 冗余问题经常发生在分布式系统中
7. 解决负载平衡 – 改进跨多个计算资源（例如计算机集群，网络链接，中央处理单元）的工作负载分布

### 1.2 核心组件图
![[Spring Cloud核心组件.png]]
### 1.3 分布式示意图
![[Pasted image 20230926105659.png]]
1. Spring Cloud 是微服务的落地, 体现了微服务的弹性设计 
2. 微服务的工作方式一般是基于分布式的
3. Spring Cloud 仍然是 Spring 家族一员，可以解决微服务的分布式工作方式带来的各种问题
4. Spring Cloud 提供很多组件，比如服务发现, 负载均衡, 链路中断, 分布式追踪和监控, 甚至提供 API gateway 功能.

![[Pasted image 20230926110508.png]]

### 1.4 Spring Cloud 组件选型
![[Pasted image 20230926110528.png]]

## 二、SpringCloud Alibaba
Spring Cloud Alibaba 致力于提供微服务开发的一站式解决方案。此项目包含开发分布式应用微服务的必需组件，方便开发者通过 Spring Cloud 编程模型轻松使用这些组件来开发分布式应用服务。

依托 Spring Cloud Alibaba，您只需要添加一些注解和少量配置，就可以将 Spring Cloud 应用接入阿里微服务解决方案，通过阿里中间件来迅速搭建分布式应用系统。

- 主要功能
	* **服务限流降级**：默认支持 WebServlet、WebFlux、OpenFeign、RestTemplate、Spring Cloud Gateway、Zuul、Dubbo 和 RocketMQ 限流降级功能的接入，可以在运行时通过控制台实时修改限流降级规则，还支持查看限流降级 Metrics 监控。
	* **服务注册与发现**：适配 Spring Cloud 服务注册与发现标准，默认集成了 Ribbon 的支持。
	* **分布式配置管理**：支持分布式系统中的外部化配置，配置更改时自动刷新。
	* **消息驱动能力**：基于 Spring Cloud Stream 为微服务应用构建消息驱动能力。
	* **分布式事务**：使用 @GlobalTransactional 注解，高效并且对业务零侵入地解决分布式事务问题。
	* **阿里云对象存储**：阿里云提供的海量、安全、低成本、高可靠的云存储服务。支持在任何应用、任何时间、任何地点存储和访问任意类型的数据。
	* **分布式任务调度**：提供秒级、精准、高可靠、高可用的定时（基于 Cron 表达式）任务调度服务。同时提供分布式的任务执行模型，如网格任务。网格任务支持海量子任务均匀分配到所有 Worker（schedulerx-client）上执行。
	* **阿里云短信服务**：覆盖全球的短信服务，友好、高效、智能的互联化通讯能力，帮助企业迅速搭建客户触达通道。
- 核心组件 
	- **[Sentinel](https://github.com/alibaba/Sentinel)**：把流量作为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性。
	- **[Nacos](https://github.com/alibaba/Nacos)**：一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。
	- **[RocketMQ](https://rocketmq.apache.org/)**：一款开源的分布式消息系统，基于高可用分布式集群技术，提供低延时的、高可靠的消息发布与订阅服务。
	- **[Seata](https://github.com/seata/seata)**：阿里巴巴开源产品，一个易于使用的高性能微服务分布式事务解决方案。
	- **[Alibaba Cloud OSS](https://www.aliyun.com/product/oss)**: 阿里云对象存储服务（Object Storage Service，简称 OSS），是阿里云提供的海量、安全、低成本、高可靠的云存储服务。您可以在任何应用、任何时间、任何地点存储和访问任意类型的数据。
	- **[Alibaba Cloud SchedulerX](https://cn.aliyun.com/aliware/schedulerx)**: 阿里中间件团队开发的一款分布式任务调度产品，提供秒级、精准、高可靠、高可用的定时（基于 Cron 表达式）任务调度服务。
	- **[Alibaba Cloud SMS](https://www.aliyun.com/product/sms)**: 覆盖全球的短信服务，友好、高效、智能的互联化通讯能力，帮助企业迅速搭建客户触达通道。

![[Pasted image 20230930103700.png]]
## 三、微服务基础环境搭建
### 3.1 创建父工程
创建父工程，用于聚合其它微服务模块
![[Pasted image 20230926112435.png]]
![[Pasted image 20230926112442.png]]
![[Pasted image 20230926112449.png]]
![[Pasted image 20230926112455.png]]
![[Pasted image 20230926112501.png]]
#### 配置父工程 `pom.xml`
配置父工程 `pom.xml`, 作为聚合其它模块
```xml
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <junit.version>4.12</junit.version>
    <log4j.version>2.17.1</log4j.version>
    <lombok.version>1.18.20</lombok.version>
    <mysql.version>5.1.47</mysql.version>
    <druid.version>1.1.17</druid.version>
    <mybatis.spring.boot.version>2.2.0</mybatis.spring.boot.version>
  </properties>
  <!-- 配置依赖及版本-->
  <dependencyManagement>
    <dependencies>
      <!-- 配置SpringBoot -->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.2.2.RELEASE</version>
        <!-- 通过 pom + import 解决maven单继承机制 -->
        <!-- 表示父项目的子模块,在引入SpringBoot相关依赖时,锁定版本为2.2.2.RELEASE -->
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!-- 配置SpringCloud -->
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Hoxton.SR1</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!-- 配置SpringCloud Alibaba -->
      <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-alibaba-dependencies</artifactId>
        <version>2.1.0.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!-- mysql -->
      <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>${mysql.version}</version>
      </dependency>
      <!-- druid -->
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>${druid.version}</version>
      </dependency>
      <!-- mybatis -->
      <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>${mybatis.spring.boot.version}</version>
      </dependency>
      <!-- log4j -->
      <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>${log4j.version}</version>
      </dependency>
      <!-- junit -->
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
        <scope>test</scope>
      </dependency>
      <!-- lombok -->
      <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>${lombok.version}</version>
      </dependency>
    </dependencies>
  </dependencyManagement>
```
删除不需要的 build 和 reporting 节点
#### 注意事项和细节
1. Maven 使用 dependencyManagement 元素来提供了一种管理依赖版本号的方式。通常在项目 packaging 为 `pom`, 中使用 `dependencvManadement` 元素。
2. 使用 `pom.xml` 中的 `dependencyManagement` 元素能让所有在子项目中引用一个依赖 , Maven 会沿着父子层次向上走，直到找到一个拥有 `dependencyManagement` 元素的项目，然后它就会使用这个 `dependencyManagement` 元素中指定的版本号。
3. 好处∶ 
	- 如果有多个子项目都引用同一样依赖，则可以避免在每个使用的子项目里都声明一个版本号，当升级或切换到另一个版本时，只需要在顶层父容器里更新，而不需要分别在子项目的修改;
	- 另外如果某个子项目需要另外的一个版本，只需要声明 version 就可
4. `dependencyManagement` 里只是声明依赖，并不实现引入，因此子项目需要显示的声明需要用的依赖。
5. 如果不在子项目中声明依赖，是不会从父项目中继承下来的; 只有在子项目中写了该依赖项，并且没有指定具体版本，才会从父项目中继承该项，并且 version 和 scope 都读取自父 pom
6. 如果子项目中指定了版本号，那么会使用子项目中指定的 jar 版本

![[Pasted image 20230926113821.png]]
### 3.2 创建会员中心微服务模块
  ```xml
    <dependencies>
    <!-- 引入sleuth +zipkin依赖 -->
    <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-zipkin</artifactId>
    </dependency>
    <!-- eureka-client -->
    <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- SpringBoot监控系统,可以实现系统的健康检测 -->
    <!--  访问 http://localhost:10000/actuator 可以看到相关链接, 还可以做相关设置.-->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    <dependency>
      <groupId>org.mybatis.spring.boot</groupId>
      <artifactId>mybatis-spring-boot-starter</artifactId>
    </dependency>
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid-spring-boot-starter</artifactId>
      <version>1.1.17</version>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <optional>true</optional>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
    </dependency>
    <dependency>
      <groupId>org.example</groupId>
      <artifactId>e_commerce_center-common-api</artifactId>
      <version>${project.version}</version>
    </dependency>
  </dependencies>
  ``` 
## 四、SpringCloud Eureka 服务注册与发现
会员中心提供服务往往是一个集群，也就是说会有多个会员中心-提供服务微服务模块
当服务消费方，发现了可以使用的服务后 (可能是多个，又存在一个问题就是到底调用 A 服务，还是 B 服务的问题，这就引出了服务注册和负载均衡)
### 4.1 Eureka 介绍
![[Pasted image 20230926163826.png]]
- 解读:
	- 会员中心-提供服务的和 Eureka Server, 在项目中做集群、提供高可用
	- Eureka 包含两个组件: Eureka Server 和 Eureka Client
	- Eureka Server 提供注册服务
		- 各个微服务节点通过配置启动后, 会在 Eureka Server 中进行注册
		- 这样 Eureka Server 中的服务注册表中将会存储所有可用服务节点的信息
	- Eureka Client 通过注册中心进行访问, 是一个客户端
		- 用于简化 Eureka Server 的交互
			- 客户端同时也具备一个内置的、使用轮询(round-robin) 负载算法的负载均衡器
		- 在应用启动后, 将会想 Eureka Server 发送心跳(默认周期为 30s)
		- 如果 Eureka Server 在多个心跳周期内没有接收到某个节点的心跳
			- Eureka Server 将会从服务注册表中把整个服务节点移除 (默认 90s)

### 4.2 Eureka 实现服务治理
[分布式开发](https://jingyan.baidu.com/article/46650658def479f549e5f83e.html)
在传统的 rpc 远程调用框架中，管理每个服务与服务之间依赖关系比较复杂，管理困难，所以需要治理服务之间依赖关系
服务治理实现服务调用、负载均衡、容错等，实现服务发现与注册
![[Eureka服务注册与发现.svg]]
- Eureka 采用了 CS 的设计架构, Eureka Server 作为服务注册功能的服务器, 它是服务注册中心.
- 系统中的其他微服务, 使用 Eureka 的客户端连接到 Eureka Server 并维持心跳连接
	- 通过 Eureka Server 来监控系统中各个微服务是否正常运行
- 在服务注册与发现中, 有一个注册中心. 当服务器启动时, 会把当前自己服务器的信息
	- 例如: 服务地址通讯地址等以别名方式注册到注册中心上
- 服务消费者 or 服务提供者, 以服务别名的方式去注册中心上获取实际的服务提供者通讯地址, 然后通过 RPC 调用服务

### 4.3 Eureka 自我保护模式
#### 自我保护模式理论
1. 在默认情况下， Eureka 启动了自我保护模式
2. 自我保证机制/模式说明
	- 默认情况下 Eureka Client 定时向 Eureka Server 发送心跳包
	- 如果 Eureka 在 server 端在一定时间内 (默认 90s) 没有收到 Eureka Client 发送心跳包
		- 便会直接从服务注册列表中剔除该服务
	- 如果 Eureka 开启了自我保护模式/机制, 那么在短时间内丢失了大量的服务实例心跳
		- 这时候 Eureka Server 会开启自我保护机制, 不会剔除该服务
			- 该现象可能出现如果网络不通或者阻塞
		- 因为客户端还能正常发送心跳, 只是网络延迟问题, 而保护机制是为了解决次问题而产生的
	- 自我保护是<font color="#0070c0">属于 CAP 里面的 AP 分支</font>,保证高可用和分区容错性
	- 自我保护模式是一种应对网络异常的安全保护措施.
		- 它的架构哲学是
			- <font color="#0070c0">宁可同时保留所有微服务（健康的微服务和不健康的微服务都会保留）也不盲目注销任何健康的微服务。</font>
		- 使用自我保护模式，可以让 Eureka 集群更加的健壮、稳定。[CAP理论详解](https://blog.csdn.net/wangliangluang/article/details/120626014)

### 4.4 搭建 Eureka Server 集群
![[Pasted image 20230926174818.png]]
>[!note]- 1. 新建 `e-commerce-eureka-server-9001` 模块, 添加依赖
> ```xml
>   <dependencies>
>     <dependency>
>       <groupId>org. springframework. cloud</groupId>
>       <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
>     </dependency>
>     <dependency>
>       <groupId>org. springframework. boot</groupId>
>       <artifactId>spring-boot-starter-actuator</artifactId>
>     </dependency>
>     <dependency>
>       <groupId>org. projectlombok</groupId>
>       <artifactId>lombok</artifactId>
>       <optional>true</optional>
>     </dependency>
>     <dependency>
>       <groupId>org. example</groupId>
>       <artifactId>e_commerce_center-common-api</artifactId>
>       <version>${project. version}</version>
>     </dependency>
>   </dependencies>
> ```

>[!note]- 2. 添加 `application.yml`
>```yaml
>server:
>   port: 9001
> # 配置 eureka-server 的基本信息
> eureka:
>   instance:
>     hostname: eureka 9001. com # eureka 服务端的实例名称
>   client:
>     register-with-eureka: false # 是否将自己注册到 eureka 服务端
>     fetch-registry: false # 表示自己是注册中心 (作用就是维护注册服务实例)，不需要去检索服务
>     service-url:
>       # 设置与 eureka server 交互的地址查询服务和注册服务都需要依赖这个地址
>       defaultZone: http://eureka9002.com:9002/eureka/
>```

>[!note]- 3. 添加启动类
> ```java
> @EnableEurekaServer
> @SpringBootApplication
> public class EurekaApplication9001 {
>   public static void main (String[] args) {
>     SpringApplication.run(EurekaApplication9001.class, args);
>   }
> }
> ```

### 4.5 获取服务注册信息-DiscoveryClient
![[Pasted image 20230926175015.png]]
```java
import org.springframework.cloud.client.discovery.DiscoveryClient;

@RestController
@RequestMapping("/member/consumer")
@Slf4j
public class MemberConsumerController {
  @Resource
  private RestTemplate restTemplate;
  @Resource
  private DiscoveryClient discoveryClient;

  // 定义 member_service_provider_url 基础url地址
  public static final String MEMBER_SERVICE_PROVIDER_URL = "http://MEMBER-SERVICE-PROVIDER";

  // 方法/接口
  @GetMapping("/find/{id}")
  public Result<?> query(@PathVariable Long id) {
    return restTemplate.getForObject(MEMBER_SERVICE_PROVIDER_URL + "/member/find/" + id, Result.class);
  }

  @PostMapping("/save")
  public Result<?> save(Member member) {
    log.info("service-consumer-member: {}", member);
    return restTemplate.postForObject
        (MEMBER_SERVICE_PROVIDER_URL + "/member/save", member, Result.class);
  }

  @GetMapping("/discovery")
  public Object discovery() {
    List<String> services = discoveryClient.getServices();
    for (String service : services) {
      log.info("服务名={}", service);
      List<ServiceInstance> instances = discoveryClient.getInstances(service);
      for (ServiceInstance instance : instances) {
        log.info("服务名={}, 主机名={}, 端口号={}, url={}",
            instance.getServiceId(), instance.getHost(), instance.getPort(), instance.getUri());
      }
    }
    return discoveryClient;
  }
}
```
## 五、SpringCloud Ribbon
### 5.1 Ribbon 介绍
1. Spring Cloud Ribbon 是基于 Netflix Ribbon 实现的一套客户端负载均衡的工具
2. Ribbon 主要功能是提供客户端负载均衡算法和服务调用
3. Ribbon 客户端组件提供一系列完善的配置项如连接超时，重试等。
4. Ribbon 会基于某种规则（如简单轮询，随机连接等）去连接指定服务
5. 一句话: Ribbon: 负载均衡+RestTemplate 调用
6. Ribbon 目前进入维护模式, 未来替换方案是 Spring Cloud LoadBalancer
### 5.2 LB (Load Balance)
1. 集中式 LB
	- 即在服务的消费方和提供方之间使用独立的 LB 设施（可以是硬件，如 F 5，也可以是软件，如 Nginx），由该设施负责把访问请求通过某种策略转发至服务的提供方;
	- LB (Load Balance 负载均衡)
2. 进程内 LB
	- 将 LB 逻辑集成到消费方，消费方从服务注册中心获知有哪些服务地址可用，然后再从这些地址中选择出一个合适的服务地址。
	- <font color="#0070c0">Ribbon 就属于进程内 LB</font>，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址
### 5.3 Ribbon 原理
![[Pasted image 20230926175705.png]]
- Ribbon 机制
	- 先选择 Eureka Server，它优先选择在同一个区域内负载较少的 server
	- 再根据用户指定的策略，在从 server 取到的服务注册列表中选择一个地址
	- Ribbon 提供了多种策略∶ 比如轮询、随机和根据响应时间加权。
### 5.4 Ribbon 常见负载算法
![[Pasted image 20230926175859.png]]
替换负载均衡算法-应用实例
```java
@Configuration
public class RibbonRule {
  // 配置负载均衡算法
  @Bean
  public IRule myRibbonRule() {
    return new com.netflix.loadbalancer.RandomRule();
  }
}
```
## 六、SpringCloud OpenFeign
### 6.1 OpenFeign 介绍
1. [OpenFeign](https://github.com/spring-cloud/spring-cloud-openfeign) 是个声明式 WebService 客户端，使用 OpenFeign 让编写 Web Service 客户端更简单
2. 它的使用方法是定义一个服务接口然后在上面添加注解
3. OpenFeign 也支持可拔插式的编码器和解码器。
4. Spring Cloud 对 OpenFeign 进行了封装使其支持了 Spring MVC 标准注解和 HttpMessageConverters
5. OpenFeign 可以与 Eureka 和 Ribbon 组合使用以支持负载均衡
### 6.2 Feign 和 OpenFeign 区别
- Feign
	- Feign 是 Spring Cloud 组件中的一个轻量级 RESTful 的 HTTP 服务客户端
	- <font color="#ff0000">Feign 内置了 Ribbon</font>，用来做客户端负载均衡，去调用服务注册中心的服务。
	- Feign 的使用方式是：使用 Feign 的注解定义接口，调用服务注册中心的服务
	- Feign 本身<font color="#ff0000">不支持 Spring MVC 的注解</font>，它有一套自己的注解
	![[Pasted image 20230927172803.png]]
- OpenFeign
	- OpenFeign 是 Spring Cloud 在 Feign 的基础上支持了 Spring MVC 的注解
	- OpenFeign 的 `@FeignClient` 可以解析 SpringMVC 的 `@RequestMapping` 注解下的接口
	- OpenFeign 通过动态代理的方式产生实现类，实现类中做负载均衡并调用其他服务
	![[Pasted image 20230927172921.png]]
### 6.3 OpenFeign-应用实例
![[Pasted image 20230927173042.png]]
![[Pasted image 20230927175718.png]]
1. 创建服务消费模块-通过 OpenFeigen 实现远程调用 
	- 创建 `member-service-consumer-openfeign-80`
2. 修改 `pom.xml`
	```xml
	    <!-- openfeign -->
	    <dependency>
	      <groupId>org.springframework.cloud</groupId>
	      <artifactId>spring-cloud-starter-openfeign</artifactId>
	    </dependency>
	```
3. 添加 `application.yml`
	```yml
	server:
	  port: 8080
	spring:
	  application:
	    name: e-commerce-consumer-openfeign-8080
	eureka:
	  client:
	    register-with-eureka: true
	    fetch-registry: true
	    service-url:
	      defaultZone: http://eureka9001.com:9001/eureka,http://eureka9002.com:9002/eureka
	logging:
	  level:
	    com.study.www.client.MemberFeignClient: debug
	```
4. 启动类
	```java
	@SpringBootApplication
	@EnableEurekaClient
	@EnableFeignClients // 开启OpenFeign
	public class MemberConsumerOpenfeignApplication {
	  public static void main(String[] args) {
	    SpringApplication.run(MemberConsumerOpenfeignApplication.class, args);
	  }
	}
	```
5. `controller`
	```java
	@RestController
	@RequestMapping("/member/consumer/feign")
	public class MemberConsumerFeignController {
	  @Resource
	  private MemberFeignClient memberFeignClient;
	
	  @GetMapping("/find/{id}")
	  public Result<?> getMemberById(@PathVariable("id") Long id) {
	    return memberFeignClient.query(id);
	  }
	}
	```
6. `client`
	```java
	@Component
	@FeignClient("MEMBER-SERVICE-PROVIDER")
	public interface MemberFeignClient {
	  /**
	   * 1. 远程调用的方式是Get
	   * 2. 调用的地址是 http://MEMBER-SERVICE-PROVIDER/member/find/{id}
	   * 3. MEMBER-SERVICE-PROVIDER 是服务提供者的服务名,是在Eureka注册中心注册的服务
	   */
	  @GetMapping("/member/find/{id}")
	  Result<?> query(@PathVariable("id") Long id);
	}
	```
### 6.4 日志配置
Feign 提供了日志打印功能，可以通过配置来调整日志级别，从而对 Feign 接口的调用情况进行监控和输出
- 日志级别
	- NONE∶ 默认的，不显示任何日志
	- BASIC∶ 仅记录请求方法、URL、响应状态码及执行时间;
	- HEADERS∶ 除了 BASIC 中定义的信息之外, 还有请求和响应的头信息
	- FULL∶ 除了 HEADERS 中定义的信息之外，还有请求和响应的正文及元数据。

- 配置日志-应用实例
	- `config`
		```java
		@Configuration
		public class OpenFeignConfig {
		  @Bean
		  public Logger.Level feignLoggerLevel() {
		    return Logger.Level.FULL;
		  }
		}
		```
	- `application.yml`
		```yml
		logging:
		  level:
		    com.study.www.client.MemberFeignClient: debug
		```

### 6.5 OpenFeign 超时
![[Pasted image 20230930103313.png]]

## 七、SpringCloud Gateway
- [Gateway](https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.1.RELEASE/reference/html/) 是什么
	- Gateway 是在 Spring 生态系统之上构建的 API 网关服务
		- 基于 Spring ，Spring Boot 和 Project Reactor 等技术。
	- Gateway 旨在提供一种简单而有效的方式来对 API 进行路由，以及提供一些强大的过滤器功能
		- 例如∶熔断、限流、重试等
- Gateway 核心功能
	- 鉴权
	- 流量控制
	- 熔断
	- 日志监控
	- 反向代理
- Gateway 特性
	- 动态路由
	- 可以对路由指定 Predicate (断言)和 Filter (过滤器) 
	- 集成 Hystrix 的断路器功能
	- 集成 Spring Cloud 服务发现功能
	- 请求限流功能
	- 支持路径重写

![[Pasted image 20230930103641.png]]

### 7.1 Gateway 和 Zuul 区别
1. SpringCloud Gateway 作为 Spring Cloud 生态系统中的网关，目标是替代 Zuul
2. SpringCloud Gateway 是基于 Spring WebFlux 框架实现的
3. Spring WebFlux 框架底层则使用了高性能的 Reactor 模式通信框架 Netty，提升了网关性能

### 7.2 Gateway 基本原理
#### Gateway 核心组件
![[Pasted image 20230930104704.png]]
- 解读:
	- web 请求，通过一些匹配条件，定位到真正的服务节点/微服务模块，在这个转发过程的前后，进行一些精细化控制
	- predicate: 就是匹配条件
	- filter: 可以理解为是网关的过滤机制。有了 predicate 和 filter，再加上目标 URL. 就可以实现一个具体的路由
##### Route (路由)
路由是构建网关的基本模块，它由 ID，目标 URI，一系列的断言和过滤器组成，如果断言为 true 则匹配该路由
##### Predicate (断言)
对 HTTP 请求中的所有内容（例如请求头或请求参数）进行匹配，如果请求与断言相匹配则进行路由

- Spring Cloud Gateway 包括<font color="#ff0000">许多内置的 Route Predicate 工厂</font>, 所有这些 Predicate 都与 HTTP 请求的不同属性匹配, 可以组合使用.
- Spring Cloud Gateway 创建 Route 对象时，使用 RoutePredicateFactory 创建 Predicate 对象，Predicate 对象可以赋值给 Route。
- 所有这些谓词都匹配 HTTP 请求的不同属性。多种谓词工厂可以组合
##### Filter (过滤)
使用过滤器，可以在<font color="#ff0000">请求被路由前或者之后</font>对请求进行处理
路由过滤器可用于修改进入的 HTTP 请求和返回的 HTTP 响应
Spring Cloud Gateway 内置了多种路由过滤器，他们都由 GatewayFilter 的工厂类来产生 
开发直接使用 GatewayFilter 较少，一般是自定义过滤器
```java
@Slf4j
@Component
public class CustomGateWayFilter implements GlobalFilter, Ordered {
  @Resource
  private JwtUtils jwtUtils;
  private final String[] allowedPaths = {"/member/login", "/member/save"};


  // 过滤器的业务逻辑
  @Override
  public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
    log.info("====== 进入 CustomGateWayFilter ======");

    ServerHttpRequest request = exchange.getRequest();
    ServerHttpResponse response = exchange.getResponse();

    String requestPath = request.getPath().toString();
    log.info("requestPath: {}", requestPath);
    boolean allowedPath = Arrays.asList(allowedPaths).contains(requestPath);
    if (allowedPath) {
      return chain.filter(exchange);
    }

    String token = request.getHeaders().getFirst("Authorization");
    if (token == null || token.isEmpty()) {
      return writeResponse(response, "401", "请先登录");
    }
    int verification = jwtUtils.verification(token);
    if (JwtConstant.TOKEN_EXPIRED == verification) {
      return writeResponse(response, "401", "登录已过期!");
    }
    if (JwtConstant.TOKEN_ABNORMAL == verification) {
      return writeResponse(response, "401", "身份异常!");
    }

    log.info("登录成功，token: {}", token);
    return chain.filter(exchange);
  }

  // 过滤器的优先级，数字越小，优先级越高
  @Override
  public int getOrder() {
    return 0;
  }

  protected Mono<Void> writeResponse(ServerHttpResponse response, String code, String msg) {
    JSONObject message = new JSONObject();
    message.set("code", code);
    message.set("message", msg);
    byte[] bytes = message.toJSONString(4).getBytes(StandardCharsets.UTF_8);
    DataBuffer buffer = response.bufferFactory().wrap(bytes);
    response.getHeaders().add("Content-Type", "application/json;charset=UTF-8");
    response.setStatusCode(HttpStatus.OK);
    return response.writeWith(Mono.just(buffer));
  }
}
```
#### 工作机制
![[Pasted image 20230930105350.png]]
1. 客户端向 Spring Cloud Gateway 发出请求。然后在 Gateway Handler Mapping 中找到与请求相匹配的路由，将其发送到 Gateway Web Handler。
2. Handler 再通过指定的过滤器链来将请求发送到我们实际的服务执行业务逻辑，然后返回
3. 过滤器可以在发送代理请求之前和之后运行相关逻辑，在 pre 前置过滤器执行后发出代理请求，代理请求执行后, 再执行 POST 后置过滤器，在没有端口的路由中定义的 URI、HTTP 和 HTTPS URI 的默认端口值 80 和 443
4. 过滤器之间用虚线分开是因为过滤器可能会在发送代理请求之前（" pre" ）或之后 （" post" ）执行业务逻辑。
5. Filter 在"pre"类型的过滤器可以做参数校验、权限校验、流量监控、日志输出、协议转换等，
6. 在"post" 类型的过滤器中可以做响应内容、响应头的修改，日志的输出，流量监控等有着非常重要的作用。
### 7.3 搭建 Gateway 微服务
![[Pasted image 20230930110409.png]]
1. 通过网关暴露的接口，实现调用真正的服务
2. 网关本身也是一个微服务模块

- 创建 `e-commerce-gateway-20000`,添加 `pom` 依赖
	```xml
	  <dependencies>
	    <!-- gateway启动器 -->
	    <dependency>
	      <groupId>org.springframework.cloud</groupId>
	      <artifactId>spring-cloud-starter-gateway</artifactId>
	    </dependency>
	    <!-- eureka-client -->
	    <dependency>
	      <groupId>org.springframework.cloud</groupId>
	      <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
	    </dependency>
	    <!--
	      与spring-boot-starter-gateway冲突
	      <dependency>
	        <groupId>org.springframework.boot</groupId>
	        <artifactId>spring-boot-starter-web</artifactId>
	      </dependency>
	      <dependency>
	        <groupId>org.springframework.boot</groupId>
	        <artifactId>spring-boot-starter-actuator</artifactId>
	      </dependency>
	    -->
	    <dependency>
	      <groupId>org.projectlombok</groupId>
	      <artifactId>lombok</artifactId>
	      <optional>true</optional>
	    </dependency>
	    <dependency>
	      <groupId>cn.hutool</groupId>
	      <artifactId>hutool-all</artifactId>
	      <version>5.8.11</version>
	    </dependency>
	    <dependency>
	      <groupId>org.example</groupId>
	      <artifactId>e_commerce_center-common-api</artifactId>
	      <version>${project.version}</version>
	      <exclusions>
	        <exclusion>
	          <groupId>org.springframework.boot</groupId>
	          <artifactId>spring-boot-starter-web</artifactId>
	        </exclusion>
	      </exclusions>
	    </dependency>
	  </dependencies>
	```
- 创建 `application.yml`,添加路由信息
	```yml
		server:
		  port: 20000
		spring:
		  application:
		    name: e-commerce-gateway
		  cloud:
		    gateway:
		      discovery:
		        locator:
		          enabled: true # 开启从注册中心动态创建路由的功能，利用微服务名进行路由
		      routes: # 路由配置
		#        - id: e-commerce-product-service  # 路由的id，没有规定规则但要求唯一，建议配合服务名
		#          uri: lb://e-commerce-product-service  # 匹配后提供服务的路由地址, lb://表示从注册中心获取服务列表
		#          predicates: # 断言，路径相匹配的进行路由
		#            - Path=/product-service/**  # 断言，路径相匹配的进行路由, 匹配失败返回404
		#          filters: # 过滤器，可以对请求或响应做一些处理
		#            - StripPrefix=1  # StripPrefix=1，表示去掉请求路径的第一部分
		#            - AddRequestHeader=X-Request-red, blue # 添加请求头
		#            - AddResponseHeader=X-Response-foo, bar # 添加响应头
		#            - StripPrefix=1 # 去掉请求路径的第一部分
		#            - RewritePath=/member/find/(?<segment>.*), /member/find/${segment} # 重写路径
		#            - Hystrix=memberHystrix # 开启熔断降级
		#            - SetPath=/member/find # 设置路径
		#            - SetStatus=500 # 设置响应状态码
		#            - SaveSession # 保存session
		#            - RemoveRequestHeader=X-Request-red # 删除请求头
		#            - RemoveResponseHeader=X-Response-foo # 删除响应头
		#            - RequestRateLimiter=redisRateLimiter # 限流
		#            - RedirectTo=302, http://www.baidu.com # 重定向
		#            - Retry=5 # 重试
		        - id: e-commerce-member-service-01
		          uri: lb://MEMBER-SERVICE-PROVIDER
		          predicates:
		            - Path=/member/find/**
		          filters:
		            - AddRequestParameter=color, red # 添加请求参数
		        - id: e-commerce-member-service-02
		          uri: lb://MEMBER-SERVICE-PROVIDER
		          predicates:
		            - Path=/member/save, /member/login
		            - After=2023-09-22T17:54:48.849+08:00[Asia/Shanghai] # 只有在指定时间之后的请求才会被路由
		            #- Cookie=key1, abc 
		            #- Header=X-Request-Id, hello 
		            #- Host=**.hspedu.** 
		            #- Method=GET 
		            - Query=email, [\w-]+@([a-zA-Z]+\.)+[a-zA-Z]+
		#        - id: e-commerce-member-service-03
		#          uri: http://www.baidu.com
		#          predicates:
		#            - Path=/
		eureka:
	  instance:
	    hostname: e-commerce-gateway
	  client:
	    register-with-eureka: true
	    fetch-registry: true
	    service-url:
	      defaultZone: http://eureka9001.com:9001/eureka,http://eureka9002.com:9002/eureka
	```
- 创建 `GateWayApplication2000` 启动类
	```java
	@SpringBootApplication
	@EnableEurekaClient
	public class GateWayApplication2000 {
	  public static void main(String[] args) {
	    SpringApplication.run(GateWayApplication2000.class, args);
	  }
	}
	```
#### 编写配置类注入 route
```java
@Configuration
public class GateWayRoutesConfig {
  @Bean
  public RouteLocator routeLocator(RouteLocatorBuilder builder) {
    return builder.routes()
        .route("e-commerce-member-service-04",
            r -> r.path("/member/find/**")
                .uri("lb://MEMBER-SERVICE-PROVIDER"))
        .build();
  }
}
```
#### 配置负载均衡算法
```java
@Configuration
public class GateWayRibbonRule {
  @Bean
  public IRule ribbonRule() {
    return new RandomRule();
  }
}
```
## 八、SpringCloud Sleuth+Zipkin
### 8.1 Sleuth/Zipkin 是什么
1. 在微服务框架中，一个由客户端发起的请求在后端系统中会经过多个不同的的服务节点调用, 来协同产生最后的请求结果，每一个请求都会形成一条复杂的分布式服务调用链路
2. 链路中的任何一环出现高延时或错误都会引起整个请求最后的失败, 因此对整个服务的调用进行链路追踪和分析就非常的重要

Sleuth 提供了一套完整的服务跟踪的解决方案并兼容 Zipkin
Sleuth 做链路追踪 , Zipkin 做数据搜集/存储/可视化

### 8.2 Sleuth 工作原理
1. Span 和 Trace 在一个系统中使用 Zipkin 的过程-图形化
	![[Pasted image 20230930113921.png]]
	- 表示一请求链路，一条链路通过 Trace Id 唯一标识， Span 标识发起的请求信息，各 span 通过 parent id 关联起来
	- Trace：类似于树结构的 Span 集合，表示一条调用链路，存在唯一标识
	- Span：基本工作单元，表示调用链路来源，通俗的理解 span 就是一次请求信息
2. spans 的 parent/child 关系图形化
	![[Pasted image 20230930114104.png]]
	- span 就是一次请求信息
	- 多个 Span 集合就构成一条调用链路
	- 在 span=C 这个节点存在分支
### 8.3 搭建链路监控实例
1. 安装/使用 [Zipkin](https://repo1.maven.org/maven2/io/zipkin/java/zipkin-server/2.12.9)
2. 执行 jar `java -jar zipkin-server-2.12.9-exec.jar` 启动, [访问](http://localhost:9411/zipkin/)
4. 服务提供方集成 Sleuth/Zipkin
	- 添加依赖
		```xml
		    <!-- 引入sleuth +zipkin依赖 -->
		    <dependency>
		      <groupId>org.springframework.cloud</groupId>
		      <artifactId>spring-cloud-starter-zipkin</artifactId>
		    </dependency>
		```
	- 补充 `application.yml` 配置
		```yml
		spring:
		  application:
		    name: member-service-provider
		  # 配置sleuth
		  zipkin:
		    base-url: http://localhost:9411
		  # 配置zipkin
		  sleuth:
		    sampler:
		      # 采样率,在0-1之间,1表示采集全部
		      probability: 1
		```
## 九、SpringCloud Alibaba Nacos
一句话: Nacos 就是注册中心(替代 Eureka)、配置中心(替代 Config)
[Nacos](https://github.com/alibaba/nacos/releases/tag/1.2.1)：Dynamic Naming and Configuration Service, 架构理论基础: CAP 理论 (支持 AP 和 CP, 可以切换)
解压运行 `sh startup.sh -m standalone`, [访问](http://localhost:8848/nacos)

### 9.1 创建 Nacos 服务提供者
![[Pasted image 20230930180746.png]]
创建 `member-service-nacos-provider-10004` 模块, 添加 `nacos` 场景启动器
```xml
    <!-- 引入nacos -->
    <dependency>
      <groupId>com.alibaba.cloud</groupId>
      <artifactId>spring-cloud-alibaba-nacos-discovery</artifactId>
    </dependency>
```
创建 `application.yml`,添加配置 
```yml
server:
  port: 10004
spring:
  application:
    name: member-service-nacos-provider
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: org.gjt.mm.mysql.Driver
    url: jdbc:mysql://localhost:3306/e_commerce_center_db?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: 123456
  # 配置nacos
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 # 配置nacos server的地址
# 配置暴露所有的监控点
management:
  endpoints:
    web:
      exposure:
        include: '*'
mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.study.www.entity
```
启动类 `MemberNacosProviderApplication10004`
```java
@EnableDiscoveryClient // 开启nacos服务注册发现功能
@SpringBootApplication
public class MemberNacosProviderApplication10004 {
  public static void main(String[] args) {
    SpringApplication.run(MemberNacosProviderApplication10004.class, args);
  }
}
```
### 9.2 创建 Nacos 服务消费者
- 创建模块, 添加依赖
	```xml
	  <dependencies>
	    <!--引入 alibaba-nacos-->
	    <dependency>
	      <groupId>com.alibaba.cloud</groupId>
	      <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
	    </dependency>
	    <dependency>
	      <groupId>org.springframework.boot</groupId>
	      <artifactId>spring-boot-starter-web</artifactId>
	    </dependency>
	    <dependency>
	      <groupId>org.springframework.boot</groupId>
	      <artifactId>spring-boot-starter-actuator</artifactId>
	    </dependency>
	    <dependency>
	      <groupId>org.projectlombok</groupId>
	      <artifactId>lombok</artifactId>
	      <optional>true</optional>
	    </dependency>
	    <dependency>
	      <groupId>org.example</groupId>
	      <artifactId>e_commerce_center-common-api</artifactId>
	      <version>${project.version}</version>
	    </dependency>
	  </dependencies>
	```
- 创建 `application.yml`,添加配置信息
	```yml
	server:
	  port: 8080
	spring:
	  application:
	    name: member-service-nacos-consumer-8080
	  cloud: # 服务注册中心
	    nacos:
	      discovery:
	        server-addr: localhost:8848 # nacos服务注册中心地址
	```
### 9.3 Nacos AP 和 CP 切换
![[Pasted image 20231002105603.png]]
- 选择 AP 还是 CP?
	- CP: 服务可以不能用，但必须要保证数据的一致性
	- AP: 数据可以短暂不一致，但最终是需要一致的，无论如何都要保证服务的可用
	- 取舍：只能在 CP 和 AP 选择一个平衡点, 大多数都是选择 AP 模式
- [AP 和 CP 切换](https://www.jianshu.com/p/c56e22c222bb)
	- Nacos 集群默认支持的是 CAP 原则中的 AP 原则，但是也可切换为 CP 原则 (<font color="#ff0000">一般不切换</font>)
	- CURL 切换命令: `curl -X PUT '$NACOS_SERVER:8848/nacos/v1/ns/operator/switches?entry=serverMode&value=CP'`
	- URL 指令：`$NACOS_SERVER:8848/nacos/v1/ns/operator/switches?entry=serverMode&value=CP`

### 9.4 Nacos 配置中心实例
![[Pasted image 20231002163715.png]]

- 在 Nacos Server 加入配置
	1. 进入到 Nacos Server
	2. 加入配置，`文件后缀.yaml`

![[Pasted image 20231002173159.png]]

- 创建 `e-commerce-nacos-config-client5000` 模块, 添加依赖
	```xml
		<!-- 引入 nacos-config 场景启动器 -->
	    <dependency>
	      <groupId>com.alibaba.cloud</groupId>
	      <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
	    </dependency>
	    <!--引入 alibaba-nacos-->
	    <dependency>
	      <groupId>com.alibaba.cloud</groupId>
	      <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
	    </dependency>
	```
- 创建 `application.yml`
	```yml
	spring:
	  profiles:
	    active: dev # 指定当前环境为dev[dev/test/prod]
	```
- 创建 `bootstrap.yml`
	1. nacos 配置客户端/当前的微服务模块, 会根据配置, 找到配置中心的数据 (配置文件)
	2. `config.server-addr: localhost:8848` 可以找到配置中心
	3. `spring.application.name` 对应是 `DataId: e-commerce-nacos-config`
	4. 在 `application.yml` 配置 `spring.profiles.active: dev`
	5. `spring.cloud.nacos.config.file-extension` 配置文件的拓展名 `.yaml`
	6. 小结: 根据配置就是到 `localhost:8848` 下的 `e-commerce-nacos-config-dev.yaml` 获取配置信息/数据
	7. 规则就是: `${spring.application.name}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}` 来定位配置中心的 Data ID
		- `e-commerce-nacos-config-client-dev.yaml`
	```yml
	server:
	  port: 5000
	spring:
	  application:
	    # 指定当前应用名称(需要参考nacos配置中心的Data Id)
	    name: e-commerce-nacos-config-client
	    # 配置nacos
	  cloud:
	    nacos:
	      discovery:
	        server-addr: localhost:8848 # 指定nacos服务地址
	      config:
	        server-addr: localhost:8848 # 指定nacos服务地址
	        file-extension: yaml # 指定配置文件格式
	```
- 创建 `NacosConfigClientController`
	```java
	@RestController
	@Slf4j
	public class NacosConfigClientController {
	  /**
	   * 1. client会拉取nacos server的e-commerce-nacos-config-client-dev.yaml
	   *  config:
	   *    ip: "122.11.22.22"
	   *    name: "fishx_study"
	   * 2. @Value("${config.ip}") 会从nacos server的e-commerce-nacos-config-client-dev.yaml中获取ip的值
	   */
	  @Value("${config.ip}")
	  private String configId;
	  @Value("${config.name}")
	  private String configName;
	
	  @GetMapping("/nacos/config")
	  public String getConfig() {
	    log.info("configId: {}, configName: {}", configId, configName);
	    return "configId: " + configId + ", configName: " + configName;
	  }
	}
	```
- 创建启动类 `NacosConfigClientApplication5000`
	```java
	@SpringBootApplication
	@EnableDiscoveryClient
	public class NacosConfigClientApplication5000 {
	  public static void main(String[] args) {
	    SpringApplication.run(NacosConfigClientApplication5000.class, args);
	  }
	}
	```
- 注意事项和细节
	- 配置文件 `application.yml` 和 `bootstrap.yml` 结合会得到配置文件/资源的地址
	- [参考文档](https://nacos.io/zh-cn/docs/quick-start-spring-cloud.html)
	![[Pasted image 20231003093400.png]]
	- 注意在 Nacos Server 的配置文件的后缀是 `.yaml` , 而不是 `.yml`
	- Springboot 中配置文件的加载是存在优先级顺序的, `bootstrap.yml` 优先级高于 `application.yml`
	- `@RefreshScope` 是 Springcloud 原生注解，实现配置信息自动刷新, 如果在 NacosServer 修改了配置数据，Client 端就会得到最新配置

### 9.5 Nacos 分类配置 (实现配置隔离)
#### DataID 方案
![[Pasted image 20231003095025.png]]
1. 在 nacos server 创建新的配置：`e-commerce-nacos-config-client-test.yaml`
	![[Pasted image 20231003095048.png]]
	![[Pasted image 20231003095112.png]]
	![[Pasted image 20231003095125.png]]
2. 修改 `application.yml`
	```yml
	spring:
	  profiles:
	    active: test # 指定当前环境为dev[dev/test/prod]
	```
#### Group 方案
![[Pasted image 20231003095237.png]]
1. 在 nacos server 创建新的配置：`order/e-commerce-nacos-config-client-dev.yaml`
	![[Pasted image 20231003095313.png]]
	![[Pasted image 20231003100436.png]]
2. 在 `bootstrap.yml` 修改配置
	```yml
	server:
	  port: 5000
	spring:
	  application:
	    # 指定当前应用名称(需要参考nacos配置中心的Data Id)
	    name: e-commerce-nacos-config-client
	    # 配置nacos
	  cloud:
	    nacos:
	      discovery:
	        server-addr: localhost:8848 # 指定nacos服务地址
	      config:
	        server-addr: localhost:8848 # 指定nacos服务地址
	        file-extension: yaml # 指定配置文件格式
	        group: order # 指定配置分组,默认为DEFAULT_GROUP
	```
#### Namespace 方案
新建命名空间 `baidu`、`alibaba`
![[Pasted image 20231003102058.png]]
新建配置 `seckill/e-commerce-nacos-config-client-dev.yaml`、`search/e-commerce-nacos-config-client-dev.yaml`
![[Pasted image 20231003102909.png]]
![[Pasted image 20231003103247.png]]
修改 `application.yml`
```yml
spring:
  profiles:
    active: dev # 指定当前环境为dev[dev/test/prod]
```
修改 `bootstrap.yml`
```yml
server:
  port: 5000
spring:
  application:
    # 指定当前应用名称(需要参考nacos配置中心的Data Id)
    name: e-commerce-nacos-config-client
  # 配置nacos
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 # 指定nacos服务地址
      config:
        server-addr: localhost:8848 # 指定nacos服务地址
        file-extension: yaml # 指定配置文件格式
        group: search # 指定配置分组,默认为DEFAULT_GROUP
        namespace: 1aab4981-7acf-412e-9f45-a6b85e0218b3 # 指定命名空间id,默认为public
```
#### Namespace/Group/DataID 关系
![[Pasted image 20231003133301.png]]
- Nacos 默认的命名空间是 public，Namespace 主要用来实现配置隔离, 隔离范围大
- Group 默认是 DEFAULT GROUP，Group 可以把不同的微服务划分到同一个分组里面去
- Service 就是微服务, 相同的 Service 可以是一个 Cluster (簇/集群) , Instance 就是微服务的实例

## 十、SpringCloud Alibaba Sentinel
 <a href='https://github.com/alibaba/Sentinel/wiki/%E4%BB%8B%E7%BB%8D'><img src="https://user-images.githubusercontent.com/9434884/43697219-3cb4ef3a-9975-11e8-9a9c-73f4f537442d.png" alt="Sentinel Logo" height="50%" width="50%"></a>
 Sentinel 的主要特性
 ![[Pasted image 20231003135303.png]]
 Sentinel 的开源生态
 ![[Pasted image 20231003141327.png]]
 >[!note] 一句话: Sentinel: 分布式系统的流量防卫兵, 保护你的微服务
 
 ### 10.1 Sentinel 核心功能
- 流量控制
	- 只卖 N 张票，就是一种限流的手段
- 熔断降级
	- 在调用系统的时候，如果调用链路中的某个资源出现了不稳定，最终会导致请求发生堆积
		- 熔断降级可以解决这个问题
			- 所谓的熔断降级就是当检测到调用链路中某个资源出现不稳定的表现
				- 例如请求响应时间长或异常比例升高的时候，则对这个资源的调用进行限制，让请求快速失败，避免影响到其它的资源而导致级联故障
- 系统负载保护
	- 根据系统能够处理的请求，和允许进来的请求，来做平衡
		- 追求的目标是在系统不被拖垮的情况下, 提高系统的吞吐率
- 消息削峰填谷
	- 某瞬时来了大流量的请求, 而如果此时要处理所有请求，很可能会导致系统负载过高，影响稳定性。但其实可能后面几秒之内都没有消息投递，若直接把多余的消息丢掉则没有充分利用系统处理消息的能力
	- Sentinel 的 Rate Limiter 模式能在某一段时间间隔内以匀速方式处理这样的请求, 充分利用系统的处理能力, 也就是削峰填谷, 保证资源的稳定性

### 10.2 Sentinel 两个组成部分
- 核心库：（Java 客户端）不依赖任何框架/库，能够运行在所有 Java 运行时环境, 对 SpringCloud 有较好的支持
- 控制台: (DashBoard)基于 SpringBoot 开发, 打包后可以直接运行, 不需要额外的 Tomcat 等应用容器

### 10.3 Sentinel 控制台
下载 `sentinel-dashboard-1.8.0.jar`, 启动 `java -jar sentinel-dashboard-1.8.0.jar`, [访问](http://localhost:8080/)
更改端口 `java -jar sentinel-dashboard-1.8.0.jar --server.port=9999`

### 10.4 Sentinel 监控微服务
使用 Sentinel 控制台对 `member-service-nacos-provider-10004` 微服务进行实时监控
![[Pasted image 20231003173012.png]]
`pom.xml` 添加依赖
```xml
	<!-- 引入alibaba-sentinel 场景启动器 -->
    <dependency>
      <groupId>com.alibaba.cloud</groupId>
      <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
    </dependency>
```
修改 `application.yml`
```yml
server:
  port: 10004
spring:
  application:
    name: member-service-nacos-provider
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: org.gjt.mm.mysql.Driver
    url: jdbc:mysql://localhost:3306/e_commerce_center_db
    username: root
    password: 123456
  # 配置nacos
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 # 配置nacos server的地址
    sentinel:
      transport:
        dashboard: localhost:8080 # 配置sentinel的dashboard地址
        # 解读 spring.cloud.sentinel.transport.port
        # 1. spring.cloud.sentinel.transport.port 端口配置会在被监控的微服务
        #     对应的机器上启动一个 Http Server
        # 2. 该 Server 会与 Sentinel 控制台做交互
        # 3. 比如 Sentinel 控制台添加了 1 个限流规则，会把规则数据 push 给这个
        #     Http Server 接收，Http Server 再将规则注册到 Sentinel 中
        # 4. 简单的说明: spring.cloud.sentinel.transport.port：指定被监控的微服务应用与
        # Sentinel 控制台交互的端口，微服务应用本地会起一个该端口占用的 Http Server
        port: 8719 # 配置sentinel的端口,假如被占用了, 会自动从 8719 开始依次+1 扫描。直至找到未被占用的端口
# 配置暴露所有的监控点
management:
  endpoints:
    web:
      exposure:
        include: '*'
mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.study.www.entity
```
访问接口, 进入到 Sentinel 查看实时监控效果
![[Pasted image 20231004092258.png]]
- 细节
	- QPS: Queries Per Second (每秒查询率)，是服务器每秒响应的查询次数
	- Sentinel 采用的是懒加载，只有调用了某个接口/服务，才能看到监控数据

### 10.5 Sentinel 流量控制
![[Pasted image 20231004092522.png]]
- 解读
	- 资源名∶唯一名称，默认请求路径
	- 针对来源∶Sentinel 可以针对调用者进行限流，填写微服务名，默认 default（不区分来源）
	- 阈值类型/单机阈值∶
		- QPS（每秒钟的请求数量）
			- 当调用该 api 的 QPS 达到阈值的时候，进行限流线程数
			- 当调用该 api 的线程数达到阈值的时候，进行限流
	- 流控模式∶ 
		- 直接∶ api 达到限流条件时，直接限流
		- 关联∶ 当关联的资源达到阈值时，就限流自己
		- 链路∶ 当从某个接口过来的资源达到限流条件时，开启限流
	- 流控效果∶
		- 快速失败∶ 直接失败，抛异常
		- Warm Up∶ 根据 codeFactor（冷加载因子，默认 3）的值，从阈值/codeFactor，经过预热时长，才达到设置的 QPS 阈值
		- 排队等待∶ 匀速排队，让请求以匀速的速度通过，阈值类型必须设置为 QPS，否则无效
- QPS 和线程数的区别?
	- 比如 QPS 和线程我们都设置阈值为 1
	- 对 QPS 而言, 如果在 1 秒内, 客户端发出了 2 次请求, 就达到阈值, 从而限流
	- 对线程数而言, 如果在 1 秒内, 客户端发出了 2 次请求, 不一定达到线程限制的阈值, 为什么呢? 
		- 假设我们 1 次请求后台会创建一个线程, 但是这个请求完成时间是 0.1 秒 (可以视为该请求对应的线程存活 0.1 秒)
			- 所以当客户端第 2 次请求时 (比如客户端是在 0.3 秒发出的), 这时第 1 个请求的线程就已经结束了
				- 因此就没有达到线程的阈值, 也不会限流
	- 如果 1 个请求对应的线程平均执行时间为 0.1 那么, 就相当于 QPS 为 10

#### 流量控制实例-QPS
当调用 `member-service-nacos-provider-10004` 的 `/member/find/1` 接口/API 时，限制 1 秒内最多访问 1 次，否则直接失败，抛异常.
![[Pasted image 20231004103435.png]]
![[Pasted image 20231004144114.png]]
- 注意事项和细节
	- 流量规则改动，实时生效，不需重启微服务 , Sentine 控制台
	- 在 sentinel 配置流量规则时, 如何配置通配符问题
		- 方案 1: 
			- 比如 `/member/find?id=1`、`/member/find?id=2` 统一使用一个规则 `/member/find` 限流.
		- 方案 2: 
			- 可以通过 UrlCleaner 接口来实现资源清洗，也就是对于 `/member/find/{id}` 这个 URL
			- 可以统一归集到 `/member/find/*` 资源下，具体配置代码如下，实现 UrlCleaner 接口，并重写 clean 方法即可
			```java
			@Component
			public class CustomUrlCleaner implements UrlCleaner {
			  @Override
			  public String clean(String originUrl) {
			    if (StrUtil.isBlank(originUrl)) {
			      return originUrl;
			    }
			    if (originUrl.startsWith("/member/find/")) {
			      // 将 /member/find/1 -> /member/find/*
			      // 1. 给sentinel放回资源名为 /member/find/*
			      // 2. 在sentinel控制台配置资源名为 /member/find/*,添加流控规则即可
			      return "/member/find/*";
			    }
			    return originUrl;
			  }
			}
			```
		- 如果 sentinel 流控规则没有持久化，当我们重启调用 API 所在微服务模块后，规则会丢失，需要重新加入

#### 流量控制实例-线程数
需求: 通过 Sentinel 实现流量控制
当调用 `member-service-nacos-provider-10004` 的 `/member/find/*` 接口/API 时，限制只有一个工作线程，否则直接失败，抛异常.

1. 为 `/member/find/*` 增加流控规则
	![[Pasted image 20231006200948.png]]
2. 在流控规则菜单，可以看到新增的流控规则
3. 访问 `http://localhost:10004/member/find/6`,同时多次刷新
	![[Pasted image 20231006201059.png]]

##### 阈值类型 QPS 和线程数的区别讨论
- 如果一个线程<font color='red'>平均执行时间</font>为 0.05 秒，就说明在 1 秒钟，可以执行 20 次 (<font color="#ff0000">相当于</font> QPS 为 20)
- 如果一个线程<font color='red'>平均执行时间</font>为 1 秒, 说明 1 秒钟，可以执行 1 次数 (<font color="#ff0000">相当于</font> QPS 为 1)
- 如果一个线程<font color='red'>平均执行时间</font>为 2 秒, 说明 2 秒钟内，才能执行 1 次请求

#### 流量控制实例-关联
>[!note] 关联: 当关联的资源达到阈值时，就限流自己

1. 新增接口 `/member/t1`、`/member/t2`
![[Pasted image 20231006203728.png]]
2. 添加流控规则
![[Pasted image 20231006203631.png]]
3. 压测接口 `/member/t2`,访问 `/member/t1`
![[Pasted image 20231006203817.png]]
![[Pasted image 20231006204057.png]]

#### 流量控制实例-Warm up
- [Warm up 介绍](https://github.com/alibaba/Sentinel/wiki/%E9%99%90%E6%B5%81---%E5%86%B7%E5%90%AF%E5%8A%A8)
	- 当流量突然增大的时候，我们常常会希望系统从空闲状态到繁忙状态的切换的时间长一些。
		- 即如果系统在此之前长期处于空闲的状态，希望处理请求的数量是缓步的增多，经过预期的时间以后，到达系统处理请求个数的最大值。
			- Warm Up (冷启动，预热)模式就是为了实现这个目的的。
	- 这个场景主要用于启动需要额外开销的场景
		- 例如建立数据库连接等
	- 应用场景: 秒杀在开启瞬间，大流量很容易造成冲垮系统，Warmup 可慢慢的把流量放入，最终将阀值增长到设置阀值

![[Pasted image 20231006204547.png]]
- 通常冷启动的过程系统允许通过的 QPS 曲线图 (上图)
- 默认 coldFactor(冷启动启动加载因子) 为 3
	- 即请求 QPS 从 threshold / 3 开始，经预热时长逐渐升至设定的 QPS 阈值
		- 这里的 threshold 就是最终要达到的 QPS 阈值.
---
- 通过 Sentinel 实现流量控制, 演示 Warm up
	- 调用 `member-service-nacos-provider-10004` 的 `/member/t2` API 接口
		- 将 QPS 设置为 9, 设置 Warm up 值为 3
			![[Pasted image 20231006205934.png]]
	- 含义为请求 `/member/t2` 的 QPS 从 threshold / 3 ( 9 /3 = 3) 开始
		- 经预热时长 (3 秒)逐渐升至设定的 QPS 阈值 (9)
	- 测试预期效果: 
		- 在前 3 秒，如果访问 `/member/t2` 的 QPS 超过 3, 会直接报错
		- 在 3 秒后访问 `/member/t2` 的 QPS 超过 3, 小于等于 9, 是正常访问 (<font color="#ff0000">QPS>9</font>)
			![[Pasted image 20231006210202.png]]
	- 一段时间没有达到阈值, Warm up 过程将重复

#### 流量控制实例-排队
>[!note] 排队方式：这种方式严格控制了请求通过的间隔时间，也即是让请求以均匀的速度通过，对应的是漏桶算法

![[Pasted image 20231006210439.png]]
- 这种方式主要用于处理间隔性突发的流量
	- 例如消息队列
		- 比如这样的场景:
			- 在某一秒有大量的请求到来，而接下来的几秒则处于空闲状态
			- 我们希望系统能够在接下来的空闲期间逐渐处理这些请求，而不是在第一秒直接拒绝多余的请求。
	- 类似前面说的削峰填谷

匀速排队，阈值必须设置为 QPS
- 通过 Sentinel 实现流量控制-排队
	- 调用 `member-service-nacos-provider-10004` 的 `/member/t2` API 接口，将 QPS 设置为 1
		![[Pasted image 20231006211908.png]]
	- 当调用 `/member/t2` 的 QPS 超过 1 时，不拒绝请求，而是排队等待, 依次执行
	- 当等待时间超过 10 秒，则为等待超时 (限流)
		![[Pasted image 20231006212145.png]]

### 10.6 Sentinel 熔断降级
>[!note]- 线程堆积引出熔断降级
> - 一个服务常常会调用别的模块，可能是另外的一个远程服务、数据库，或者第三方 API
>	- 例如:
>		- 支付的时候，可能需要远程调用银联提供的 API
>		- 查询某个商品的价格，可能需要进行数据库查询
>	- 然而，这个被依赖服务的稳定性是不能保证的。
>	- 如果依赖的服务出现了不稳定的情况，请求的响应时间变长，那么调用服务的方法的响应时间也会变长，线程会产生堆积，最终可能耗尽业务自身的线程池，服务本身也变得不可用

对<font color="#ff0000">不稳定的服务进行[熔断降级](https://sentinelguard.io/zh-cn/docs/circuit-breaking.html) </font>，让其快速返回结果，不要造成线程堆积

![[Pasted image 20231007083702.png]]
- 解读:
	- 现代微服务架构都是分布式的，由非常多的服务组成。不同服务之间相互调用，组成复杂的调用链路
	- 链路调用中会产生放大的效果。复杂链路上的某一环不稳定，就可能会层层级联，最终导致整个链路都不可用
	- 因此需要对不稳定的弱依赖服务调用进行熔断降级，暂时切断不稳定调用，避免局部不稳定因素导致整体的雪崩

#### 熔断, 降级, 限流三者的关系
- 熔断强调的是服务之间的调用能实现自我恢复的状态
- 限流是从系统的流量入口考虑, 从进入的流量上进行限制, 达到保护系统的作用
- 降级, 是从系统业务的维度考虑，流量大了或者频繁异常, 可以牺牲一些非核心业务，保护核心流程正常使用
>[!note] 总结
> - 熔断是降级方式的一种
> - 降级又是限流的一种方式
> - 三者都是为了通过一定的方式在流量过大或者出现异常时, 保护系统的手段

#### 熔断策略
##### 慢调用比例
1. 慢调用比例 (SLOW_REQUEST_RATIO)：
	- 选择以慢调用比例作为阈值,需要设置允许的慢调用 RT (即最大的响应时间)，请求的响应时间大于该值则统计为慢调用
2. 当单位统计时长 (statIntervalMs)内请求数目大于设置的最小请求数目，并且慢调用的比例大于阈值，则接下来的熔断时长内请求会自动被熔断
	- `QPS` > `最小QPS` && `慢调用的比例` > `阈值`
		- 慢调用比例: 需要设置 `RT(最大响应时间)`、`请求的响应时间` > `RT`
3. 熔断时长后, 熔断器会进入探测恢复状态 (HALF-OPEN 状态)，若接下来的一个请求响应时间小于设置的慢调用 RT 则结束熔断，若大于设置的慢调用 RT 则会再次被熔断
4. 配置参考
	![[Pasted image 20231010193034.png]]
5. 示例
	- 当调用 `member-service-nacos-provider-10004` 的 `/member/t3` API 接口时
		- 如果在 1 s 内持续进入了 5 个请求，并且请求的平均响应时间超过 200 ms, 
			- 那么就在未来 10 秒钟内，断路器打开, 让 `/member/t3` API 接口微服务不可用
	```java
	  @GetMapping("/t3")
	  public Result<String> t3() {
	    // 慢调用 -> 平均响应慢 => 休眠
	    try {
	      TimeUnit.MILLISECONDS.sleep(300);
	    } catch (InterruptedException e) {
	      throw new RuntimeException(e);
	    }
	    return Result.success("Enter t3()...");
	  }
	```
	![[Pasted image 20231010195428.png]]
	当 `QPS >= 5` && `慢调用的比例` (`请求的响应时间` > `RT`) `> 0%` 时, 发生熔断
	平均响应时间超出阈值且在 1 s 内通过的请求>=5，两个条件同时满足后触发降级
	熔断时间过后，关闭断路器，访问恢复正常
##### 异常比例
1. 异常比例 (ERROR_RATIO)：
	- 当单位统计时长 (statIntervalMs)内请求数目大于设置的最小请求数目，并且异常的比例大于阈值，则接下来的熔断时长内请求会自动被熔断
		- 满足在单位时间内 `QPS` > `最小QPS` && `异常比例` > `阈值` 接下来的 `熔断时长内的请求` 发生自动熔断
2. 经过熔断时长后熔断器会进入探测恢复状态 (HALF-OPEN 状态)
3. 若接下来的一个请求成功完成 (没有错误)则结束熔断, 否则会再次被熔断 
4. 异常比率的阈值范围是 `[0.0, 1.0]`
	- 代表 0% - 100% 
5. 配置参考
	![[Pasted image 20231010193057.png]]
6. 工作示意图
	![[Pasted image 20231010193210.png]]
7. 示例

##### 异常数
1. 异常数 (ERROR_COUNT)：当单位统计时长内的异常数目超过阈值之后会自动进行熔断
	- `异常数` > `阈值`
2. 经过熔断时长后熔断器会进入探测恢复状态 (HALF-OPEN 状态)
3. 若接下来的一个请求成功完成 (没有错误)则结束熔断，否则会再次被熔断
4. 配置参考
	![[Pasted image 20231010193338.png]]


