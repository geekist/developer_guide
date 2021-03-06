

# Handler

## Handler的概念和使用场景

Handler是Android提供的一套异步消息处理机制，用来处理不同线程之间的通信。

一个非常广泛的使用场景是UI的处理只能在主线程中进行，我们可以将handler定义在主线程中，然后将其传递到其他线程中。当其他线程需要处理UI时，通过子线程中的handler传递消息，handler接受到消息后，在UI线程中处理UI的变化。

## Handler原理
使用Handler方式进行异步消息处理主要由Message，Handler，MessageQueue，Looper四部分组成：

* Message

线程之间传递的消息，用于不同线程之间的数据交互。Message中的what字段用来标记区分多个消息，arg1、arg2 字段用来传递int类型的数据，obj可以传递任意类型的字段。

* Handler

用于发送和处理消息。其中的sendMessage()用来发送消息，handleMessage()用于消息处理，进行相应的UI操作。

* MessageQueue

消息队列（先进先出），用于存放Handler发送的消息，一个线程只有一个消息队列。

* Looper

可以理解为消息队列的管理者，当发现MessageQueue中存在消息，Looper就会将消息传递到handleMessage()方法中，同样，一个线程只有一个Looper。

要使用Handler实现异步消息处理，首先我们需要在主线程中创建Handler对象并重写handleMessage()方法，然后当子线程中需要进行UI操作时，就创建一个Message对象，并通过Handlerr将这条消息发送出去。之后这条消息会被添加到MessageQueue的队列中等待被处理，而Looper则会一直尝试从MessageQueue中取出待处理消息，最后分发回Handler的handleMessage()方法中。由于Halldler是在主线程中创建的，所以此时handleMessage()方法中的代码也会在主线程中运行，从而实现子线程通过Handler机制实现UI线程操作的目的。

## handler用法示例：
```java
public class MainActivity extends AppCompatActivity {

     MyHandler timerHandler = new MyHandler(this);
	//定义一个handler在主线程中
	//这里的构造函数里的this是默认获得了主线程的looper，即Looper.getMainLooper()

	private static class MyHandler extends Handler {
        WeakReference<MainActivity> weakReference;

        public MyHandler(MainActivity activity) {
            weakReference = new WeakReference<>(activity);
        }

        @Override
        public void handleMessage(@NonNull Message msg) {
            switch (msg.what) {
                case 1:
					//handleMessage定义在主线程中，消息相应也在主线程中
                    MainActivity activity = weakReference.get();
                    activity.setTitle("hear me?");
                    break;
            }

            super.handleMessage(msg);
        }
    }

    //@SuppressLint("HandlerLeak")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

		//启动一个线程

        Timer timer = new Timer();
        TimerTask timerTask = new TimerTask() {
            @Override
            public void run() {
				//在线程中handler发送一个消息
                Message message = new Message();
                message.what = 1;
                timerHandler.sendMessage(message);
            }
        };

        timer.schedule(timerTask, 1000);
    }
}
```

## handler与内存泄露
方法一：通过程序逻辑进行保护：

（1）在关闭Activity时停掉对应的后台线程。线程停止就相当于切断了Handle和外部链接的线，Activity自然会在合适的时候被回收。

（2）如果Handler是被delay的Message持有了引用，那就使用Handler的removeCallbacks()方法将消息对象从消息队列移除即可。

方法二：将Handler声明为静态类，静态类不持有外部类的对象，所以Activity可以被随意回收。此处使用了弱引用WeakReference，也就是说当在内存不足时，系统会销毁弱/回收引用引用的对象，从而达到优化内存的目的。



