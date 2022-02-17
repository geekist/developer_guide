
## 一、Swagger介绍

Swagger is the world’s largest framework of API developer tools for the OpenAPI Specification(OAS), enabling development across the entire API lifecycle, from design and documentation, to test and deployment。

swagger符合OpenAPI的世界上最流行的API开发工具，它可以跨越API的整个生命周期，从设计到文档，到测试到部署。简单来说，swagger就是一款api生成工具，能够帮助我们构建功能强大的API，我们可以使用swagger来进行测试等等。

Swagger官方网站:[swagger官方](https://swagger.io/)

## 二、Swagger包含的工具集：

- Swagger Editor： Swagger Editor允许您在浏览器中编辑YAML中的OpenAPI规范并实时预览文档。

- Swagger UI： Swagger UI是HTML，Javascript和CSS资产的集合，可以从符合OAS标准的API动态生成漂亮的文档。

- Swagger Codegen：允许根据OpenAPI规范自动生成API客户端库（SDK生成），服务器存根和文档。

- Swagger Parser：用于解析来自Java的OpenAPI定义的独立库。

- Swagger Core：与Java相关的库，用于创建，使用和使用OpenAPI定义。

- Swagger Inspector（免费）： API测试工具，可让您验证您的API并从现有API生成OpenAPI定义。

- SwaggerHub（免费和商业）： API设计和文档，为使用OpenAPI的团队构建。

## 三、在项目中集成swagger

### 3.1 添加swagger和swagger-ui的依赖

```xml

<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2
-->
<dependency>
<groupId>io.springfox</groupId>
<artifactId>springfox-swagger2</artifactId>
<version>2.9.2</version>
</dependency>

<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swaggerui
-->

<dependency>
<groupId>io.springfox</groupId>
<artifactId>springfox-swagger-ui</artifactId>
<version>2.9.2</version>
</dependency>


```