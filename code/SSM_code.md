# 项目全局配置 web.xml
```xml
<web-app>  
  <display-name>Archetype Created Web Application</display-name>  
  <!--
  一、配置启动Spring容器:  
    主要配置和业务相关的,比如数据源、事务控制等  
  -->  
  <context-param>  
    <param-name>contextConfigLocation</param-name>  
    <param-value>classpath:applicationContext.xml</param-value>  
  </context-param>  
  
  <!-- 四、配置字符编码,解决中文乱码 -->  
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
  
  <!--  
  五、配置HiddenHttpMethodFilter,使用Rest风格的url  
    作用：把以POST方式提交的delete和put请求进行转换  
  -->  
  <filter>  
    <filter-name>hiddenHttpMethodFilter</filter-name>  
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>  
  </filter>  
  <filter-mapping>  
    <filter-name>hiddenHttpMethodFilter</filter-name>  
    <url-pattern>/*</url-pattern>  
  </filter-mapping>  
  
  <!--  
  二、创建侦听器自动装配ApplicationContext:  
    1. ContextCleanupListener 侦听器  
      (作用：启动web容器时自动装配ApplicationContext的配置信息)  
    2. 它实现了ServletContextListener接口,  
      在web.xml配置该监听器,启动容器时,会默认执行它实现的方法  
  -->  
  <listener>  
    <listener-class>org.springframework.web.context.ContextCleanupListener</listener-class>  
  </listener>  
  
  <!--  
  三、配置中央控制器/分发控制器  
  没有指定SpringMVC配置文件,因此按默认${servletName}-servlet.xml匹配  
  -->  
  <servlet>  
    <servlet-name>dispatcher</servlet-name>  
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
    <!-- 在web项目启动时,就自动加载DispatcherServlet -->  
    <load-on-startup>1</load-on-startup>  
  </servlet>  
  <servlet-mapping>  
    <servlet-name>dispatcher</servlet-name>  
    <url-pattern>/</url-pattern>  
  </servlet-mapping>  
</web-app>
```
# SpringMVC 配置 
`dispatcher-servlet.xml`
```xml
<!--配置自动扫描包-->  
<context:component-scan base-package="cn.furn">  
  <!--只配置标注有@Controller的类,只包括控制器-->  
  <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>  
</context:component-scan>  
  
<!--配置视图解析器-->  
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">  
  <!--配置属性prefix和suffix-->  
  <property name="prefix" value="/WEB-INF/views/"/>  
  <property name="suffix" value=".html"/>  
</bean>  
  
<!--  
两个常规配置:  
  1. 将SpringMVC不能处理的请求交给Tomcat  
  2. 开启SpringMVC高级功能(JSR303校验...)  
-->  
<mvc:default-servlet-handler/>  
<mvc:annotation-driven/>
```

# Spring 配置文件
**Spring 的配置文件** `resources/applicationContext.xml`
主要配置和业务逻辑有关的，比如数据源，事务控制等
```xml
<context:component-scan base-package="cn.furn">  
  <!--不扫描控制器-->  
  <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>  
</context:component-scan>  
  
<!--引入properties配置文件-->  
<context:property-placeholder location="classpath:mysql.properties"/>  
<!--配置数据源对象-DataSource Druid数据源-->  
<bean class="com.alibaba.druid.pool.DruidDataSource" id="druidDataSource">  
  <property name="username" value="${jdbc.user}"/>  
  <property name="password" value="${jdbc.password}"/>  
  <property name="driverClassName" value="${jdbc.driver}"/>  
  <property name="url" value="${jdbc.url}"/>  
</bean>  
  
<!--配置spring与mybatis整合-->  
<bean class="org.mybatis.spring.SqlSessionFactoryBean" id="sessionFactory">  
  <!--指定mybatis全局配置文件-->  
  <property name="configLocation" value="classpath:mybatis-config.xml"/>  
  <!--指定数据源-->  
  <property name="dataSource" ref="druidDataSource"/>  
  <!--  
  指定mybatis的Mapper文件[Mapper.xml]位置  
    在开发中，通常将mapper.xml放在类路径：resources/mapper  
  -->  
  <property name="mapperLocations" value="classpath:mapper/*.xml"/>  
</bean>  
  
<!--配置扫描器,将mybatis接口的实现加入IOC容器中-->  
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">  
  <!--扫描所有的dao接口的实现,加入ioc容器-->  
  <property name="basePackage" value="cn.furn.dao"/>  
</bean>  
  
<!--配置事务管理器-->  
<bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" id="transactionManager">  
  <property name="dataSource" ref="druidDataSource"/>  
</bean>  
  
<!--开启基于注解的事务并指定切入点-->  
<aop:config>  
  <!-- 切入点表达式 -->  
  <aop:pointcut id="txPoint" expression="execution(* cn.furn.service..*(..))"/>  
  <!-- 配置事务增强/规则: 使用txAdvice 指定规则对 txPoint进行切入-->  
  <aop:advisor advice-ref="txAdvice" pointcut-ref="txPoint"/>  
</aop:config>  
<!-- 配置事务增强【指定事务规则】，也就是指定事务如何切入-->  
<tx:advice id="txAdvice">  
  <tx:attributes>  
    <!-- *代表所有方法都是事务方法-->  
    <tx:method name="*"/>  
    <!-- 以get开始的所有方法 ，我们认为是只读，进行调优-->  
    <tx:method name="get*" read-only="true"/>  
  </tx:attributes>  
</tx:advice>
```

# MyBatis 逆向工程生成
- 创建表，使用逆向工程生成 `Bean`、`XxxMapper` 和 `XxxMapper.xml`
	- 根路径创建 `mbg.xml`
		```xml
		<?xml version="1.0" encoding="UTF-8"?>  
		<!DOCTYPE generatorConfiguration  
		    PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"  
		    "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">  
		<generatorConfiguration>  
		  <context id="DB2Tables" targetRuntime="MyBatis3">  
		    <!--生成没有注释的bean-->  
		    <commentGenerator>  
		      <property name="suppressAllComments" value="true"/>  
		    </commentGenerator>  
		    <!--配置数据库连接信息-->  
		    <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"  
		                    connectionURL="jdbc:mysql://localhost:3306/furn_ssm?characterEncoding=utf8"  
		                    userId="root"  
		                    password="admin">  
		    </jdbcConnection>  
		    <javaTypeResolver>  
		      <property name="forceBigDecimals" value="false"/>  
		    </javaTypeResolver>  
		    <!--指定javaBean生成的位置-->  
		    <javaModelGenerator targetPackage="cn.furn.entity" targetProject="./src/main/java">  
		      <property name="enableSubPackages" value="true"/>  
		      <property name="trimStrings" value="true"/>  
		    </javaModelGenerator>  
		    <!--指定sql映射文件生成的位置,要根据自己的实际情况指定-->  
		    <sqlMapGenerator targetPackage="mapper" targetProject="./src/main/resources">  
		      <property name="enableSubPackages" value="true"/>  
		    </sqlMapGenerator>  
		    <!--指定dao接口生成的位置, 也就是mapper接口-->  
		    <javaClientGenerator type="XMLMAPPER" targetPackage="cn.furn.dao" targetProject="./src/main/java">  
		      <property name="enableSubPackages" value="true"/>  
		    </javaClientGenerator>  
		    <!--指定要逆向生成的表和生成策略-->  
		    <table tableName="furn" domainObjectName="Furn"/>  
		  </context>  
		</generatorConfiguration>
		```
	- 编写测试方法, 生成对应文件
		```java
		public void generator() {	
			List<String> warnings = new ArrayList<>();  
			boolean flag = true;  
			File configFile = new File("mbg.xml");  
			ConfigurationParser cp = new ConfigurationParser(warnings);  
			Configuration config = cp.parseConfiguration(configFile);  
			DefaultShellCallback callback = new DefaultShellCallback(flag);  
			MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);  
			myBatisGenerator.generate(null);  
			if (warnings.isEmpty()) {  
			  System.out.println("文件生成成功！");  
			} else {  
			  for (String warning : warnings) {  
			    System.out.println(warning);  
			  }  
			}
		}
		```
	- 踩坑: `Mybatis-Generator根据配置文件生成的XxxMapper.xml,有两个<resultMap>节点`
		- 删除其一即可