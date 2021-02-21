# 状态模式

## 定义
状态模式是一种行为设计模式。适用于当对象的内在状态改变它自身的行为时。

如果想基于对象的状态来改变自身的行为，通常利用对象的状态变量及if-else条件子句来扮演针对对象的不同行为。状态模式Context(环境)和State(状态)分离的方式既保证状态与行为的联动变化，又使得这种变化是条理明晰且松耦合的。

## 场景
![](https://design-patterns.readthedocs.io/zh_CN/latest/_images/State.jpg)
## 实例

假如想实现电视遥控器，使用简单按键来表现动作。如果状态是ON，电视将被打开，如果状态是OFF，电视将被关闭。

```java
package com.journaldev.design.state;

public class TVRemoteBasic {

 private String state="";

 public void setState(String state){
 	this.state=state;
 }

 public void doAction(){
 	if(state.equalsIgnoreCase("ON")){
 		System.out.println("TV is turned ON");
 	}else if(state.equalsIgnoreCase("OFF")){
 		System.out.println("TV is turned OFF");
 	}
 }

 public static void main(String args[]){
 	TVRemoteBasic remote = new TVRemoteBasic();

 	remote.setState("ON");
 	remote.doAction();

 	remote.setState("OFF");
 	remote.doAction();
 }

}

```

* 将状态抽象出来，并且实现doAction方法

```java
package com.journaldev.design.state;

public interface State {

 public void doAction();
}

```
具体State实现
自此例子中，包含两个状态：一个是打开电视的状态，一个关闭电视的状态。因此，需要创建两个具体状态类代表这两个行为。

```java
package com.journaldev.design.state;

public class TVStartState implements State {

 @Override
 public void doAction() {
 System.out.println("TV is turned ON");
 }

}

```

Context类实现，能够基于内在对象来改变自身的行为。

```java
package com.journaldev.design.state;

public class TVContext implements State {

 private State tvState;

 public void setState(State state) {
 this.tvState=state;
 }

 public State getState() {
 return this.tvState;
 }

 @Override
 public void doAction() {
 this.tvState.doAction();
 }

}

```
实例代码

```java

package com.journaldev.design.state;

public class TVRemote {

 public static void main(String[] args) {
 TVContext context = new TVContext();
 State tvStartState = new TVStartState();
 State tvStopState = new TVStopState();

 context.setState(tvStartState);
 context.doAction();

 context.setState(tvStopState);
 context.doAction();

 }

}
```

