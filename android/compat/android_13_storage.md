
- [一、Android中内部存储，外部存储的概念](#一android中内部存储外部存储的概念)
  - [1.1 内部存储定义](#11-内部存储定义)
  - [1.2 应用的内部存储/data/data/目录](#12-应用的内部存储datadata目录)
    - [1.2.1 files目录](#121-files目录)
    - [1.2.2 cache目录](#122-cache目录)
    - [1.2.3 shared\_prefs目录](#123-shared_prefs目录)
    - [1.2.4 databses目录](#124-databses目录)
- [二、Android调用内部存储和外部存储的方法](#二android调用内部存储和外部存储的方法)
- [三、文件的其他调用方法](#三文件的其他调用方法)



# 一、Android中内部存储，外部存储的概念


## 1.1 内部存储定义

内部存储顾名思义就是手机自带的存储空间，一般情况下，系统和应用都是安装在内部存储空间中的。

在没有root的情况下，普通用户是无法查看内部存储中的文件的。

## 1.2 应用的内部存储/data/data/<packageName>目录

对于开发者而言，我们熟悉的是/data/data/<packageName>这个目录。这个目录实际上是/data/user/<current_user_id>/<package>的一个链接，注意是current_user_id。
（android 6.0之前，/data/user/0/<package>目录。0是指当前用户，在手机没有支持多用户之前）

这个路径是应用的内部存储私有目录，保存到这个路径下的文件是应用的私有文件，其他应用不能访问这些文件（除非拥有 Root 访问权限），非常适合保存用户无需直接访问的内部应用数据。

当我们卸载应用后，保存在私有路径中的文件也会被删除。因此，我们不应该将那些希望应用卸载以后还保留的数据文件放在私有路径中。

主要有以下几个常用的目录：

### 1.2.1 files目录

完整路径为：/data/data/<package>/files。用于保存应用创建的文件，可以通过Context对象获取其路径：

```
// java
String path = context.getFilesDir().getAbsolutePath();

// kotlin

val path = context.filesDir.absolutePath
```

### 1.2.2 cache目录

完整路径为：/data/data/<package>/cache。用于保存应用的临时缓存文件，可以通过Context对象获取其路径：

```
// java
String path = context.getCacheDir().getAbsolutePath();

// kotlin
val path = context.cacheDir.absolutePath
```

### 1.2.3 shared_prefs目录

完整路径为：/data/data/<package>/shared_prefs。用于保存SharedPreferences的数据文件。


### 1.2.4 databses目录

databses目录

完整路径为：/data/data/<package>/databses。用于保存Sqlite数据库文件。

1.3、外部存储

# 二、Android调用内部存储和外部存储的方法

# 三、文件的其他调用方法
