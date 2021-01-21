
# 责任链模式

## 定义

责任链模式是一种对象的行为模式。在责任链模式里，很多对象由每一个对象对其下家的引用而连接起来形成一条链。请求在这个链上传递，直到链上的某一个对象决定处理此请求。发出这个请求的客户端并不知道链上的哪一个对象最终处理这个请求，这使得系统可以在不影响客户端的情况下动态地重新组织和分配责任。

## 场景

![](https://img-blog.csdn.net/20170622092325576?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcWlhbjUyMGFv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 实例

Handler : 抽象处理者，定义出一个处理请求的接口，抽象方法handleRequest()规范子类处理请求的操作。

ConcreteHandler : 具体处理者，具体处理者接到请求后，可以选择将请求处理掉，或者将请求传给下家。

Client : 客户端，设置不同的具体处理者的层级。


```java
使用责任链模式（不纯）重构请假流程
请假信息类，包含请假人姓名和请假天数
@Data
@AllArgsConstructor
public class LeaveRequest {
    private String name;    // 请假人姓名
    private int numOfDays;  // 请假天数
}



抽象处理者类 Handler，维护一个 nextHandler 属性，该属性为当前处理者的下一个处理者的引用；声明了抽象方法 process
@Data
public abstract class Handler {
    protected String name; // 处理者姓名
    protected Handler nextHandler;  // 下一个处理者

    public Handler(String name) {
        this.name = name;
    }

    public abstract boolean process(LeaveRequest leaveRequest); // 处理请假
}

三个具体处理类，分别实现了抽象处理类的 process 方法
// 主管处理者
public class Director extends Handler {
    public Director(String name) {
        super(name);
    }

    @Override
    public boolean process(LeaveRequest leaveRequest) {
        boolean result = (new Random().nextInt(10)) > 3; 

// 随机数大于3则为批准，否则不批准
        String log = "主管<%s> 审批 <%s> 的请假申请，请假天数： <%d> ，审批结果：<%s> ";
        System.out.println(String.format(log, this.name, leaveRequest.getName(), leaveRequest.getNumOfDays(), result == true ? "批准" : "不批准"));

        if (result == false) {  // 不批准
            return false;
        } else if (leaveRequest.getNumOfDays() < 3) { // 批准且天数小于3，返回true
            return true;
        }
        return nextHandler.process(leaveRequest);   // 批准且天数大于等于3，提交给下一个处理者处理
    }
}

// 经理
public class Manager extends Handler {
    public Manager(String name) {
        super(name);
    }

    @Override
    public boolean process(LeaveRequest leaveRequest) {
        boolean result = (new Random().nextInt(10)) > 3; // 随机数大于3则为批准，否则不批准
        String log = "经理<%s> 审批 <%s> 的请假申请，请假天数： <%d> ，审批结果：<%s> ";
        System.out.println(String.format(log, this.name, leaveRequest.getName(), leaveRequest.getNumOfDays(), result == true ? "批准" : "不批准"));

        if (result == false) {  // 不批准
            return false;
        } else if (leaveRequest.getNumOfDays() < 7) { // 批准且天数小于7
            return true;
        }
        return nextHandler.process(leaveRequest);   // 批准且天数大于等于7，提交给下一个处理者处理
    }
}

// 总经理
public class TopManager extends Handler {
    public TopManager(String name) {
        super(name);
    }

    @Override
    public boolean process(LeaveRequest leaveRequest) {
        boolean result = (new Random().nextInt(10)) > 3; // 随机数大于3则为批准，否则不批准
        String log = "总经理<%s> 审批 <%s> 的请假申请，请假天数： <%d> ，审批结果：<%s> ";
        System.out.println(String.format(log, this.name, leaveRequest.getName(), leaveRequest.getNumOfDays(), result == true ? "批准" : "不批准"));

        if (result == false) { // 总经理不批准
            return false;
        }
        return true;    // 总经理最后批准
    }
}


复制代码客户端测试
public class Client {
    public static void main(String[] args) {
        Handler zhangsan = new Director("张三");
        Handler lisi = new Manager("李四");
        Handler wangwu = new TopManager("王五");

        // 创建责任链
        zhangsan.setNextHandler(lisi);
        lisi.setNextHandler(wangwu);

        // 发起请假申请
        boolean result1 = zhangsan.process(new LeaveRequest("小旋锋", 1));
        System.out.println("最终结果：" + result1 + "\n");

        boolean result2 = zhangsan.process(new LeaveRequest("小旋锋", 4));
        System.out.println("最终结果：" + result2 + "\n");

        boolean result3 = zhangsan.process(new LeaveRequest("小旋锋", 8));
        System.out.println("最终结果：" + result3 + "\n");
    }
}

作者：小旋锋
链接：https://juejin.cn/post/6844903702260629512
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```