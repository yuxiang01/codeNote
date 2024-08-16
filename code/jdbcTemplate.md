# jdbcTemplate 使用

1. 通过 Spring 可以配置数据源，从而完成对数据表的操作
2. JdbcTemplate 是 Spring 提供的访问数据库的技术。可以将 JDBC 的常用操作封装为模板方法。

## 1. 创建配置文件 `src/jdbc.properties` 
```properties
jdbc.user = root  
jdbc.password = admin  
jdbc.driver = com.mysql.cj.jdbc.Driver  
jdbc.url = jdbc:mysql://localhost:3306/MHL?serverTimezone=GMT%2B8&Unicode=true&characterEncoding=utf-8
```

## 2. 创建配置文件 `src/JdbcTemplate_ioc.xml`
```xml
<!--配置扫描的包-->  
<context:component-scan base-package="spring.jdbcTemplate.dao"/>  
<!--引入mysql.properties配置文件-->  
<context:property-placeholder location="classpath:mysql.properties"/>  
<!--配置数据源对象-DateSource-->  
<bean class="com.mchange.v2.c3p0.ComboPooledDataSource" id="dataSource">  
  <!--配置数据源对象属性-->  
  <property name="user" value="${jdbc.user}"/>  
  <property name="password" value="${jdbc.password}"/>  
  <property name="jdbcUrl" value="${jdbc.url}"/>  
  <property name="driverClass" value="${jdbc.driver}"/>  
</bean>  
<!--配置JdbcTemplate对象-->  
<bean class="org.springframework.jdbc.core.JdbcTemplate">  
  <property name="dataSource" ref="dataSource"/>  
</bean>  
<!-- 配置 NamedParameterJdbcTemplate,支持具名参数 -->  
<bean class="org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate">  
  <constructor-arg name="dataSource" ref="dataSource"/>  
</bean>
```

## 3. 测试连接
```java
public void TestConnection() throws SQLException {  
  ApplicationContext ioc = new ClassPathXmlApplicationContext("JdbcTemplate.xml");  
  DataSource dataSource = ioc.getBean(DataSource.class);  
  Connection connection = dataSource.getConnection();  
  System.out.println("获取到连接：" + connection);  
  connection.close();  
  System.out.println("ok");  
}
```

## 4. 添加数据
```java
public void addDataByJdbcTemplate() {  
  ApplicationContext ioc = new ClassPathXmlApplicationContext("JdbcTemplate.xml");  
  JdbcTemplate jdbcTemplate = ioc.getBean(JdbcTemplate.class);  
  // 添加方式1  
  String sql = "INSERT INTO `Employee` (`pwd`, `name`, `job`) VALUES ('12345', 'fish', '副经理');";  
  jdbcTemplate.execute(sql);  
  //  添加方式2  
  String sql = "INSERT INTO `Employee` (`empId`,`pwd`, `name`, `job`) VALUES (?, ?, ?, ?);";  
  jdbcTemplate.update(sql, "20221110", "12345", "范肆", "经理");  
  System.out.println("add ok...");  
}
```

## 5. 修改数据
```java
public void updateDataByJdbcTemplate() {  
  ApplicationContext ioc = new ClassPathXmlApplicationContext("JdbcTemplate.xml");  
  JdbcTemplate jdbcTemplate = ioc.getBean(JdbcTemplate.class);  
  String sql = "UPDATE `Employee` SET `empId` = ? WHERE `id` = ?;";  
  int rows = jdbcTemplate.update(sql, "20211111", 5);  
  System.out.println("update ok... 影响行数：" + rows);  
}
```

## 6. 批量处理数据
```java
public void addBatchDataByJdbcTemplate() {  
  ApplicationContext ioc = new ClassPathXmlApplicationContext("JdbcTemplate.xml");  
  JdbcTemplate jdbcTemplate = ioc.getBean(JdbcTemplate.class);  
  String sql = "INSERT INTO `Employee` (`empId`,`pwd`, `name`, `job`) VALUES (?, ?, ?, ?);";  
  List<Object[]> listArgs = new ArrayList<>();  
  listArgs.add(new Object[]{"001", "12345", "小龙女", "古墓派创始人"});  
  listArgs.add(new Object[]{"002", "12345", "杨过", "阳过杨过"});  
  int[] ints = jdbcTemplate.batchUpdate(sql, listArgs);  
  for (int anInt : ints) {  
    System.out.println("anInt = " + anInt);  
  }  
  System.out.println("Batch add ok...");  
}
```

## 7. 查询单行数据并封装成对象
```java
public void queryDataByJdbcTemplate() {  
  ApplicationContext ioc = new ClassPathXmlApplicationContext("JdbcTemplate.xml");  
  JdbcTemplate jdbcTemplate = ioc.getBean(JdbcTemplate.class);  
  String sql = "SELECT id,empId,pwd,`name`,job FROM Employee WHERE id = 8";  
  RowMapper<Employee> rowMapper = new BeanPropertyRowMapper<>(Employee.class);  
  Employee employee = jdbcTemplate.queryForObject(sql, rowMapper);  
  System.out.println("employee = " + employee);  
  System.out.println("查询ok...");  
}
```

## 8. 查询多行数据、封装并返回集合
```java
public void queryMulDataByJdbcTemplate() {  
  ApplicationContext ioc = new ClassPathXmlApplicationContext("JdbcTemplate.xml");  
  JdbcTemplate jdbcTemplate = ioc.getBean(JdbcTemplate.class);  
  String sql = "SELECT id,empId,pwd,`name`,job FROM Employee WHERE id >= ?";  
  RowMapper<Employee> rowMapper = new BeanPropertyRowMapper<>(Employee.class);  
  List<Employee> employeeList = jdbcTemplate.query(sql, rowMapper, 3);  
  for (Employee employee : employeeList) {  
    System.out.println(employee);  
  }  
}
```

## 9. 查询单行单列
```java
public void queryScalarByJdbcTemplate() {  
  ApplicationContext ioc = new ClassPathXmlApplicationContext("JdbcTemplate.xml");  
  JdbcTemplate jdbcTemplate = ioc.getBean(JdbcTemplate.class);  
  String sql = "SELECT `name` FROM Employee WHERE id = ?";  
  String name = jdbcTemplate.queryForObject(sql, String.class, 3);  
  System.out.println("name = " + name);  
}
```

## 10. 具名参数
具名参数形式需要使用 NamedParameterJdbcTemplate 类 (先去配置)
```java
public void DataByNamedParameterJdbcTemplate() {  
  ApplicationContext ioc = new ClassPathXmlApplicationContext("JdbcTemplate.xml");  
  NamedParameterJdbcTemplate namedParameterJdbcTemplate  
      = ioc.getBean(NamedParameterJdbcTemplate.class);  
  String sql = "INSERT INTO `Employee` (`empId`,`pwd`, `name`, `job`) VALUES (:empId,:pwd,:name,:job);";  
  Map<String, Object> map_parameter = new HashMap<>();  
  map_parameter.put("empId", "20111110");  
  map_parameter.put("pwd", "10010");  
  map_parameter.put("name", "莉丝");  
  map_parameter.put("job", "服务员");  
  namedParameterJdbcTemplate.update(sql, map_parameter);  
  System.out.println("add data ok~");  
}
```

## 11. 使用 sqlparametersoruce 来封装具名参数
```java
public void operDataBySqlparametersoruce() {  
  ApplicationContext ioc = new ClassPathXmlApplicationContext("JdbcTemplate.xml");  
  NamedParameterJdbcTemplate namedParameterJdbcTemplate  
      = ioc.getBean(NamedParameterJdbcTemplate.class);  
  String sql = "INSERT INTO `Employee` (`empId`,`pwd`, `name`, `job`) VALUES (:empId,:pwd,:name,:job);";  
  Employee employee = new Employee("003", "111222", "张三", "前台");  
  SqlParameterSource sqlParameterSource = new BeanPropertySqlParameterSource(employee);  
  int rows = namedParameterJdbcTemplate.update(sql, sqlParameterSource);  
}
```

## 12. Dao 对象中使用 JdbcTemplate 完成对数据的操作
```java
@Repository  
public class EmployeeDao {  
  @Resource  
  private JdbcTemplate jdbcTemplate;  
  
  public void save(Employee employee) {  
    String sql = "INSERT INTO `Employee` (`empId`,`pwd`, `name`, `job`) VALUES (?, ?, ?, ?);";  
    int row = jdbcTemplate.update  
        (sql, employee.getEmpId(), employee.getPwd(), employee.getName(), employee.getJob());  
    System.out.println("受影响行数：" + row);  
  }  
}
```

```java
public void employeeDaoTest() {  
  ApplicationContext ioc = new ClassPathXmlApplicationContext("JdbcTemplate.xml");  
  EmployeeDao employeeDao = ioc.getBean(EmployeeDao.class);  
  Employee employee = new Employee("004", "111222", "李四", "前厅");  
  employeeDao.save(employee);  
}
```