
# 命令模式

## 定义

将一个请求封装为一个对象，从而使你可以用不同的请求对客户进行参数化


命令模式的本质是对命令进行封装，将发出命令的责任和执行命令的责任分割开。

命令模式使请求本身成为一个对象，这个对象和其他对象一样可以被存储和传递。

命令模式的关键在于引入了抽象命令接口，且发送者针对抽象命令接口编程，只有实现了抽象命令接口的具体命令才能与接收者相关联。

## 场景
将军-命令-士兵-传令兵

Retrofit的get、post、命令等

![](https://duanguangguang.github.io/2017/09/17/designPattern/command/dp1802.jpg)



## 实例

小女孩茱丽(Julia)有一个盒式录音机，此录音机有播音(Play)、倒带(Rewind)和停止(Stop)功能，录音机的键盘便是请求者(Invoker)角色；茱丽(Julia)是客户端角色，而录音机便是接收者角色。Command类扮演抽象命令角色，而PlayCommand、StopCommand和RewindCommand便是具体命令类。茱丽(Julia)不需要知道播音(play)、倒带(rewind)和停止(stop)功能是怎么具体执行的，这些命令执行的细节全都由键盘(Keypad)具体实施。茱丽(Julia)只需要在键盘上按下相应的键便可以了。

　　录音机是典型的命令模式。录音机按键把客户端与录音机的操作细节分割开来。


抽象命令类


```java
public interface Command {
    /**
     * 执行方法
     */
    public void execute();
}
```

具体的命令类


```java
public class PlayCommand implements Command {

    private AudioPlayer myAudio;
    
    public PlayCommand(AudioPlayer audioPlayer){
        myAudio = audioPlayer;
    }
    /**
     * 执行方法
     */
    @Override
    public void execute() {
        myAudio.play();
    }

}

public class RewindCommand implements Command {

    private AudioPlayer myAudio;
    
    public RewindCommand(AudioPlayer audioPlayer){
        myAudio = audioPlayer;
    }
    @Override
    public void execute() {
        myAudio.rewind();
    }

}
```

命令执行者：


```java
public class AudioPlayer {
    
    public void play(){
        System.out.println("播放...");
    }
    
    public void rewind(){
        System.out.println("倒带...");
    }
    
    public void stop(){
        System.out.println("停止...");
    }
}
````

命令请求者：


```java
public class Keypad {
    private Command playCommand;
    private Command rewindCommand;
    private Command stopCommand;
    
    public void setPlayCommand(Command playCommand) {
        this.playCommand = playCommand;
    }
    public void setRewindCommand(Command rewindCommand) {
        this.rewindCommand = rewindCommand;
    }
    public void setStopCommand(Command stopCommand) {
        this.stopCommand = stopCommand;
    }
    /**
     * 执行播放方法
     */
    public void play(){
        playCommand.execute();
    }
    /**
     * 执行倒带方法
     */
    public void rewind(){
        rewindCommand.execute();
    }
    /**
     * 执行播放方法
     */
    public void stop(){
        stopCommand.execute();
    }
}
```

客户端：

```java

public class Julia {
    public static void main(String[]args){
        //创建接收者对象
        AudioPlayer audioPlayer = new AudioPlayer();
        //创建命令对象
        Command playCommand = new PlayCommand(audioPlayer);
        Command rewindCommand = new RewindCommand(audioPlayer);
        Command stopCommand = new StopCommand(audioPlayer);
        //创建请求者对象
        Keypad keypad = new Keypad();
        keypad.setPlayCommand(playCommand);
        keypad.setRewindCommand(rewindCommand);
        keypad.setStopCommand(stopCommand);
        //测试
        keypad.play();
        keypad.rewind();
        keypad.stop();
        keypad.play();
        keypad.stop();
    }
}

```