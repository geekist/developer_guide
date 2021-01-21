
# 访问者

## 定义

访问者模式（VisitorPattern），顾名思义使用了这个模式后就可以在不修改已有程序结构的前提下，通过添加额外的访问者来完成对已有代码功能的提升，它属于行为模式。访问者模式的目的是封装一些施加于某种数据结构元素之上的操作。一旦这些操作需要修改的话，接受这个操作的数据结构则可以保持不变。

其主要目的是将数据结构与数据操作分离。

## 场景

![](https://img-blog.csdnimg.cn/20181105203837733.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FhendzeHBjbQ==,size_16,color_FFFFFF,t_70)

## 实例
图书馆有一台电脑，有两个账户，其中一个是管理员的账户，拥有所有权限，但是设置了密码；另一个账户是不需要密码，但是只能玩游戏和看图片。张三和李四先后使用了这台电脑，那么他们就可以当作是访问者。
那么我们便可以根据这里例子来使用访问者模式进行开发，首先定义一个抽象的访问者，拥有玩游戏和看图片的方法；然后再定义一个抽象节点电脑，接受这个请求。

```java
interface Visitor {
   void visit(Games games);
   void visit(Photos photos);
}

interface Computer {
   void accept(Visitor visitor);
}

定义好该抽象类之后，我们需要设计不同的访问者对节点进行不同的处理。并且需要设计具体节点类实现刚刚抽象节点的方法。

那么代码如下:

class ZhangSan implements Visitor {
   @Override
   public void visit(Games games) {
   	games.play();
   }

   @Override
   public void visit(Photos photos) {
   	photos.watch();
   }
}

class LiSi implements Visitor {
   @Override
   public void visit(Games games) {
   	games.play();
   }
   @Override
   public void visit(Photos photos) {
   	photos.watch();
   }
}

class Games implements Computer {
   @Override
   public void accept(Visitor visitor) {
   	visitor.visit(this);
   }

   public void play() {
   	System.out.println("play lol");
   }
}

class Photos implements Computer {
   @Override
   public void accept(Visitor visitor) {
   	visitor.visit(this);
   }
   
   public void watch() {
   	System.out.println("watch scenery photo");
   }
}

最后我们还需要定义一个结构对象角色，提供一个的接口并允许该访问者进行访问，它可以对这些角色进行增加、修改或删除等操作和遍历。
代码如下:

class ObjectStructure {

	private List<Computer> computers = new ArrayList<Computer>();

	public void action(Visitor visitor) {
		computers.forEach(c -> {
			c.accept(visitor);
		});
	}
	public void add(Computer computer) {
		computers.add(computer);
	}
}
编写好之后，那么我们来进行测试。
测试代码如下:


public static void main(String[] args) {
   	// 创建一个结构对象
   	ObjectStructure os = new ObjectStructure();
   	// 给结构增加一个节点
   	os.add(new Games());
   	// 给结构增加一个节点
   	os.add(new Photos());
   	// 创建一个访问者
   	Visitor visitor = new ZhangSan();
   	os.action(visitor);

}


```