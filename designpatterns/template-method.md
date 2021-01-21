# 模板方法

## 定义

模板模式（Template Pattern），定义一个操作中算法的骨架，而将一些步骤延迟到子类中，模板方法使得子类可以不改变算法的结构，只是重定义该算法的某些特定步骤。这种类型的设计模式属于行为型模式。

## 场景

![](https://user-gold-cdn.xitu.io/2017/12/11/16044b4b9701301c?imageslim)

## 实例

```java


//抽象模板
public abstract class Game {
 
	//具体方法
 	public void initialize(){
 		System.out.println("game initialize .........");
 	}
 
 	//抽象方法
 	abstract void startPlay();
 	abstract void endPlay();
 
	//钩子方法
 	boolean isNeedEnd(){
 		return false;
 	}
 

 	//模板方法
 	//final修饰 不允许子类修改
 	public final void play(){
 		//初始化游戏
 		initialize();

 		//开始游戏
 		startPlay();

 		if(isNeedEnd()){
 			//结束游戏
 			endPlay();
 		}
 	}
}

/**
 * 具体模板
 * 足球游戏
 */
public class Football extends Game{
 	@Override
 	void endPlay() {
 		System.out.println("Football Game Finished!");
 	}

 	@Override
 	void startPlay() {
 		System.out.println("Football Game Started. Enjoy the game!");
 	}

 	@Override
 	boolean isNeedEnd() {
 		return false;
 	}
}

/**
 * 模板测试类
 */
public class TemplateModeTest {
	public static void main(String[] args) {
 		Game game = new Basketball();
 		game.play();
 		game = new Football();
 		game.play();
 	}
}

```