育伢工程中有两处使用了逆向生成工具：

1、模块yychildren-generator，编译后生成可执行文件yychildren-generator.jar,

## 插件：
mybatis-generator-lombok-plugin

插件下载地址：

https://github.com/GuoGuiRong/mybatis-generator-lombok-plugin



主要整合了lombok插件实现getter/setter等通用方法的自动生成，同时自定义实现了一个注释生成器， 通过抓取数据库表里面的注释作为实体类的注释内容。

导入方法：


如果你想在你的maven中使用,就直接git clone这个项目到你的IDEA，然后使用maven clean install将这个项目添加到Maven仓库里去。 之后你只要在你的要使用这个插件的项目的pom.xml中加入如下内容便可：
```xml
<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.2</version>
    <configuration>
        <overwrite>true</overwrite>
    </configuration>
    <dependencies>
        <dependency>
            <groupId>com.chrm</groupId>
            <artifactId>mybatis-generator-lombok-plugin</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
</plugin>
```

同时添加配置文件generatorConfig.xml,使用的时候请根据项目需要自行修改对应配置


## 插件

本项目基于已存在的 mybatis-generator 做了扩展处理, 增加更多插件功能。
自动添加swagger2注解到实体类
扩展set方法,返回this实例;方便链式调用
增加只添加java注释
兼容swagger3 且 可以生成全包名路径



```xml
<dependency>
	<groupId>com.github.misterchangray.mybatis.generator.plugins</groupId>
	<artifactId>myBatisGeneratorPlugins</artifactId>
	<version>1.2</version>
</dependency>
```
地址：

https://github.com/MisterChangRay/mybatis-generator-plugins
