# Intent传递数据

## 1、利用intent在Activity或者其他组件间传递简单数据

```java
传递
val intent = Intent(this,SecondActivity::class.java)
intent.putExtra("String_data","hello")
intent.putExtra("int_data",100)
startActivity(intent)

接受：
intent.getStringExtra("string_data","")
intent.getIntExtra("int_data",0)

```

## 2、序列化和反序列化

复杂对象的传递，需要通过序列化的方式来进行。

* 序列化： 将数据结构或对象转换成二进制串的过程。

* 反序列化：将在序列化过程中所生成的二进制串转换成数据结构或者对象的过程。

简单来说，序列化就是将我们生成的对象进行存储起来(比如磁盘上)，以用来将来使用或者在网络上进行传输，而反序列化就是由我们的之前序列化生成的二进制串重新生成对象的过程。注意，这里我们反复说的序列化啦，反序列化啦，都是针对的对象，而非类。因为我们是针对对象进行存取与传输的，而非类，当我们需要重新获取之前的对象的时候，是直接读取出来的(从文件或网络中)，而非根据类new出一个对象，这点是需要注意的。

## 3、通过实现seriable接口方法实现序列化和反序列化

这种序列化方式是Java提供的，它的优点是简单，其实Serializable接口是个空接口，因而我们并不需要实现什么抽象方法，但是我们却往往需要在类中声明一个静态变量标识(serialVersionUID)，但这不是必须的，我们不声明，依然可以实现序列化，但是这样的话会对反序列化产生一定的影响，可能会在我们对类做了修改之后而造成对象的反序列化失败。声明方式如下：

* 实现seriable接口


```java
public class Person implements Serializable
{
    private static final long serialVersionUID = -7060210544600464481L;
    private String name;
    private int age;
    
    public String getName()
    {
        return name;
    }
    
    public void setName(String name)
    {
        this.name = name;
    }
    
    public int getAge()
    {
        return age;
    }
    
    public void setAge(int age)
    {
        this.age = age;
    }
}

```

* 序列化数据：
```java

Person person = new Person()
intent.putSeriableExtra("person_data",person)

或者：
Bundle mBundle = new Bundle();  
mBundle.putSerializable("person_data",person);  
intent.putExtras(mBundle);  

```

* 反序列化数据：


```java

Person person = intent.getSeriableExtra("person_data")

```

## 4、通过Parcelable实现序列化和反序列化

* 实现parcelable接口


```java
ments Parcelable {
	private int color=Color.BLACK;
	
	MyColor(){
		color=Color.BLACK;
	}
	
	MyColor(Parcel in){
		color=in.readInt();
	}
	
	public int getColor(){
		return color;
	}
	
	public void setColor(int color){
		this.color=color;
	}
	
	@Override
	public int describeContents() {
		return 0;
	}
 
	@Override
	public void writeToParcel(Parcel dest, int flags) {
		dest.writeInt(color);
	}
 
    public static final Parcelable.Creator<MyColor> CREATOR
	    = new Parcelable.Creator<MyColor>() {
		public MyColor createFromParcel(Parcel in) {
		    return new MyColor(in);
		}
		
		public MyColor[] newArray(int size) {
		    return new MyColor[size];
		}
	};
}

```


* 序列化数据：
```java


Color color = new Color()
color.setColor(Color.RED);
intent.putExtra("color_data", color);
startActivityForResult(intent,SUB_ACTIVITY);	
或者：
Bundle mBundle = new Bundle();  
mBundle.putParcelable("color_data",person);  
intent.putExtras(mBundle);  
```

* 反序列化数据：
* 
```java

Color color = intent.getParcelableExtra("color_data")

```

## 4、Seriable和Parcelable的区别以及在性能优化上的分析

1、Seriable将整个类对象序列化，Parcelable将类成员分解成原子元素再序列化，前者效率低，后者效率高。 

2、Serializable是将序列化后的对象存储在硬盘上，使用I/O读写的方式，而Parcelable是将其存储在内存中，是针对内存的读写，内存的读写速度显然要远远大于I/O的读写速度，推荐使用Parcelable这种方式来实现对象的序列化。

3、Serializable会使用反射，序列化和反序列过程需要大量I/O操作，Parcelable自己实现封装和解封操作不需要用反射，数据也存放在Natvie内存中，效率要快很多，





