# Fiddler 手机抓包备忘

参考文档：

fiddler下载和基本使用方法

https://juejin.cn/post/7021763777460699150

fiddler 手机抓包 ios

https://juejin.cn/post/6978086089600794631


## 一、使用Fiddler手机抓包的条件：

1：手机连的网络或WiFi必须和电脑（使用fiddler）连的网络或WiFi是一样的

2：手机不能离开电脑太远距离，一定要保持手机可以上网（不能断网）

## 二、连接步骤：

### 1、pc端配置端口号和“远程连接”选项。
1，启动Fiddler，打开菜单栏中的 Tools > Fiddler Options，打开“Fiddler Options”对话框。

2， 在Fiddler Options”对话框切换到“Connections”选项卡，然后勾选“Allow romote computers to connect”后面的复选框，然后点击“OK”按钮。
注意要记住端口号（8888也可以是9999）
 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210312093259861.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lhbmd3dTAwNw==,size_16,color_FFFFFF,t_70#pic_center)

### 2.手机配置代理服务器
 在本机命令行输入：ipconfig，找到本机的ip地址。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210312094531611.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lhbmd3dTAwNw==,size_16,color_FFFFFF,t_70)

 
打开手机设备的“设置”->“WLAN”，找到你要连接的网络，在上面长按，然后选择“修改网络”，弹出网络设置对话框，然后勾选“显示高级选项”。

5， 在“代理”后面的输入框选择“手动”，在“代理服务器主机名”后面的输入框输入电脑的ip地址，

在“代理服务器端口”后面的输入框输入8888（之前在fiddler里面设置的），然后点击“保存”按钮。

证书下载和安装
在浏览器输入192.xxx.xxx.xx:xxxx,然后下载证书，安装后选择信任，即可开始。