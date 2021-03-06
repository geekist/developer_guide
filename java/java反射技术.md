# Java反射技术详解

# 什么是反射？
反射是Java的特征之一，是一种间接操作目标对象的机制，核心是JVM在运行的时候才动态加载类，并且对于任意一个类，都能够知道这个类的所有属性和方法，调用方法/访问属性，不需要提前在编译期知道运行的对象是谁，他允许运行中的Java程序获取类的信息，并且可以操作类或对象内部属性。程序中对象的类型一般都是在编译期就确定下来的，而当我们的程序在运行时，可能需要动态的加载一些类，这些类因为之前用不到，所以没有加载到jvm，这时，使用Java反射机制可以在运行期动态的创建对象并调用其属性，它是在运行时根据需要才加载。

# 反射的原理是什么？
首先我们需要回顾一下java语言的编译和运行过程。在Java编程语言中，所有源代码首先都以纯文本文件编写，扩展名为.java。然后，这些源文件由javac编译器编译为.class文件。 .class文件是包含Java虚拟机（JVM）字节码的机器语言。程序运行时，JVM的实例首先启动，然后装载class文件到内存中开始运行。

![软件开发过程](https://img-blog.csdnimg.cn/20201016145652459.png#pic_center)
Java反射就是当程序运行时，在内存中获得class对象后，反向获取该class对象的类的属性和方法等各种信息。下面的图片来源于[Java反射介绍](https://blog.csdn.net/grandgrandpa/article/details/84832343)一文，详细描述了java反射原理。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016145940918.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lhbmd3dTAwNw==,size_16,color_FFFFFF,t_70#pic_center)

# 为什么要使用反射技术？
反射是框架设计的灵魂。增加程序的灵活性
 
反射的应用很多，很多框架都有用到
反射被广泛地用于那些需要在运行时检测或修改程序行为的程序中。这是一个相对高级的特性，只有那些语言基础非常扎实的开发者才应该使用它。如果能把这句警示时刻放在心里，那么反射机制就会成为一项强大的技术，可以让应用程序做一些几乎不可能做到的事情。
spring 的 ioc/di 也是反射....
javaBean和jsp之间调用也是反射....
struts的 FormBean 和页面之间...也是通过反射调用....
JDBC 的 classForName()也是反射.....
hibernate的 find(Class clazz) 也是反射....

1、反编译：.class-->.java

2、通过反射机制访问java对象的属性，方法，构造方法等

3、当我们在使用IDE,比如Ecplise时，当我们输入一个对象或者类，并想调用他的属性和方法是，一按点号，编译器就会自动列出他的属性或者方法，这里就是用到反射。

4、反射最重要的用途就是开发各种通用框架。比如很多框架（Spring）都是配置化的（比如通过XML文件配置Bean），为了保证框架的通用性，他们可能需要根据配置文件加载不同的类或者对象，调用不同的方法，这个时候就必须使用到反射了，运行时动态加载需要的加载的对象。

5、例如，在使用Strut2框架的开发过程中，我们一般会在struts.xml里去配置Action，比如

<action name="login" class="org.ScZyhSoft.test.action.SimpleLoginAction" method="execute">   
    <result>/shop/shop-index.jsp</result>           
    <result name="error">login.jsp</result>       
</action>
比如我们请求login.action时，那么StrutsPrepareAndExecuteFilter就会去解析struts.xml文件，从action中查找出name为login的Action，并根据class属性创建SimpleLoginAction实例，并用Invoke方法来调用execute方法，这个过程离不开反射。配置文件与Action建立了一种映射关系，当View层发出请求时，请求会被StrutsPrepareAndExecuteFilter拦截，然后StrutsPrepareAndExecuteFilter会去动态地创建Action实例。

比如，加载数据库驱动的，用到的也是反射。

# 如何使用反射技术？

五、反射机制常用的类：
Java.lang.Class;
Java.lang.reflect.Constructor;
Java.lang.reflect.Field;
Java.lang.reflect.Method;
Java.lang.reflect.Modifier;


六、反射的基本使用：
1、获得Class：主要有三种方法：
（1）Object-->getClass
（2）任何数据类型（包括基本的数据类型）都有一个“静态”的class属性
（3）通过class类的静态方法：forName(String className)（最常用）

package fanshe;
public class Fanshe {
	public static void main(String[] args) {
		//第一种方式获取Class对象  
		Student stu1 = new Student();//这一new 产生一个Student对象，一个Class对象。
		Class stuClass = stu1.getClass();//获取Class对象
		System.out.println(stuClass.getName());
		
		//第二种方式获取Class对象
		Class stuClass2 = Student.class;
		System.out.println(stuClass == stuClass2);//判断第一种方式获取的Class对象和第二种方式获取的是否是同一个
		
		//第三种方式获取Class对象
		try {
			Class stuClass3 = Class.forName("fanshe.Student");//注意此字符串必须是真实路径，就是带包名的类路径，包名.类名
			System.out.println(stuClass3 == stuClass2);//判断三种方式是否获取的是同一个Class对象
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
		
	}
}

注意，在运行期间，一个类，只有一个Class对象产生，所以打印结果都是true；
三种方式中，常用第三种，第一种对象都有了还要反射干什么，第二种需要导入类包，依赖太强，不导包就抛编译错误。一般都使用第三种，一个字符串可以传入也可以写在配置文件中等多种方法。


2、判断是否为某个类的示例：

一般的，我们使用instanceof 关键字来判断是否为某个类的实例。同时我们也可以借助反射中Class对象的isInstance()方法来判断时候为某个类的实例，他是一个native方法。

public native boolean isInstance(Object obj);
 
3、创建实例：通过反射来生成对象主要有两种方法：

（1）使用Class对象的newInstance()方法来创建Class对象对应类的实例。

Class<?> c = String.class;
Object str = c.newInstance();

（2）先通过Class对象获取指定的Constructor对象，再调用Constructor对象的newInstance()方法来创建对象，这种方法可以用指定的构造器构造类的实例。

//获取String的Class对象
Class<?> str = String.class;
//通过Class对象获取指定的Constructor构造器对象
Constructor constructor=c.getConstructor(String.class);
//根据构造器创建实例：
Object obj = constructor.newInstance(“hello reflection”);
 
4、通过反射获取构造方法并使用：

（1）批量获取的方法：
public Constructor[] getConstructors()：所有"公有的"构造方法
public Constructor[] getDeclaredConstructors()：获取所有的构造方法(包括私有、受保护、默认、公有)

（2）单个获取的方法，并调用：
public Constructor getConstructor(Class... parameterTypes):获取单个的"公有的"构造方法：
public Constructor getDeclaredConstructor(Class... parameterTypes):获取"某个构造方法"可以是私有的，或受保护、默认、公有；

（3） 调用构造方法：

Constructor-->newInstance(Object... initargs)
newInstance是 Constructor类的方法（管理构造函数的类）

api的解释为：newInstance(Object... initargs) ，使用此 Constructor 对象表示的构造方法来创建该构造方法的声明类的新实例，并用指定的初始化参数初始化该实例。
它的返回值是T类型，所以newInstance是创建了一个构造方法的声明类的新实例对象，并为之调用。

例子：
Student类：共六个构造方法。

package fanshe;
public class Student {
	//---------------构造方法-------------------
	//（默认的构造方法）
	Student(String str){
		System.out.println("(默认)的构造方法 s = " + str);
	}
	//无参构造方法
	public Student(){
		System.out.println("调用了公有、无参构造方法执行了。。。");
	}
	//有一个参数的构造方法
	public Student(char name){
		System.out.println("姓名：" + name);
	}
	//有多个参数的构造方法
	public Student(String name ,int age){
		System.out.println("姓名："+name+"年龄："+ age);//这的执行效率有问题，以后解决。
	}
	//受保护的构造方法
	protected Student(boolean n){
		System.out.println("受保护的构造方法 n = " + n);
	}
	//私有构造方法
	private Student(int age){
		System.out.println("私有的构造方法   年龄："+ age);
	}
}
测试类：

package fanshe;
import java.lang.reflect.Constructor;
 
/*
 * 通过Class对象可以获取某个类中的：构造方法、成员变量、成员方法；并访问成员；
 * 
 * 1.获取构造方法：
 * 		1).批量的方法：
 * 			public Constructor[] getConstructors()：所有"公有的"构造方法
            public Constructor[] getDeclaredConstructors()：获取所有的构造方法(包括私有、受保护、默认、公有)
 * 		2).获取单个的方法，并调用：
 * 			public Constructor getConstructor(Class... parameterTypes):获取单个的"公有的"构造方法：
 * 			public Constructor getDeclaredConstructor(Class... parameterTypes):获取"某个构造方法"可以是私有的，或受保护、默认、公有； 		
 * 		3).调用构造方法：
 * 			Constructor-->newInstance(Object... initargs)
 */
public class Constructors {
 
	public static void main(String[] args) throws Exception {
		//1.加载Class对象
		Class clazz = Class.forName("fanshe.Student");
		
		//2.获取所有公有构造方法
		System.out.println("**********************所有公有构造方法*********************************");
		Constructor[] conArray = clazz.getConstructors();
		for(Constructor c : conArray){
			System.out.println(c);
		}
		
		System.out.println("************所有的构造方法(包括：私有、受保护、默认、公有)***************");
		conArray = clazz.getDeclaredConstructors();
		for(Constructor c : conArray){
			System.out.println(c);
		}
		
		System.out.println("*****************获取公有、无参的构造方法*******************************");
		Constructor con = clazz.getConstructor(null);
		//1>、因为是无参的构造方法所以类型是一个null,不写也可以：这里需要的是一个参数的类型，切记是类型
		//2>、返回的是描述这个无参构造函数的类对象。
		System.out.println("con = " + con);
 
		//调用构造方法
		Object obj = con.newInstance();
	//	System.out.println("obj = " + obj);
	//	Student stu = (Student)obj;
		
		System.out.println("******************获取私有构造方法，并调用*******************************");
		con = clazz.getDeclaredConstructor(char.class);
		System.out.println(con);
		//调用构造方法
		con.setAccessible(true);//暴力访问(忽略掉访问修饰符)
		obj = con.newInstance('男');
	}
}
控制台输出：

**********************所有公有构造方法*********************************
public fanshe.Student(java.lang.String,int)
public fanshe.Student(char)
public fanshe.Student()
************所有的构造方法(包括：私有、受保护、默认、公有)***************
private fanshe.Student(int)
protected fanshe.Student(boolean)
public fanshe.Student(java.lang.String,int)
public fanshe.Student(char)
public fanshe.Student()
fanshe.Student(java.lang.String)
*****************获取公有、无参的构造方法*******************************
con = public fanshe.Student()
调用了公有、无参构造方法执行了。。。
******************获取私有构造方法，并调用*******************************
public fanshe.Student(char)
姓名：男
 

5、获取成员变量并调用：

Student类：
package fanshe.field;
public class Student {
	public Student(){
		
	}
	//**********字段*************//
	public String name;
	protected int age;
	char sex;
	private String phoneNum;
	
	@Override
	public String toString() {
		return "Student [name=" + name + ", age=" + age + ", sex=" + sex
				+ ", phoneNum=" + phoneNum + "]";
	}
}
测试类：

package fanshe.field;
import java.lang.reflect.Field;
/*
 * 获取成员变量并调用：
 * 
 * 1.批量的
 * 		1).Field[] getFields():获取所有的"公有字段"
 * 		2).Field[] getDeclaredFields():获取所有字段，包括：私有、受保护、默认、公有；
 * 2.获取单个的：
 * 		1).public Field getField(String fieldName):获取某个"公有的"字段；
 * 		2).public Field getDeclaredField(String fieldName):获取某个字段(可以是私有的)
 * 
 * 	 设置字段的值：
 * 		Field --> public void set(Object obj,Object value):
 * 					参数说明：
 * 					1.obj:要设置的字段所在的对象；
 * 					2.value:要为字段设置的值；
 */
public class Fields {
		public static void main(String[] args) throws Exception {
			//1.获取Class对象
			Class stuClass = Class.forName("fanshe.field.Student");
			//2.获取字段
			System.out.println("************获取所有公有的字段********************");
			Field[] fieldArray = stuClass.getFields();
			for(Field f : fieldArray){
				System.out.println(f);
			}
			System.out.println("************获取所有的字段(包括私有、受保护、默认的)********************");
			fieldArray = stuClass.getDeclaredFields();
			for(Field f : fieldArray){
				System.out.println(f);
			}
			System.out.println("*************获取公有字段**并调用***********************************");
			Field f = stuClass.getField("name");
			System.out.println(f);
			//获取一个对象
			Object obj = stuClass.getConstructor().newInstance();//产生Student对象--》Student stu = new Student();
			//为字段设置值
			f.set(obj, "刘德华");//为Student对象中的name属性赋值--》stu.name = "刘德华"
			//验证
			Student stu = (Student)obj;
			System.out.println("验证姓名：" + stu.name);
			
			
			System.out.println("**************获取私有字段****并调用********************************");
			f = stuClass.getDeclaredField("phoneNum");
			System.out.println(f);
			f.setAccessible(true);//暴力反射，解除私有限定
			f.set(obj, "18888889999");
			System.out.println("验证电话：" + stu);
			
		}
	}
控制台输出：

************获取所有公有的字段********************
public java.lang.String fanshe.field.Student.name
************获取所有的字段(包括私有、受保护、默认的)********************
public java.lang.String fanshe.field.Student.name
protected int fanshe.field.Student.age
char fanshe.field.Student.sex
private java.lang.String fanshe.field.Student.phoneNum
*************获取公有字段**并调用***********************************
public java.lang.String fanshe.field.Student.name
验证姓名：刘德华
**************获取私有字段****并调用********************************
private java.lang.String fanshe.field.Student.phoneNum
验证电话：Student [name=刘德华, age=0, sex=
 

6、获取成员方法并调用：

Student类：

package fanshe.method;
 
public class Student {
	//**************成员方法***************//
	public void show1(String s){
		System.out.println("调用了：公有的，String参数的show1(): s = " + s);
	}
	protected void show2(){
		System.out.println("调用了：受保护的，无参的show2()");
	}
	void show3(){
		System.out.println("调用了：默认的，无参的show3()");
	}
	private String show4(int age){
		System.out.println("调用了，私有的，并且有返回值的，int参数的show4(): age = " + age);
		return "abcd";
	}
}
测试类：

package fanshe.method;
import java.lang.reflect.Method;
 
/*
 * 获取成员方法并调用：
 * 
 * 1.批量的：
 * 		public Method[] getMethods():获取所有"公有方法"；（包含了父类的方法也包含Object类）
 * 		public Method[] getDeclaredMethods():获取所有的成员方法，包括私有的(不包括继承的)
 * 2.获取单个的：
 * 		public Method getMethod(String name,Class<?>... parameterTypes):
 * 					参数：
 * 						name : 方法名；
 * 						Class ... : 形参的Class类型对象
 * 		public Method getDeclaredMethod(String name,Class<?>... parameterTypes)
 * 
 * 	 调用方法：
 * 		Method --> public Object invoke(Object obj,Object... args):
 * 					参数说明：
 * 					obj : 要调用方法的对象；
 * 					args:调用方式时所传递的实参；
):
 */
public class MethodClass {
 
	public static void main(String[] args) throws Exception {
		//1.获取Class对象
		Class stuClass = Class.forName("fanshe.method.Student");
		//2.获取所有公有方法
		System.out.println("***************获取所有的”公有“方法*******************");
		stuClass.getMethods();
		Method[] methodArray = stuClass.getMethods();
		for(Method m : methodArray){
			System.out.println(m);
		}
		System.out.println("***************获取所有的方法，包括私有的*******************");
		methodArray = stuClass.getDeclaredMethods();
		for(Method m : methodArray){
			System.out.println(m);
		}
		System.out.println("***************获取公有的show1()方法*******************");
		Method m = stuClass.getMethod("show1", String.class);
		System.out.println(m);
		//实例化一个Student对象
		Object obj = stuClass.getConstructor().newInstance();
		m.invoke(obj, "刘德华");
		
		System.out.println("***************获取私有的show4()方法******************");
		m = stuClass.getDeclaredMethod("show4", int.class);
		System.out.println(m);
		m.setAccessible(true);//解除私有限定
		Object result = m.invoke(obj, 20);//需要两个参数，一个是要调用的对象（获取有反射），一个是实参
		System.out.println("返回值：" + result);	
	}
}
控制台输出：

***************获取所有的”公有“方法*******************
public void fanshe.method.Student.show1(java.lang.String)
public final void java.lang.Object.wait(long,int) throws java.lang.InterruptedException
public final native void java.lang.Object.wait(long) throws java.lang.InterruptedException
public final void java.lang.Object.wait() throws java.lang.InterruptedException
public boolean java.lang.Object.equals(java.lang.Object)
public java.lang.String java.lang.Object.toString()
public native int java.lang.Object.hashCode()
public final native java.lang.Class java.lang.Object.getClass()
public final native void java.lang.Object.notify()
public final native void java.lang.Object.notifyAll()
***************获取所有的方法，包括私有的*******************
public void fanshe.method.Student.show1(java.lang.String)
private java.lang.String fanshe.method.Student.show4(int)
protected void fanshe.method.Student.show2()
void fanshe.method.Student.show3()
***************获取公有的show1()方法*******************
public void fanshe.method.Student.show1(java.lang.String)
调用了：公有的，String参数的show1(): s = 刘德华
***************获取私有的show4()方法******************
private java.lang.String fanshe.method.Student.show4(int)
调用了，私有的，并且有返回值的，int参数的show4(): age = 20
返回值：abcd
 

7、反射main方法：
Student类：
package fanshe.main;
 
public class Student {
	public static void main(String[] args) {
		System.out.println("main方法执行了。。。");
	}
}
测试类：

package fanshe.main;
import java.lang.reflect.Method;
 
/**
 * 获取Student类的main方法、不要与当前的main方法搞混了
 */
public class Main {
	
	public static void main(String[] args) {
		try {
			//1、获取Student对象的字节码
			Class clazz = Class.forName("fanshe.main.Student");
			
			//2、获取main方法
			 Method methodMain = clazz.getMethod("main", String[].class);//第一个参数：方法名称，第二个参数：方法形参的类型，
			//3、调用main方法
			// methodMain.invoke(null, new String[]{"a","b","c"});
			 //第一个参数，对象类型，因为方法是static静态的，所以为null可以，第二个参数是String数组，这里要注意在jdk1.4时是数组，jdk1.5之后是可变参数
			 //这里拆的时候将  new String[]{"a","b","c"} 拆成3个对象。。。所以需要将它强转。
			 methodMain.invoke(null, (Object)new String[]{"a","b","c"});//方式一
			// methodMain.invoke(null, new Object[]{new String[]{"a","b","c"}});//方式二			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
控制台输出：

main方法执行了。。。
 

8、利用发射创建数值：
数组在Java里是比较特殊的一种类型，它可以赋值给一个Object Reference。

public static void testArray() throws ClassNotFoundException {
        Class<?> cls = Class.forName("java.lang.String");
        Object array = Array.newInstance(cls,25);
        //往数组里添加内容
        Array.set(array,0,"hello");
        Array.set(array,1,"Java");
        Array.set(array,2,"fuck");
        Array.set(array,3,"Scala");
        Array.set(array,4,"Clojure");
        //获取某一项的内容
        System.out.println(Array.get(array,3));
    }
 

9、反射方法的其他使用--通过反射运行配置文件内容：
Student类：

public class Student {
	public void show(){
		System.out.println("is show()");
	}
}
配置文件以txt文件为例子：

className = cn.fanshe.Student
methodName = show
测试类：

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.lang.reflect.Method;
import java.util.Properties;
 
/*
 * 我们利用反射和配置文件，可以使：应用程序更新时，对源码无需进行任何修改
 * 我们只需要将新类发送给客户端，并修改配置文件即可
 */
public class Demo {
	public static void main(String[] args) throws Exception {
		//通过反射获取Class对象
		Class stuClass = Class.forName(getValue("className"));//"cn.fanshe.Student"
		//2获取show()方法
		Method m = stuClass.getMethod(getValue("methodName"));//show
		//3.调用show()方法
		m.invoke(stuClass.getConstructor().newInstance());
		
	}
	
	//此方法接收一个key，在配置文件中获取相应的value
	public static String getValue(String key) throws IOException{
		Properties pro = new Properties();//获取配置文件的对象
		FileReader in = new FileReader("pro.txt");//获取输入流
		pro.load(in);//将流加载到配置文件对象中
		in.close();
		return pro.getProperty(key);//返回根据key获取的value值
	}
}
控制台输出：

is show()
需求：

当我们升级这个系统时，不要Student类，而需要新写一个Student2的类时，这时只需要更改pro.txt的文件内容就可以了。代码就一点不用改动。

public class Student2 {
	public void show2(){
		System.out.println("is show2()");
	}
}
配置文件更改为：

className = cn.fanshe.Student2
methodName = show2
 

10、反射方法的其他使用--通过反射越过泛型检查：
泛型用在编译期，编译过后泛型擦除（消失掉），所以是可以通过反射越过泛型检查的
测试类：
import java.lang.reflect.Method;
import java.util.ArrayList;
 
/*
 * 通过反射越过泛型检查
 * 例如：有一个String泛型的集合，怎样能向这个集合中添加一个Integer类型的值？
 */
public class Demo {
	public static void main(String[] args) throws Exception{
		ArrayList<String> strList = new ArrayList<>();
		strList.add("aaa");
		strList.add("bbb");
		
	//	strList.add(100);
		//获取ArrayList的Class对象，反向的调用add()方法，添加数据
		Class listClass = strList.getClass(); //得到 strList 对象的字节码 对象
		//获取add()方法
		Method m = listClass.getMethod("add", Object.class);
		//调用add()方法
		m.invoke(strList, 100);
		
		//遍历集合
		for(Object obj : strList){
			System.out.println(obj);
		}
	}
}
控制台输出：

aaa
bbb
100

# java反射技术在annotation中的一个应用
/**定义限额注解*/
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface BankTransferMoney {
    double maxMoney() default 10000;
}
/**转账处理业务类*/
public class BankService {
/**
* @param money 转账金额
*/
@BankTransferMoney(maxMoney = 15000)
public static void TransferMoney(double money){
    System.out.println(processAnnotationMoney(money));
}

private static String processAnnotationMoney(double money) {
    try {
        Method transferMoney = BankService.class.getDeclaredMethod("TransferMoney",double.class);
        boolean annotationPresent = transferMoney.isAnnotationPresent(BankTransferMoney.class);
        if(annotationPresent){
            BankTransferMoney annotation = transferMoney.getAnnotation(BankTransferMoney.class);
            double l = annotation.maxMoney();
            if(money>l){
                return "转账金额大于限额，转账失败";
            }else {
                return"转账金额为:"+money+"，转账成功";
            }
        }
    } catch ( NoSuchMethodException e) {
        e.printStackTrace();
    }
    return "转账处理失败";
}

public static void main(String[] args){
    TransferMoney(10000);
}
}


 