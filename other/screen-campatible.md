## 

之前我们产品里的启动页比较简单，就是背景色加上我们的logo、slogan等，这种形式适配起来比较容易，把元素切出来交给开发写出来就行。最近由于运营的需求，我们需要在APP启动页展示广告，广告图更新频繁、样式复杂，不可能交给开发适配，也不可能让设计师每次手动做多个尺寸（光想想就要抓狂），所以我开始思考用一个尺寸适配所有屏幕的方法。

### 确定基础尺寸

我们主要适配的是Android和iPhone这两个主流平台，常见分辨率如下。

![](./assets/resolution.webp)

从上表可以看出，iPhone 5/5s、iPhone 6/7/8、iPhone 6/7/8 Plus都是等比例的，屏幕的长宽比大约是1.77；而Android目前最主流的1080P、720P也都是这个比例。所以，为了兼顾两个平台，我采用这个比例下的最大尺寸，即iPhone 6/7/8 Plus的1242*2204为基础尺寸做图，其他尺寸用这张图去适配，这样可以最大限度地让尽可能多的屏幕完美显示，同时在较高分辨率下又不至于模糊。

### 如何裁剪

想用一张图适配不同尺寸的屏幕，图片肯定会被裁剪，那么图片是怎样被裁剪的呢？

为了填满屏幕的空间，图片需要按照一定的比例去等比放大或缩小，缩放的中心点为图片的中心点。当遇到 长宽比 比自己大（即高瘦）的屏幕时，为了填满屏幕高度，图片需要放大到与屏幕等高的大小，此时就会裁剪掉图片的左右两侧；而反过来，当遇到 长宽比 比自己小（即矮胖）的屏幕时，就会裁剪掉图片上下超出的部分。如下图示例。

![](./assets/crop-1.webp)

简而言之，由矮胖往高瘦适配，裁剪掉左右；反过来则裁剪掉上下。

### 确定安全区域

确定了基础尺寸，弄清楚了裁剪的规则，那我们可以在图片上确定一块「安全区域」。所谓「安全区域」，就是适配时确保不会被裁掉的区域，设计师在设计时可以把文字、logo、slogan等重要内容安排在该区域内，从而保证核心内容不会被裁掉。

从上面的裁剪规则我们知道了，长宽比越小（越矮胖）的屏幕，上下部分会被裁剪得更多；长宽比越大（越高瘦）的屏幕，左右两侧则被裁剪得更多。因此，按照长宽比最小的屏幕尺寸和长宽比最大的屏幕尺寸，可以分别确定上下左右的裁剪边界，边界里的即为安全区域。

而在我们适配的范围内，长宽比最小的尺寸是320x480（mdpi，iPhone4），最大的是375x812（iPhoneX），因此最终确定的安全区域是1020x1863，如下图所示。

![](./assets/crop-2.webp)

### 适配效果

最终同一张图在各个分辨率下的适配效果如图，为了方便查看，图中对安全区域的四个角做了标记。

![](./assets/crop-3.webp)

### 最后

如果你的启动页除了展示广告外，还需要固定展示品牌信息（如网易云音乐的启动页，下面会固定有一块区域展示logo、版权信息等），可以在下面留出固定比例的空间，如留出15%的高度，剩下的空间按照上面的方法确定尺寸和安全区域就好啦。





