## UI

* [控件常用的属性][common]
* [TextView][textview]
* [Button][button]
* [EditText][edittext]
* [ImageView][imageview]
* [ProgressBar][progressbar]
* [AlertDialog][alertdialog]
* [ListView][listview]
* [RecyclerView][recyclerview]
* [ActionBar][actionbar]
* [][]
* [][]
* [][]
* [][]
* [Toast][toast]
* [Menu][menu]
* [自定义控件][self]


[common]:https://github.com/geekist/developer_guide/blob/main/android/ui/Common.md
[toast]:https://github.com/geekist/developer_guide/blob/main/android/ui/Toast.md

[menu]:https://github.com/geekist/developer_guide/blob/main/android/ui/Menu.md
[textview]:https://github.com/geekist/developer_guide/blob/main/android/ui/TextView.md
[button]:https://github.com/geekist/developer_guide/blob/main/android/ui/Button.md
[edittext]:https://github.com/geekist/developer_guide/blob/main/android/ui/EditText.md
[imageview]:https://github.com/geekist/developer_guide/blob/main/android/ui/ImageView.md
[progressbar]:https://github.com/geekist/developer_guide/blob/main/android/ui/ProgressBar.md
[alertdialog]:https://github.com/geekist/developer_guide/blob/main/android/ui/AlertDialog.md

[listview]:https://github.com/geekist/developer_guide/blob/main/android/ui/ListView.md
[recyclerview]:https://github.com/geekist/developer_guide/blob/main/android/ui/RecyclerView.md

[self]:https://github.com/geekist/developer_guide/blob/main/android/ui/自定义控件.md

[actionbar]:https://github.com/geekist/developer_guide/blob/main/android/ui/ActionBar.md

## Mfaterial Design UI
* 依赖项的引入
在app的build.gradle中加入下面的依赖项
```java
dependencies {
    implementation 'com.google.android.material:material:1.2.1'
}
```

* 命名空间
注意引入material Design后，布局文件中增加了命名空间：
```xml
 xmlns:android="http://schemas.android.com/apk/res/android"
 xmlns:app="http://schemas.android.com/apk/res-auto"
 xmlns:tools="http://schemas.android.com/tools"
```
引入app命名空间，是因为许多MaterialDesign的属性是新增加的，原来的android命名空间中并不存在，所以为了兼容老系统，增加了一个新的命名空间。

* [Toolbar][toolbar]



[toolbar]:https://github.com/geekist/developer_guide/blob/main/android/ui/Toolbar.md