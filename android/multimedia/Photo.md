

# Photo

## 应用关联缓存目录
应用关联缓存目录是指：getExternalCacheDir方法得到的目录

其路径是指：/sdcad/Android/data/<package name>/cache

从android6.0开始，读取sdcard上的其他目录被认为是危险权限，需要申请权限。
从android7.0开始，直接使用本地真实路径的uri被认为是不安全的，必须使用FileProvider。
从android10.0开始，共有的sd卡目录不允许引用程序访问，要使用作用域存储。

* 建立一个FileProvider

step1： 在Android.manifest文件中可以创建一个供读取的FileProvider
```xml
        <provider
            android:authorities="com.jon.chapter0902.FileProvider"
            android:name="androidx.core.content.FileProvider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_paths">
            </meta-data>
        </provider>
```
step2:创建一个xml文件存放可以提供给外界访问的目录
```xml
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:androidid="http://schemas.android.com/apk/res/android">
    <external-path name="my_images" path="/" >
    </external-path>
</paths>
```

step3:文件的读取和存储可以通过FileProvider来进行。
```kotlin
lateinit var imageUri: Uri
lateinit var outputImage: File 
outputImage = Fie(externalCacheDir,"output_image.jpg")
imageUri =  FileProvider.getUriForFile(this,
										"com.jon.chapter0902.FileProvider",
										outputImage)        
```
```kotlin
 val intent = Intent("android.media.action.IMAGE_CAPTURE")
 intent.putExtra(MediaStore.EXTRA_OUTPUT,imageUri)// 启动拍照功能并将照片存储到制定目录下
 startActivityForResult(intent,takePhoto)

//从指定位置读取照片
 val bitmap = BitmapFactory.decodeStream(contentResolver.openInputStream(imageUri))
 imageView.setImageBitmap(bitmap)
```

## 调用摄像头拍照并存储
```java
 		outputImage = File(externalCacheDir,"output_image.jpg")
        if(outputImage.exists()) {
            outputImage.delete()
        }
        outputImage.createNewFile()
        imageUri = if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
            FileProvider.getUriForFile(this,"com.jon.chapter0902.FileProvider",outputImage)
        }else {
            Uri.fromFile(outputImage)
        }

        val intent = Intent("android.media.action.IMAGE_CAPTURE")
        intent.putExtra(MediaStore.EXTRA_OUTPUT,imageUri)
        startActivityForResult(intent,takePhoto)
```

## 从存储位置读取照片
* 从本地缓存目录读取照片
```kotlin
  val bitmap = BitmapFactory.decodeStream(contentResolver.openInputStream(imageUri))
  imageView.setImageBitmap(bitmap)
```

* 从相册中读取照片


    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

打开系统相册
```java
  val intent = Intent(Intent.ACTION_OPEN_DOCUMENT)
                intent.addCategory(Intent.CATEGORY_OPENABLE)
                intent.type = "image/*"
                startActivityForResult(intent, fromAlbum)

```
系统相册返回时，data.data是一个uri
```java
  data.data?.let { uri ->
                            // 将选择的照片显示
                            val bitmap = getBitmapFromUri(uri)
                            imageView.setImageBitmap(bitmap)

 private fun getBitmapFromUri(uri: Uri) 
= contentResolver.openAssetFileDescriptor(uri, "r")?.use {
        BitmapFactory.decodeFileDescriptor(it.fileDescriptor)
    }

```



## 照片处理

* 读取exif信息
```kotlin
 val exif = ExifInterface(outputImage.path)
        val orientation = exif.getAttributeInt(ExifInterface.TAG_ORIENTATION,ExifInterface.ORIENTATION_NORMAL)
        return when(orientation) {
            ExifInterface.ORIENTATION_ROTATE_90 -> rotateBitmap(bitmap, 90)
            ExifInterface.ORIENTATION_ROTATE_180 -> rotateBitmap(bitmap, 180)
            ExifInterface.ORIENTATION_ROTATE_270 -> rotateBitmap(bitmap, 270)
            else ->bitmap
        }
```
* 旋转图片

```kotlin
    	val matrix = Matrix()
        matrix.postRotate(degree.toFloat())
        val rotateBitmap = Bitmap.createBitmap(bitmap,0,0,bitmap.width,bitmap.height,matrix,true)
        bitmap.recycle()
        return rotateBitmap

```