# ARCore系列文章之三：------  ARCore核心类介绍

## ARCore核心类介绍

### ArCoreApk

com.google.ar.core.ArCoreApk类，管理ARCore在设备上的状态，是否avaliable，是否需要安装，安装相关的UI提示等，里边都是静态方法。

### Session

com.google.ar.core.Session类，Session管理AR系统状态并处理Session生命周期。 

该类是ARCore API的主要入口点。 该类允许用户创建Session，配置Session，启动/停止Session，最重要的是接收com.google.ar.core.Frame，以允许访问Camera image和device pose。


### Config

com.google.ar.core.Config类，用户保存session的设置，light estimate模式，udpate模式等。

### Camera

com.google.ar.core.Camera类，提供了Cmeara相关的信息，是一个长期存在的对象，Camera的属性随着Session.update()更新。

### Frame

com.google.ar.core.Frame类，通过调用Session.update()方法可以获得该对象，获取AR系统信息，如Camera image， Anchors等。

### HitResult

com.google.ar.core.HitResult类，该类定义了命中射线与估算的真实世界的几何图形的交叉点，也就是命中点结果。

### Anchor

com.google.ar.core.Anchor类，描述了现实世界中的一个固定位置和方向。 为了保持物理空间的固定位置，这个位置的数字描述信息将随着ARCore对空间的理解的不断改进而更新。

### Plane

com.google.ar.core.Plane类，描述了现实世界平坦表面的最新信息。有两种平面类型，一种是朝下的如天花板，另外一种是朝上的如桌面，地板等。

### Point

com.google.ar.core.Point类，它代表ARCore正在跟踪的空间中的一个点。 它是创建锚点（调用createAnchor方法）时，或者进行命中检测（调用hitTest方法）时，返回的结果。

### PointCloud

com.google.ar.core.PointCloud类，它包含一组观察到的3D点和信心值。

### Pose

com.google.ar.core.Pose类，姿势表示从一个坐标空间到另一个坐标空间位置不变的转换。 在所有的ARCore API里，姿势总是描述从对象本地坐标空间到世界坐标空间的转换。

随着ARCore对环境的了解不断变化，它将调整坐标系模式以便与真实世界保持一致。 这时，Camera和锚点的位置（坐标）可能会发生明显的变化，以便它们所代表的物体处理恰当的位置。

这意味着，每一帧图像都应被认为是在一个完全独立的世界坐标空间中。锚点和Camera的坐标不应该在渲染帧之外的地方使用，如果需考虑到某个位置超出单个渲染框架的范围，则应该创建一个锚点或者应该使用相对于附近现有锚点的位置。

### ImageMetadata

com.google.ar.core.ImageMetadata类，提供了对Camera图像捕捉结果的元数据的访问。

### LightEstimate

com.google.ar.core.LightEstimate类，保存关于真实场景光照的估计信息。 通过 getLightEstimate()得到。

### Trackable

com.google.ar.core.Trackable类，是一个接口类，代表了一些ARCore可以跟踪并且锚点可以贴上的对象。比如Point，Plane等他们都是Trackable的子类。

### TrackingState

com.google.ar.core.TrackingState类，描述了Trackable的跟踪状态，有三种状态paused, stopped, tracking。


参考资料：

* [ARCore-API参考资料](https://developers.google.com/ar/reference)