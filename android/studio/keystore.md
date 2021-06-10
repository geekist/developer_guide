
## 首先将android studio的keystore文件转换成为jks文件

再androidstudio的jre目录下找到keytool文件，执行：

keytool -importkeystore -srckeystore c:\sign\yuya_parent.jks -srcstoretype JKS -deststoretype PKCS12 -destkeystore c:\sign\yuya_parent.p12

keytool -v -importkeystore -srckeystore c:\sign\yuya_parent.p12 -srcstoretype PKCS12 -destkeystore c:\sign\yuya_parent.keystore -deststoretype JKS

## 然后再java的jdk目录bin下执行

jarsigner -verbose -keystore c:\sign\yuya_parent.keystore -signedjar c:\sign\signed.apk c:\sign\tap_unsign.apk yuya

注意：最后一个是签名文件的别名