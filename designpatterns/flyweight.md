## 享元模式 

## 定义

1）享元模式（Flyweight Pattern）也叫蝇量模式 ：运用共享技术有效地支持大量细粒度的对象

 2）常用于系统底层开发，解决系统的性能问题。像数据库连接池，里面都是创建好的连接对象，在这些连接对象中有我们需要的则直接拿来用，避免重写创建，如果没有我们需要的，则创建一个

 3）享元模式能够解决重复对象的内存浪费的问题，当系统中有大量相似对象，需要缓冲池时。不需总是创建新对象，可以从缓冲池里拿。这样可以降低系统内存，同时提高效率。

 4）享元模式经典的应用场景就是池技术了，String常量池、数据库连接池、缓冲池等等都是享元模式的应用，享元模式是池技术的重要实现方式。


如果程序里使用大量相同或者相似的对象，造成内存的大量耗费，且大多都是外部状态，这时候就应该考虑使用享元模式了，比如围棋、五子棋等小游戏

## 场景 

![](http://www.jasongj.com/img/designpattern/flyweight/FlyWeight.png)

## 实例

小型的外包项目，给客户 A 做一个产品展示网站，客户 A 的朋友感觉效果不错，也希望做这样的产品展示网 站，但是要求都有些不同:

有客户要求以新闻的形式发布
有客户人要求以博客的形式发布
有客户希望以微信公众号的形式发布

```java


public abstract class WebSite {
	public abstract void use(User user);
}


public class ConcreteWebSite extends WebSite{
	
	/**
	 * 网站发布的形式（类型）
	 */
	private String tyep;

	/**
	 * 构造器
	 * @param type
	 */
	public ConcreteWebSite(String type) {
		// TODO Auto-generated constructor stub
		this.tyep = type;
	}
	
	@Override
	public void use(User user) {
		// TODO Auto-generated method stub
		System.out.println("网站的发布形式为 ：" + tyep + " 在使用中 。。 使用者是 " + user.getName());
	}

}

public class WebSiteFactory {
	
	/**
	 * 集合，充当池的作用
	 */
	private Map<String, ConcreteWebSite> poolMap = new HashMap<String, ConcreteWebSite>();

	/**
	 * 根据网站的类型，返回一个网站，如果没有就创建一个网站，并放入池中，并返回
	 * @param type
	 * @return
	 */
	public WebSite getWebSiteCategory(String type) {
		if (!poolMap.containsKey(type)) {
			// 就创建一个网站，并放入池中
			poolMap.put(type, new ConcreteWebSite(type));
		}
		return (WebSite)poolMap.get(type);
	}
	
	/**
	 * 获取网站分类的总数（池中有多少个网站类型）
	 * @return
	 */
	public int getWebSiteCount() {
		return poolMap.size();
	}
}


public class User {
	
	private String name;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public User(String name) {
		super();
		this.name = name;
	}

}


public class Client {

	public static void main(String[] args) {

		// 创建一个工厂类
		WebSiteFactory factory = new WebSiteFactory();

		// 客户要一个以新闻形式发布的网站
		WebSite webSite = factory.getWebSiteCategory("新闻");
		webSite.use(new User("tom"));

		// 客户要一个以博客形式发布的网站
		WebSite webSite2 = factory.getWebSiteCategory("博客");
		webSite2.use(new User("jack"));

		// 客户要一个以博客形式发布的网站
		WebSite webSite3 = factory.getWebSiteCategory("博客");
		webSite3.use(new User("titm"));
	}

}

```