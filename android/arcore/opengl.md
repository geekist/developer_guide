# ARCore系列文章之五：------ OpenGL ES 3.0简介


- [一、OpenGL简介](#一、OpenGL简介)
- [二、基础概念](#二、基础概念)
	- [1、Context上下文](#1、Context上下文)
	- [2、EGL](2、EGL)
	- [3、Texture纹理 ](#3、Texture纹理 )
	- [4、坐标系](#4、坐标系)
	- [5、渲染](#5、渲染)

## 一、OpenGL简介
Android 可通过开放图形库 (OpenGL®)（特别是 OpenGL ES API）来支持高性能 2D 和 3D 图形。

OpenGL 是一种跨平台的图形 API，用于为 3D 图形处理硬件指定标准的软件接口。

OpenGL ES （OpenGL for Embedded Systems）是三维图形 API OpenGL 的子集，针对手机、PDA 和游戏主机等嵌入式设备而设计。

Android 支持多版 OpenGL ES API：
OpenGL ES 1.0 和 1.1 - 此 API 规范受 Android 1.0 及更高版本的支持。
OpenGL ES 2.0 - 此 API 规范受 Android 2.2（API 级别 8）及更高版本的支持。
OpenGL ES 3.0 - 此 API 规范受 Android 4.3（API 级别 18）及更高版本的支持。
OpenGL ES 3.1 - 此 API 规范受 Android 5.0（API 级别 21）及更高版本的支持。

## 二、基础概念
### 1、Context上下文
OpenGL 是一个仅仅关注图像渲染的图像接口库，在渲染过程中它需要将顶点信息、纹理信息、编译好的着色器等渲染状态信息存储起来，而存储这些信息的数据结构就可以看作 OpenGL 的上下文。

调用任何 OpenGL 函数前，必须已经创建了 OpenGL Context，GL Context 存储了OpenGL 的状态变量以及其他渲染有关的信息。OpenGL 是个状态机，有很多状态变量，是个标准的过程式操作过程，改变状态会影响后续所有操作，这和面向对象的解耦原则不符，毕竟渲染本身就是个复杂的过程。OpenGL 采用 Client-Server 模型来解释 OpenGL 程序，即 Server 存储 GL Context（可能不止一个），Client 提出渲染请求，Server 给予响应，一般 Server 和 Client 都在我们的 PC 上，但 Server 和 Client 也可以是通过网络连接。

之后的渲染工作就要依赖这些渲染状态信息来完成，当一个上下文被销毁时，它所对应的 OpenGL 渲染工作也将结束。

### 2、EGL
EGL 是 OpenGL ES 和 Android 底层平台视窗系统之间的接口，在 OpenGL 的输出与设备屏幕之间架接起一个桥梁，承担了为 OpenGL 提供上下文环境以及管理窗口的职责。

在 OpenGL 的设计中，OpenGL 是不负责管理窗口的，窗口的管理交由各个设备自己来完成，具体来讲，IOS 平台上使用 EAGL 提供本地平台对 OpenGL 的实现，在 Android 平台上使用 EGL 提供本地平台对 OpenGL 的实现。

EGL 为双缓冲工作模式，即有一个 Back Frame Buffer 和一个 Front Frame Buffer，正常绘制的目标都是 Back Frame Buffer，绘制完成后再调用 eglSwapBuffer API，将绘制完毕的 FrameBuffer 交换到 Front Frame Buffer 并显示出来。

从代码层面来看，OpenGL ES 的 opengles 包下定义了平台无关的绘图指令，EGL则定义了控制 displays，contexts 以及 surfaces 的统一的平台接口。

Display（EGLDisplay） 是对实际显示设备的抽象
Surface（EGLSurface）是对用来存储图像的内存区域 FrameBuffer 的抽象，包括 Color Buffer、Stencil Buffer、Depth Buffer
Context（EGLContext）存储 OpenGL ES 绘图的一些状态信息
![image](https://img-blog.csdnimg.cn/20210116104628807.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lhbmd3dTAwNw==,size_16,color_FFFFFF,t_70)

使用 EGL 绘图的一般步骤：

获取 EGLDisplay 对象
初始化与 EGLDisplay 之间的连接
获取 EGLConfig 对象
创建 EGLContext 实例
创建 EGLSurface 实例
连接 EGLContext 和 EGLSurface
使用 GL 指令绘制图形
断开并释放与 EGLSurface 关联的 EGLContext 对象
删除 EGLSurface 对象
删除 EGLContext 对象
终止与 EGLDisplay 之间的连接

**在 Android 平台上开发 OpenGL ES 应用，无需按照上述步骤来绘制图形，可以直接使用 GLSurfaceView 控件，该控件提供了对 Display、Surface 以及 Context 的管理，大大简化了开发流程。**


### 3、Texture纹理
纹理（Texture）是一个 2D 图片（甚至也有 1D 和 3D 的纹理），它可以用来添加物体的细节；你可以想象纹理是一张绘有砖块的纸，无缝折叠贴合到你的 3D 的房子上，这样你的房子看起来就像有砖墙外表了。因为我们可以在一张图片上插入非常多的细节，这样就可以让物体非常精细而不用指定额外的顶点。

### 4、坐标系
OpenGL 要求输入的顶点坐标都是标准化设备坐标，即每个顶点的 x、y、z 都在 -1 到 1 之间，由标准化设备坐标转换为屏幕坐标的过程中会经历变换多个坐标系统，在这些特定的坐标系中，一些操作和计算可以更加方便。

![image](https://img-blog.csdnimg.cn/20210116104910891.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lhbmd3dTAwNw==,size_16,color_FFFFFF,t_70)

1. 局部坐标
顶点坐标起始于局部空间（Local Space），在这里称为局部坐标，是以物体某一点为原点而建立的，该坐标系仅对该物体适用，用来简化对物体各部分坐标的描述。物体放到场景中时，各部分经历的坐标变换相同，相对位置不变。
2. 世界坐标
局部坐标通过模型矩阵进行位移、缩放、旋转，将物体从局部变换到世界空间，并和其他物体一起相对于世界的原点摆放。
3. 观察坐标
将世界空间坐标转化为用户视野前方的坐标，通常是由一系列的位移和旋转的组合（观察矩阵）来完成。
4. 裁剪坐标
坐标到达观察空间之后，通过投影矩阵会将指定范围内的坐标变换为标准化设备坐标的范围(-1.0, 1.0)，所有在范围外的坐标会被裁剪掉。
5. 屏幕坐标
将裁剪坐标位于(-1.0, 1.0)范围的坐标变换到由 glViewport 函数所定义的坐标范围内，最后变换出来的坐标将会送到光栅器，将其转化为片段。

### 5、渲染
在OpenGL中，任何事物都在3D空间中，而屏幕和窗口却是2D像素数组，这导致OpenGL的大部分工作都是关于把3D坐标转变为适应屏幕的2D像素。

3D坐标转为2D坐标的处理过程是由OpenGL的图形渲染管线（Graphics Pipeline，大多译为管线，实际上指的是一堆原始图形数据途经一个输送管道，期间经过各种变化处理最终出现在屏幕的过程）管理的。图形渲染管线可以被划分为两个主要部分：

* **把3D坐标转换为2D坐标。**

*  **把2D坐标转变为实际的有颜色的像素。**

大致渲染过程如图：
![image.png](https://img-blog.csdnimg.cn/20210116102807995.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lhbmd3dTAwNw==,size_16,color_FFFFFF,t_70)

具体的执行步骤如下：

 1. 调用API填充顶点数组（要绘制的图形顶点信息）
  
 2. 传递到顶点着色器，将坐标信息转换为OpenGL 内部坐标信息

 3. 有了图形的坐标信息后，OpenGL 将坐标信息装配为基本的几何图形

 4. OpenGL 通过默认的几何着色器对上一步的图形进行处理
    
  5. 对上一步的几何图形进行光栅化,所谓的光栅化其实是把线性的几何图形映射到一个一个的像素上
    
  6. 通过片段着色器给每一个像素赋予颜色
    
  7. 最后将图像传递到帧缓冲（Framebuffer）中提供给屏幕进行刷新操作。

![在这里插入图片描述](https://img-blog.csdnimg.cn/202101161031281.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lhbmd3dTAwNw==,size_16,color_FFFFFF,t_70)

下面我们来一个一个详细解释每个阶段

* **顶点着色器(Vertex Shader)**

顶点着色器负责坐标和图形的描述。

在OpenGL中有且只有3中基本图形，点、线、三角形，其他的图形都需要使用这3种基本图形进行描述
其中OpenGL中的显示区域位于x,y均在[-1,1]之内的空间。如下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210116103318161.png)
点，位于3维空间，其坐标用(x,y,z)表示
线，2点连成1线
三角形，3点构成一个三角形


* **图元装配**

图元装配负责将顶点着色器输出的坐标信息进行组装和裁剪。图元（Primitive）是上述中的点、线、三角形等集合对象，对于每个图元，都要进行如下图的操作，从而把3d图元变成屏幕上的2d图元。

![image.png](https://img-blog.csdnimg.cn/20210116103351132.png)


* **光栅化**

光栅化负责的是把线性的几何图形转化为可供片段着色器使用的片段(Fragment)，即把线性的几何图形转化为屏幕上的像素方块(Pixel)，在这里我们联想我们数学中的相关知识，线可以看成是由无数的点组成，面(三角形)也是同理。

![image](https://img-blog.csdnimg.cn/20210116103407720.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lhbmd3dTAwNw==,size_16,color_FFFFFF,t_70)


* **片段着色器(Fragment Shader)**
片段着色器负责对光栅化生成的片段进行逐片段操作（给每一个片段图上颜色）从而使片段成为完整的像素点。

同时我们可以编写Fragament Shader的脚本来实现对每个像素颜色的变换来达到一些效果，如纹理贴图，光照，环境光，阴影。

参考资料：

* [Android OpenGL ES](https://developer.android.google.cn/guide/topics/graphics/opengl)



