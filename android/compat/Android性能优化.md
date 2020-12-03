
我们知道，在 Android 自定义 View 的时候，需要使用 TypedArray 来获取 XML layout 中的属性值，使用完之后，需要调用 recycle() 方法将 TypedArray 回收。

TypedArray是一个存储属性值的数组，使用完之后应该调用recycle()回收,用于后续调用时可复用之。当调用该方法后，不能再操作该变量。至于深层次的原因，文档没说。这就需要我们自己去研究源码了。

池中默认大小为5。 程序在运行时维护了一个 TypedArray的池，程序调用时，会向该池中请求一个实例，用完之后，调用 recycle() 方法来释放该实例，从而使其可被其他模块复用。
那为什么要使用这种模式呢？答案也很简单，TypedArray的使用场景之一，就是上述的自定义View，会随着 Activity的每一次Create而Create，因此，需要系统频繁的创建array，对内存和性能是一个不小的开销，如果不使用池模式，每次都让GC来回收，很可能就会造成OutOfMemory。