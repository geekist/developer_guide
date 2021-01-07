# Android 自动化测试框架

## 1.Monkey（socket）

* 原理：

Monkey是Android SDK自带的测试工具，在测试过程中通过socket通讯的方式来模拟用户会向系统发送伪随机的用户事件流，如按键输入、触摸屏输入、手势输入等)，实现对正在开发的应用程序进行压力测试，也有日志输出。实际上该工具只能做程序做一些压力测试，由于测试事件和数据都是随机的，不能自定义，所以有很大的局限性。Monkey 是一个命令行工具，可以运行在模拟器或实际设备中，通过向系统发送伪随机的用户事件流，实现对全系统或某个应用程序进行压力测试。

* 使用场景：

多用于进行稳定性测试。

* 利弊分析：

测试的对象仅为应用程序包，有一定的局限性。Monkey测试使用的事件流数据流是随机的，不能进行自定义。

## 2.Monkeyrunner（adb）

* 原理：

MonkeyRunner也是Android SDK提供的测试工具。严格意义上来说MonkeyRunner其实是一个Api工具包，比Monkey强大，可以编写测试脚本来自定义数据、事件。缺点是脚本用Python来写，对测试人员来说要求较高，有比较大的学习成本。通过adb进行对屏幕的元素dump进行解析，并且基于坐标发送点击操作，然后在这个基础上做封装。

* 使用场景：

多用于UI自动化测试

* 利弊分析：


1、可以完成一定复杂程度的测试用例，避免重复的人工工作量；

2、结合heap、strict mode等工具，可以实现压力测试，性能优化等任务；

3、某些API对机型兼容不好，例如4.0以下的机型，对viewnode server支持不友善，导致drag api不能正常工作；

4、由于monkeyrunner的实现基于python脚本，性能比较低下，对脚本的编写有质量要求。

## 3.Instrumentation

* 原理： 
 
base on robotium

Instrumentation是早期Google提供的Android自动化测试工具类，虽然在那时候JUnit也可以对Android进行测试，但是Instrumentation允许你对应用程序做更为复杂的测试，甚至是框架层面的。通过Instrumentation你可以模拟按键按下、抬起、屏幕点击、滚动等事件。Instrumentation是通过将主程序和测试程序运行在同一个进程来实现这些功能，你可以把Instrumentation看成一个类似Activity或者Service并且不带界面的组件，在程序运行期间监控你的主程序。

* 应用场景：

多用于单元测试

* 利弊分析：

缺点是对测试人员来说编写代码能力要求较高，需要对Android相关知识有一定了解，还需要配置AndroidManifest.xml文件，不能跨多个App。

## 4.Robotium

* 原理：

robotium是基于Instrumentation框架，通过InstrumentTestRunner调用起应用，通过java反射的原理，获取应用的界面元素，然后对界面元素进行操作。Robotium也是基于Instrumentation的测试框架，目前国内外用的比较多，资料比较多，社区也比较活跃。缺点是对测试人员来说要有一定的Java基础，了解Android基本组件，不能跨App。

* 应用场景：

主要针对Android平台的应用进行黑盒自动化测试

* 利弊分析：

缺点是对测试人员来说要有一定的Java基础，了解Android基本组件，不能跨App。

## 5.UIautomator

原理：

4.2之前是selendroid；4.2之后是Uiautomator
是Android提供的自动化测试框架，封装了selenium，UiAutomator是Google仿照微软Uiautomation提供的一套自动化框架，基于Android AccessilibilityService提供（注AndroidAccessilibilityService，是一个可访问服务，是一个为增强用户界面并帮助残疾用户的应用程序，或者用户可能无法完全与设备的交互。例如，用户在开车。那么用户就有可能需要添加额外的或者替代的用户反馈方式）。基本上支持所有的Android事件操作，对比Instrumentation它不需要测试人员了解代码实现细节（可以用UiAutomatorviewer抓去App页面上的控件属性而不看源码）。基于Java，测试代码结构简单、编写容易、学习成本，一次编译，所有设备或模拟器都能运行测试，能跨App（比如：很多App有选择相册、打开相机拍照，这就是跨App测试）。缺点是只支持SDK 16（Android 4.1）及以上，不支持Hybird App、WebApp。

* 应用场景：


Android平台的应用进行黑盒自动化测试，

* 利弊分析：

基本上支持所有的Android事件操作，对比Instrumentation它不需要测试人员了解代码实现细节（可以用UiAutomatorviewer抓去App页面上的控件属性而不看源码）。基于Java，测试代码结构简单、编写容易、学习成本，一次编译，所有设备或模拟器都能运行测试，能跨App（比如：很多App有选择相册、打开相机拍照，这就是跨App测试。

## 6.Appium

* 原理：

Appium是一个开源、跨平台的测试框架，可以用来测试原生及混合的移动端应用。Appium支持IOS、Android及FirefoxOS平台。Appium使用WebDriver的json wire协议，来驱动Apple系统的UIAutomation库、Android系统的UIAutomator框架。Appium对IOS系统的支持得益于Dan Cuellar’s对于IOS自动化的研究。Appium也集成了Selendroid，来支持老android版本。

* 应用场景：

Ios、android黑盒自动化测试

* 优点：

　　· 开源；

　　· 支持Native App、Hybird App、Web App；

　　· 支持Android、iOS、Firefox OS；

　　· Server也是跨平台的，你可以使用Mac OS X、Windows或者Linux；

　　它的哲理是：

　　· 用Appium自动化测试不需要重新编译App；

　　· 移动端自动化测试应该是开源的；

* 利弊分析：

Appium支持Selenium WebDriver支持的所有语言，如java、Object-C、JavaScript、Php、Python、Ruby、C#、Clojure，或者Perl语言，更可以使用Selenium WebDriver的Api。不需要为了自动化测试来重造轮子，因为扩展了WebDriver。（WebDriver是测试WebApps的一种简单、快速的自动化测试框架，所以有Web自动化测试经验的测试人员可以直接上手）Appium支持任何一种测试框架。如果只使用Apple的UIAutomation，我们只能用javascript来编写测试用例，而且只能用Instruction来运行测试用例。同样，如果只使用Google的UIAutomation，我们就只能用java来编写测试用例。Appium实现了真正的跨平台自动化测试。如果你在Windows使用Appium，你没法使用预编译专用于OS X的.app文件，因为Appium依赖OS X专用的库来支持iOS测试，所以在Windows平台你不能测试iOS Apps。这意味着你只能通过在Mac上来运行iOS测试。

## 7.Espresso

是Google的开源自动化测试框架。相对于Robotium和UIAutomator，它的特点是规模更小、更简洁，API更加精确，编写测试代码简单，容易快速上手。因为是基于Instrumentation的，所以不能跨App。



## 8.Selendroid

也是基于Instrumentation的测试框架，可以测试Native App、Hybird App、Web App，但是网上资料较少，社区活跃度也不大。



##9.Athrun

是淘宝出的一个移动测试框架/平台，同时支持iOS和Android。Android部分也是基于Instrumentation，在Android原有的

ActivityInstrumentationTestCase2类基础上进行了扩展，提供一整套面向对象的API。

**总结：**

综上所述，用于黑盒自动化测试功能最为强大的工具当属appium，monkey以及monkeyrunner功能有限，robotium 平台有限制，uiautomator对语言要求比较高，所以建议初学者可以先学appium，下次小编将与大家聊一聊appium自动化的那些事儿~
    