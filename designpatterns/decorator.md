# 装饰器模式

## 定义

装饰模式又名包装(Wrapper)模式。装饰模式以对客户端透明的方式扩展对象的功能，是继承关系的一个替代方案。

总之就是动态的给对象添加一些额外的职责，类似钢铁侠可以组装不同武器。


## 场景

![](https://img-blog.csdn.net/20180908152940266?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpYW41MjBhbw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

抽象组件(IComponentBitmap) ：定义装饰bmp的规范

被装饰者(ConcreteBitmap) ：bmp具体对象（拥有通用属性），要对它做装饰拓展。

装饰者组件(BitmapDecorator) ：定义具体装饰者的行为规范，可以做统一基础处理。

具体装饰(ConcreteDecorator) ：负责给bmp装饰附加的功能。


## 实例

```java
抽象组件(IComponentBitmap)

public interface IComponentBitmap {
    /**
     * 定义方法规范
     * 同样的也可以增加更多的抽象方法
     */
    public void bmpOperation(Bitmap bmp);

}


被装饰者(ConcreteBitmap)

/**
 * 要接收附加装饰的基类
 * 具有通用属性
 */
public class ConcreteBitmap implements IComponentBitmap{

    @Override
    public void bmpOperation(Bitmap bmp) {
        bmp.description=new StringBuffer("原始bmp");
    }

}



装饰者组件(BitmapDecorator)

/**
 * 装饰角色 持有一个Component对象的实例
 * 对具体装饰者可以做统一基础处理
 */
public class BitmapDecorator implements IComponentBitmap {

    protected IComponentBitmap mComponent;

    public BitmapDecorator(IComponentBitmap component) {
        mComponent = component;
    }

    @Override
    public void bmpOperation(Bitmap bmp) {
        mComponent.bmpOperation(bmp);//可以做基础操作
    }

}

以下是具体装饰类。

/**
 * 具体装饰类
 * 滤镜装饰
 */
public class FilterBitmapDecorator extends BitmapDecorator{

    public FilterBitmapDecorator(IComponentBitmap component) {
        super(component);
    }

    @Override
    public void bmpOperation(Bitmap bmp) {
        bmp.description.append("+滤镜效果");
    }

}

/**
 * 具体装饰类
 * 老电影效果
 */
public class MovieBitmapDecorator extends BitmapDecorator{

    public MovieBitmapDecorator(IComponentBitmap component) {
        super(component);
    }

    @Override
    public void bmpOperation(Bitmap bmp) {
        bmp.description.append("+老电影效果");
    }

}

客户端类 ：

        Bitmap bmp=new Bitmap();

        IComponentBitmap concreteBmp=new ConcreteBitmap();//创建被装饰者
        concreteBmp.bmpOperation(bmp);//无任何装饰
        System.out.println(bmp.description);
        /*****************原始bmp*****************/

        new FilterBitmapDecorator(concreteBmp).bmpOperation(bmp);;
        System.out.println(bmp.description);
        /*****************原始bmp+滤镜效果*****************/

        new MovieBitmapDecorator(concreteBmp).bmpOperation(bmp);
        System.out.println(bmp.description);
        /*****************原始bmp+滤镜效果+老电影效果*****************/
```

