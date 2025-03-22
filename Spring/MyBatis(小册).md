## 一、概述与环境搭建
### 1.1 MyBatis 的整体架构
![[MyBatis框架.png]]
从整体上讲，MyBatis 的整体架构可以分为三层：
![[Pasted image 20230528215926.png]]
- 接口层：`SqlSession` 是我们平时与 MyBatis 完成交互的核心接口
	- （包括后续整合 SpringFramework 后用到的 `SqlSessionTemplate` ）；
- 核心层：`SqlSession` 执行的方法
	- 底层需要经过配置文件的解析、SQL 解析
	- 以及执行 SQL 时的参数映射、SQL 执行、结果集映射
	- 另外还有穿插其中的扩展插件
- 支持层：核心层的功能实现，是基于底层的各个模块，共同协调完成的

### 1.2 数据库初始化
> [!note]- 数据库初始化
> ```sql
> -- 创建数据库
> CREATE DATABASE brochure;
> -- 创建数据表
> CREATE TABLE department (
>   id varchar(32) NOT NULL,
>   name varchar(32) NOT NULL,
>   tel varchar(18) DEFAULT NULL,
>   PRIMARY KEY (id)
> ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
> -- 添加数据
> INSERT INTO department VALUES
> ('00000000000000000000000000000000', '全部部门', '-'),
> ('18ec781fbefd727923b0d35740b177ab', '开发部', '123'),
> ('53e3803ebbf4f97968e0253e5ad4cc83', '测试产品部', '789'),
> ('ee0e342201004c1721e69a99ac0dc0df', '运维部', '456');
> 
> CREATE TABLE user (
>   id varchar(32) NOT NULL,
>   version int(11) NOT NULL DEFAULT '0',
>   name varchar(32) NOT NULL,
>   age int(3) DEFAULT NULL,
>   birthday datetime DEFAULT NULL,
>   department_id varchar(32) NOT NULL,
>   sorder int(11) NOT NULL DEFAULT '1',
>   deleted tinyint(1) NOT NULL DEFAULT '0',
>   PRIMARY KEY (id)
> ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
> 
> INSERT INTO user(id, version, name, age, birthday, department_id, sorder, deleted) VALUES
> ('09ec5fcea620c168936deee53a9cdcfb', 0, '阿熊', 18, '2003-08-08 10:00:00', '18ec781fbefd727923b0d35740b177ab', 1, 0), 
> ('5d0eebc4f370f3bd959a4f7bc2456d89', 0, '老狗', 30, '1991-02-20 15:27:20', 'ee0e342201004c1721e69a99ac0dc0df', 1, 0);
> ```

### 1.3 Maven 项目搭建
>[!note]- `pom.xml` 添加依赖
> ```xml
> <dependencies>
>     <dependency>
>       <groupId>org.mybatis</groupId>
>       <artifactId>mybatis</artifactId>
>       <version>3.5.9</version>
>     </dependency>
>     <dependency>
>       <groupId>mysql</groupId>
>       <artifactId>mysql-connector-java</artifactId>
>       <version>8.0.32</version>
>     </dependency>
>     <dependency>
>       <groupId>log4j</groupId>
>       <artifactId>log4j</artifactId>
>       <version>1.2.12</version>
>     </dependency>
>   </dependencies>
> ```

> [!note]- `mybatis-config.xml` 配置相关信息
> ```xml
> <?xml version="1.0" encoding="UTF-8" ?>
> <!DOCTYPE configuration
>     PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
>     "http://mybatis.org/dtd/mybatis-3-config.dtd">
> <configuration>
>   <settings>
>     <setting name="logImpl" value="LOG4J"/>
>   </settings>
>   <typeAliases>
>     <package name="com.fishx.mybatis.entity"/>
>   </typeAliases>
>   <environments default="development">
>     <environment id="development">
>       <transactionManager type="JDBC"/>
>       <dataSource type="POOLED">
>         <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
>         <property name="url" value="jdbc:mysql://localhost:3306/brochure?characterEncoding=utf-8&amp;useUnicode=true"/>
>         <property name="username" value="root"/>
>         <property name="password" value="admin"/>
>       </dataSource>
>     </environment>
>   </environments>
>   <mappers>
>     <mapper resource="mapper/department.xml"/>
>   </mappers>
> </configuration>
> ```

> [!note]- `MyBatisUtils.java` 工具类用于返回 `SqlSession` 对象
> ```java
> public class MyBatisUtils {
>   private static SqlSessionFactory sqlSessionFactory;
> 
>   // 静态代码块,初始化SqlSessionFactory
>   static {
>     // 指定资源文件(配置文件)
>     String resource = "mybatis-config.xml";
>     try {
>       InputStream inputStream = Resources.getResourceAsStream(resource);
>       sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
>     } catch (IOException e) {
>       e.printStackTrace();
>     }
>   }
> 
>   // 返回SqlSession对象
>   public static SqlSession getSqlSession() {
>     return sqlSessionFactory.openSession();
>   }
> }
> ```
### 1.4 单表增删改查
>[!note]- 单表 CURD
> `department.xml` sql 映射
> ```java
> <?xml version="1.0" encoding="UTF-8" ?>
> <!DOCTYPE mapper
>     PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
>     "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
> <mapper namespace="com.fishx.mybatis.mapper.DepartmentMapper">
>   <select id="findAll" resultType="Department">
>     SELECT id, name, tel
>     FROM department
>   </select>
>   <insert id="insert" parameterType="Department">
>     INSERT INTO department
>     VALUES (#{id}, #{name}, #{tel})
>   </insert>
>   <delete id="remove" parameterType="String">
>     DELETE FROM department WHERE id = #{id}
>   </delete>
>   <update id="update" parameterType="Department">
>     UPDATE department SET name=#{name},tel=#{tel} WHERE id = #{id}
>   </update>
>   <select id="findById" resultType="Department">
>     SELECT id, name, tel
>     FROM department
>     WHERE id = #{id}
>   </select>
> </mapper>
> ```
> 
> ```java
> public class DepartmentTest {
>   private SqlSession sqlSession;
>   private DepartmentMapper departmentMapper;
> 
>   @Before
>   public void init() {
>     sqlSession = MyBatisUtils.getSqlSession();
>     departmentMapper = sqlSession.getMapper(DepartmentMapper.class);
>     System.out.println("departmentMapper: " + departmentMapper.getClass());
>   }
> 
>   @Test
>   public void list() {
>     List<`Department`> departmentList = departmentMapper.findAll();
>     departmentList.forEach(System.out::println);
>     if (sqlSession != null) sqlSession.close();
>   }
> }
> ```

### 1.5 关联表查询
#### 多对一
User 对 Department 多对一
关联多对一的查询，分两种情况：立即加载，延迟加载
>[!note]- 立即加载和延迟加载的区别: 
> - 立即加载
> 	- 两表联查
> - 延迟加载
> 	- 两次查询
> 		- 先查主表, 再拿关联字段, 去查级联数据
##### 立即加载
1. 添加 `User` 实体对象
	```java
	@Data
	public class User {
	  private String id;
	  private Integer version;
	  private String name;
	  private Integer age;
	  private Date birthday;
	  private String department_id;
	  private Integer sorder;
	  private Integer deleted;
	  // 持有 Department对象
	  private Department department;
	}
	```
2. 添加 `UserMapper` 接口
	```java
	public interface UserMapper {
	  List<User> findAll();
	}
	```
3. 添加与 `UserMapper` 接口对应的方法
	```xml
	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper
	    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<mapper namespace="com.fishx.mybatis.mapper.UserMapper">
	  <resultMap id="userMap" type="User">
	    <id property="id" column="id"/>
	    <result property="name" column="name"/>
	    <result property="age" column="age"/>
	    <result property="birthday" column="birthday"/>
	    <association property="department" javaType="Department">
	      <id property="id" column="department_id"/>
	      <result property="name" column="dep_name"/>
	    </association>
	  </resultMap>
	  <select id="findAll" resultMap="userMap">
	    SELECT user.*, department.`name` dep_name FROM user
	    LEFT JOIN department ON user.department_id = department.id;
	  </select>
	</mapper>
	```
4. 测试编码运行
	```java
	public class UserTest {
	  private SqlSession sqlSession;
	  private UserMapper userMapper;
	
	  @Before
	  public void init() {
	    sqlSession = MyBatisUtils.getSqlSession();
	    userMapper = sqlSession.getMapper(UserMapper.class);
	    System.out.println("departmentMapper: " + userMapper.getClass());
	  }
	
	  @Test
	  public void list() {
	    userMapper.findAll().forEach(System.out::println);
	    if (sqlSession != null) sqlSession.close();
	  }
	}
	```

 ##### 延迟加载 
`Department` 延迟加载 (两次 sql 查询) 
如果需要实现像 Hibernate 那样的关联对象延迟加载，可以通过修改 `resultMap` ，使其延迟加载
1. 接口
	```java
	public interface UserMapper {
	  List<User> findAllLazy();
	}
	```
2. `mapper/user.xml`
	```xml
	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper
	    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<mapper namespace="com.fishx.mybatis.mapper.UserMapper">
	  <resultMap id="userLazy" type="User">
	    <id property="id" column="id"/>
	    <result column="name" property="name"/>
	    <result column="age" property="age"/>
	    <result column="birthday" property="birthday"/>
	    <association property="department" javaType="Department"
	                 column="department_id"
	                 select="com.fishx.mybatis.mapper.DepartmentMapper.findById"/>
	  </resultMap>
	  <select id="findAllLazy" resultMap="userLazy">
	    SELECT * FROM user
	  </select>
	</mapper>
	```
3. 测试编码运行
	```java
	public class UserTest {
	  private SqlSession sqlSession;
	  private UserMapper userMapper;
	
	  @Before
	  public void init() {
	    sqlSession = MyBatisUtils.getSqlSession();
	    userMapper = sqlSession.getMapper(UserMapper.class);
	    System.out.println("departmentMapper: " + userMapper.getClass());
	  }
	  
	  @Test
	  public void find() {
	    User user = userMapper.findByDepartmentIdAndAge("18ec781fbefd727923b0d35740b177ab", 18);
	    System.out.println("\n" + user + "\n");
	    if (sqlSession != null) sqlSession.close();
	  }
	}
	```

- 可以发现控制台打印的 `Department` 中，tel 属性不为 null 了：
	```bash
	User{id='09ec5fcea620c168936deee53a9cdcfb', name='阿熊', department=Department{id='18ec781fbefd727923b0d35740b177ab', name='开发部', tel='123'}}
	User{id='5d0eebc4f370f3bd959a4f7bc2456d89', name='老狗', department=Department{id='ee0e342201004c1721e69a99ac0dc0df', name='运维部', tel='456'}}
	```

#### 一对多
Department 对 User 一对多
##### 延迟加载
1. Department 实体类修改
	```java
	@Data
	public class Department implements Serializable {
	    private String id;
	    private String name;
	    private String tel;
	    private Set<User> users;   
	}
	```
	`Department` 要对 `User` 一对多，首先要从实体类的模型修改，在 `Department` 中组合一个 `Set<User>`
1. 接口 
	`UserMapper`
	```java
	public interface UserMapper {
	  User findByDeptOnUser(String id);
	}
	```
	`DepartmentMapper`
	```java
	public interface DepartmentMapper {
	  List<Department> findAll();
	}
	```
3. `mapper.xml` 修改 
	```xml
	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper
	    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<mapper namespace="com.fishx.mybatis.mapper.DepartmentMapper"> 
	  <!--懒加载,两次SELECT查询-->
	  <resultMap id="department" type="Department">
	    <id column="id" property="id"/>
	    <result property="name" column="name"/>
	    <result property="tel" column="tel"/>
	    <collection property="users" ofType="User" column="id"
	                select="com.fishx.mybatis.mapper.UserMapper.findByDeptOnUser"/>
	  </resultMap>
	  <select id="findAll" resultMap="department"> <!--resultType="Department"-->
	    SELECT id, name, tel
	    FROM department
	  </select>
	</mapper>
	```
4. 测试编码运行
	```java
	@Test
	public void oneToManyLazyLoading() {
	  departmentMapper.findAll().forEach(System.out::println);
	  if (sqlSession != null) sqlSession.close();
	}
	```