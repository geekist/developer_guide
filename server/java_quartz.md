
## Spring 定时任务


### 一、java内置的实现定时功能的类

在 JDK 中，内置了两个类，可以实现定时任务的功能：

* java.util.Timer ：

可以通过创建 java.util.TimerTask 调度任务，在同一个线程中串行执行，相互影响。也就是说，对于同一个 Timer 里的多个 TimerTask 任务，如果一个 TimerTask 任务在执行中，其它 TimerTask 即使到达执行的时间，也只能排队等待。因为 Timer 是串行的，同时存在 坑坑 ，所以后来 JDK 又推出了 ScheduledExecutorService ，Timer 也基本不再使用。

* java.util.concurrent.ScheduledExecutorService ：

在 JDK 1.5 新增，基于线程池设计的定时任务类，每个调度任务都会被分配到线程池中并发执行，互不影响。这样，ScheduledExecutorService 就解决了 Timer 串行的问题。

* 缺点：

在日常开发中，我们很少直接使用 Timer 或 ScheduledExecutorService 来实现定时任务的需求。主要有几点原因：

1、它们仅支持按照指定频率，不直接支持指定时间的定时调度，需要我们结合 Calendar 自行计算，才能实现复杂时间的调度。例如说，每天、每周五、2019-11-11 等等。

2、它们是进程级别，而我们为了实现定时任务的高可用，需要部署多个进程。此时需要等多考虑，多个进程下，同一个任务在相同时刻，不能重复执行。

3、项目可能存在定时任务较多，需要统一的管理，此时不得不进行二次封装。

### 二、Spring 定时任务

关于“任务”的叫法，也有叫“作业”的。在英文上，有 Task 也有 Job 。本质是一样的，本文两种都会用。一般来说是调度任务，定时执行。

在 Spring 体系中，内置了两种定时任务的解决方案：

#### 2.1 Spring Task

第一种，Spring Framework 的 Spring Task 模块，提供了轻量级的定时任务的实现。

* 配置

spring task 不需要特殊配置，是spring framework内置的；

* 创建和执行任务

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;
import java.util.concurrent.atomic.AtomicInteger;

@Component
//@Configuration // 1.主要用于标记配置类，兼备Component的效果。
@EnableScheduling // 2.开启定时任务
public class DemoJob {
    private final AtomicInteger counts = new AtomicInteger();

    @Scheduled(fixedRate = 2000)
    public void execute() {
        String str = "[execute][定时第" + counts.incrementAndGet() + "次执行]\"";
        System.out.println(str);
    }
}


```


@EnableScheduling 开启对定时任务的支持
@Scheduled 可以作为一个触发源添加到一个方法中

   其中Scheduled注解中有以下几个参数：

　　1.cron是设置定时执行的表达式，如 0 0/5 * * * ?每隔五分钟执行一次 秒 分 时 天 月

　　2.zone表示执行时间的时区

　　3.fixedDelay 和fixedDelayString 表示一个固定延迟时间执行，上个任务完成后，延迟多长时间执行

　　4.fixedRate 和fixedRateString表示一个固定频率执行，上个任务开始后，多长时间后开始执行

　　5.initialDelay 和initialDelayString表示一个初始延迟时间，第一次被调用前延迟的时间





#### 2.2 Spring Boot Quartz

第二种，Spring Boot 2.0 版本，整合了 Quartz 作业调度框架，提供了功能强大的定时任务的实现。

>注：Spring Framework 已经内置了 Quartz 的整合。Spring Boot 1.X 版本未提供 Quartz 的自动化配置，而 2.X 版本提供了支持。


在 Java 生态中，还有非常多优秀的开源的调度任务中间件：

Elastic-Job

唯品会基于 Elastic-Job 之上，演化出了 Saturn 项目。

Apache DolphinScheduler

XXL-JOB




