# Android版本兼容性问题

[**Android版本名称及Api版本对照表**](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)


[**Android屏幕分辨率使用情况**](https://developer.android.com/about/dashboards)

一个好的APP最好支持90%设备，由于不同版本系统提供的API可能不同，所以了解不同版本间系统差异很重要，这样才能更好的适配更多的智能设备。你的应用足不足够健壮要看你的应用在主流版本运行是否流畅。这篇文章记录开发过程中遇到的相对重要以及常用的适配方案，希望对读者有所帮助。

## Android5

### 1、Android运行时由Android核心库集和Dalvike虚拟机改成Android核心库集和ART（Android Runtime）模式

两者的区别就是Dalvike虚拟机采用了一种被称为JIT（just-in-time）的解释器进行动态编译,而ART模式则在用户安装App是进行预编译AOT（Ahead-of-time），将android5.X的运行速度提高了3倍左右。

ART的特性：

1: 用户安装应用时就进行预编译操作，将原本在程序运行中时的编译动作提前到应用安装时。在省去解释代码这一过程之后，应用的运行效率会更高。

缺点：(1) 安装时间增加 (2) 安装后的文件占用更多空间？(外存储器)


2: 解决垃圾回收 (GC) 问题
在 Dalvik 中，应用常常发现显式调用 System.gc() 非常有用，可促进垃圾回收 (GC)。对 ART 而言这种做法的必要性低得多，尤其是当您需要通过垃圾回收来预防出现 GC_FOR_ALLOC 类型或减少碎片时。
而且，Android 开源项目 (AOSP) 中正在开发一种紧凑型垃圾回收器，以改善内存管理。

3：预防 JNI 问题
ART 的 JNI 比 Dalvik 的 JNI 更为严格一些。使用 CheckJNI 模式来捕获常见问题是一种特别实用的方法。

1): 检查 JNI 代码中的垃圾回收问题

2): 错误处理 ART 的 JNI 会在多种情况下引发错误，而 Dalvik 则不然。（同样地，您可以通过使用 CheckJNI 执行测试来捕获大量此种情况）

3): 预防堆栈大小问题  Dalvik 具有单独的原生代码堆栈和 Java 代码堆栈，并且默认的 Java 堆栈大小为 32KB，默认的原生堆栈大小为 1MB。

### 2、从5.0开始，在同一个layout下，即使你在Button上覆盖了相应的View，Button将总是位于最上层。

产生原因：stateListAnimator属性。谷歌在Material Design中推出,是一个非常简单的方法用来实现在可视状态之间平滑过渡。这个属性可以通过android:stateListAnimator进行设置，可以使控件在点击时产生不同的交互。对于Button，点击时默认有个阴影的效果用于表示按下的状态（5.0以前就是简单的变色）。 

解决方法：可以使用 android:stateListAnimator="@null" 去掉阴影效果而使Button可以被正常的覆盖。
```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:stateListAnimator="@null"/>
```

### 3、5.0之后隐式打开服务需要指明包名

```xml
/*把之前的一个android测试项目安装至android 5.0上进行测试后，程序崩溃，控制台报如下错误：

java.lang.IllegalArgumentException: Service Intent must be explicit
如错误提示所示，在android 5.0版本以后，service intent必须为显式指出。*/
//例如：
        Intent intent = new Intent("action that you explicit");
        intent.setPackage("your package");
        bindService(intent, connection, BIND_AUTO_CREATE);
        //关于aidl还需要指出的一点是：在传递数据时一定要序列化（即实现Parcelable接口，基本数据类型除外）


```
## Android6

### 1、Android6.0之后涉及隐私的权限必须动态申请

动态权限适配是 Android 6.0 最先开始的，也是 Android 系统对开发者影响最大的改动之一。系统权限主要分为两类，正常权限和危险权限。不管哪个版本的android，你应用中所用到的所有权限，不管是正常权限还是危险权限，都需要在应用Manifest中申明。你的目标SDK(targetSdkVersion)是23以及23以上版本：应用必须在Manifest中罗列出所有的权限，并且在程序运行时，它必须请求用户授予每一个危险权限，此时用户可以授予或者拒绝每一个权限，并且应用程序可以继续运行有限的功能，即使用户拒绝了权限请。在 Android 6.0 ~ Android 8.0中，如果应用在运行时请求权限并且被授予该权限，系统会错误地将属于同一权限组并且在清单中注册的其他权限也一起授予应用，即对于同一组内的权限，只要有一个被同意，其他的都会被同意。在 Android 8.0 之后，此行为已被纠正。系统只会授予应用明确请求的权限。然而一旦用户为应用授予某个权限，则所有后续对该权限组中权限的请求都将被自动批准，但是若没有请求相应的权限而进行操作的话就会出现应用 crash 的情况。

[Android危险权限分组说明](https://developer.android.com/guide/topics/permissions/overview?hl=zh-cn)
```xml
<!--CALENDAR-->
<uses-permission android:name="android.permission.READ_CALENDAR"/>
<uses-permission android:name="android.permission.WRITE_CALENDAR"/>
<!--CAMERA-->
<uses-permission android:name="android.permission.CAMERA"/>
<!--CONTACTS-->
<uses-permission android:name="android.permission.READ_CONTACTS"/>
<uses-permission android:name="android.permission.WRITE_CONTACTS"/>
<uses-permission android:name="android.permission.GET_ACCOUNTS"/>
<!--LOCATION-->
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<!--MICROPHONE-->
<uses-permission android:name="android.permission.RECORD_AUDIO"/>
<!--PHONE-->
<uses-permission android:name="android.permission.READ_PHONE_STATE"/>
<uses-permission android:name="android.permission.CALL_PHONE"/>
<uses-permission android:name="android.permission.READ_CALL_LOG"/>
<uses-permission android:name="android.permission.ADD_VOICEMAIL"/>
<uses-permission android:name="android.permission.WRITE_CALL_LOG"/>
<uses-permission android:name="android.permission.USE_SIP"/>
<uses-permission android:name="android.permission.PROCESS_OUTGOING_CALLS"/>
<uses-permission android:name="android.permission.ANSWER_PHONE_CALLS"/>
<uses-permission android:name="android.permission.READ_PHONE_NUMBERS"/>
<!--SENSORS-->
<uses-permission android:name="android.permission.BODY_SENSORS"/>
<!--SMS-->
<uses-permission android:name="android.permission.SEND_SMS"/>
<uses-permission android:name="android.permission.RECEIVE_SMS"/>
<uses-permission android:name="android.permission.READ_SMS"/>
<uses-permission android:name="android.permission.RECEIVE_WAP_PUSH"/>
<uses-permission android:name="android.permission.RECEIVE_MMS"/>
<!--STORAGE-->
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```
一个简单的规避办法：
```xml
android {
    ...
    defaultConfig {
        ...
        targetSdkVersion 22 // 不使用api:23以及以上的动态权限策略
        ...
    }
}


```

### 2、取消支持 Apache HTTP 客户端
解决办法：
在build.gradle中加入：

android {
    useLibrary 'org.apache.http.legacy'
}
声明编译时依赖项：


需要在应用的AndroidManifest.xml文件中添加：
<uses-library 
android:name="org.apache.http.legacy" 
android:required="false"/>


### 3、对wifi的使用要求更加严格
* 1、首先在AndroidManifest.xml文件中增加以下权限
```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"></uses-permission>
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"></uses-permission>

```

* 2、其次还需要动态申请定位权限组
```java
ActivityCompat.requestPermissions(getActivity(),new String[]{Manifest.permission.ACCESS_FINE_LOCATION,Manifest.permission.ACCESS_COARSE_LOCATION},REQUEST_CODE_ACCESS_COARSE_LOCATION);


```

* 3、最后如果是用getScanResults()获取Wifi列表的话还需要打开GPS(位置信息)开关。
```java
if(!isGPSOpen()){
    //跳转到手机原生设置页面,打开定位功能
    Intent intent = new Intent(Settings.ACTION_LOCATION_SOURCE_SETTINGS);
    this.startActivityForResult(intent,GPS_REQUEST_CODE);
}else{
    //你的业务逻辑
}

/**
 * 检查有没打开定位
 */
private boolean isGPSOpen() {
    LocationManager locationManager = (LocationManager) activity.getSystemService(Context.LOCATION_SERVICE);
    return locationManager.isProviderEnabled(LocationManager.GPS_PROVIDER);
}

```

## Andrid7
### 1、FileProvider在应用间共享文件
在官方7.0的以上的系统中，尝试传递 file://URI可能会触发FileUriExposedException。要应用间共享文件，您应发送一项 content:// URI，并授予 URI 临时访问权限。进行此授权的最简单方式是使用 FileProvider类。

* 1、创建新的FileProvider
```java
/**
 * 继承FileProvider，防止冲突
 */
public class RoProvider extends FileProvider {

}

```


* 2、创建file_path的xml文件
```xml
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <root-path name="root" path="." />
    <files-path name="files" path="" />
    <cache-path name="cache" path="" />
    <external-path name="external" path="" />
    <external-files-path name="external-files" path="" />
    <external-cache-path name="external-cache" path="" />
</paths>


```
* 3、在manifest文件中注册fileProvider
```xml
<provider
    android:name=".RoProvider"
    android:authorities="${applicationId}.fileProvider"
    android:exported="false"
    android:grantUriPermissions="true">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/file_paths" />
</provider>



```

### 2、SharedPreferences闪退问题
```java
// MODE_WORLD_READABLE：Android 7.0以后不能使用这个获取，会闪退
// 应修改成MODE_PRIVATE
SharedPreferences read = getSharedPreferences(RELEASE_POOL_DATA, MODE_WORLD_READABLE);
```

## Android8

### 1、通知权限默认关闭
Android 8.0之后通知权限默认都是关闭的，无法默认开启以及通过程序去主动开启，需要程序员读取权限开启情况，然后提示用户去开启。

* 判断权限是否开启
```java
/**
 * 判断通知权限是否开启
 * @param context 上下文
 */
public static boolean isNotificationEnabled(Context context){
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
        return ((NotificationManager) context.getSystemService(Context.NOTIFICATION_SERVICE)).areNotificationsEnabled();
    } else if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
        AppOpsManager appOps = (AppOpsManager) context.getSystemService(Context.APP_OPS_SERVICE);
        ApplicationInfo appInfo = context.getApplicationInfo();
        String pkg = context.getApplicationContext().getPackageName();
        int uid = appInfo.uid;

        try {
            Class<?> appOpsClass = Class.forName(AppOpsManager.class.getName());
            Method checkOpNoThrowMethod = appOpsClass.getMethod("checkOpNoThrow", Integer.TYPE, Integer.TYPE, String.class);
            Field opPostNotificationValue = appOpsClass.getDeclaredField("OP_POST_NOTIFICATION");
            int value = (Integer) opPostNotificationValue.get(Integer.class);
            return (Integer) checkOpNoThrowMethod.invoke(appOps, value, uid, pkg) == 0;
        } catch (NoSuchMethodException | NoSuchFieldException | InvocationTargetException | IllegalAccessException | RuntimeException | ClassNotFoundException ignored) {
            return true;
        }
    } else {
        return true;
    }
}


``` 

* 前往设置开启权限

```java
/**
 * 打开设置页面打开权限
 *
 * @param activity activity
 * @param requestCode 这里的requestCode和onActivityResult中requestCode要一致
 */
public static void startSettingActivity(@NonNull Activity activity, int requestCode) {
    try {
        Intent intent = new Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS, Uri.parse("package:" + activity.getPackageName()));
        intent.addCategory(Intent.CATEGORY_DEFAULT);
        activity.startActivityForResult(intent, requestCode);
    } catch (Exception e) {
        e.printStackTrace();
    }
}


```
 
### 2、新增通知渠道
Android 8.0中，为了更好的管制通知的提醒，不想一些不重要的通知打扰用户，新增了通知渠道，用户可以根据渠道来屏蔽一些不想要的通知。
* 创建通知
```java
private void createNotificationChannel() {
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
        NotificationManager notificationManager = (NotificationManager)
                getSystemService(Context.NOTIFICATION_SERVICE);
        //分组（可选）
        //groupId要唯一
        String groupId = "group_001";
        NotificationChannelGroup group = new NotificationChannelGroup(groupId, "广告");
        //创建group
        notificationManager.createNotificationChannelGroup(group);
        //channelId要唯一
        String channelId = "channel_001";
        NotificationChannel adChannel = new NotificationChannel(channelId,
                "推广信息", NotificationManager.IMPORTANCE_DEFAULT);
        //补充channel的含义（可选）
        adChannel.setDescription("推广信息");
        //将渠道添加进组（先创建组才能添加）
        adChannel.setGroup(groupId);
        //创建channel
        notificationManager.createNotificationChannel(adChannel);
        //创建通知时，标记你的渠道id
        Notification notification = new Notification.Builder(MainActivity.this, channelId)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher))
                .setContentTitle("一条新通知")
                .setContentText("这是一条测试消息")
                .setAutoCancel(true)
                .build();
        notificationManager.notify(1, notification);
    }
}
```

### 3、自适应启动图标
从Android 8.0系统开始，应用程序的图标被分为了两层：前景层和背景层。也就是说，我们在设计应用图标的时候，需要将前景和背景部分分离，前景用来展示应用图标的Logo，背景用来衬托应用图标的Logo。需要注意的是，背景层在设计的时候只允许定义颜色和纹理，但是不能定义形状。注意图标图层的大小，两层的尺寸必须为108x108dp，前景图层中间的72x72dp图层就是在手机界面上展示的应用图标范围。这样系统在四面各留出18dp以产生有趣的视觉效果，如视差或脉冲（动画视觉效果由受支持的启动器生成，视觉效果可能因发射器而异）。
* mipmap-anydpi-v26文件夹
```xml
<?xml version="1.0" encoding="utf-8"?>
<adaptive-icon xmlns:android="http://schemas.android.com/apk/res/android">
    <background android:drawable="@drawable/ic_launcher_background" />
    <foreground android:drawable="@drawable/ic_launcher_foreground" />
</adaptive-icon>

```

### 4、App安装功能必须添加权限
Android 8.0去除了“允许未知来源”选项，如果我们的App具备安装App的功能，那么AndroidManifest文件需要包含REQUEST_INSTALL_PACKAGES权限，未声明此权限的应用将无法安装其他应用。当然，如果你不想添加这个权限，也可以通过getPackageManager().canRequestPackageInstalls()查询是否有此权限，没有的话使用Settings.ACTION_MANAGE_UNKNOWN_APP_SOURCES这个action将用户引导至安装未知应用权限界面去授权。

### 5、静态注册的广播无法正常接收
Google官方声明：从android 8.0（API26）开始，对清单文件中静态注册广播接收者增加了限制，建议大家不要在清单文件中静态注册广播接收者，改为动态注册。当然，如果你还是想用静态注册的方式也是有方法的，Intent里添加Component参数可实现。
解决办法：
```java
Intent intent = new Intent( "广播的action" );
intent.setComponent( new ComponentName( "包名(如:com.yhd.rocket)","接收器的完整路径(如:com.yhd.rocket.receiver.RoReceiver)" ) );
sendBroadcast(intent);
```

## Android9

### 1、刘海屏Api支持

Android 9 支持最新的全面屏，其中包含为摄像头和扬声器预留空间的屏幕缺口。 通过 DisplayCutout类可确定非功能区域的位置和形状，这些区域不应显示内容。 要确定这些屏幕缺口区域是否存在及其位置，使用 getDisplayCutout() 函数。
* 取得区域位置
* 
```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.P) {
    View decorView = getWindow().getDecorView();
    WindowInsets rootWindowInsets = decorView.getRootWindowInsets();
    if (rootWindowInsets != null) {
        DisplayCutout cutout = rootWindowInsets.getDisplayCutout();
        List<Rect> boundingRects = cutout.getBoundingRects();
        if (boundingRects != null && boundingRects.size() > 0) {
            String msg = "";
            for (Rect rect : boundingRects) {
                msg = msg +"left-" + rect.left;
                Log.d(TAG, msg);
            }
         }
    }
}


```

* 允许程序请求是否在挖孔区域布局
* 
```java
class WindowManager.LayoutParams {
    //布局参数
    int layoutInDisplayCutoutMode;
    //默认情况下，全屏窗口不会使用到挖孔区域，非全屏窗口可正常使用挖孔区域。
    final int LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT;
    //窗口声明使用挖孔区域
    final int LAYOUT_IN_DISPLAY_CUTOUT_MODE_ALWAYS;
    //窗口声明不使用挖孔区域
    final int LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER;
}


```
* 设置代码
* 
```java
WindowManager.LayoutParams lp = getWindow().getAttributes();
lp.layoutInDisplayCutoutMode = WindowManager.LayoutParams.LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT;
getWindow().setAttributes(lp);


```

### 2、禁止http方式访问网络
Android P 限制了明文流量的网络请求，非加密的流量请求(http)都会被系统禁止掉。
解决办法：
* 在资源文件新建xml目录，新建文件network_security_config.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true" />
</network-security-config>


```


* 清单文件配置：
```xml
<application
    android:networkSecurityConfig="@xml/network_security_config">
</application>
```

### 3、自定义view绘制问题
在自定义绘制View过程中会遇到 Android 9.0 兼容问题导致的Crash，解决方案：

```java
if (Build.VERSION.SDK_INT >= 26){
  canvas.clipPath(mPath); 
} else {
  canvas.clipPath(mPath, Region.Op.REPLACE);
}
```
### 4、前台服务需要添加权限
在安卓9.0版本之后，必须要授予FOREGROUND_SERVICE权限，才能够使用前台服务，否则会抛出异常。对此，我们只需要在AndroidManifest添加对应的权限即可，这个权限是普通权限，不需要动态申请。

<uses-permission android:name="android.permission.FOREGROUND_SERVICE" />

### 5、禁止非SDK接口访问
在 Android 9.0 版本中，谷歌加入了非 SDK 接口使用限制，无论是通过调用、反射还是JNI等方式，开发者都无法对非 SDK 接口进行访问，此接口的滥用将会带来严重的系统兼容性问题。 在开发过程中，开发者如果调用了非 SDK 接口，会导致应用出现crash，无法启动；或在运行过程中出现崩溃、闪退等现象；也可能导致应用功能不可用等严重兼容性问题，其影响范围波及所有调用此接口的应用。
例如我们通过反射修改Dialog窗体的颜色

## ANdroid10
[Android 10 适配攻略](https://weilu.blog.csdn.net/article/details/104513170)


## Android11
### 1、存储机制更新
强制执行分区存储

为了给开发者更多时间进行测试，以 Android 10（API 级别 29）为目标平台的应用仍可请求 requestLegacyExternalStorage 属性。应用可以利用此标记暂时停用与分区存储相关的变更，例如授予对不同目录和不同类型的媒体文件的访问权限。当您将应用更新为以 Android 11 为目标平台后，将无法使用 requestLegacyExternalStorage 来停用分区存储。

管理设备存储空间
不能再删除其他应用的缓存文件，即使您的应用具有所有文件访问权限也是如此。通过调用 ACTION_MANAGE_STORAGE intent 操作来检查可用空间以及提示用户同意让您的应用调用 ACTION_CLEAR_APP_CACHE intent清除所有缓存。

权限变更
WRITE_EXTERNAL_STORAGE 权限和 WRITE_MEDIA_STORAGE 特许权限将不再提供任何其他访问权限。

所有文件访问权限

某些应用的核心用例需要访问大量的文件，如文件管理操作或备份和恢复操作。声明 MANAGE_EXTERNAL_STORAGE 权限，使用 ACTION_MANAGE_ALL_FILES_ACCESS_PERMISSION intent 操作将用户引导至一个系统设置页面开启。

### 2、权限更新

一次性权限
在 Android 11 中，每当应用请求与位置信息、麦克风或摄像头相关的权限时，面向用户的权限对话框就会包含仅限这一次选项。如果用户在对话框中选择此选项，系统会向您的应用授予临时的一次性权限。当您应用的 Activity 可见时，您的应用可以继续访问相关数据。如果用户随后将您的应用转到后台，则您的应用对相关数据的访问权限会在一小段时间后被撤消。

数据访问审核

提供了支持服务来帮助开发者审核数据访问，并将数据访问与应用中的特定功能相关联。

### 3、位置信息更新
Android 11 进一步强调了用户对位置信息的控制，方法是添加了一次性权限，并去除了用户通过应用内提示授予 ACCESS_BACKGROUND_LOCATION 权限的能力，可以创建自定义界面，向用户解释您的应用为什么需要 ACCESS_BACKGROUND_LOCATION 权限。

### 4、软件包可见性

Android 11 更改了应用查询同一设备上的其他已安装应用及与之交互的方式。您可能需要在应用的清单文件中添加 <queries> 元素，以便系统知道要向您的应用显示哪些其他应用。<queries> 元素可用于描述您的应用可能需要与哪些其他应用交互。

### 5、前台服务类型

如果您的应用以 Android 11 为目标平台并且在某项前台服务中访问这些类型的数据，则您需要在该前台服务的声明的 foregroundServiceType 属性中添加新的 camera 和 microphone 类型。

<manifest>
    <service android:foregroundServiceType="location|camera|microphone"/>
</manifest>


### 6、消息框的更新

出于安全方面的考虑，同时也为了保持良好的用户体验，如果包含自定义视图的消息框是以 Android 11 为目标平台的应用从后台发送的，则系统会屏蔽这些消息框。请注意，仍允许使用文本消息框；此类消息框是使用 Toast.makeText() 创建的，并不调用 setView()。



