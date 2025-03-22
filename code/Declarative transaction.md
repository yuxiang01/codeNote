```xml
<context:component-scan base-package="spring.tx.dao"/>  
<context:component-scan base-package="spring.tx.service"/>  
<context:property-placeholder location="classpath:goods.properties"/>  
<!--配置数据源对象-DateSource-->  
<bean class="com.mchange.v2.c3p0.ComboPooledDataSource" id="dataSource2">  
  <property name="user" value="${jdbc.user}"/>  
  <property name="driverClass" value="${jdbc.driver}"/>  
  <property name="jdbcUrl" value="${jdbc.url}"/>  
  <property name="password" value="${jdbc.password}"/>  
</bean>  
<!--配置JdbcTemplate对象-->  
<bean class="org.springframework.jdbc.core.JdbcTemplate">  
  <property name="dataSource" ref="dataSource2"/>  
</bean>  
<!--配置事务管理对象-->  
<bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" id="transactionManager">  
  <property name="dataSource" ref="dataSource2"/>  
</bean>  
<!--开启基于注解的声明式事务功能-->  
<tx:annotation-driven transaction-manager="transactionManager"/>
```

```java
@Repository  
public class GoodsDao {  
  @Resource  
  private JdbcTemplate jdbcTemplate;  
  
  public Float queryPriceById(Integer id) {  
    String sql = "SELECT price From goods Where goods_id=?";  
    Float price = jdbcTemplate.queryForObject(sql, Float.class, id);  
    return price;  
  }  
  
  public void decreaseBalance(Integer user_id, Float money) {  
    String sql = "UPDATE user_account SET money=money-? Where user_id=?";  
    jdbcTemplate.update(sql, money, user_id);  
  }  
  
  public void decreaseAmount(Integer goods_id, int amount) {  
    String sql = "UPDATE goods_amount SET goods_num=goods_num-? Where goods_id =?";  
    jdbcTemplate.update(sql, amount, goods_id);  
  }  
}
```

```java
@Service  
public class GoodsService {  
  @Resource  
  private GoodsDao goodsDao;  
  
  /*  
   1. 使用@Transactional 可以进行声明式事务控制(即将标识的方法中的，对数据库的操作作为一个事务管理)  
   2. @Transactional 底层使用的仍然是AOP机制,再使用动态代理对象来调用buyGoods  
   3. 在执行buyGoodsByTx() 方法 先调用 事务管理器的 doBegin(), 调用 buyGoods()
	   如果执行没有发生异常，则调用 事务管理器的 doCommit()
	   如果发生异常 调用事务管理器的 doRollback()  
   */  
  @Transactional  
  public void buyGoods(int userId, int goodsId, int amount) {  
    Float price = goodsDao.queryPriceById(goodsId);  
    goodsDao.decreaseBalance(userId, price * amount);  
    goodsDao.decreaseAmount(goodsId, amount);  
  }  
}
```

```java
@Test  
public void buyGoods(){  
  ApplicationContext ioc = new ClassPathXmlApplicationContext("tx.xml");  
  GoodsService goodsService = ioc.getBean(GoodsService.class);  
  goodsService.buyGoods(1,1,1);  
}
```