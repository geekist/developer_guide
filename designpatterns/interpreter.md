# 解释器模式

## 定义 

解释器模式是类的行为模式。给定一个语言之后，解释器模式可以定义出其文法的一种表示，并同时提供一个解释器。客户端可以使用这个解释器来解释这个语言中的句子。

## 场景

![](https://img-my.csdn.net/uploads/201206/15/1339734799_8167.jpg)

## 实例


```java
public interface Expression {
	public int interpret(Context context);
}

public class Plus implements Expression {
	@Override
	public int interpret(Context context) {
		return context.getNum1()+context.getNum2();
	}
}

public class Minus implements Expression {

	@Override 
	public int interpret(Context context) {
		return context.getNum1()-context.getNum2();
	} 
}

public class Context {
	private int num1;
	private int num2;

	public Context(int num1, int num2) {
		this.num1 = num1; 
		this.num2 = num2; 
	}

	public int getNum1() {
		return num1;
	}	

	public void setNum1(int num1) {
		this.num1 = num1; 
	} 

	public int getNum2() {
		return num2; 
	}

	public void setNum2(int num2) {
		this.num2 = num2; 
	} 
}

public class Test {
	public static void main(String[] args) {
	Context context = new Context(1,2);
	int result = new Plus().interpret(context);
} 
```