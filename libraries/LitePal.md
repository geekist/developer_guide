LitePal

## github地址：https://github.com/guolindev/LitePal

## 1、依赖项添加：

dependencies {

    implementation 'org.litepal.guolindev:core:3.2.2'

}

## 2、配置litepal.xml
接着在项目的assets目录下面新建一个litepal.xml文件，并将以下代码拷贝进去：

```java

<?xml version="1.0" encoding="utf-8"?>
<litepal>
    <dbname value="demo" ></dbname>
 
    <version value="1" ></version>
 
    <list>
    </list>
</litepal>

```
配置文件相当简单，<dbname>用于设定数据库的名字，<version>用于设定数据库的版本号，<list>用于设定所有的映射模型，我们稍后就会用到。
项目的assets目录是指main/assets,这个值是在工程创建的时候已经确定的，放置APP所需的固定文件，且该文件被打包到APK中时，不会被编码到二进制文件。访问时需要用AssetManager来访问。

## 3、配置LitePalApplication

新建一个MyApplication继承自LitepalApplication,主要用于得到context。


```java

public class MyApplication  extends LitePalApplication {
}

```

```java

<manifest>
    <application
        android:name="com.example.MyApplication"
        ...
    >
    ...
    </application>
</manifest>

```

## 4、建立表格

```java

public class News {
   // private int id;

    private String title;

    private String content;

    private String commentCount;
}




```

```java
<?xml version="1.0" encoding="utf-8"?>
<litepal>
    <dbname value="demo" ></dbname>
 
    <version value="1" ></version>
 
    <list>
        <mapping class="com.example.databasetest.model.News"></mapping>
    </list>
</litepal>


```

## 5、版本升级
版本升级非常简单，只需要在xml中更改版本号就可以了。

## 6、表的关联
一对一关联的实现方式是用外键，多对一关联的实现方式也是用外键，多对多关联的实现方式是用中间表
例如：news和comment是一对多，news和introduction是一对一，news和category是多对多

```java
public class Introduction {
	
	private int id;
	
	private String guide;
	
	private String digest;

	//private News news;
	
	// 自动生成get、set方法
}

```

```java
public class Comment {
	
	private int id;
	
	private String content;

	private long publishDate;

	private News news;
	
	// 自动生成get、set方法
}
```

```java
public class Category {
	
	private int id;
	
	private String name;

	private List<News> newsList = new ArrayList<News>();//多对多的关系

	
	// 自动生成get、set方法

```

```java

public class News {
   // private int id;

    private String title;

    private String content;

    private String commentCount;

	private Introduction introduction;  //一对一的关系
	
	private List<Comment> commentList = new ArrayList<Comment>();  //一对多的关系

	private List<Category> categoryList = new ArrayList<Category>();//多对多的关系

}
```

## 6、增删改查操作


- 增加obj.save();或 obj.saveThrow();

- 修改用update  或updateAll


```java
//将news表中id是2的数据项的title改为xxx
ContentValues values = new ContentValues();
values.put("title", "今日iPhone6发布");
DataSupport.update(News.class, values, 2);

//将news表中title = xxx的数据的title改为xxx
ContentValues values = new ContentValues();
values.put("title", "今日iPhone6 Plus发布");
DataSupport.updateAll(News.class, values, "title = ?", "今日iPhone6发布");

//另外的用法
DataSupport.updateAll(News.class, values, "title = ? and commentcount > ?", "今日iPhone6发布", "0");

//将所有数据项都修改
ContentValues values = new ContentValues();
values.put("title", "今日iPhone6 Plus发布");
DataSupport.updateAll(News.class, values);

```

- 删除用delete 或deleteAll

```java

//删除id是2的记录
DataSupport.delete(News.class, 2);

//条件删除
DataSupport.deleteAll(News.class, "title = ? and commentcount = ?", "今日iPhone6发布", "0");

//全部删除
DataSupport.deleteAll(News.class);

```

- 查找用find

```java

//
News news = DataSupport.find(News.class, 1);

//
News firstNews = DataSupport.findFirst(News.class);


//
News lastNews = DataSupport.findLast(News.class);

//
List<News> newsList = DataSupport.findAll(News.class, 1, 3, 5, 7);

//
long[] ids = new long[] { 1, 3, 5, 7 };
List<News> newsList = DataSupport.findAll(News.class, ids);



//
List<News> allNews = DataSupport.findAll(News.class);


//
List<News> newsList = DataSupport.where("commentcount > ?", "0").find(News.class);

//
List<News> newsList = DataSupport.select("title", "content")
		.where("commentcount > ?", "0")
		.order("publishdate desc").find(News.class);

//
List<News> newsList = DataSupport.select("title", "content")
		.where("commentcount > ?", "0")
		.order("publishdate desc").limit(10).offset(10)
		.find(News.class);

//查找关联数据
News news = DataSupport.find(News.class, 1, true);
List<Comment> commentList = news.getCommentList();

```

## 7、聚合查询

- count()

int result = DataSupport.count(News.class);
int result = DataSupport.where("commentcount = ?", "0").count(News.class);

- sum()

int result = DataSupport.sum(News.class, "commentcount", int.class);

- average()

double result = DataSupport.average(News.class, "commentcount");

- max()
int result = DataSupport.max(News.class, "commentcount", int.class);

- min()
int result = DataSupport.min(News.class, "commentcount", int.class);













