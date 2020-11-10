Activity+Fragments+TableLayout+ViewPager

# 如何向应用中添加一个activity

## 1、依赖项配置

如果没有特殊的依赖项，这里可以不需要。


## 2、为activity新建一个布局文件

res->layout->右键添加一个布局文件，命名位activity_xxx(其中xxx是功能）。

下面的示例是android studio新建一个空的empty时生成的布局文件。
```java

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

### 3、创建一个Activity文件，命名为XXXActivity。

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
		
		//将布局文件加入到Activity中
        setContentView(R.layout.activity_main);
    }
}
```

### 4、在Android.manifest配置清单文件中声明Activity及其属性。

```java
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.jon.myapplication">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication">
        
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>

```

### 5、调用activity的方法：

- startActivity()

显示调用

```java
Intent intent = new Intent(this, SignInActivity.class);
startActivity(intent);

```
隐式调用


```java
Intent intent = new Intent(Intent.ACTION_SEND);
intent.putExtra(Intent.EXTRA_EMAIL, recipientArray);
startActivity(intent);

```

- startActivityForResult()

```java
public class MyActivity extends Activity {
     // ...

     static final int PICK_CONTACT_REQUEST = 0;

     public boolean onKeyDown(int keyCode, KeyEvent event) {
         if (keyCode == KeyEvent.KEYCODE_DPAD_CENTER) {
             // When the user center presses, let them pick a contact.
             startActivityForResult(
                 new Intent(Intent.ACTION_PICK,
                 new Uri("content://contacts")),
                 PICK_CONTACT_REQUEST);
            return true;
         }
         return false;
     }

     protected void onActivityResult(int requestCode, int resultCode,
             Intent data) {
         if (requestCode == PICK_CONTACT_REQUEST) {
             if (resultCode == RESULT_OK) {
                 // A contact was picked.  Here we will just display it
                 // to the user.
                 startActivity(new Intent(Intent.ACTION_VIEW, data));
             }
         }
     }
 }

```


### 6、根据情况完成activity的其他回调方法：
- onCreate()

系统创建Activity 时触发。应该在该方法中初始化 Activity 的基本组件：例如，创建视图并将数据绑定到列表。
 
**必须在此处调用 setContentView() 来定义 Activity 界面的布局**。

```java

@Override
public void onCreate(Bundle savedInstanceState) {
    // call the super class onCreate to complete the creation of activity like
    // the view hierarchy
    super.onCreate(savedInstanceState);

   

    // set the user interface layout for this activity
    // the layout file is defined in the project res/layout/main_activity.xml file
    setContentView(R.layout.main_activity);

    // initialize member TextView so we can manipulate it later
    textView = (TextView) findViewById(R.id.text_view);
}

```

- onStart()

onCreate() 完成后，下一个回调将是 onStart()。

onCreate() 退出后，Activity 将进入“已启动”状态，并对用户可见。此回调包含 Activity 进入前台与用户进行互动之前的最后准备工作。


- onResume()
 
系统会在 Activity 开始与用户互动之前调用此回调。此时，该 Activity 位于 Activity 堆栈的顶部，并会捕获所有用户输入。应用的大部分核心功能都是在 onResume() 方法中实现的。
onResume() 回调后面总是跟着 onPause() 回调。


- onPause()

当 Activity 失去焦点并进入“已暂停”状态时，系统就会调用 onPause()。例如，当用户点按“返回”或“最近使用的应用”按钮时，就会出现此状态。当系统为您的 Activity 调用 onPause() 时，从技术上来说，这意味着您的 Activity 仍然部分可见，但大多数情况下，这表明用户正在离开该 Activity，该 Activity 很快将进入“已停止”或“已恢复”状态。

如果用户希望界面继续更新，则处于“已暂停”状态的 Activity 也可以继续更新界面。例如，显示导航地图屏幕或播放媒体播放器的 Activity 就属于此类 Activity。即使此类 Activity 失去了焦点，用户仍希望其界面继续更新。

您不应使用 onPause() 来保存应用或用户数据、进行网络呼叫或执行数据库事务。有关保存数据的信息，请参阅保存和恢复 Activity 状态。

onPause() 执行完毕后，下一个回调为 onStop()或 onResume()，具体取决于 Activity 进入“已暂停”状态后发生的情况。

- onStop()

当 Activity 对用户不再可见时，系统会调用 onStop()。出现这种情况的原因可能是 Activity 被销毁，新的 Activity 启动，或者现有的 Activity 正在进入“已恢复”状态并覆盖了已停止的 Activity。在所有这些情况下，停止的 Activity 都将完全不再可见。

系统调用的下一个回调将是 onRestart()（如果 Activity 重新与用户互动）或者 onDestroy()（如果 Activity 彻底终止）。

如果您的 Activity 不再对用户可见，说明其已进入“已停止”状态，因此系统将调用 onStop() 回调。例如，当新启动的 Activity 覆盖整个屏幕时，可能会发生这种情况。如果 Activity 已结束运行并即将终止，系统还可以调用 onStop()。

当 Activity 进入已停止状态时，与 Activity 生命周期相关联的所有生命周期感知型组件都将收到 ON_STOP 事件。这时，生命周期组件可以停止在组件未显示在屏幕上时无需运行的任何功能。

在 onStop() 方法中，应用应释放或调整在应用对用户不可见时的无用资源。例如，应用可以暂停动画效果，或从精确位置更新切换到粗略位置更新。使用 onStop() 而非 onPause() 可确保与界面相关的工作继续进行，即使用户在多窗口模式下查看您的 Activity 也能如此。

您还应使用 onStop() 执行 CPU 相对密集的关闭操作。例如，如果您无法找到更合适的时机来将信息保存到数据库，可以在 onStop() 期间执行此操作。以下示例展示了 onStop() 的实现，它将草稿笔记内容保存到持久性存储空间中：

```java
@Override
protected void onStop() {
    // call the superclass method first
    super.onStop();

    // save the note's current draft, because the activity is stopping
    // and we want to be sure the current note progress isn't lost.
    ContentValues values = new ContentValues();
    values.put(NotePad.Notes.COLUMN_NAME_NOTE, getCurrentNoteText());
    values.put(NotePad.Notes.COLUMN_NAME_TITLE, getCurrentNoteTitle());

    // do this update in background on an AsyncQueryHandler or equivalent
    asyncQueryHandler.startUpdate (
            mToken,  // int token to correlate calls
            null,    // cookie, not used here
            uri,    // The URI for the note to update.
            values,  // The map of column names and new values to apply to them.
            null,    // No SELECT criteria are used.
            null     // No WHERE columns are used.
    );
}

```


- onRestart()

当处于“已停止”状态的 Activity 即将重启时，系统就会调用此回调。onRestart() 会从 Activity 停止时的状态恢复 Activity。

此回调后面总是跟着 onStart()。



- onDestroy()


系统会在销毁 Activity 之前调用此回调。
此回调是 Activity 接收的最后一个回调。通常，实现 onDestroy() 是为了确保在销毁 Activity 或包含该 Activity 的进程时释放该 Activity 的所有资源。

销毁 Ativity 之前，系统会先调用 onDestroy()。系统调用此回调的原因如下：

Activity 即将结束（由于用户彻底关闭 Activity 或由于系统为 Activity 调用 finish()），或者
由于配置变更（例如设备旋转或多窗口模式），系统暂时销毁 Activity
当 Activity 进入已销毁状态时，与 Activity 生命周期相关联的所有生命周期感知型组件都将收到 ON_DESTROY 事件。这时，生命周期组件可以在 Activity 被销毁之前清理所需的任何数据。

您应使用 ViewModel 对象来包含 Activity 的相关视图数据，而不是在您的 Activity 中加入逻辑来确定 Activity 被销毁的原因。如果因配置变更而重新创建 Activity，ViewModel 不必执行任何操作，因为系统将保留 ViewModel 并将其提供给下一个 Activity 实例。如果不重新创建 Activity，ViewModel 将调用 onCleared() 方法，以便在 Activity 被销毁前清除所需的任何数据。

您可以使用 isFinishing() 方法区分这两种情况。

如果 Activity 即将结束，onDestroy() 是 Activity 收到的最后一个生命周期回调。如果由于配置变更而调用 onDestroy()，系统会立即新建 Activity 实例，然后在新配置中为新实例调用 onCreate()。

onDestroy() 回调应释放先前的回调（例如 onStop()）尚未释放的所有资源。

### 7、创建和销毁时数据的保存和重新加载
- 保存数据

```java
static final String STATE_SCORE = "playerScore";
static final String STATE_LEVEL = "playerLevel";
// ...

@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
    // Save the user's current game state
    savedInstanceState.putInt(STATE_SCORE, currentScore);
    savedInstanceState.putInt(STATE_LEVEL, currentLevel);

    // Always call the superclass so it can save the view hierarchy state
    super.onSaveInstanceState(savedInstanceState);
}
```
- 恢复数据

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState); // Always call the superclass first

    // Check whether we're recreating a previously destroyed instance
    if (savedInstanceState != null) {
        // Restore value of members from saved state
        currentScore = savedInstanceState.getInt(STATE_SCORE);
        currentLevel = savedInstanceState.getInt(STATE_LEVEL);
    } else {
        // Probably initialize members with default values for a new instance
    }
    // ...
}

```
您可以选择实现系统在 onStart() 方法之后调用的 onRestoreInstanceState()，而不是在 onCreate() 期间恢复状态。仅当存在要恢复的已保存状态时，系统才会调用 onRestoreInstanceState()，因此您无需检查 Bundle 是否为 null：

```java
public void onRestoreInstanceState(Bundle savedInstanceState) {
    // Always call the superclass so it can restore the view hierarchy
    super.onRestoreInstanceState(savedInstanceState);

    // Restore state members from saved instance
    currentScore = savedInstanceState.getInt(STATE_SCORE);
    currentLevel = savedInstanceState.getInt(STATE_LEVEL);
}

```

### 8、任务和返回堆栈

- 在Android.manifest的launchMode属性中声明启动模式：

**android:launchMode**

有关应如何启动 Activity 的指令。共有四种模式可与 Intent 对象中的 Activity 标记（FLAG_ACTIVITY_* 常量）协同工作，以确定在调用 Activity 处理 Intent 时应执行的操作。这些模式是：
“standard”
“singleTop”
“singleTask”
“singleInstance”

默认模式是“standard”。

standard”	
会创建多个Activity实例并向其传送Intent。


“singleTop”	
可能会创建多个Activity实例。
如果目标任务的顶部已存在 Activity实例，则系统会通过调用该实例的onNewIntent()方法向其传送 Intent，而非创建新的 Activity 实例。

“singleTask”
只会创建一个Activity的实例。
系统会在新任务的根位置创建Activity并向其传送 Intent。不过，如果已存在 Activity 实例，则系统会调用该实例的onNewIntent() 方法（而非创建新的 Activity 实例），向其传送 Intent。

“singleInstance”
只会创建一个Activity的实例。
与“singleTask"”相同，只是系统不会将任何其他 Activity启动到包含实例的任务中。该 Activity 始终是其任务中的唯一 Activity。

- 在startActivity()方法中声明启动模式。
