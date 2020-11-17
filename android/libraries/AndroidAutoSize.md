# AndroidAutoSize

## 添加gradle的依赖项：
```java
 implementation 'me.jessyan:autosize:1.2.1'
```

## 使用方法：(只此一步）
```java
<manifest>
    <application>            
        <meta-data
            android:name="design_width_in_dp"
            android:value="360"/>
        <meta-data
            android:name="design_height_in_dp"
            android:value="640"/>           
     </application>           
</manifest>

```

## 自定义autosize


## 关于屏幕适配的延展话题

### 屏幕配置
Android 可在各种尺寸的设备上运行，包括手机、平板电脑和电视。为了按照屏幕类型对设备进行分类，Android 为每种设备定义了两个特征：
  
* 屏幕尺寸（屏幕的物理尺寸）

* 屏幕密度（屏幕上像素的物理密度，称为 DPI）。为了简化不同的配置，Android 将这些变体归纳成组，使它们更容易作为定位目标：

四种广义的尺寸
* 小

* 标准

* 大

* 特大
还有几种广义的密度：

* mdpi（中）

* hdpi（高）

* xhdpi（超高

* xxhdpi（超超高）等。

默认情况下，您的应用会兼容所有屏幕尺寸和密度，因为系统会根据需要对各个屏幕的界面布局和图片资源进行相应的调整。不过，您应针对不同的屏幕尺寸添加专门的布局，针对常见的屏幕密度添加优化的位图图片，以优化每种屏幕配置的用户体验。

`dpi像素密度`

像素密度是屏幕单位面积内的像素数，称为 dpi（每英寸的点数）.

`分辨率`

是屏幕上的总像素数。

`dp`

dp 是一个虚拟像素单位，1 dp 约等于中密度屏幕（160dpi；“基准”密度）上的 1 像素。对于其他每个密度，Android 会将此值转换为相应的实际像素数。
dp和px的转换： px = dp * (dpi / 160)
```java
    // The gesture threshold expressed in dp
    private static final float GESTURE_THRESHOLD_DP = 16.0f;

    // Get the screen's density scale
    final float scale = getResources().getDisplayMetrics().density;
    // Convert the dps to pixels, based on density scale
    mGestureThreshold = (int) (GESTURE_THRESHOLD_DP * scale + 0.5f);

    // Use mGestureThreshold as a distance in pixels...
```
如果您有一个可绘制位图资源，它在中密度屏幕上的大小为 48x48 像素，那么它在其他各种密度的屏幕上的大小应该为：

36x36 (0.75x) - 低密度 (ldpi)

48x48（1.0x 基准）- 中密度 (mdpi)

72x72 (1.5x) - 高密度 (hdpi)

96x96 (2.0x) - 超高密度 (xhdpi)

144x144 (3.0x) - 超超高密度 (xxhdpi)

192x192 (4.0x) - 超超超高密度 (xxxhdpi)

然后，将生成的图片文件置于 res/ 下的相应子目录中，系统将自动根据运行您的应用的设备的屏幕密度选取正确的文件：




