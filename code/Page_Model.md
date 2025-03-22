- `page` 实体类

	![[Pasted image 20221219144926.png]]
```java
public class Page<T> implements Serializable {  
  private static final long serialVersionUID = 3047980899233088614L;  
  public static final Integer PAGE_SIZE = 3;  
  // 当前页  
  private Integer pageIndex;  
  // 每页展示的数据量  
  private Integer pageSize = PAGE_SIZE;  
  // 共有多少页 = [pageTotal / pageSize]
  private Integer totalPage;  
  // 总共行数, 从DB来 => DAO
  private Integer totalCount;  
  // 当前页,要显示的数据, 从DB来 => DAO  
  private List<T> list;  
  // 分页导航的字符串  
  private String url;  
  
  public Integer getPageNo() {  
    return pageNo;  
  }  
  
  public void setPageNo(Integer pageNo) {  
    this.pageNo = pageNo;  
  }  
  
  public Integer getPageSize() {  
    return pageSize;  
  }  
  
  public void setPageSize(Integer pageSize) {  
    this.pageSize = pageSize;  
  }  
  
  public Integer getPageTotal() {  
    return pageTotal;  
  }  
  
  public void setPageTotal(Integer pageTotal) {  
    this.pageTotal = pageTotal;  
  }  
  
  public Integer getTotalRow() {  
    return totalRow;  
  }  
  
  public void setTotalRow(Integer totalRow) {  
    this.totalRow = totalRow;  
  }  
  
  public List<T> getItems() {  
    return items;  
  }  
  
  public void setItems(List<T> items) {  
    this.items = items;  
  }  
  
  public String getUrl() {  
    return url;  
  }  
  
  public void setUrl(String url) {  
    this.url = url;  
  }  
}
```

> Page的哪些属性是可以直接从数据库中获取,就把这个初始化任务放在DAO层

- Dao接口
```java
public interface FurnDao {
  List<Furn> getPageItems(int begin, int pageSize);  
  
  List<Furn> getPageItemsByName(String name, int begin, int pageSize);  
}
```

- Dao实现类
```java
public class FurnDaoImpl extends BasicDao<Furn> implements FurnDao {
  @Override  
  public List<Furn> getPageItems(int begin, int pageSize) {  
    String sql = "SELECT id,`name`,maker,price,sales,stock," +  
        "img_path imgPath FROM `furn` LIMIT ?,?;";  
    return query(sql, Furn.class, begin, pageSize);  
  }  
  
  @Override  
  public List<Furn> getPageItemsByName(String name, int begin, int pageSize) {  
    String sql = "SELECT id,`name`,maker,price,sales,stock," +  
        "img_path imgPath FROM `furn` WHERE name Like ? LIMIT ?,?;";  
    return query(sql, Furn.class, "%" + name + "%", begin, pageSize);  
  }  
}
```

- service接口
```java
public interface FurnService {
  Page<Furn> page(int pageNo, int pageSize);  
  
  Page<Furn> pageByName(String name, int pageNo, int pageSize);  
}
```

- service实现类
```java
public class FurnServiceImpl implements FurnService {  
  FurnDao furnDao = new FurnDaoImpl();
  @Override  
  public Page<Furn> page(int pageNo, int pageSize) {  
    Page<Furn> furnPage = new Page<>();  
    // 当前页  
    furnPage.setPageNo(pageNo);  
    // 每页展示数  
    furnPage.setPageSize(pageSize);  
    // 总共行数  
    int totalRow = furnDao.getTotalRow();  
    furnPage.setTotalRow(totalRow);  
    // 总页数 = 总共行数 / 每页展示数  
    int pageTotal = totalRow / pageSize;  
    if (totalRow % pageSize > 0) pageTotal += 1;
    int pageTotal = totalRow % pageSize == 0 ? totalRow / pageSize : (totalRow / pageSize) +1
    furnPage.setPageTotal(pageTotal);  
    // Limit 分页查询  
    int page = (pageNo - 1) * pageSize;  
    List<Furn> pageItems = furnDao.getPageItems(page, pageSize);  
    furnPage.setItems(pageItems);  
    return furnPage;  
  }  
  
  @Override  
  public Page<Furn> pageByName(String name, int pageNo, int pageSize) {  
    Page<Furn> furnPage = new Page<>();  
    // 当前页  
    furnPage.setPageNo(pageNo);  
    // 每页展示数  
    furnPage.setPageSize(pageSize);  
    // 总共行数  
    int totalRow = furnDao.getTotalRowByName(name);  
    furnPage.setTotalRow(totalRow);  
    // 总页数 = 总共行数 / 每页展示数  
    int pageTotal = totalRow / pageSize;  
    if (totalRow % pageSize > 0) pageTotal += 1;  
    furnPage.setPageTotal(pageTotal);  
    // Limit 分页查询  
    int page = (pageNo - 1) * pageSize;  
    List<Furn> pageItems = furnDao.getPageItemsByName(name, page, pageSize);  
    furnPage.setItems(pageItems);  
    return furnPage;  
  }  
}
```