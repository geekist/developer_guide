# Animation

## [动画资源](https://developer.android.google.cn/guide/topics/resources/animation-resource#Property)

## [官方文档](https://developer.android.google.cn/training/animation)

## 基础知识
Android中动画的实现分两种方式，一种方式是补间动画 Teen Animation，就是说你定义一个开始和结束，中间的部分由程序运算得到。另一种叫逐帧动画 Frame Animation，就是说一帧一帧的连起来播放就变成了动画。

### 1-补间动画 Teen Animation

Android提供了四个基本的动画类：
* 透明动画(AlphaAnimation)
 
* 旋转动画(RotateAnimation)

* 移动动画(TranslateAnimation)
 
* 和缩放动画(ScaleAnimation)。

有两种方法可以实现动画，一个是纯代码方式，另外一个是用代码和xml结合的方式来实现：
#### 纯代码方式实现

举例如下：

```java
 private val alphaAnimation by lazy {
        AlphaAnimation(0.5f, 1.0f).apply {
            duration = splashDuration
            fillAfter = true
        }
    }

    private val scaleAnimation by lazy {
        ScaleAnimation(1f,
            1.05f,
            1f,
            1.05f,
            Animation.RELATIVE_TO_SELF,
            0.5f,
            Animation.RELATIVE_TO_SELF,
            0.5f).apply {
            duration = splashDuration
            fillAfter = true
        }
    }


 imageView.startAnimation(alphaAnimation)
 textView.startAnimation(scaleAnimation)

```

#### 代码和xml方式结合来实现

实现补间动画的思路是这样的：

1、首先用XML定义一个动画效果 

2、依据这个XML使用AnimationUtils工具类创建一个Animationd对象 

3、调用View组件的startAnimation方法实现动画。

* 步骤一、 在res目录下创建一个anim目录，添加一个xml文件，名为“xx_alpha"
```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:shareInterpolator="false"
    android:zAdjustment="top">
    <translate
        android:duration="300"
        android:fillAfter="true"
        android:fillBefore="true"
        android:fillEnabled="true"
        android:fromXDelta="100%"
        android:interpolator="@android:anim/decelerate_interpolator"
        android:toXDelta="0" />
</set>

alpha的例子：
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
       android:duration="1000"
       android:fromAlpha="0"
       android:toAlpha="1">
</alpha>

translate的例子：
<translate xmlns:android="http://schemas.android.com/apk/res/android" 
          android:duration="1000" 
          android:fromXDelta="0" 
          android:fromYDelta="0" 
          android:toXDelta="80" 
          android:toYDelta="80">
</translate>

scale的例子
<scale xmlns:android="http://schemas.android.com/apk/res/android"
       android:duration="1000"
       android:fromXScale="0"
       android:fromYScale="0"
       android:toXScale="1"
       android:toYScale="1">
</scale>

rotate的例子
<rotate xmlns:android="http://schemas.android.com/apk/res/android"        
       android:duration="1000"
       android:fromDegrees="0"
       android:pivotX="50%" 
       android:pivotY="50%" 
       android:toDegrees="360">
</rotate>





```
* 步骤二、 用代码来实现动画

```java
Animation animation = AnimationUtils.loadAnimation(
	                            getApplicationContext(), R.anim.translate_animation);
view.startAnimation(animation);

```

### 2-逐帧动画

使用animationDrawable对象或者使用animatedVectorDrawable对象。

* 步骤一、首先定义一个xml文件，包含animation-list节点和一系列的item节点
```xml
    <animation-list xmlns:android="http://schemas.android.com/apk/res/android"
        android:oneshot="true">
        <item android:drawable="@drawable/rocket_thrust1" android:duration="200" />
        <item android:drawable="@drawable/rocket_thrust2" android:duration="200" />
        <item android:drawable="@drawable/rocket_thrust3" android:duration="200" />
    </animation-list>
```

* 步骤二、然后可以在代码中调用

```kotlin
    private lateinit var rocketAnimation: AnimationDrawable

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main)

        val rocketImage = findViewById<ImageView>(R.id.rocket_image).apply {
            setBackgroundResource(R.drawable.rocket_thrust)
            rocketAnimation = background as AnimationDrawable
        }

        rocketImage.setOnClickListener({ rocketAnimation.start() })
    }
    
```





### animatedVectorDrawable
矢量可绘制对象是一种无需像素化或进行模糊处理即可缩放的可绘制对象。借助 AnimatedVectorDrawable 类（以及用于实现向后兼容的 AnimatedVectorDrawableCompat），您可以为矢量可绘制对象的属性添加动画效果，例如旋转或更改路径数据以将其变为其他图片。

您通常在三个 XML 文件中定义添加动画效果之后的矢量可绘制对象：

- 一个矢量可绘制对象，其中 <vector> 元素位于 res/drawable/

```xml
    <vector xmlns:android="http://schemas.android.com/apk/res/android"
        android:height="64dp"
        android:width="64dp"
        android:viewportHeight="600"
        android:viewportWidth="600">
        <group
            android:name="rotationGroup"
            android:pivotX="300.0"
            android:pivotY="300.0"
            android:rotation="45.0" >
            <path
                android:name="v"
                android:fillColor="#000000"
                android:pathData="M300,70 l 0,-70 70,70 0,0 -70,70z" />
        </group>
    </vector>
    

```
- 一个添加动画效果之后的矢量可绘制对象，其中 <animated-vector> 位于 res/drawable/
```xml
    <animated-vector xmlns:android="http://schemas.android.com/apk/res/android"
      android:drawable="@drawable/vectordrawable" >
        <target
            android:name="rotationGroup"
            android:animation="@animator/rotation" />
        <target
            android:name="v"
            android:animation="@animator/path_morph" />
    </animated-vector>
    

```


- 一个或多个对象 Animator，其中 <objectAnimator> 元素位于 res/animator/
```xml
    <objectAnimator
        android:duration="6000"
        android:propertyName="rotation"
        android:valueFrom="0"
        android:valueTo="360" />
  
```


```xml
    <set xmlns:android="http://schemas.android.com/apk/res/android">
        <objectAnimator
            android:duration="3000"
            android:propertyName="pathData"
            android:valueFrom="M300,70 l 0,-70 70,70 0,0   -70,70z"
            android:valueTo="M300,70 l 0,-70 70,0  0,140 -70,0 z"
            android:valueType="pathType" />
    </set>
    

```


添加动画效果之后的矢量可绘制对象可以为 <group> 和 <path> 元素的属性添加动画效果。<group> 元素定义一组路径或子组，而 <path> 元素定义要绘制的路径。



## 为view的可见性和动作添加动画

### 属性动画系统（android.animation)和视图动画系统（android.view.animation）区别

视图动画系统仅提供为 View 对象添加动画效果的功能，因此，如果您想为非 对象添加动画效果，则必须实现自己的代码才能做到。视图动画系统也存在一些限制，因为它仅公开 对象的部分方面来供您添加动画效果；例如，您可以对视图的缩放和旋转添加动画效果，但无法对背景颜色这样做。

视图动画系统的另一个缺点是它只会在绘制视图的位置进行修改，而不会修改实际的视图本身。例如，如果您为某个按钮添加了动画效果，使其可以在屏幕上移动，该按钮会正确绘制，但能够点击按钮的实际位置并不会更改，因此您必须通过实现自己的逻辑来处理此事件。

有了属性动画系统，您就可以完全摆脱这些束缚，还可以为任何对象（视图和非视图）的任何属性添加动画效果，并且实际修改的是对象本身。属性动画系统在执行动画方面也更为强健。概括地讲，您可以为要添加动画效果的属性（例如颜色、位置或大小）分配 Animator，还可以定义动画的各个方面，例如多个 Animator 的插值和同步。

不过，视图动画系统的设置需要的时间较短，需要编写的代码也较少。如果视图动画可以完成您需要执行的所有操作，或者现有代码已按照您需要的方式运行，则无需使用属性动画系统。在某些用例中，也可以针对不同的情况同时使用这两种动画系统。

### 使用Animator创建动画
Animator 类提供了创建动画的基本结构。您通常不会直接使用此类，因为它只提供极少的功能，这些功能必须经过扩展才能全面支持为值添加动画效果。

* ValueAnimator	属性动画的主计时引擎，它也可计算要添加动画效果的属性的值。它具有计算动画值所需的所有核心功能，同时包含每个动画的计时详情、有关动画是否重复播放的信息、用于接收更新事件的监听器以及设置待评估自定义类型的功能。为属性添加动画效果分为两个步骤：计算添加动画效果之后的值，以及对要添加动画效果的对象和属性设置这些值。ValueAnimator 不会执行第二个步骤，因此，您必须监听由 ValueAnimator 计算的值的更新情况，并使用您自己的逻辑修改要添加动画效果的对象。如需了解详情，请参阅使用 ValueAnimator 添加动画效果部分。

```java
    ValueAnimator animation = ValueAnimator.ofFloat(0f, 100f);
    animation.setDuration(1000);
    animation.start();
    
    ValueAnimator animation = ValueAnimator.ofObject(new MyTypeEvaluator(), startPropertyValue, endPropertyValue);
    animation.setDuration(1000);
    animation.start();
    
    animation.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
        @Override
        public void onAnimationUpdate(ValueAnimator updatedAnimation) {
            // You can use the animated value in a property that uses the
            // same type as the animation. In this case, you can use the
            // float value in the translationX property.
            float animatedValue = (float)updatedAnimation.getAnimatedValue();
            textView.setTranslationX(animatedValue);
        }
    });
    

```


* ObjectAnimator	ValueAnimator 的子类，用于设置目标对象和对象属性以添加动画效果。此类会在计算出动画的新值后相应地更新属性。在大多数情况下，您不妨使用 ObjectAnimator，因为它可以极大地简化对目标对象的值添加动画效果这一过程。不过，有时您需要直接使用 ValueAnimator，因为 ObjectAnimator 存在其他一些限制，例如要求目标对象具有特定的访问器方法。
```java
    ObjectAnimator animation = ObjectAnimator.ofFloat(textView, "translationX", 100f);
    animation.setDuration(1000);
    animation.start();
    

    ObjectAnimator.ofFloat(targetObject, "propName", 1f)
    
```

* AnimatorSet	此类提供一种将动画分组在一起的机制，以使它们彼此相对运行。您可以将动画设置为一起播放、按顺序播放或者在指定的延迟时间后播放。如需了解详情，请参阅使用 AnimatorSet 编排多个动画部分。
```java
    AnimatorSet bouncer = new AnimatorSet();
    bouncer.play(bounceAnim).before(squashAnim1);
    bouncer.play(squashAnim1).with(squashAnim2);
    bouncer.play(squashAnim1).with(stretchAnim1);
    bouncer.play(squashAnim1).with(stretchAnim2);
    bouncer.play(bounceBackAnim).after(stretchAnim2);
    ValueAnimator fadeAnim = ObjectAnimator.ofFloat(newBall, "alpha", 1f, 0f);
    fadeAnim.setDuration(250);
    AnimatorSet animatorSet = new AnimatorSet();
    animatorSet.play(bouncer).before(fadeAnim);
    animatorSet.start();
```
### 动画监听器

您可以使用下述监听器来监听动画播放期间的重要事件。

Animator.AnimatorListener
onAnimationStart() - 在动画开始播放时调用。
onAnimationEnd() - 在动画结束播放时调用。
onAnimationRepeat() - 在动画重复播放时调用。
onAnimationCancel() - 在动画取消播放时调用。取消的动画也会调用 onAnimationEnd()，无论它们以何种方式结束。
ValueAnimator.AnimatorUpdateListener
onAnimationUpdate() - 对动画的每一帧调用。监听此事件即可使用 ValueAnimator 在动画播放期间生成的计算值。要使用该值，请查询传递到事件中的 ValueAnimator 对象，以使用 getAnimatedValue() 方法获取当前添加动画效果之后的值。如果使用了 ValueAnimator，则必须实现此监听器。

根据您要添加动画效果的属性或对象，您可能需要对视图调用 invalidate()，以强制屏幕上的相应区域使用添加动画效果之后的新值重新绘制自身。例如，如果为可绘制对象的颜色属性添加动画效果，则仅当该对象重新绘制自身时，屏幕才会刷新。视图的所有属性 setter（如 setAlpha() 和 setTranslationX()）都会使视图失效，因此，在使用新值调用这些方法时，您无需使视图失效。

如果您不一定需要实现 Animator.AnimatorListener 接口的所有方法，则可以扩展 AnimatorListenerAdapter 类，而非实现 接口。AnimatorListenerAdapter 类提供了方法的空实现，可供您选择替换。

例如，以下代码段可仅为 onAnimationEnd() 回调创建 AnimatorListenerAdapter：
```java




```

### 基本动画


### 急于物理特性的动作

## 为布局更改添加动画

## 在Activity之间添加动画

## fragment动画 

