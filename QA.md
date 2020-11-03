## UI
 * textview如何多行显示，且带有滚动条？


```java
  <TextView
        android:inputType="textMultiLine"
        android:scrollbars="vertical"
        android:singleLine="false"
        
        />

```
然后在代码中加上：
```java
//      设置内容可滚动
        tv_dialog_flower_content.movementMethod = ScrollingMovementMethod.getInstance()
```

## Android stuido


## 其他