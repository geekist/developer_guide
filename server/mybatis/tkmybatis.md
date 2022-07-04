# 一、tkmybatis介绍

Tkmybatis 是基于 Mybatis 框架开发的一个工具，通过调用它提供的方法实现对单表的数据操作，不需要写任何 sql 语句，这极大地提高了项目开发效率。

官方文档：https://github.com/abel533/Mapper/wiki/1.integration

官方使用说明：https://blog.csdn.net/isea533/article/details/83045335

# 二、使用tkmybatis

## 添加依赖引用

在 pom.xml 中引入 tk.mybatis 的引用。
```xml
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-spring-boot-starter</artifactId>
    <version>2.1.5</version>
</dependency>
```

## 创建DO 对象
```java
@Table(name = "t_plan")
public class PopMerchantPlanDO{

	/**
	 * id
	 */
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
	/**
	 * 编号
	 */
	private String code;
}
```

用@Table注解映射数据库表和实体对象，类似的注解还有 @Column、@ColumnType、@Transient 等等。

## 实现Mapper接口继承tkmybatis的BaseMapper
```java
public interface BaseDao<T> extends BaseMapper<T>, MySqlMapper<T>, IdsMapper<T>, ConditionMapper<T>, ExampleMapper<T> {
}
```

Dao 对象继承的这些接口帮你封装了一系列单表的操作，使你不用再为每个单表编写繁杂重复的 sql 语句。

当你使用 Dao 对象操作数据库的时候，只需要调用这些接口中封装好的方法，就能完成一系列对单表的 CURD 操作，比如下面的 selectAll 方法，鉴于篇幅，更多的 CURD 方法，大家可以打开 Mapper 接口自行查看，一目了然。



## 实现原理

```java
@RegisterMapper
public interface SelectAllMapper<T> {

    /**
     * 查询全部结果
     *
     * @return
     */
    @SelectProvider(type = BaseSelectProvider.class, method = "dynamicSQL")
    List<T> selectAll();
}
```

## 使用
实际使用中，可以直接注入mapper
```java
public class SpringXmlTest {

    private ClassPathXmlApplicationContext context;

    @Test
    public void testCountryMapper() {
        context = new ClassPathXmlApplicationContext("tk/mybatis/mapper/xml/spring.xml");
        CountryMapper countryMapper = context.getBean(CountryMapper.class);
		//获取全部信息
        List<Country> countries = countryMapper.selectAll();
        Assert.assertNotNull(countries);
        Assert.assertEquals(183, countries.size());
    }
}
```

# 三、总结

对于复杂的 SQL 语句，比如 JOIN 操作等等，仍然需要自行编写 xml 文件和 SQL 语句。

Tkmybatis 减少了一系列对单表的单调接口和繁杂的 SQL 语句，但也降低了代码的可读性，另外这种广而全的数据库操作，必然会牺牲一部分的查询性能。

可以在一些并发量不大、对性能要求不高的项目中尝试下 Tkmybatis ，对一些比较大的项目来说，还是希望自己对 SQL 的控制更强一点。