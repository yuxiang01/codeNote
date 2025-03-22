>[!tip]- SSM 家居项目
> - 整合 SSM (Spring、SpringMVC、MyBatis)
> - 前端 vue3+TypeScript、后端 SSM 
> - IDEA 版本: 2023.1

# 一、项目基础、环境搭建
>[!note]- 1.1 项目创建
![[Pasted image 20230402174746.png | 650]]

>[!note]- 1.2 完善目录
创建 `java`、`test` 相关目录
![[Pasted image 20230402202244.png]]

>[!note]- 1.3 引入 jar 包
> 1. 引入 `SpringMVC` 依赖
> 	![[Pasted image 20230402181749.png]]
> 2. 引入 `Spring jdbc` 支持事务相关
> 	![[Pasted image 20230402234224.png]]
> 3. 引入 `Spring aspect` 切面编程所需依赖
> 	![[Pasted image 20230402182200.png]]
> 4. 引入 `Mybatis` 依赖和 `Druid` 德鲁伊连接池
> 	![[Pasted image 20230402182809.png]]
> 5. 引入 `Mysql` 数据库连接驱动
> 	![[Pasted image 20230402183349.png]]

>[!info]- 为什么不用引入 Spring?
> - 当我们引入 `SpringMvc` 时, Maven 会关联引入 `Spring` 
> - 因为 `SpringMvc` 依赖于 `Spring` 

>[!note]- 1.4 配置 Tomcat 服务
> ![[Pasted image 20230402184131.png]]

>[!note]- 1.5 项目全局配置 `web.xml` 
> 1. 配置启动 Spring 容器
> 	- 主要配置和业务相关
> 		- 例如数据源、事务控制等
> 		![[Pasted image 20230402210943.png]]
> 		classpath: 是一个前缀, 表示从类路径下加载资源
> 		![[Pasted image 20230402200801.png]]
> 2. 配置 SpringMvc 中央控制器
> 	![[Pasted image 20230402204401.png]]
> 3. 配置 characterEncodingFilter 
> 	- 过滤器设置编码 (需要放置在所有过滤器之前)
> 	![[Pasted image 20230402210357.png]]
> 4. 配置 HiddenHttpMethodFilter
> 	- 支持 Rest 风格的 url 
> 	- 作用：把以 POST 方式提交的 delete 和 put 请求进行转换 
> 	![[Pasted image 20230402211928.png]]
> 5. [[SSM_code#项目全局配置 web.xml|完整配置]] 

>[!note]- 1.6 配置 SpringMVC `dispatcher-servlet.xml` 
> 1. 在 webapp/WEB-INF 文件夹下创建 `dispatcher-servlet.xml` SpringMVC 配置文件
> 2. 创建相关的包
> 	![[Pasted image 20230402214355.png]]
> 3. 配置扫描 `controller` 包, SpringMVC 只接管控制器
> 	![[Pasted image 20230402215154.png]]
> 4. 配置视图解析器
> 	![[Pasted image 20230402220143.png]]
> 5. 加入两个常规配置
> 	- `xmlns:mvc="http://www.springframework.org/schema/mvc"`
> 	![[Pasted image 20230402220726.png]]
> 6. [[SSM_code#SpringMVC 配置|完整配置]]

> [!note]- 1.7 配置 Spring `applicationContext.xml`
> 1. 创建 spring 的配置文件 `resources/applicationContext.xml`
> 	- 控制器由 SpringMVC 管理
> 	![[Pasted image 20230402223322.png]]
> 2. 创建 `resources/mysql.properties`
> 	- 数据库配置信息
> 		 ```properties
> 		 jdbc.userName=root
> 		 jdbc.password=admin
> 		 jdbc.driverClass=com.mysql.cj.jdbc.Driver
> 		 jdbc.url=jdbc:mysql://localhost:3306/furn_ssm?useSSL=true&useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
> 		 ```
> 3. 配置数据源 `applicationContext.xml` 
> 	![[Pasted image 20230402230005.png]]
> 4. 整合 Spring 和 Mybatis 
> 	1. 引入适配包
> 		![[Pasted image 20230402230608.png]]
> 	2. 配置与 mybatis 的整合
> 		![[Pasted image 20230402231614.png]]
> 	3. 配置扫描器, 将 Mybatis 接口的实现加入 IOC 容器中
> 		- mapper 接口放在 `com.fishx.furn.dao` 中
> 			- 因为 Mybatis 就是 DAO 层
> 			![[Pasted image 20230402232249.png]]
> 5. 配置事务管理器-对象
> 	![[Pasted image 20230402234515.png]]
> 6. 开启基于注解的事务并指定切入点
>	![[Pasted image 20230402235719.png]]
> 7. [[SSM_code#Spring 配置文件|Spring完整配置]]


>[!note]- 1.8 配置 Mybatis `mybatis-config.xml`
> 1. 配置日志输出 (查看 sql 语句)
> 	![[Pasted image 20230403085050.png]]
> 2. 配置包路径的别名
> 	![[Pasted image 20230403084125.png]]


>[!note]- 1.9 Mybatis 逆向工程生成 `Bean、XxxMapper、XxxMapper.xml` 
> 1. 引入 Mybatis 逆向工程依赖
> 	![[Pasted image 20230403084337.png]]
> 2. 项目下创建逆向工程所需[配置文件](https://mybatis.org/generator/configreference/xmlconfig.html) `mbg.xml`
> 	- [[SSM_code#MyBatis 逆向工程生成 | MyBatis 逆向工程完整配置]]


![[Pasted image 20230403010000.png]]

>[!note]- 2.0 Vue 3+TypeScript 项目搭建及环境配置
> - 具体搭建参考 [[Ts_Vue3_CMS项目]]
>	```shell
> 	## Vue 3
> 	npm init vite@latest
> 	## Ant Design Vue
> 	npm install --save @ant-design/icons-vue
>	```