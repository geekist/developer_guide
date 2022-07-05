# 一、介绍

代码生成工具，能够生成：

* Controll类

* Service接口

* Service impl类

* Entity数据类

（生成Entity时集成lombok，可以生成@Data注解）

* Mapper接口

* Mapperxml文件

lombok，swagger的代码生成工具，

集成lombok；
集成swagger；
集成Mapper；
的代码生成工具，让你不再为繁琐的注释和简单的接口实现而烦恼：entity集成，格式校验，swagger; dao自动加@ mapper，service自动注释和依赖; 控制器实现单表的增副改查，并集成swagger实现api文档。如果有缘看见，期望得到你的star，very thx.

github官网地址：https://github.com/flying-cattle/mybatis-dsc-generator

# 二、使用方法：

## 使用官方Mybatis Generator生成实体类、mapper接口和xml文件

配置mybatisconfiguration.xml文件，可以生lombok的entity类，mapper接口和mapperxml文件。

### 1、配置xml文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
<context id="Mysql" targetRuntime="MyBatis3">
    <!--官方序列化插件-->
    <plugin type="org.mybatis.generator.plugins.SerializablePlugin" />

    <!--Mapper插件，在生成的mapper接口中自动添加不依赖xml的sql方法-->
    <plugin type="tk.mybatis.mapper.generator.MapperPlugin">
        <property name="mappers" value="tk.mybatis.mapper.common.Mapper" />
        <property name="caseSensitive" value="false"/>
    </plugin>

    <!--lombok插件，在生成的entity类中添加@data注解-->
    <plugin type="com.chrm.mybatis.generator.plugins.LombokPlugin">
        <property name="hasLombok" value="true" />
    </plugin>

    <!-- 自动为entity生成swagger2文档-->
    <plugin type="mybatis.generator.plugins.GeneratorSwagger2Doc">
        <property name="apiModelAnnotationPackage" value="io.swagger.annotations.ApiModel" />
        <property name="apiModelPropertyAnnotationPackage" value="io.swagger.annotations.ApiModelProperty" />
    </plugin>

    <!--数据库连接-->
    <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                    connectionURL="jdbc:mysql://rm-bp1jzyd0u8yict1n1eo.mysql.rds.aliyuncs.com:3306/test_qlm_user?useSSL=false"
                    userId="employ"
                    password="employ_2020">
    </jdbcConnection>

    <!--生成Entity文件命名空间和文件路径-->
    <javaModelGenerator targetPackage="com.yuya.common.entity.console.organ"
                        targetProject="D:\yychildren\mybatisGenerator"/>

    <!--生成mapper接口类-->
    <sqlMapGenerator targetPackage="mapper"
                     targetProject="D:\yychildren\mybatisGenerator"/>

    <!--生成mapperxml-->
    <javaClientGenerator targetPackage="com.yuya.console.mapper.organ"
                         targetProject="D:\yychildren\mybatisGenerator" type="XMLMAPPER" />

    <table tableName="u_institution_apply_manager">
        <generatedKey column="id" sqlStatement="Mysql" />
    </table>
</context>

</generatorConfiguration>

```

### 2.用MybatisGenerator类生成文件
```java
package com.github.flying.cattle.mdg;

import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.Context;
import org.mybatis.generator.config.TableConfiguration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

import java.util.ArrayList;
import java.util.List;

public class GeneratorMapper {
    public static void main(String args[]) throws Exception {
        List<String> warnings = new ArrayList<String>();
        boolean overwrite = true;
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(
                GeneratorMapper.class.getResourceAsStream("/generator/generatorConfig.xml"));
        Context context=config.getContext("Mysql");
        List<TableConfiguration> configs= config.getContext("Mysql").getTableConfigurations();
        for(TableConfiguration c:configs){
            c.setCountByExampleStatementEnabled(false);
            c.setUpdateByExampleStatementEnabled(false);
            c.setUpdateByPrimaryKeyStatementEnabled(false);
            c.setDeleteByExampleStatementEnabled(false);
            c.setSelectByExampleStatementEnabled(false);
        }
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
        myBatisGenerator.generate(null);
    }
}
```


## 用代码生成Controller类、Service类和ServiceImpl类
```java
/**
 * @filename:UserController 2019年4月9日
 * @project wallet-manage  V1.0
 * Copyright(c) 2018 flying-cattle Co. Ltd. 
 * All right reserved. 
 */
package com.flying.cattle.mdg;

import java.sql.SQLException;
import java.util.Date;

import com.github.flying.cattle.mdg.entity.BasisInfo;
import com.github.flying.cattle.mdg.util.EntityInfoUtil;
import com.github.flying.cattle.mdg.util.Generator;
import com.github.flying.cattle.mdg.util.MySqlToJavaUtil;
/**   
 * Copyright: Copyright (c) 2019 
 * 
 * <p>说明： 自动生成工具</P>
 * <p>源码地址：https://gitee.com/flying-cattle/mybatis-dsc-generator</P>
 */
public class MyGenerator {
		// 基础信息：项目名、作者、版本
		public static final String PROJECT = "wallet-sign"; 
		public static final String AUTHOR = "BianPeng";
		public static final String VERSION = "V1.0";
		// 数据库连接信息：连接URL、用户名、秘密、数据库名
		public static final String URL = "jdbc:mysql://127.0.0.1:3306/buybit_wallet_sign?useUnicode=true&characterEncoding=utf-8&autoReconnect=true&failOverReadOnly=false&useSSL=true&serverTimezone=UTC";
		public static final String NAME = "";
		public static final String PASS = "";
		public static final String DATABASE = "buybit_wallet_sign";
		// 类信息：类名、对象名（一般是【类名】的首字母小些）、类说明、时间
		public static final String CLASSNAME = "CollectionRoute";
		public static final String TABLE = "collection_route";
		public static final String CLASSCOMMENT = "资金归集";
		public static final String TIME = "2019年6月12日";
		public static final String AGILE = new Date().getTime() + "";
		// 路径信息，分开路径方便聚合工程项目，微服务项目
		public static final String ENTITY_URL = "com.buybit.ws.entity";
		public static final String DAO_URL = "com.buybit.ws.dao";
		public static final String XML_URL = "com.buybit.ws.dao.impl";
		public static final String SERVICE_URL = "com.buybit.ws.service";
		public static final String SERVICE_IMPL_URL = "com.buybit.ws.service.impl";
		public static final String CONTROLLER_URL = "com.buybit.ws.web";
		//是否是Swagger配置
		public static final String IS_SWAGGER = "true";
		
	public static void main(String[] args) {
		BasisInfo bi = new BasisInfo(PROJECT, AUTHOR, VERSION, URL, NAME, PASS, DATABASE, TIME, AGILE, ENTITY_URL,
				DAO_URL, XML_URL, SERVICE_URL, SERVICE_IMPL_URL, CONTROLLER_URL,IS_SWAGGER);
		bi.setTable(TABLE);
		bi.setEntityName(MySqlToJavaUtil.getClassName(TABLE));
		bi.setObjectName(MySqlToJavaUtil.changeToJavaFiled(TABLE));
		bi.setEntityComment(CLASSCOMMENT);
		try {
			bi = EntityInfoUtil.getInfo(bi);
			String fileUrl = "E:\\a_item_work\\wallet\\wallet-manage\\wallet-manage-web\\src\\test\\java\\";// 生成文件存放位置
			//开始生成文件
			String aa1 = Generator.createEntity(fileUrl, bi).toString();
			String aa2 = Generator.createDao(fileUrl, bi).toString(); 
			String aa3 = Generator.createDaoImpl(fileUrl, bi).toString();
			String aa4 = Generator.createService(fileUrl, bi).toString(); 
			String aa5 = Generator.createServiceImpl(fileUrl, bi).toString(); 
			String aa6 = Generator.createController(fileUrl, bi).toString();
			// 是否创建swagger配置文件
			String aa7 = Generator.createSwaggerConfig(fileUrl, bi).toString();
			
			System.out.println(aa1);
			System.out.println(aa2); System.out.println(aa3); System.out.println(aa4);
			System.out.println(aa5); System.out.println(aa6); System.out.println(aa7);
			
			//System.out.println(aa7);
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
}
```

# 三、生成的文件展示

## 生成的Controller类

带有其他的import文件，实际使用中需要修改

```java
/**
 * @filename:UInstitutionApplyManagerController 
 * @project YUYA  V1.0
 * Copyright(c) 2020 杭州育伢 Co. Ltd. 
 * All right reserved. 
 */
package com.yuya.console.controller.organ;

import com.alibaba.dubbo.config.annotation.Reference;
import com.yuya.common.entity.console.UInstitutionApplyManager;
import com.yuya.console.service.organ.UInstitutionApplyManagerService;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.Date;
import com.hzcytech.common.page.PageList;
import com.hzcytech.common.page.Paginator;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;


import io.swagger.annotations.Api;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
/**
 *
 * <p>说明： API接口层</P>
 * @version: V1.0
 * @author: 杭州育伢
 * @time    
 *
 */
@Api(description = "",value="" )
@RestController
@RequestMapping("/manage/uInstitutionApplyManager")
public class UInstitutionApplyManagerController{

	Logger logger = LoggerFactory.getLogger(this.getClass());

    @Reference
    UInstitutionApplyManagerService uInstitutionApplyManagerService;
    
    @ResponseBody
    @PostMapping("/add")
    public void insertUInstitutionApplyManager(UInstitutionApplyManager uInstitutionApplyManager){
        uInstitutionApplyManager.setCreateTime(new Date());
        uInstitutionApplyManager.setIsDeleted(false);
        uInstitutionApplyManagerService.save(uInstitutionApplyManager);
    }
    
    @ResponseBody
    @PostMapping("/delete")
    public void deleteUInstitutionApplyManager(Integer id){
        UInstitutionApplyManager uInstitutionApplyManager  = uInstitutionApplyManagerService.selectByKey(id);
        if(null!=uInstitutionApplyManager) uInstitutionApplyManager.setIsDeleted(true);
        uInstitutionApplyManager.setUpdateTime(new Date());
        uInstitutionApplyManagerService.updateNotNull(uInstitutionApplyManager);
    }
    
    @ResponseBody
    @PostMapping("/modify")
    public void modifyUInstitutionApplyManager(UInstitutionApplyManager uInstitutionApplyManager){
        uInstitutionApplyManager.setUpdateTime(new Date()) ;
        uInstitutionApplyManagerService.updateNotNull(uInstitutionApplyManager);
    }
    
    @ResponseBody
    @PostMapping("/list")
    public PageList findUInstitutionApplyManagerPageList(UInstitutionApplyManager param,Paginator p){
        return uInstitutionApplyManagerService.findUInstitutionApplyManagerPageList(param,p);
    }
    
    @ResponseBody
    @PostMapping("/findById")
    public UInstitutionApplyManager findUInstitutionApplyManagerById(Integer id){
        return uInstitutionApplyManagerService.selectByKey(id);
    }
}
```

## 生成的Service类接口

## 生成的ServiceImpl类

## 生成的Mapper类接口

## 生成的Mapperxml文件
