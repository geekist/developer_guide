@[TOC]
# 前言：
依赖注入框架Dagger一经推出，就受到了很大关注。经过几年的发展，Dagger逐渐成为android开发体系的一部分，google接手Dagger后，相继推出了dagger.android和kilt，并在官方文档中极力推荐使用这种编程实践。google如此推崇Dagger当然是可以理解的，依赖项注入（Dependency injection ，DI) 最为面向对象的编程模式，其主要目的是为了降低类和类之间的依赖关系，为良好的应用架构奠定基础，尤其是在大型项目的开发中，使用Dagger可以减少很多模板代码，大大提高编程效率，减少代码错误，并给测试带了很大的方便。

然而相比其他框架，Dagger的学习曲线相对比较陡峭，网上的学习资料也不太全面，相信每个开发者和我一样，在学习Dagger的时候都经历了一些曲折，在此我整理了自己关于Dagger的一些理解和实践，作为一个总结，也希望能够对Dagger的初学者有一些启发。

Dagger是建立在Java注解@Annotation之上的，因此，如果你对java的注解不太熟悉的话，建议先学习这方面的知识，然后再开始学习Dagger。

[Dagger的官方网站：](https://dagger.dev/)

[Android官网中关于依赖注入的Dagger部分](https://developer.android.com/training/dependency-injection/dagger-basics)

# 一、依赖和依赖注入
## 1、依赖注入的定义

在我们的开发实践中，随处可见一个类的定义或者实现需要用到另外一个或多个类。例如：

```java
public class Engine {

    public Engine() {

    }
}

public class Car{
	private Engine engine;  //在Car类中包含了一个Engine类
	
	public Car() {
	}
}
```

```java
public class Teacher{
	public int id；
	public String name;
	
    public Teacher() {

    }
}

public class Student{
	private int teacherId  //在Student类中需要Teacher类的信息
	
	public Student(Teacher teacher) {
		teacherId = teacher.id;
	}
}
```


这种一个类对于另外一个或多个类的引用就是依赖。这样的依赖贯穿于我们每天的coding之中。上面的例子中，我们可以说Teacher类就是Student类的依赖项，Engine类就是Car类的依赖项。或者说，“学校”类依赖于“学生”类，“汽车”类依赖于“引擎”类。

为目标类提供依赖的方式可以分为两种，一种是直接将依赖项的构造方式暴露出来，在目标类中直接调用依赖项的构造方法，比如：
```java
public class Car{
	private Engine engine;
	
	public Car() {
		engine = new Engine(); //直接调用依赖项的构造方法
	}
}
```
另外一种方法是将依赖对象和其构造方式解耦，通过其他的方式传递依赖给目标调用者，即依赖是被“注入”进来的，如：
```java
public class Car{
	private Engine engine;
	
	public Car() {
	}
	
	public void setEngine(Engine engine) {//通过参数传递的方式实现依赖
		this.engine = engine;
	}
}
```
这用依赖实现方式就称为依赖注入。

显然，相比于前者，依赖注入的方式有更明显的优点：
- 构造和使用分离：
将依赖项和目标调用者解耦，当依赖项的构造方式改变时，调用者不需要做任何代码上的改动。
- 便于对依赖项单元测试：
因为实现了依赖类和目标类的解耦，依赖注入更方便做单元测试，我们可以很容易地为上面的类编写单元测试的案例。
-  便于独立/并行开发模块化的代码.
依赖注入相当于给目标调用类提供了一个依赖项的接口，目标类和依赖类可以并行开发自己的模块，不会互相干扰。

## 2、依赖注入的方式

通常依赖注入有以下四种方式

- 1、通过构造方法注入：
```java
public class Car{    
	Engine engine;    
	public void Car(Engine engine) {  //通过带参数的构造器实现依赖注入      
		this.engine = engine;    
	}
}
```
- 2、通过set方法注入
```java
public class Car{    
	Engine engine;    
	public void setEngine(Engine engine) { //通过set方法实现依赖注入        
		this.engine = engine;    
	}
}
```
- 3、通过接口注入：
```java
interface EngineInterface {    
	public void engine(Engine engine);
}

public class Car implements EngineInterface {    
	Engine engine;        

	@override      
	public void engine(Engine engine) {            
		this.engine = engine;        
	}  
}
```

- 4、通过注解的方式注入：
 ```java
 public Car {    
	//需要依赖注入框架的支持，如Dagger2    
	@inject    
	Engine engine;    

	public Car() {} {
	}

}
 ```
Dagger就是一个使用注解实现依赖注入的框架库。

# 二、 Dagger2依赖注入框架

## 1、Dagger2介绍

Dagger是由大名鼎鼎的Square 公司开发的，我们熟悉的 RxJava、Butterknife、Retrofit、OKHttp 等等框架都是Square 提供的。

 Dagger2 基于 Dagger ，由 Google 公司开发并维护。在此基础上，google相继推出android dagger和Hilt依赖注入库， 提供了将 Dagger 纳入 Android 应用的标准方法。目前，DI技术已经广泛应用于Android的开发之中。

## 2、Dagger2 API
Dagger2框架提供了下面几个常用的API：
```java
public @interface Component {    
	Class<?>[] modules() default {};   
 	Class<?>[] dependencies() default {};
}

public @interface Subcomponent { 
   Class<?>[] modules() default {};
}

public @interface Module {    
	Class<?>[] includes() default {};
}

public @interface Provides {}

public @interface MapKey 
{    
	boolean unwrapValue() default true;
}

public interface Lazy<T> { 
   T get();
}
```
以及在Dagger 2中用到的，定义在 JSR-330 （Java中依赖注入的标准）中的其它元素：
```java
public @interface Inject {}

public @interface Scope {}

public @interface Qualifier {}
```

## 3、在AndroidStudio中配置Dagger依赖项
当前AndroidStudio的版本已经是4.1版本了，在次版本之上配置Dagger的依赖项，需要在模块的build.gradle文件中添加如下代码：
```java
dependencies {
	......
    def dagger_version = "2.29.1"
    implementation "com.google.dagger:dagger:$dagger_version"
    annotationProcessor "com.google.dagger:dagger-compiler:$dagger_version"
}
```
## 4、@Inject 和 @Component注解
从一个最简单的例子开始：
假设在Android项目的MainActivity中，我们需要用到一个Teacher类：
```java
public class Teacher{
    public Teacher() {
    }
}
```

```java
public class MainActivity extends AppCompatActivity {
    public Teacher teacher; //MainActivity的依赖项

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ......
    }
}
```
在Dagger2中使用@Inject注解和@Component注解配合，我们可以实现最简单的依赖注入。这里我们首先介绍Dagger2实现依赖注入的方法和步骤，然后抽丝剥茧，看依赖注入是如何实现的。

- 1、将@Inject标注在依赖项的构造方法中，这个注解用来告诉Dagger2，可以用这个构造方法来创建依赖对象。

@Inject是JSR-330标准中的一部分，在Dagger2中它只用来标记那些应该被依赖注入框架注入的依赖。


```java
public class Teacher{
	@Inject    //告诉Dagger2可以使用这个构造方法来创建依赖对象。
    public Teacher() { 
    }
}
```

- 2、将@Inject标注在目标类对依赖的属性声明或set方法上。告诉Dagger2需要将依赖注入到这里。


```java
public class MainActivity extends AppCompatActivity {
	@Inject //告诉dagger2需要在这里注入依赖
	public Teacher teacher;
	......
}
```
或者：
```java
public class MainActivity extends AppCompatActivity {
	private Teacher teacher;

	@Inject //告诉dagger2需要在这里注入依赖
	public  void setTeacher(Teacher teacher) {    
		this.teacher = teacher;
	}
}
```
上面对目标类中依赖项的标注方法有两种，第一种标注方式称为属性标注，第二种标注方式称为方法标注，两种标注的功能基本相同，使用第二种方法的一个原因是因为在目标类中@inject不能标注私有属性，因此如果要标注私有的属性，要标注在属性的设置方法上。


- 3、手动声明一个component接口类，Dagger2将会实现这个接口类，后面我们将用这个实现类来完成依赖项的注入。
```java
@Component
public interface MainActivityComponent {
    public void inject(MainActivity activity);
}
```
在这里我们定义了一个接口，并且用@Component注解。@Component可以看做为注入器，在目标类MainActivity中使用Component的实例来完成注入。在我的理解中，@Component可以说是Dagger2中最重要的一个注解。Dagger2框架以Component中定义的方法作为入口，在目标类中寻找所有@Inject标注，自动生成一系列提供依赖的类，最终通过实现inject方法完成依赖的注入。

接口的命名方式推荐为：目标类名+Component。在接口中定义一个inject（Object obj）的方法，其中Object就是目标类。


编译代码，我们看到Dagger2自动生成了几个文件，这里我们暂时忽略，稍后再做解释。
![Dagge2自动生成了类](https://img-blog.csdnimg.cn/20201020083952258.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lhbmd3dTAwNw==,size_16,color_FFFFFF,t_70#pic_center)


- 4、在目标类中MainActivity中调用以DaggerXXX命名的类，注入依赖。
```java
public class MainActivity extends AppCompatActivity {
    @Inject
    public Teacher teacher;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        DaggerMainActivityComponent.builder().build().inject(this);
        int hashCode = teacher.hashCode();
        Log.d("msg","hashCode: " + hashCode);
    }
}
```
至此，我们完成了对Teacher类的注入。回顾一下我们所做的事情：

1、在依赖项的构造函数中添加@Inject标注，告诉Dagger2可以使用这个构造方法来创建依赖实例。

2、在目标类声明依赖的地方添加@Inject标注，告诉Dagger2需要在这里注入依赖。

3、定义一个commponent接口，告诉dagger2实现该接口的方法以便于完成依赖注入。

4、编译程序后，在目标类中调用注入方法完成依赖注入。


下面我们来看看Dagger2生成的文件，代码不多，我们很容易理解：

**Tacher_factory.java**

```java
public final class Teacher_Factory implements Factory<Teacher> {
  @Override
  public Teacher get() {
    return newInstance();
  }

  public static Teacher_Factory create() {
    return InstanceHolder.INSTANCE;
  }

  public static Teacher newInstance() {
    return new Teacher();
  }

  private static final class InstanceHolder {
    private static final Teacher_Factory INSTANCE = new Teacher_Factory();
  }
}
```
显然这是一个工厂类，用来创建依赖项Tacher类的实例。

**MainActivity_MembersInjector.java**
```java
public final class MainActivity_MembersInjector implements MembersInjector<MainActivity> {
  private final Provider<Teacher> teacherProvider;

  public MainActivity_MembersInjector(Provider<Teacher> teacherProvider) {
    this.teacherProvider = teacherProvider;
  }

  public static MembersInjector<MainActivity> create(Provider<Teacher> teacherProvider) {
    return new MainActivity_MembersInjector(teacherProvider);
  }

  @Override
  public void injectMembers(MainActivity instance) {
    injectTeacher(instance, teacherProvider.get());
  }

  @InjectedFieldSignature("com.jon.dagger2sample.ui.MainActivity.teacher")
  public static void injectTeacher(MainActivity instance, Teacher teacher) {
    instance.teacher = teacher;
  }
}
```

顾名思义，这是MainActivity为成员变量teacher注入依赖的类，其主要实现是在最后的一个方法之中。
```java
 @InjectedFieldSignature("com.jon.dagger2sample.ui.MainActivity.teacher")
  public static void injectTeacher(MainActivity instance, Teacher teacher) {
    instance.teacher = teacher;
  }
```

**MainActivityComponent,java**
```java
public final class DaggerMainActivityComponent implements MainActivityComponent {
  private DaggerMainActivityComponent() {

  }

  public static Builder builder() {
    return new Builder();
  }

  public static MainActivityComponent create() {
    return new Builder().build();
  }

  @Override
  public void inject(MainActivity activity) {
    injectMainActivity(activity);
  }

  private MainActivity injectMainActivity(MainActivity instance) {
    MainActivity_MembersInjector.injectTeacher(instance, new Teacher());
    return instance;
  }

  public static final class Builder {
    private Builder() {
    }

    public MainActivityComponent build() {
      return new DaggerMainActivityComponent();
    }
  }
}
```
这个类是Dagger生成的MainActivityComponent接口实现类。DaggerMainActivityComponent通过内部类builder的build()的build方法创建了一个 DaggerMainActivityComponent的实例，然后调用inject方法实现注入。
```java
@Override
  public void inject(MainActivity activity) {
    injectMainActivity(activity);
  }

  private MainActivity injectMainActivity(MainActivity instance) {
    MainActivity_MembersInjector.injectTeacher(instance, new Teacher());
    return instance;
  }
```
回顾一下Dagger2所做的事情：

1、生成依赖项工厂类，用来创建依赖项。

2、生成目标项的依赖注入类，用来给依赖项赋值。

3、生成以DaggerXXXComponent生成的注入器类，实现对目标类的依赖注入。

至此，通过@Inject和@Component注解，我们就可以在目标类通过Dagger2框架来实现依赖注入。

但是，用@Inject和@Component注解，只能实现最简单的依赖注入，即依赖项的构造方法可以添加标注，且该构造函数是无参数的构造方法。

我们尝试修改Teacher类，将构造方法改为带参数的构造器：
```java

public class Teacher {
    int id;
    
    @Inject
    public Teacher(int id) {

    }
}

```

编译后会有错误信息：
```java
D:\work\github\android\dagger2-sample\Dagger2Sample\app\src\main\java\com\jon\dagger2sample\bean\MainActivityComponent.java:8:
 ����: [Dagger/MissingBinding] java.lang.Integer cannot be provided without an @Inject constructor or an @Provides-annotated method.
```
另外，当我们使用第三方的库，无法得到依赖项的构造方法，更不用说在其上添加@Inject注解了。又或者，当我们使用了抽象类的实现作为依赖，也没有办法标注@Inject注解。这时，我们就需要用另外的注解方法来实现依赖注入了。

## 5、@Provides 和@Module注解 
前面我们提到，至少@Inject和@Component无法在以下三种情况下使用：
- 依赖项的构造方法带有参数
- 第三方库作为依赖项
- 抽象类的实现作为依赖项

Dagger2在@Inject和@Componnet注解的基础上，添加了@Provides和@Module注解来解决这个问题。简单地说，就是需要我们用@provides和@Module注解来告诉Dagger2，如何生成依赖项的实例，然后使用DaggerXXXComponent完成对依赖项的注入。

我们依次来看看这三种方式的依赖注入如何实现。

### 1、依赖项的构造方法带有参数
```java
public class Teacher {
    private int id;

	//构造方法带有参数，无法使用@Inject注解
    public Teacher(int id) {
        this.id = id;
    }
}
```
- 1、在目标类声明依赖项的地方添加@Inject注解，告诉Dagger2这里需要依赖注入。

这一步骤和前面是一样的。

```java

public class MainActivity extends AppCompatActivity {
    @Inject
    Teacher teacher;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```
- 2、手动为Teacher类创建一个命名位TeacherModule的类，为该类添加@Module注解，并在类中提供实现Tacher实例的方法，该方法用@Provides注解。
```java
@Module  //告诉Dagger2可以从这里获取依赖
public class TeacherModule {
    @Provides    //告诉Dagger2可以用该方法具体创建依赖实例
    Teacher provideTeacher(int id) {
        return new Teacher(id);
    }

    @Provides //告诉Dagger2依赖项的需要的参数可以在这里创建
    int provideId() {
        return 5;
    }
}
```

以上是定义一个Module的方式。

该类以XXXModule命名，用@Module注解。@Module是告诉Component，可以从这里获取依赖对象。Component会去找被@Provide标注的方法，相当于构造器的@Inject。

在类中提供以@Provides注解的方法，方法以provideXXX命名。 @Provodes标记在方法上，表示可以通过这个方法获取依赖。依赖项创建过程中需要的参数也在这里提供。

@Module需要和@Provides需要配对使用才能起作用。

- 3、在Component中添加Module类的参数，告诉Dagger2获取依赖的方式
```java
@Component(modules = TeacherModule.class) //Comoonent的modules参数接受Module类
public interface ActivityComponent {
    public void inject(MainActivity activity);
}
```

- 4、编译代码后，在目标类中调用DaggerXXXComponnet的方法实现依赖注入
```java

public class MainActivity extends AppCompatActivity {
    @Inject
    Teacher teacher;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

		//调用DaggerXXXComponent类的方法实现依赖注入。
        DaggerActivityComponent.builder().build().inject(this);
        int hashCode = teacher.hashCode();
        Log.d("msg","hash code: " + hashCode);
    }
}
```
到这里我们就实现了@Provides和@Module注解实现依赖注入的方法。总结一下我们的操作：
- 1、在目标类的依赖项属性中添加@Inject注解
- 2、手动添加一个以@Module注解，以XXXModule命名的类，位Dagger2提供创建依赖项的方法。具体的依赖项以provideXXX方法提供，并带有@Provides注解。
- 3、创建以@Componet注解的component类，并在该注解的modules属性中添加XXXModule.class。
- 4、在目标类中调用DaggerXXXComponent的方法实现依赖注入。

观察Dagger2自动生成的代码，和使用@Inject和@Component注解生成的方法类似，这里就不详细分析了。

### 2、第三方库的依赖注入
```java
public class ThirdPartObj {
    //第三方库，无法得到构造器类的实现，无法再构造器中添加@Inject标注
}
```
同样首先为依赖项创建Module类，在Module类中完成依赖项的实例化。
```java
@Module //Module类创建依赖项的实例
public class ThirdPartObjModule {
    @Provides
    public ThirdPartObj provideThirdPartObj() {
        return new ThirdPartObj();
    }
}
```
然后在@Component注解的modules参数中添加Module类
```java
@Component(modules = {TeacherModule.class,ThirdPartObjModule.class})
public interface ActivityComponent {
    public void inject(MainActivity activity);
}
```
最后在目标类MainActivity中调用DaggerXXXComponent实现依赖注入
```java
public class MainActivity extends AppCompatActivity {
    @Inject
    ThirdPartObj tpObj;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        DaggerActivityComponent.builder().build().inject(this);
        int hashCode = tpObj.hashCode();
        Log.d("msg","hash code: " + hashCode);
    }
}

```
### 3、抽象类的实现作为依赖项注入
```java
public abstract class Fruit {
}

public class Apple extends Fruit {
    public Apple() {
    }
}
```


```java
@Module
public class FruitModule {
    @Provides
    public Fruit provideFruit() {
        return new Apple();
    }
}
```

```java
@Component(modules = {TeacherModule.class,ThirdPartObjModule.class,FruitModule.class})
public interface ActivityComponent {
    public void inject(MainActivity activity);
}
```
```java
public class MainActivity extends AppCompatActivity {
    @Inject
    Fruit fruit;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        DaggerActivityComponent.builder().build().inject(this);
        int hashCode = fruit.hashCode();
        Log.d("msg","hash code: " + hashCode);
    }
}
```

## 6、@Qualifier和@Named
回到前面关于Teacher类的例子，我们稍微修改一下代码，让Teacher类的构造方法需要两个相同类型的参数：
```java
public class Teacher {
    private int id;
    private int age;

    public Teacher(int id,int age) {
        this.id = id;
        this.age = age;
    }
}
```
修改Module类，添加provide方法：

```java
@Module
public class TeacherModule {

    @Provides
    Teacher provideTeacher(int id,int age) {
        return new Teacher(id,age);
    }

    @Provides
    int provideId() {
        return 5;
    }

    @Provides
    int provideAge() {
        return 36;
    }
}
```
编译无法通过，原因是Dagger2无法识别两个返回值是相同类型的provide方法，该如何分配给不同的参数。此时，我们就需要给这两个provide方法加上@Named注解来区分。
```java
@Module
public class TeacherModule {
    @Provides
    Teacher provideTeacher(@Named("id")int id,@Named("age") int age) {
        return new Teacher( id,age);
    }

    @Provides
    @Named("id")
    int provideId() {
        return 5;
    }

    @Provides
    @Named("age")
    int provideAge() {
        return 36;
    }
}
```
编译后可以得到正确的结果。

现在我们来观察一下@Named注解：
```java
@Qualifier
@Documented
@Retention(RUNTIME)
public @interface Named {

    /** The name. */
    String value() default "";
}

```
可以看到Named其实就是一个自定义的@Qualifier注解。我们当然可以自定义新的注解
来取代@Named注解，实际上我们更推荐使用@Qualifier，因为@Named需要手写字符串，容易出错。

我们通过Fruit的例子来演示一下如何定义自己的@Qualifier注解：

Fruit类有两个子类Apple和Banana
```java
public abstract class Fruit {

}

public class Apple extends Fruit {
    public Apple() {
    }

    @NonNull
    @Override
    public String toString() {
        return "I am an apple ";
    }
}

public class Banana extends Fruit {
    public Banana() {
    }

    @NonNull
    @Override
    public String toString() {
        return "I am a banna";
    }
}
```
在Fruit的Module类中添加相同类型的provide方法：
```java
@Module
public class FruitModule {
    @Provides
    public Fruit provideApple() {
        return new Apple();
    }

    @Provides
    public Fruit provideBanana() {
        return new Banana();
    }
}
```
此时编译无法通过，因为Dagger2无法在两个相同类型的provide方法中做出选择。

我们可以添加自定义的注解来区分不同的provide方法：

```java
@Qualifier
@Retention(RetentionPolicy.RUNTIME)
public @interface AppleFruit {
}

@Qualifier
@Retention(RetentionPolicy.RUNTIME)
public @interface BananaFruit {
}
```
将自定义的注解添加在provide方法上面：
```java
@Module
public class FruitModule {
    @Provides
    @AppleFruit
    public Fruit provideApple() {
        return new Apple();
    }

    @Provides
    @BananaFruit
    public Fruit provideBanana() {
        return new Banana();
    }
}
```
在目标类的依赖项声明中，添加同样的注解：
```java

public class MainActivity extends AppCompatActivity {
    @Inject
    @AppleFruit
    Fruit apple;

    @Inject
    @BananaFruit
    public Fruit banana;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        DaggerActivityComponent.builder().build().inject(this);
        apple.toString();
        banana.toString();
     }
```
运行代码可以看到apple 和banana被正确注入到了MainActivity中。

## 7、@Component的dependencies和@SubComponent
Dagger2使用@Component 管理着依赖实例，当两个依赖项之间产生了依赖的时候，即依赖项ClassA又依赖于ClassB，这种情况下实现依赖注入有两种方式：
- 依赖方式实现注入，一个 Component 依赖其他 Compoent公开的依赖实例，用 Component 中的dependencies声明。
- Sub-Component实现注入，用一个 Component 叫扩展另一个 Component 提供更多的依赖。

假设有ClassA和ClassB两个类，ClassB中有ClassA的引用，即ClassB依赖于ClassA,我们先来看看用@Component的dependencies实现依赖注入的方法

```java

public class ClassA {
}

public class ClassB {
    private ClassA cA;

    public ClassB(ClassA cA) {
		this.CA = cA;
    }
}
```
### 1、@Component的dependencies实现依赖注入
- 第一步：两个类创建Module类，在Module类中实现各自的实例化方法
```java

@Module
public class ClassAModule {

    @Provides
    ClassA provideClassA() {
        return new ClassA();
    }
}


@Module
public class ClassBModule {
    
    @Provides
    ClassB provideClassB() {
        return new ClassB();
    }
}

```
- 第二步：为ClassA创建一个component。
```java
@Component(modules = ClassAModule.class)
public interface ClassAComponent {
    public ClassA getClassA();
}
```

- 第三步：在ClassBComponent（即前面提到的ActivityComponent）中将ClassA的component作为依赖参数输入。
```java
@Component(modules = ClassBModule.class,dependencies = ClassAComponent.class)
public interface ClassBComponent{
    public void inject(MainActivity activity);
}
```
- 第四步：在MainActivity类中实现依赖项注入。
```java
public class MainActivity extends AppCompatActivity {
    @Inject
    ClassB cB;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ClassAComponent cAComponent = DaggerClassAComponent.create();
        DaggerClassBComponent.builder().classAComponent(cAComponent).build().inject(this);

        int hashCode = cB.hashCode();
        Log.d("msg","hash code :" + hashCode);
    }
}
```
运行代码，可以看到ClassB的实例被注入到MainActivity中。

### 2、用@SubComponnet实现依赖注入
这里出现了SubComponent的概念，ClassB依赖于ClassA，则ClassBComponent就称为ClassAComponent的下一级component，即目标类的注入器component成为了依赖项的注入器的下一级component。
- 第一步：创建ClassB的Module类，在Module类中实现自己的实例化方法.
```java
@Module
public class ClassBModule {
    @Provides
    public ClassB provideChild() {
        return new ClassB();
    }
}
```
这一步骤和前面是相同的。

- 第二步：为ClassB创建一个component,添加@SubComponent注解，并且添加一个新的内部接口builder，返回该component
```java
@Subcomponent(modules = ClassBModule.class)
public interface ClassBComponent {
    void inject(MainActivity activity);

    @Subcomponent.Builder
    interface Builder {
        ClassBComponent build();
    }
}
```
第三步：为ClassA创建Module类,并在@Module注解下添加subComponent参数。
```java
@Module(subcomponents = ClassBComponent.class)
public class ClassAModule {
    @Provides
    public ClassA provideClassA() {
        return new ClassA();
    }
}
```

- 第四步：创建ClassAComponent，在接口中定义一个获取ClassBComponnet的方法。
```java
@Component(modules = ClassAModule.class)
public interface ClassAComponent {
    ClassBComponent.Builder buildClassBComponent();
}
```
- 第五步：在MainActivity类中实现依赖项注入。
```java
public class MainActivity extends AppCompatActivity {
    @Inject
    ClassB cB;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ClassAComponent cAComponent = DaggerClassAComponent.create();
        DaggerClassBComponent.builder().classAComponent(cAComponent).build().inject(this);

        int hashCode = cB.hashCode();
        Log.d("msg","hash code :" + hashCode);
    }
}
```
首先创建上一级component的实例，然后构建出sub-component的实例，用sub-component的实例实现依赖注入。运行代码，可以看到ClassB的实例被注入到MainActivity中。

### 3、两种依赖注入方式的区别
从上面的实例可以看出，用dependencies的方式，只需要创建ClassA的component，代码间的依赖性更少，用SubComponent的方式，在module和componnet的定义中会出现相互关联的情况，个人更倾向于dependencies方式的实现。
## 8、@Scope和@Singleton
我们在日常开发中经常要使用到各种单例，比如，在 Android 中开发，我们会经常编写这样的代码：
```java
public class DBManager {
    private static DBManager instance;

    private DBManager() {
    }

    public static DBManager getInstance() {
        if (instance == null) {
            synchronized (DBManager.class) {
                if (instance == null) {
                    instance = new DBManager();
                }
            }
        }
        return instance;
    }
}
```
当DBManger作为单例的依赖项注入时，我们可以使用@Singleton注解，减少模板代码的书写。
下面我们来看看@Singleton注解在依赖注入中的使用：首先定义一个DBManager的类，作为MainActivity的依赖项。

```java
public class DBManager {

}
```
在DBManagerModule 中添加@Singleton的标注：
```java

@Module
public class DBManagerModule {
    @Provides
    @Singleton
    DBManager provideDBManager() {
        return new DBManager();
    }
}

```
在ActivityComponent中也加上@Singleton的标注：
```java
@Singleton
@Component(modules = DBManagerModule.class)
public interface ActivityComponent {
    public void inject(MainActivity activity);
}
```

在目标类MainActivity中实现依赖项的注入：
```java
public class MainActivity extends AppCompatActivity {
    @Inject
    DBManager dbManager1;

    @Inject
    DBManager dbManager2;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

       DaggerActivityComponent.builder().build().inject(this);
       String str = "dbManager1:" + dbManager1.hashCode() + " dbManager2: " +  dbManager2.hashCode();
       Toast.makeText(this,str,Toast.LENGTH_LONG).show();
    }
}
```
运行后可以看到dbManager1和dbManager2的哈希值是同一个，说明是同一个实例。

查看@Singleton的定义,可以看到它实际上是一个自定义的@Scope注解。

```java
@Scope
@Documented
@Retention(RUNTIME)
public @interface Singleton {}
```
而@Scope的作用只是保证依赖在@Component中是唯一的，可以理解为“局部单例”。因此，这里的单例并不是我们所认为的单例，该单例只是在@Component的范围内唯一，本例中是MainActivity。

那么，我们如何实现一个全局的单例依赖项呢？我们可以尝试将依赖项的Component放入App类中实例化，然后每一次都使用该compoent创建实例即可。

## 9、@MapKey
@MapKey注解用来定义一些依赖集合。
```java
@Inject
Map<String, String> map;

```

首先定义一个自己的MapKey注解
```java
@MapKey(unwrapValue = true)
@interface TestKey {
    String value();
}
```
提供依赖
```java
@Provides(type = Type.MAP)
@TestKey("foo")
String provideFooKey() {
    return "foo value";
}
 
@Provides(type = Type.MAP)
@TestKey("bar")
String provideBarKey() {
    return "bar value";
}
```



## 10、@Lazy
Dagger2支持Lazy模式，在依赖注入的时候并不初始化依赖项，在需要使用的时候，调用Lazy类的get方法来获取实例。
```java
public class MainActivity extends AppCompatActivity {
    @Inject
    Lazy<Teacher> teacherLazy;
 
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
 
        DaggerMainActivityComponent.builder().build().inject(this);
        Teacher teacher = teacherLazy.get();
        int hashCode = teacher.hashCode();
		Log.d("msg","hash code : " + hashCode   
 	}
}
```
# 后记
关于Dagger2的总结和整理就到这里了，欢迎大家批评指正。
[本文代码的github地址](https://github.com/geekist/dagger2-sample)
