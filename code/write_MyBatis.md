- 自己实现 MyBatis 底层机制
	- 封装 Sqlsession 到执行器 
	- Mapper 接口和 `Mapper. xml` 
	- MapperBean 
	- 动态代理 Mapper 的方法

# 阶段一、完成读取配置文件，得到数据库连接
说明: 通过配置文件，获取数据库连接
![[Pasted image 20230128095517.png]]

- 步骤
	- 创建 `F_Configuration.java` 用于读取 xml 文件, 建立连接.
		- 声明属性 `ClassLoader` 类加载器
		- 构建 `build()` 返回 `Connection`, 并获取 `saxRead` 解析器, 得到 root 节点 
			- `evalDataSource(Element root)` 获取 root 节点里的属性和值 
			- 并通过反射声明类, 返回连接

# 阶段二、编写执行器，输入 SQL 语句，完成操作
说明: 把对数据库的操作，会封装到一套 Executor 机制中，程序具有更好的扩展性, 结构更加清晰.  
![[Pasted image 20230128231818.png]]
- 具体实现
	- 新建 `实体类Monster`, 使用 lombok 注解辅助完成封装
	- 新建 `Executor接口` 和 `Executor实现类` 完成对数据库的操作方法
		- 得到连接, sql 预处理器, 返回结果集, 封装 javabean, 关闭相关资源

# 阶段三、将 Sqlsession 封装到执行器
搭建 Configuration (连接) 和 Executor 之间的桥梁 
![[Pasted image 20230128233932.png]]
![[Pasted image 20230128233939.png]]
- 具体实现
	- 新建 `F_SqlSession`, 声明属性「执行器和配置」
		- 编写方法, 返回一条记录
			- 核心还是调用的执行器的方法

# 阶段四、开发 Mapper 接口和对应 xml 
![[Pasted image 20230130210604.png]]

```java
com.entity.Monster
// 声明对db的CURD操作  
public interface MonsterMapper {  
  Monster getMonsterById(Integer id);  
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<mapper namespace="com.f_mybatis.mapper.MonsterMapper">  
  <select id="getMonsterById" resultType="com.entity.Monster">  
    SELECT * FROM monster WHERE id = ?  
  </select>  
	</mapper>``
```
# 阶段五、映射 Mapper 接口和 xml 文件
![[Pasted image 20230130210706.png]]
- 实现步骤
	- `config.Function.java` 配置方法参数
		```java
		@Setter  
		@Getter  
		@ToString  
		public class Function {  
		  private String sqlType;  
		  private String funcName;  
		  private String sql;  
		  private Object resultType;  
		  private String parameterType;  
		}
	   ```
	- `config/MapperBean.java` 
		```java
		private String interfaceName; // 接口全类名  
		private List<Function> functions; // 映射文件中所有方法
		```
	- 在 F_Configuration, 读取 `XxxMapper.xml`，能够创建 MappperBean 对象
		- `sqlsession/F_Configuration.java` 中增加 `readMapper(String path)` 
			- 负责读取、初始化参数并返回 `mapperBean` ·
# 阶段六、实现动态代理 Mapper 的方法

![[Pasted image 20230130213116.png]]

- 动态代理生成 Mapper 对象, 调用 F_Executor
	- `sqlsession/F_MapperProxy.java` 并实现 `InvocationHandler 接口`
		- 实现 `invoke()`
			- 判断是否为接口所对应 xml 文件
			- 取出 mapper 里的方法
			- 执行方法
	- 三属性: `sqlSession`、`mapperFile`、`configuration`