# ImmersionBar

android 4.4以上沉浸式状态栏和沉浸式导航栏管理，适配横竖屏切换、刘海屏、软键盘弹出等问题，可以修改状态栏字体颜色和导航栏图标颜色，以及不可修改字体颜色手机的适配，适用于Activity、Fragment、DialogFragment、Dialog，PopupWindow，一句代码轻松实现

## github： https://github.com/gyf-dev/ImmersionBar

## wiki：https://www.jianshu.com/p/2a884e211a62

##依赖项：
implementation 'com.gyf.immersionbar:immersionbar-ktx:3.0.0'

如果你的项目中使用了AndroidX支持库，请在你的gradle.properties加入如下配置，如果已经配置了，请忽略
   android.useAndroidX=true
   android.enableJetifier=true

## 基本用法：

java用法

 ImmersionBar.with(this).init();

kotlin用法

 immersionBar {
     statusBarColor(R.color.colorPrimary) 
     navigationBarColor(R.color.colorPrimary)
 }


销毁：
 destroyImmersionBar(dialog)
