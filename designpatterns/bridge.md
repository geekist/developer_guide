# 桥接模式

## 定义

桥接模式（Bridge Pattern），将抽象部分与它的实现部分分离，使它们都可以独立地变化。更容易理解的表述是：实现系统可从多种维度分类，桥接模式将各维度抽象出来，各维度独立变化，之后可通过聚合，将各维度组合起来，减少了各维度间的耦合。

## 场景 

不必要的继承导致类爆炸
汽车可按品牌分（本例中只考虑BMT，BenZ，Land Rover），也可按手动档、自动档、手自一体来分。如果对于每一种车都实现一个具体类，则一共要实现3*3=9个类。
![](http://www.jasongj.com/img/designpattern/bridge/Bridge.png)


## 实例
```java
抽象车

package com.jasongj.brand;

public abstract class AbstractCar {

  protected Transmission gear;
  
  public abstract void run();
  
  public void setTransmission(Transmission gear) {
    this.gear = gear;
  }
  
}
按品牌分，BMW牌车

package com.jasongj.brand;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class BMWCar extends AbstractCar{

  private static final Logger LOG = LoggerFactory.getLogger(BMWCar.class);
  
  @Override
  public void run() {
    gear.gear();
    LOG.info("BMW is running");
  };

}
BenZCar

package com.jasongj.brand;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class BenZCar extends AbstractCar{

  private static final Logger LOG = LoggerFactory.getLogger(BenZCar.class);
  
  @Override
  public void run() {
    gear.gear();
    LOG.info("BenZCar is running");
  };

}
LandRoverCar

public class LandRoverCar extends AbstractCar{

  private static final Logger LOG = LoggerFactory.getLogger(LandRoverCar.class);
  
  @Override
  public void run() {
    gear.gear();
    LOG.info("LandRoverCar is running");
  };

}
抽象变速器

public abstract class Transmission{

  public abstract void gear();

}
手动档

public class Manual extends Transmission {

  private static final Logger LOG = LoggerFactory.getLogger(Manual.class);

  @Override
  public void gear() {
    LOG.info("Manual transmission");
  }
}
自动档

public class Auto extends Transmission {

  private static final Logger LOG = LoggerFactory.getLogger(Auto.class);

  @Override
  public void gear() {
    LOG.info("Auto transmission");
  }
}
有了变速器和品牌两个维度各自的实现后，可以通过聚合，实现不同品牌不同变速器的车，如下

package com.jasongj.client;

import com.jasongj.brand.AbstractCar;
import com.jasongj.brand.BMWCar;
import com.jasongj.brand.BenZCar;
import com.jasongj.transmission.Auto;
import com.jasongj.transmission.Manual;
import com.jasongj.transmission.Transmission;

public class BridgeClient {

  public static void main(String[] args) {
    Transmission auto = new Auto();
    AbstractCar bmw = new BMWCar();
    bmw.setTransmission(auto);
    bmw.run();
    

    Transmission manual = new Manual();
    AbstractCar benz = new BenZCar();
    benz.setTransmission(manual);
    benz.run();
  }

}


```
