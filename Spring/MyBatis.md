简化数据库操作的持久层框架

![[Pasted image 20230127202602.png]]

1. MyBatis 是一个持久层框架
2. 前身是 ibatis, 在 ibatis3.x 时, 更名为 MyBatis 
3. MyBatis 在 java 和 sql 之间提供更灵活的映射方案
4. mybatis 可以将对数据表的操作 (sql, 方法) 等等直接剥离，写到 xml 配置文件，实现和 java 代码的解耦
5. mybatis 通过 SQL 操作 DB, 建库建表的工作需要程序员完成

![[MyBatis 工作示意图.excalidraw|880]]

# 1. 快速入门
要求: 开发一个 MyBatis 项目，通过 MyBatis 的方式可以完成对 monster 表的 crud 操作
## 1.1 maven 父项目、子模块配置
创建 maven 父项目, 然后删除 src 目录

![[Pasted image 20230128090431.png]]

父项目引入子模块所共需的依赖至 `pom.xml` 

再创建模块, 选 maven 再次创建子模块

![[Pasted image 20230128090728.png]]

## 1.2 配置 mybatis-config 
创建 `resources/mybatis-config.xml`

![[Pasted image 20230128090830.png]]

```xml
<configuration>  
  <!--配置自带的日志输出-->  
  <settings>  
    <setting name="logImpl" value="STDOUT_LOGGING"/>  
  </settings>  
  
  <!--配置别名-->  
  <typeAliases>  
    <typeAlias type="com.entity.Monster" alias="Monster"/>  
  </typeAliases>  
  
  <environments default="development">  
    <environment id="development">  
      <!--配置事务管理器-->  
      <transactionManager type="JDBC"/>  
      <!--配置数据源-->  
      <dataSource type="POOLED">  
        <property name="driver" value="com.mysql.cj.jdbc.Driver"/>  
        <property name="url"  
                  value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>  
        <property name="username" value="root"/>  
        <property name="password" value="admin"/>  
      </dataSource>  
    </environment>  
  </environments>  
  
  <!--配置需要关联的Mapper-->  
  <mappers>  
    <mapper resource="com/mapper/MonsterMapper.xml"/>  
  </mappers>  
</configuration>
```

## 1.3 创建 MonsterMapper 接口

```java
/**  
 * 该接口用于声明操作monster表的方法  
 * 这些方法可以通过注解or配置文件xml来实现  
 */  
public interface MonsterMapper {  
  // 添加monster  
  void addMonster(Monster monster);  
  
  // 根据id删除monster  
  void delMonster(Integer id);  
  
  // 修改monster  
  void updateMonster(Monster monster);  
  
  // 通过id查找monster  
  Monster getMonsterById(Integer id);  
  
  List<Monster> getMonsterList();  
}
```

## 1.4 创建 `Mapper.xml` 文件 

`xxMapper.xml` 文件是用来映射管理 xxMapper 接口

```xml
<mapper namespace="com.mapper.MonsterMapper">  
  <insert id="addMonster" parameterType="com.entity.Monster" useGeneratedKeys="true" keyProperty="id">  
    INSERT INTO `monster` (`age`, `birthday`, `email`, `gender`, `name`, `salary`)  
    VALUES (#{age}, #{birthday}, #{email}, #{gender}, #{name}, #{salary});  
  </insert>  
  
  <delete id="delMonster" parameterType="java.lang.Integer">  
    DELETE FROM `monster` WHERE id = #{id};  
  </delete>  
  
  <update id="updateMonster" parameterType="Monster">  
    UPDATE `monster` SET `age` = #{age}, `salary` = #{salary} WHERE `id` = #{id};  
  </update>  
  
  <select id="getMonsterById" resultType="Monster">  
    SELECT * FROM `monster` WHERE id = #{id};  
  </select>  
  
  <select id="getMonsterList" resultType="Monster">  
    SELECT * FROM `monster`;  
  </select>  
</mapper>
```

## 1.5 创建 MyBatisUtils 工具类 
创建 MyBatisUtils 工具类, 返回一个 SqlSession 对象 
```java
// MyBatisUtils工具类返回一个SqlSession对象  
public class MyBatisUtils {  
  private static SqlSessionFactory sqlSessionFactory;  
  
  // 静态代码块,初始化SqlSessionFactory  
  static {  
    // 指定资源文件(配置文件)  
    String resource = "mybatis-config.xml";  
    try {  
      InputStream inputStream = Resources.getResourceAsStream(resource);  
      sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);  
      System.out.println("sqlSessionFactory: " + sqlSessionFactory.getClass());  
    } catch (IOException e) {  
      e.printStackTrace();  
    }  
  }  
  
  // 返回SqlSession对象  
  public static SqlSession getSqlSession() {  
    return sqlSessionFactory.openSession();  
  }  
}
```

## 1.6 获取及测试

```java
public class MonsterMapperTest {  
  private SqlSession sqlSession;  
  private MonsterMapper monsterMapper;  
  
  // 当方法标识@Before,表示在执行你的目标测试方法前,会先执行该方法  
  @Before  
  public void init() {  
    sqlSession = MyBatisUtils.getSqlSession();  
    monsterMapper = sqlSession.getMapper(MonsterMapper.class);  
    System.out.println("monsterMapper: " + monsterMapper.getClass());  
  }  
  
  @Test  
  public void t1() {  
    System.out.println("t1()...");  
  }  
  
  @Test  
  public void addMonster() {  
    List<Monster> monsters = new ArrayList<>();  
    monsters.add(new Monster(3, 12, "黑山老妖", "heishan@163.com", new Date(), 12300, 1));  
    monsters.add(new Monster(4, 24, "白骨精", "baigu@163.com", new Date(), 22300, 0));  
    for (Monster monster : monsters) {  
      monsterMapper.addMonster(monster);  
    }  
    // DML操作,需要提交  
    if (sqlSession != null) {  
      sqlSession.commit();  
      sqlSession.close();  
    }  
  }  
  
  @Test  
  public void delMonster() {  
    monsterMapper.delMonster(1);  
    if (sqlSession != null) {  
      sqlSession.commit();  
      sqlSession.close();  
    }  
  }  
  
  @Test  
  public void updateMonster() {  
    Monster monster = new Monster(4, 31, "白骨精", "baigu@163.com", new Date(), 22300, 1);  
    monsterMapper.updateMonster(monster);  
    if (sqlSession != null) {  
      sqlSession.commit();  
      sqlSession.close();  
    }  
  }  
  
  @Test  
  public void getMonsterById() {  
    System.out.println(monsterMapper.getMonsterById(3));  
    if (sqlSession != null) sqlSession.close();  
  }  
  
  @Test  
  public void getMonsterList() {  
    System.out.println(monsterMapper.getMonsterList());  
    if (sqlSession != null) sqlSession.close();  
  }  
}
```

## 1.7 解决 maven 项目资源丢失问题

![[Pasted image 20230128093629.png]]

父项目的 `pom.xml` 添加 `build` 节点

```xml
<!--在build中配置resources,来防止资源导出失败的问题-->  
<build>  
  <resources>  
    <resource>  
      <directory>src/main/java</directory>  
      <includes>  
        <include>**/*.xml</include>  
      </includes>  
    </resource>  
    <resource>  
      <directory>src/main/resources</directory>  
      <includes>  
        <include>**/*.xml</include>  
        <include>**/*.properties</include>  
      </includes>  
    </resource>  
  </resources>  
</build>
```

# 2. MyBatis 整体架构分析
![[Pasted image 20230128094258.png]]

1) mybatis 的核心配置文件 `mybatis-config.xml`: 
	- 进行全局配置，全局只能有一个这样的配置文件 `XxxMapper.xml` 配置多个 SQL，可以有多个 `XxxMapper.xml` 配置文件
2) 通过 `mybatis-config.xml` 配置文件得到 SqlSessionFactory
3) 通过 SqlSessionFactory 得到 SqlSession，用 SqlSession 就可以操作数据了
4) SqlSession 底层是 Executor (执行器), 有 2 重要的实现类, 有很多方法
	![[Pasted image 20230128094525.png]]
	![[Pasted image 20230128094533.png]]
5) MappedStatement 是通过 `mybatis-config.xml` 中定义, 生成的 statement 对象
6) 参数输入执行并输出结果集, 无需手动判断参数类型和参数下标位置, 且自动将结果集映射为 Java 对象

# 3. 原生的API & 注解的方式 
## 3.1 MyBatis-原生的 API 调用
```java
public class MyBatisNativeTest {
  //这个是 Sql 会话,通过它可以发出 sql 语句 
  private SqlSession sqlSession; 
  private MonsterMapper monsterMapper;
  
  @Before 
  public void init() throws Exception {
	// 通过 SqlSessionFactory 对象获取一个 SqlSession 会话 
	sqlSession = MyBatisUtils.getSqlSession(); 
	//获取 MonsterMapper 接口对象, 该对象实现了 MonsterMapper 
	monsterMapper = sqlSession.getMapper(MonsterMapper.class); 
	System.out.println(monsterMapper.getClass());
  }

  //使用 sqlSession 原生的 API 调用我们编写的方法[了解]
  @Test 
  public void myBatisNativeCrud() {
    Monster monster = new Monster(); 
    monster.setAge(200); 
    monster.setBirthday(new Date()); 
    monster.setEmail("hspedu100@sohu.com"); 
    monster.setGender(2); 
    monster.setName("白骨精"); 
    monster.setSalary(9234.89);
    sqlSession.insert("com.hspedu.mapper.MonsterMapper.addMonster", monster);

	if (sqlSession != null) { 
	  sqlSession.commit(); 
	  sqlSession.close(); 
	} 
	System.out.println("操作成功!");
  }
}
```

## 3.2 注解的方式操作 MyBatis

```java
public interface MonsterTest {  
  // 添加monster  
  @Insert("INSERT INTO `monster` (`age`, `birthday`, `email`, `gender`, `name`, `salary`) " +  
      "VALUES (#{age}, #{birthday}, #{email}, #{gender}, #{name}, #{salary});")  
  // useGeneratedKeys返回自增值,keyProperty自增值对应对象属性,keyColumn自增值对应的表的字段  
  @Options(useGeneratedKeys = true, keyProperty = "id", keyColumn = "id")  
  void addMonster(Monster monster);  
  
  // 根据id删除monster  
  @Delete("DELETE FROM `monster` WHERE id = #{id};")  
  void delMonster(Integer id);  
  
  // 修改monster  
  @Update("UPDATE `monster` SET `age` = #{age}, `salary` = #{salary} WHERE `id` = #{id};")  
  void updateMonster(Monster monster);  
  
  // 通过id查找monster  
  @Select("SELECT * FROM `monster` WHERE id = #{id};")  
  Monster getMonsterById(Integer id);  
  
  @Select("SELECT * FROM `monster`;")  
  List<Monster> getMonsterList();  
}
```

```java
public class MonsterAnnotationTest {  
  private SqlSession sqlSession;  
  private MonsterTest monsterTest;  
  
  @Before  
  public void init() {  
    sqlSession = MyBatisUtils.getSqlSession();  
    monsterTest = sqlSession.getMapper(MonsterTest.class);  
    System.out.println("monsterAnnotation = " + monsterTest.getClass());  
  }  
  
  @Test  
  public void select() {  
    System.out.println(monsterTest.getMonsterById(5));  
  }  
  
  @Test  
  public void add() {  
    Monster monster = new Monster(null, 23, "略萨", "lusa@163.com", new Date(), 22300, 1);  
    monsterTest.addMonster(monster);  
    System.out.println("添加的id：" + monster.getId());  
    if (sqlSession != null) {  
      sqlSession.commit();  
      sqlSession.close();  
    }  
    System.out.println("ok...");  
  }  
}
```

- 注意事项和说明
	- 如果是通过注解的方式，就不再使用 `mybatis-config.xml` 文件，但是需要在文件中注册含注解的类/接口 `MonsterMapper.xml`  
	- 使用注解方式, 添加时, 如果要返回自增长 id 值, 可以使用@Option 注解 , 组合使用

# 4. `mybatis-config.xml` 配置文件详解
mybatis 的核心配置文件 `mybatis-config.xml`，比如配置 jdbc 连接信息，注册 mapper 等等, 我们需要对这个配置文件有[详细的了解](https://mybatis.org/mybatis-3/zh/configuration.html) 
## 4.1 properties 属性
通过该属性，可以指定一个外部的 `mysql.properties` 文件，引入我们的 jdbc 连接信息

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver  
jdbc.url=jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8  
jdbc.username=root  
jdbc.password=admin
```

```xml
<dataSource type="POOLED">  
  <property name="driver" value="${jdbc.driver}"/>  
  <property name="url" value="${jdbc.url}"/>  
  <property name="username" value="${jdbc.username}"/>  
  <property name="password" value="${jdbc.password}"/>  
</dataSource>
```

![[Pasted image 20230131122812.png]]

## 4.2 settings 全局参数定义 
settings 列表，通常使用默认
![[Pasted image 20230131122943.png]]

## 4.3 typeAliases 别名处理器
1. 别名是为 Java 类型命名一个短名字。它只和 XML 配置有关，用来减少类名重复的部分
2. 如果指定了别名, 我们的 `MappperXxxx.xml` 文件就可以做相应的简化处理
3. 也可以指定一个包名, MyBatis 会在包名下面搜索需要的 JavaBean ·
	```xml
	<typeAliases>
	  <package name="domain.blog"/>
	</typeAliases>
	```
4. 注意指定别名后，还是可以使用全名的


## 4.4 typeHandlers 类型处理器
1. 用于 java 类型和 jdbc 类型映射
2. Mybatis 的映射基本已经满足，不太需要重新定义
3. 这个我们使用默认即可，也就是 mybatis 会自动的将 java 和 jdbc 类型进行转换.
4. [java 类型和 jdbc 类型映射关系一览](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)

## 4.5 environments 环境
1. resource 注册 Mapper 文件: `XXXMapper.xml` 文件
2. class: 接口注解实现
3. url: 外部路径, 使用很少，不推荐
4. package 方式注册

# 5. `XxxMapper.xml` SQL 映射文件 
> XML 映射器[](https://mybatis.org/mybatis-3/zh/sqlmap-xml.html#xml-%E6%98%A0%E5%B0%84%E5%99%A8)

```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper  
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
    "https://mybatis.org/dtd/mybatis-3-mapper.dtd">  
  
<mapper namespace="com.fishx.mapper.MonsterMapper">
  <!-- parameterType(输入参数类型) !-->
  <select id="findMonsterByNameORId" parameterType="Monster" resultType="Monster">  
    SELECT * FROM monster WHERE `name` = #{name} OR `id` = #{id}  
  </select>  
  
  <select id="findMonsterByName" parameterType="String" resultType="Monster">  
    SELECT * FROM monster WHERE `name` LIKE '%${name}%'  
  </select>  

  <!-- 传入 HashMap,返回JavaBean !-->
  <select id="findMonsterByIdAndSalary" parameterType="map" resultType="Monster">  
    SELECT * FROM monster WHERE `id` > #{id} AND `salary` > #{salary}  
  </select>  

  <!-- 传入和返回都是Map !-->
  
</mapper>
```

```java
public interface MonsterMapper {  
  // 通过id或者名字查询(or)  
  // @Select("SELECT * FROM monster WHERE `name` = #{name} OR `id` = #{id}")  
  List<Monster> findMonsterByNameORId(Monster monster);  
  
  // 查询名字中含'精'的妖怪(Like)  
  // @Select("SELECT * FROM monster WHERE `name` LIKE '%${name}%'")  
  List<Monster> findMonsterByName(String name);  

  // 查询 (id > 10 && salary > 40),要求传入的参数是 HashMap
  List<Monster> findMonsterByIdAndSalary(Map<String, Object> map);

  // 查询,要求传入和返回都是 HashMap
  List<Map<String, Object>> find_ReturnAndParameterMap(Map<String, Object> map);
}
```

## 5.1 parameterType (输入参数类型)
1. 传入简单类型，比如按照 id 查 Monster (前讲过)
2. 传入 POJO 类型，查询时需要有多个筛选条件
3. 当有多个条件时，传入的参数就是 Pojo 类型的 Java 对象, 比如这里的 Monster 对象
4. 当传入的参数类是 String 时，也可以使用 ${} 来接收参数

## 5.2 传入 HashMap (重点)
HashMap 传入参数更加灵活，比如可以灵活的增加查询的属性，而不受限于 Monster 这个 Pojo 属性本身

```java
public void findMonsterByIdAndSalary() {  
  Map<String, Object> map = new HashMap<>();  
  map.put("id", 4);  
  map.put("salary", 11100);  
  List<Monster> monster = monsterMapper.findMonsterByIdAndSalary(map);  
  for (Monster m : monster) {  
    System.out.println(m);  
  }  
  if (sqlSession != null) sqlSession.close();  
  System.out.println("ok...");  
}
```

传入和返回都是 HashMap
```java
public void find_ReturnAndParameterMap() {  
  Map<String, Object> map = new HashMap<>();  
  map.put("id", 4);  
  map.put("salary", 11100);  
  List<Map<String, Object>> monsterList = monsterMapper.find_ReturnAndParameterMap(map);  
  for (Map<String, Object> monsterMap : monsterList) {  
    System.out.println("{");  
    for (Map.Entry<String, Object> entry : monsterMap.entrySet()) {  
      System.out.println(entry.getKey() + " : " + entry.getValue());  
    }  
    System.out.println("}");  
  }  
  if (sqlSession != null) sqlSession.close();  
  System.out.println("ok...");  
}
```

## 5.3 resultMap (结果集映射)
> - `resultMap` – 描述如何从数据库结果集中加载对象，是最复杂也是最强大的元素。

当实体类的属性和表的字段名字不一致时，我们可以通过 resultMap 进行映射，从而屏蔽实体类属性名和表的字段名的不同.

```sql
CREATE TABLE `user` (
  `user_id` INT NOT NULL AUTO_INCREMENT,
  `user_email` VARCHAR(255) DEFAULT '',
  `user_name` VARCHAR(255) DEFAULT '',
   PRIMARY KEY (`user_id`) 
) CHARSET = utf8
```

```java
@Data
public class User {
  private Integer user_id;
  private String username;
  private String useremail;
}
```

此时,db 表字段与 entity 对象属性不一致

```xml
<!--  
使用resultMap,解决字段名不同,赋值失败问题  
column="user_email" 表的字段  
property="useremail" 对象的属性  
-->  
<resultMap id="findAllUserMap" type="User">  
  <result column="user_email" property="useremail"/>  
  <result column="user_name" property="username"/>  
</resultMap>  
<select id="findAllUser" resultMap="findAllUserMap">  
  SELECT * FROM `user`  
</select>
```

- 说明
	- 解决表字段和对象属性名不一致, 也支持 sql 语句使用字段别名 (复用性差)
	- 如果是 MyBatis-Plus 处理就比较简单, 可以使用注解@TableField 来解决实体字段名和表字段名不一致的问题，还可以使用@TableName 来解决实体类名和表名不一致的问题 

# 6. 动态 SQL 语句
> [动态 SQL 语句](https://mybatis.org/mybatis-3/zh/dynamic-sql.html) - 满足更复杂的查询业务需求  

## 6.1 动态 SQL-基本介绍
- 为什么需要动态 SQL
	- 动态 SQL 是 MyBatis 的强大特性之一
	- 减少拼接 SQL 语句麻烦, 更加高效可用
		- 使用 JDBC 或其它类似的框架，根据不同条件拼接 SQL 语句非常麻烦，例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号等
- 动态 SQL 必要性 
	1. 比如我们查询 Monster 时，如果程序员输入的 age 不大于 0, 我们的 sql 语句就不带 age 。
	2. 更新 Monster 对象时，没有设置的新的属性值，就保持原来的值，设置了新的值，才更新.
- 动态 SQL 常用标签
	1. if [判断]
	2. where [拼接 where 子句]
	3. choose/when/otherwise [类似 java 的 switch 语句, 注意是单分支]
	4. foreach [类似 in ]
	5. trim [替换关键字/定制元素的功能]
	6. set [在 update 的 set 中，可以保证进入 set 标签的属性被修改，而没有进入 set 的，保持原来的值]

## 6.2 动态 SQL-案例演示
- `<if>`
	```xml
	<select id="findMonsterByAge" parameterType="Integer" resultType="Monster">  
	  SELECT * FROM `monster` WHERE 1 = 1  
	  <if test="age >= 0">  
	    AND age > ${age}  
	  </if>  
	</select>
	```
- `<where>`
	```xml
	<select id="findMonsterByIdAndName" parameterType="Monster" resultType="Monster">  
	  SELECT * FROM `monster`
	  <!-- 
	    使用 where 标签开始拼接		
	    1.会自动的加入 where 子句 
	    2.mybatis 会自动的去掉多余的 AND
	  !-->  
	  <where>  
	    <if test="id >= 0">  
	      AND id = #{id}  
	    </if>  
	    <if test="name != null and name != ''">  
	      AND name = #{name}  
	    </if>  
	  </where>  
	</select>
	```
- `<choose>、<when>、<otherwise>`
	```xml
	<select id="findByIdOrName_choose" parameterType="map" resultType="Monster">  
	  SELECT * FROM `monster`  
	  <choose>  
	    <when test="name != null and name != ''">  
	      WHERE `name` = #{name}  
	    </when>  
	    <when test="id > 0">  
	      WHERE `id` > #{id}  
	    </when>  
	    <otherwise>  
	      WHERE `salary` > #{salary}  
	    </otherwise>  
	  </choose>  
	</select>
	```
- `<forEach>`
	```xml
	<select id="findMonsterById_forEach" parameterType="map" resultType="Monster">  
	  SELECT * FROM `monster`  
	  <if test="ids != null and ids != ''">  
	    <where>  
	      id IN  
	      <!--  
	      1.collection遍历的数组/集合  
	      2.item当前项目  
	      3.open开始,separator以x间隔,close闭合  
	      -->  
	      <foreach collection="ids" item="id" open="(" separator="," close=")">  
	        #{id}  
	      </foreach>  
	    </where>  
	  </if>  
	</select>
	```
	
	```java
	Map<String, Object> map = new HashMap<>();  
	map.put("ids", Arrays.asList(2, 4, 6));  
	List<Monster> monsters = monsterMapper.findMonsterById_forEach(map);  
	for (Monster monster : monsters) System.out.println(monster);  
	if (sqlSession != null) sqlSession.close();  
	System.out.println("ok...");
	```

- `<trim>` -- 使用较少 
	```xml
	<select id="findMonsterByIdAndName" parameterType="Monster" resultType="Monster">  
	  SELECT * FROM `monster`  
	  <trim prefix="WHERE" prefixOverrides="and|or|fishx">  
	    <if test="id >= 0">  
	      fishx id = #{id}  
	    </if>  
	    <if test="name != null and name != ''">  
	      AND name = #{name}  
	    </if>  
	  </trim>  
	</select>
	```
- `<set>`
	- set 元素可以用于动态包含需要更新的列，忽略其它不更新的列
	  ```xml
	  <select id="updateMonster_set" parameterType="map">  
	    UPDATE `monster`  
	    <set>  
	      <if test="age != null and age != ''">  
	        `age` = #{age},  
	      </if>  
	      <if test="name != null and name != ''">  
	        `name` = #{name},  
	      </if>  
	      <if test="email != null and email != ''">  
	        `email` = #{email},  
	      </if>  
	      <if test="birthday != null and birthday != ''">  
	        `birthday` = #{birthday},  
	      </if>  
	      <if test="salary != null and salary != ''">  
	        `salary` = #{salary},  
	      </if>  
	      <if test="gender != null and gender != ''">  
	        `gender` = #{gender},  
	      </if>  
	    </set>  
	    WHERE id = #{id}  
	  </select>
	  ```

## 6.3 Mybatis 数据绑定小疑问
- MyBatis 中 `${}` 与 `#{}` 的区别
	- `${}` 语法称为“非转义”占位符
		- 即占位符内的内容直接拼接成 SQL 语句，没有任何类型的 SQL 注入保护
			- 这会危及数据库的安全
			- 同时数据没有转化, 直接拼接可能会导致 sql 语句执行失败
	- `#{}` 语法是一个“转义”的占位符  
		- 也就是说 MyBatis 会自动对占位符里面的内容进行转义
			- 以防止 SQL 注入攻击

# 7. 映射关系一对一
项目中 1 对 1 的关系是一个基本的映射关系，比如: Person (人) --- IDCard (身份证)
## 7.1 映射方式 
1. 通过配置 `XxxMapper.xml` 实现 1 对 1 [配置方式]
2. 通过注解的方式实现 1 对 1 [注解方式] 
## 7.2 应用实例
- `IdenCard.java`
	```java
	public class IdenCard {  
	  private Integer id;  
	  private String card_sn;  
	}
	```
- `Person.java`
	```java
	public class Person {  
	  private Integer id;  
	  private String name;  
	  // 此处需要进行映射,完成级联操作  
	  private IdenCard card;  
	  // private Integer card_id;  
	}
	```

### 通过 xml 配置方式实现一对一映射
通过配置 `XxxMapper.xml` 的方式来实现下面的 1 对 1 的映射关系，实现级联查询, 通过 person 可以获取到对应的 idencard 信息

#### 方式一: 多表联查
- `PersonMapper.xml`
	```xml
	<mapper namespace="cn.mapper.PersonMapper">  
	  <!--通过Person的id获取Person,包括Person所关联的IdeaCard对象[级联查询]-->  
	  <resultMap id="PersonResultMap" type="Person">  
	    <!--<result property="id" column="id"/>-->  
	    <!-- 
	    id 一个id结果：标记出作为ID的结果可以帮助提高整体性能  
	    1. property="id" 表示person属性id，通常是主键  
	    2. column="id" 表示表的字段  
	    -->  
	    <id property="id" column="id"/>  
	    <result property="name" column="NAME"/>  
	    <!--  
	    association 一个复杂类型的关联  
	    1. property = "card" 表示Person对象的card属性  
	    2. javaType = "IdenCard" 表示card属性的类型  
	    -->  
	    <association property="card" javaType="IdenCard">  
	      <result property="id" column="id"/>  
	      <result property="card_sn" column="card_sn"/>  
	    </association>  
	  </resultMap>  
	  <select id="getPersonById" parameterType="Integer" resultMap="PersonResultMap">  
	    SELECT * FROM `person`,`idencard` WHERE person.id = #{id}  
	    AND person.card_id = idencard.id  
	  </select>  
	</mapper>
	```

#### 方式二: 多次单查
- `IdenCardMapper.xml`
	```xml
	<mapper namespace="cn.mapper.IdenCardMapper">  
	  <select id="getIdenCardById" parameterType="Integer" resultType="IdenCard">  
	    SELECT * FROM idencard WHERE `id` = #{id}  
	  </select>  
	</mapper>
	```
- `PersonMapper.xml`
	```xml
	<!--  
	第二种方式：  
	  1）先通过 SELECT * FROM `person` WHERE id = #{id} 返回person信息  
	  2）再通过 person.card_id 值,接着查,最后得到IdenCard数据  
	-->  
	<resultMap id="PersonResultMap2" type="Person">  
	  <id property="id" column="id"/>  
	  <result property="name" column="NAME"/>  
	  <!--
	  核心思想：将这个多表联查拆分成两个单表操作,能够实现方法的复用，简单明了，易于维护
		  1. property="card" 表示person对象的card属性
		  2. column="card_id" 这是第一个查询返回的card_id字段
		  3. 会作为入参传递给getIdenCardById()
	  -->  
	  <association property="card" column="card_id"  
	               select="cn.mapper.IdenCardMapper.getIdenCardById"/>  
	</resultMap>  
	<select id="getPersonById2" parameterType="Integer" resultMap="PersonResultMap2">  
	  SELECT * FROM `person` WHERE id = #{id}  
	</select>
	```

### 通过注解方式实现一对一映射 
> 注解是配置方式的另一种形式

- `IdenCardMapperAnnotation.java`
	```java
	@Select("SELECT * FROM idencard WHERE `id` = #{id}")  
	IdenCard getIdenCardById(Integer id);
	```
- `PersonMapperAnnotation.java`
	```java
	@Select("SELECT * FROM `person` WHERE id = #{id}")  
	@Results({  
	    @Result(id = true, property = "id", column = "id"),  
	    @Result(property = "name", column = "NAME"),  
	    @Result(property = "card", column = "card_id",  
	        one = @One(select = "cn.mapper.IdenCardMapperAnnotation.getIdenCardById"))  
	})  
	Person getPersonById(Integer card_id);
	```

# 8. 映射关系多对一 
项目中多对一的关系是一个基本的映射关系, 多对一, 也可以理解成是一对多.

## 8.1 映射方式 
1. 通过配置 `XxxMapper.xml` 实现多对一
2. 通过注解的方式实现多对一
 
## 8.2 应用实例

- 双向的多对一的关系: 
	- 比如通过 User 可以查询到对应的 Pet, 反过来，通过 Pet 也可以级联查询到对应的 User 信息.
- `Pet.java`
	```java
	@Getter  
	@Setter  
	public class Pet {  
	  private Integer id;  
	  private String nickname;  
	  private User user;  
	}
	```
- `User.java`
	```java
	@Getter  
	@Setter  
	public class User {  
	  private Integer id;  
	  private String name;  
	  private List<Pet> pets;  
	}
	```
- <span style="color:red">⚠️注意: </span>
	- **<u>不能去重写 `toString ()`, 两者相互调用形成死循环, 导致堆栈溢出异常!</u>** 
- 配置映射流程:
	- `User getUserById(Integer id);`
		1. 先通过 `User.id` 获取 `User` 对象
			- `User` 对象中含有 `List<Pet> pets` 属性, 需要做级联操作
		2. 再通过 `Pet.user_id` 去获取 `Pet` 对象 (可能有多个)
			- `Pet` 对象中含有 `User user` 属性 => 级联操作
		3. 最后通过 `User.id` 获取 `User` 对象 
	- `Pet getPetById(Integer id);`
		1. 先通过 `Pet.id` 获得 `Pet` 对象
			- `Pet` 对象中含有 `User user` 级联属性 => 级联操作 
		2. 再通过 `Pet.user_id` 获得 `User` 对象
			- `User` 对象含有 `List<Pet> pets` 级联属性 => 级联操作
		3. 最后通过 `Pet.user_id` 去获取 `Pet` 对象 (可能有多个)
### 通过 xml 配置方式实现双向多对一映射
- `PetMapper.java`
	```java
	public interface PetMapper {  
	  // 通过User的id来获取pet对象,可能有多个  
	  List<Pet> getPetByUserId(Integer userId);  
	  
	  // 通过pet的id获取pet对象  
	  Pet getPetById(Integer id);  
	}
	```
- `PetMapper.xml`
	```XML
	<mapper namespace="cn.mapper.many_to_one.PetMapper">  
	  <resultMap id="PetResultMap" type="Pet">  
	    <id property="id" column="id"/>  
	    <result property="nickname" column="nickname"/>  
	    <association property="user" column="user_id"  
	                 select="cn.mapper.many_to_one.UserMapper.getUserById"/>  
	  </resultMap>  
	  <select id="getPetByUserId" parameterType="Integer" resultMap="PetResultMap">  
	    SELECT * FROM mybatis_pet WHERE user_id = #{userId};  
	  </select>  
	  
	  <!--通过Pet.Id得到Pet对象(需要级联操作),此处体现了ResultMap的复用-->  
	  <select id="getPetById" parameterType="Integer" resultMap="PetResultMap">  
	    SELECT * FROM mybatis_pet WHERE id = #{id};  
	  </select>  
	</mapper>
	```
- `UserMapper.java`
	```java
	public interface UserMapper {  
	  User getUserById(Integer id);  
	}
	```
- `UserMapper.xml`
	```xml
	<mapper namespace="cn.mapper.many_to_one.UserMapper">  
	  <!--  
	  实现getUserById(Integer id)思路：  
	    1. 先通过user_id查询得到User信息  
	    2. 再根据user_id查询对应的Pet  
	    3. 映射到User的List<Pet> pets中  
	  -->  
	  <resultMap id="UserResultMap" type="User">  
	    <id property="id" column="id"/>  
	    <result property="name" column="NAME"/>  
	    <!--pets属性是集合,因此需要使用collection配置映射-->  
	    <!--ofType指定集合返回类型Pet-->  
	    <collection property="pets" column="id" ofType="Pet"  
	                select="cn.mapper.many_to_one.PetMapper.getPetByUserId"/>  
	  </resultMap>  
	  <select id="getUserById" parameterType="Integer" resultMap="UserResultMap">  
	    SELECT * FROM mybatis_user WHERE id = #{id};  
	  </select>  
	</mapper>
	```
- `UserMapperTest.java`
	```java
	public void getUserById() {  
	  User user = userMapper.getUserById(1);  
	  System.out.println("\n{");  
	  System.out.println("  user_id: " + user.getId());  
	  System.out.println("  user_name: " + user.getName());  
	  System.out.println("  user_pets: ");  
	  System.out.println("    {");  
	  for (Pet pet : user.getPets()) {  
	    System.out.println(
		    "\t\t 宠物编号："+pet.getId()  
	        + "\t 昵称：" + pet.getNickname()  
	        + "\t 主人：" + pet.getUser().getName()+" ("+pet.getUser()+")"
	    );  
	  }  
	  System.out.println("    }");  
	  System.out.println("}\n");  
	  if (sqlSession != null) sqlSession.close();  
	}
	```
	输出结果:
	```text
	{
	  user_id: 1
	  user_name: 宋江
	  user_pets: 
	    {
			 宠物编号：1	 昵称：黑背	 主人：宋江 (cn.entity.many_to_one.User@2794eab6)
			 宠物编号：2	 昵称：小哈	 主人：宋江 (cn.entity.many_to_one.User@2794eab6)
	    }
	}
	```

### 通过注解方式配置/实现双向多对一映射
- `PetMapperAnnotation.java`
	```java
	@Select("SELECT * FROM mybatis_pet WHERE user_id = #{userId};")  
	@Results(id = "PetResultMap", value = {  
	    @Result(id = true, column = "id", property = "id"),  
	    @Result(property = "nickname", column = "nickname"),  
	    @Result(property = "user", column = "user_id",  
	        one = @One(select = "cn.mapper.many_to_one.UserMapperAnnotation.getUserById"))  
	})  
	List<Pet> getPetByUserId(Integer userId);  
	  
	@Select("SELECT * FROM mybatis_pet WHERE id = #{id};")  
	@ResultMap("PetResultMap")  
	Pet getPetById(Integer id);
	```
- `UserMapperAnnotation.java`
	```java
	@Select("SELECT * FROM mybatis_user WHERE id = #{id};")  
	@Results({  
	    @Result(id = true, property = "id", column = "id"),  
	    @Result(property = "name", column = "NAME"),  
	    @Result(property = "pets", column = "id",  
	        many = @Many(select = "cn.mapper.many_to_one.PetMapperAnnotation.getPetByUserId"))  
	})  
	User getUserById(Integer id);
	```


# 9. 缓存 —— 提高检索效率的利器 
> - MyBatis 内置了一个强大的事务性查询[缓存](https://mybatis.org/mybatis-3/zh/sqlmap-xml.html#cache)机制
> - 默认情况下，只启用了本地的会话缓存，它仅仅对一个会话中的数据进行缓存

## 9.1 一级缓存 (本地缓存)
同一个 SqlSession 接口对象调用了相同的 select 语句, 会直接从缓存里面获取，而不是再去查询数据库
![[Pasted image 20230203174959.png]]
### 9.1.1 sqlSession 的结构示意图
![[Pasted image 20230203194742.png]]
### 9.1.2 一级缓存失效分析
1. 关闭 sqlSession 会话后, 再次查询，会到数据库查询
2. 当执行 `sqlSession.clearCache()` 会使一级缓存失效 
3. 当对同一个 monster 修改，该对象在一级缓存会失效

## 9.2 二级缓存
1. 二级缓存和一级缓存都是为了提高检索效率的技术
2. 最大的区别就是作用域的范围不一样，一级缓存的作用域是 sqlSession 会话级别, 在一次会话有效，而二级缓存作用域是全局范围，针对不同的会话都有效
![[Pasted image 20230203195810.png]]
### 9.2.1 快速入门
1. mybatis-config. xml 配置中开启二级缓存
	```xml
	<settings>
		<!-- 开启二级缓存 -->
		<!--全局性地开启或关闭所有映射器配置文件中已配置的任何缓存, 默认就是 true--> 
		<setting name="cacheEnabled" value="true"/>
	</settings>
	```
2. 使用二级缓存时 entity 类实现序列化接口 (serializable)，因为二级缓存可能使用到序列化技术
3. 在对应的 `XxxMapper.xml` 中设置二级缓存的策略
	```xml
	<!--  
	配置二级缓存:  
	  1. FIFO 先进先出: 按对象进入缓存顺序移除  
	  2. flushInterval: 刷新间隔60000毫秒  
	  3. size="512": 引用数目,默认为1024  
	  4. readOnly="true" 只读属性（只用于读取、不修改,建议设置）  
	-->  
	<cache eviction="FIFO" flushInterval="60000" size="512" readOnly="true"/>
	```
### 9.2.2 注意事项和使用陷阱
1. 理解二级缓存策略的参数
	```xml
	<!--创建了 FIFO 的策略，每隔 30 秒刷新一次，最多存放 360 个对象而且返回的对象被认为是 只读的-->
	<cache eviction="FIFO" flushInterval="60000" size="512" readOnly="true"/>
	```
2. 四大策略
	1. LRU – 最近最少使用的: 移除最长时间不被使用的对象，它是默认
	2. FIFO – 先进先出: 按对象进入缓存的顺序来移除它们
	3. SOFT – 软引用: 移除基于垃圾回收器状态和软引用规则的对象
	4. WEAK – 弱引用: 更积极地移除基于垃圾收集器状态和弱引用规则的对象
3. 如何禁用二级缓存
	1. `mybatis-config.xml` 配置关闭
		```xml
		<settings>
		 <setting name="logImpl" value="STDOUT_LOGGING"/>
		  <!--全局性地开启或关闭所有映射器配置文件中已配置的任何缓存, 默认就是 true-->
		   <setting name="cacheEnabled" value="false"/>
		</settings>
		```
	2. `XxxMapper.xml` 注释 `<cache .../>`
	3. 或者更加细粒度的, 在配置方法上指定
		![[Pasted image 20230204084454.png]]
		设置 useCache=false 可以禁用当前 select 语句的二级缓存，即每次查询都会发出 sql 去查询，默认情况是 true，即该 sql 使用二级缓存。 
4. mybatis 刷新二级缓存的设置
	- insert、update、delete 操作数据后需要刷新缓存，如果不执行刷新缓存会出现脏读默认为 true，默认情况下为 true 即刷新缓存，一般不用修改。
		```xml
		<update id="updateMonster" parameterType="Monster" flushCache="true">
			UPDATE mybatis_monster SET NAME=#{name},age=#{age} WHERE id=#{id}
		</update>
		```

## 9.3 一级缓存和二级缓存执行顺序
- 缓存执行顺序是：二级缓存 -> 一级缓存 -> 数据库
- 不会出现一级缓存和二级缓存中有同一个数据。因为二级缓存 (数据) 是在一级缓存关闭之后才有的
```java
/**
* 分析缓存执行顺序: 二级缓存 -> 一级缓存 -> DB  
* 二级缓存(数据)是在一级缓存关闭之后才有的 
*/   
@Test  
public void cacheSeqTest2() {  
  
  System.out.println("查询第1次");  
  
  //DB , 会发出 SQL, cache hit ratio 0.0  Monster monster1 = monsterMapper.getMonsterById(3);  
  System.out.println(monster1);  
  
  // 这里我们没有关闭sqlSession  
  
  System.out.println("查询第2次");  
  //从一级缓存获取id=3 , cache hit ratio 0.0, 不会发出SQL  
  Monster monster2 = monsterMapper.getMonsterById(3);  
  System.out.println(monster2);  
  
  System.out.println("查询第3次");  
  //还是从一级缓存获取id=3, cache hit ratio 0.0, 不会发出SQL  
  Monster monster3 = monsterMapper.getMonsterById(3);  
  System.out.println(monster3);  
  
  if (sqlSession != null) sqlSession.close();  
  
  System.out.println("操作成功");  
}
```

## 9.4 EhCache 缓存
1. [EhCache](https://www.cnblogs.com/zqyanywn/p/10861103.html) 是一个纯 Java 的缓存框架，具有快速、精干等特点
2. MyBatis 有自己默认的二级缓存，但是在实际项目中，往往使用的是更加专业的第三方缓存产品作为 MyBatis 的二级缓存, EhCache 就是非常优秀的缓存产品
![[Pasted image 20230204094633.png]]

### 9.4.1 配置和使用 EhCache
- 加入相关依赖
	```xml
	<dependencies>  
	  <dependency>  
	    <groupId>net.sf.ehcache</groupId>  
	    <artifactId>ehcache</artifactId>  
	    <version>2.10.6</version>  
	  </dependency>  
	  <dependency>  
	    <groupId>org.slf4j</groupId>  
	    <artifactId>slf4j-api</artifactId>  
	    <version>1.7.25</version>  
	  </dependency>  
	  <dependency>  
	    <groupId>org.mybatis.caches</groupId>  
	    <artifactId>mybatis-ehcache</artifactId>  
	    <version>1.2.1</version>  
	  </dependency>  
	</dependencies>
	```
- `mybatis-config.xml` 仍然打开二级缓存
- 添加 `ehcache.xml` [配置文件](https://www.cnblogs.com/zqyanywn/p/10861103.html)

### 9.4.2 EhCache 缓存-细节说明
- 如何理解 EhCache 和 MyBatis 缓存的关系
	1. MyBatis 提供了一个接口 Cache【找到 `org.apache.ibatis.cache.Cache`, 关联源码包就可以看到 Cache 接口】<br> 
		![[Pasted image 20230204103428.png]]
	2. 只要实现了该 Cache 接口，就可以作为二级缓存产品和 MyBatis 整合使用, Ehcache 就是实现了该接口
	3. MyBatis 默认情况 (即一级缓存) 是使用的 PerpetualCache 类实现 Cache 接口的, 是核心类
	![[Pasted image 20230204103013.png]]
	4. 当我们使用了 Ehcahce 后，就是 EhcacheCache 类实现 Cache 接口的，是核心类.
	5. 缓存的本质就是 Map<Object,Object>
		![[Pasted image 20230204103748.png]]



