

* 创建一个Maven项目，定义groupid和arifactId。

![](./assets/spring_cloud_alibaba_2_1.png)

* 创建好后，删除项目的src目录

![](./assets/spring_cloud_alibaba_2_2.png)

* 将pom.xml的packaging标签设置为：pom

```xml
<modelVersion>4.0.0</modelVersion>
<groupId>com.ytech</groupId>
<artifactId>warrior</artifactId>
<version>1.0-SNAPSHOT</version>
<packaging>pom</packaging>
<name>warrior</name>
```

