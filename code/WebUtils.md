# JavaWeb工具类

## 1. 获取时间

```java
public static String getYearMonthDay() {
	LocalDateTime ldt = new LocalDateTime.now();
	int year = ldt.getYear();  
	int month = ldt.getMonthValue();  
	int day = ldt.getDayOfMonth();  
	return String.format("%d/%d/%d/", year, month, day);
}
```

^2e41b9
