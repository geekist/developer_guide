# 性能调优

## Question：在欢迎页启动的线程由于请求和处理的数据量过大而，导致欢迎页在出现之前界面上会有一个短暂的白色闪屏停留。

解决方案：
1、修改主题样式为黑色，防止白色过于刺眼。
2、为Activity设置背景图，程序启动时先加载背景图，避免黑屏
3、为theme设置透明属性，启动时不会显示黑色

## 使用了material的控件后，theme尽量使用"Theme.MaterialComponents.DayNight.DarkActionBar">

## 使用了appcompt的activity后，theme只能用appcomp的theme
   
