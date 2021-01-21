
# Iterator 迭代器模式

## 定义

提供一种方法顺序访问一个聚合对象中各个元素, 而又不需暴露该对象的内部表示。

## 使用场景

![](https://upload-images.jianshu.io/upload_images/8813136-e53f8a00e965b352.png)

## 实例

```java
public abstract class Aggregate {
    /**
     * 工厂方法，创建相应迭代子对象的接口
     */
    public abstract Iterator createIterator();
}

public interface Iterator {
    /**
     * 迭代方法：移动到第一个元素
     */
    public void first();
    /**
     * 迭代方法：移动到下一个元素
     */
    public void next();
    /**
     * 迭代方法：是否为最后一个元素
     */
    public boolean isDone();
    /**
     * 迭代方法：返还当前元素
     */
    public Object currentItem();
}

public class ConcreteAggregate extends Aggregate {
    
    private Object[] objArray = null;
    /**
     * 构造方法，传入聚合对象的具体内容
     */
    public ConcreteAggregate(Object[] objArray){
        this.objArray = objArray;
    }
    
    @Override
    public Iterator createIterator() {
        
        return new ConcreteIterator(this);
    }
    /**
     * 取值方法：向外界提供聚集元素
     */
    public Object getElement(int index){
        
        if(index < objArray.length){
            return objArray[index];
        }else{
            return null;
        }
    }
    /**
     * 取值方法：向外界提供聚集的大小
     */
    public int size(){
        return objArray.length;
    }
}

public class ConcreteIterator implements Iterator {
    //持有被迭代的具体的聚合对象
    private ConcreteAggregate agg;
    //内部索引，记录当前迭代到的索引位置
    private int index = 0;
    //记录当前聚集对象的大小
    private int size = 0;
    
    public ConcreteIterator(ConcreteAggregate agg){
        this.agg = agg;
        this.size = agg.size();
        index = 0;
    }
    /**
     * 迭代方法：返还当前元素
     */
    @Override
    public Object currentItem() {
        return agg.getElement(index);
    }
    /**
     * 迭代方法：移动到第一个元素
     */
    @Override
    public void first() {
        
        index = 0;
    }
    /**
     * 迭代方法：是否为最后一个元素
     */
    @Override
    public boolean isDone() {
        return (index >= size);
    }
    /**
     * 迭代方法：移动到下一个元素
     */
    @Override
    public void next() {

        if(index < size)
        {
            index ++;
        }
    }

}



Iterator it = list.iterator();
while(it.hasNext()){
    Object obj = it.next();

}

```
