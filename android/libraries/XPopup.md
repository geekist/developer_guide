# XPopup

XPopup是一个弹窗库，可能是Android平台最好的弹窗库。它从设计的时候就本着实用的原则，兼顾了美观和优雅的交互。用户都喜欢自然舒适的UI和交互

## github地址： https://github.com/li-xiaojun/XPopup

## 依赖项：

implementation 'com.lxj:xpopup:2.2.5'

## wiki   https://github.com/li-xiaojun/XPopup/wiki


## 简单使用举例：

```java

new XPopup.Builder(getContext()).asConfirm("我是标题", "我是内容",
                           new OnConfirmListener() {
                               @Override
                               public void onConfirm() {
                                  toast("click confirm");
                               }
                           })
                           .show();


```