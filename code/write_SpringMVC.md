- 自己实现 SpringMVC 底层机制 
	- 核心分发
	- 控制器 Controller 和 Service 注入容器
	- 对象自动装配 
	- 控制器方法获取参数
	- 视图解析
	- 返回 JSON 格式数据 

![[SpingMVC 执行流程.excalidraw | 880]]
# 任务一、开发 F_DispatcherServlet  

说明: 编写 F_DispatcherServlet 充当原生的 DispatcherServlet (即核心控制器)

1. 创建 `com.springmvc.F_DispatcherServlet.java` 并继承 `HttpServlet`
2. 创建空白文件 `springmvc.xml`
3. 配置 `F_DispatcherServlet` 作为中央控制器
	1. 给 `F_DispatcherServlet` 配置参数, 指定要操作的 Spring 容器配置文件 
		- key: `contextConfigLocation` value: `springmvc.xml`
	2. 首先自动加载
4. 配置 Tomcat

目标: 启动服务, 发送请求 `F_DispatcherServlet` 有输出即可

# 任务二、完成浏览器可以请求控制层
## 1. 创建 Controller 和自定义注解
- com **项目结构**
	- springmvc **模拟 SprinMVC 框架支持**
		- annotation
			- `@Controller`
			- `@RequestMapping`
		- servlet
			- `F_DispatcherServlet.java`
	- controller **业务组件**
		- `MonsterHandler.java`
## 2. 配置 `springmvc.xml`
指定要扫描的基本包及子包的 java 类 `<component-scan base-package="com.controller"/>` 
## 3. 编写 `XMLParser` 工具类去解析 xml 
1. 通过类加载器以流形式获取资源,得到输入流
2. 获取 SAXReader 解析器, 并调用 read 方法, 得到 document 根节点
3. 通过根节点获取 component-scan 节点, 然后通过 attributeValue 属性值
## 4. 开发 `WebApplicationContext` 充当 Spring 容器
开发 `WebApplicationContext`，充当 Spring 容器, 得到扫描类的全路径列 
## 5. 完善 Spring 容器-实例化对象到容器中
完善 `WebApplicationContext` Spring 容器, 实例化对象到容器中
## 6. 完成请求 url 和控制器方法的映射关系
## 7. 完成 `F_DispatcherServlet` 分发请求到对应控制器方法