
# 一、Kaptcha介绍

kaptcha 是一个很有用的验证码生成工具。有了它，你能够生成各种样式的验证码，由于它是可配置的。

kaptcha工作的原理是调用com.google.code.kaptcha.servlet.KaptchaServlet，生成一个图片。

同一时候将生成的验证码字符串放到 HttpSession中。

配置项：

| 配置项 | 描述 | 可选值 | 默认值 |
| ---- | ---- | ---- | ---- |
|kaptcha.border	|图片边框	|yes,no	|yes |
|kaptcha.border.color|	边框颜色 |	r,g,b(and optional alpha) 或者 white,black,blue|	black |
|kaptcha.border.thickness|	边框厚度 |	>0|	1 |
|kaptcha.image.width|	图片宽|	>0|	200|
|kaptcha.image.height|	图片高|	>0|	50|
|kaptcha.producer.impl|	图片实现类	|	|com.google.code.kaptcha.impl.DefaultKaptcha|
|kaptcha.textproducer.impl|	文本实现类	|	|com.google.code.kaptcha.text.impl.DefaultTextCreator|
|kaptcha.textproducer.char.string|	文本集合|	|	abcde2345678gfynmnpwx|
|kaptcha.textproducer.char.length|	验证码长度||		5|
|kaptcha.textproducer.font.names|	字体|	|	Arial, Courier|
|kaptcha.textproducer.font.size	|字体大小||		40px|
|kaptcha.textproducer.font.color|	字体颜色|	r,g,b 或者 white,black,blue|	black|
|kaptcha.textproducer.char.space|	文字间隔||		2|
|kaptcha.noise.impl	|干扰实现类	|	|com.google.code.kaptcha.impl.DefaultNoise|
|kaptcha.noise.color |	干扰颜色	|r,g,b 或者 white,black,blue|	black|
|kaptcha.obscurificator.impl |	图片样式||	水纹com.google.code.kaptcha.impl.WaterRipple
鱼眼com.google.code.kaptcha.impl.FishEyeGimpy
阴影com.google.code.kaptcha.impl.ShadowGimpy	|com.google.code.kaptcha.impl.WaterRipple|
|kaptcha.background.impl |	背景实现类	|	|com.google.code.kaptcha.impl.DefaultBackground |
|kaptcha.background.clear.from |	背景颜色渐变，开始颜色 | |lightGray |
|kaptcha.background.clear.to |背景颜色渐变，结束颜色 | |white |
|kaptcha.word.impl |	文字渲染器	|	|com.google.code.kaptcha.text.impl.DefaultWordRenderer|
|kaptcha.session.key	|session key	||	KAPTCHA_SESSION_KEY|
|kaptcha.session.date |	session date	|	|KAPTCHA_SESSION_DATE |

# 二、在Spring boot中使用kaptcha

## 2.1 引入kaptcha的maven依赖
```xml
<!-- kaptcha验证码 -->
<dependency>
	<groupId>com.github.penggle</groupId>
	<artifactId>kaptcha</artifactId>
	<version>2.3.2</version>
</dependency>
```

## 2.2 编写kaptcha的配置类
```java
package com.yuya.console.config;

import com.google.code.kaptcha.impl.DefaultKaptcha;
import com.google.code.kaptcha.util.Config;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import java.util.Properties;

@Configuration
public class KaptchaConfig {

    private final static String CODE_LENGTH = "4";

    private final static String SESSION_KEY = "handsome_yang";

    @Bean
    public DefaultKaptcha defaultKaptcha() {
        DefaultKaptcha defaultKaptcha = new DefaultKaptcha();
        Properties properties = new Properties();
        // 设置边框，合法值：yes , no
        properties.setProperty("kaptcha.border", "yes");
        // 设置边框颜色，合法值： r,g,b (and optional alpha) 或者 white,
        properties.setProperty("kaptcha.border.color", "98,211,194");
        // 设置字体颜色， r,g,b 或者 white,black,blue.
        properties.setProperty("kaptcha.textproducer.font.color", "black");
        // 设置图片宽度
//        properties.setProperty("kaptcha.image.width", "118");
//        // 设置图片高度
//        properties.setProperty("kaptcha.image.height", "40");
        properties.setProperty("kaptcha.textproducer.char.space", "6");
        // 设置字体尺寸
//        properties.setProperty("kaptcha.textproducer.font.size", "0");
        // 设置session key
        properties.setProperty("kaptcha.session.key", SESSION_KEY);
        // 设置验证码长度
        properties.setProperty("kaptcha.textproducer.char.length", CODE_LENGTH);
        // 设置字体
        properties.setProperty("kaptcha.textproducer.font.names", "Arial,Courier,cmr10,宋体,楷体,微软雅黑");
        Config config = new Config(properties);
        defaultKaptcha.setConfig(config);
    	
        return defaultKaptcha;
    }
}
```

## 2.3 编写kaptcha的控制层

```java
 package com.yuya.console.controller.system;

import java.awt.image.BufferedImage;
import java.io.ByteArrayOutputStream;

import javax.imageio.ImageIO;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.google.code.kaptcha.impl.DefaultKaptcha;
import com.yuya.common.base.BaseController;

import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;

/**
 * 图片验证码（支持算术形式）
 * 
 * @author yuya
 */
@RestController
@RequestMapping("/captcha")
@Api(value = "验证码管理" , tags = "验证码管理（已完成）")
public class SysCaptchaController extends BaseController {
	
	 /**
     * 验证码工具
     */
    @Autowired
    DefaultKaptcha defaultKaptcha;

    @GetMapping("/kaptcha")
    @ApiOperation(value = "获取验证码", tags = "验证码管理（已完成）")
    public void defaultKaptcha(HttpServletRequest request, HttpServletResponse response) throws Exception {
        byte[] captcha = null;
        ByteArrayOutputStream out = new ByteArrayOutputStream();

        try {
            // 将生成的验证码保存在session中
            String createText = defaultKaptcha.createText();
            request.getSession().setAttribute("rightCode", createText);
            BufferedImage bi = defaultKaptcha.createImage(createText);
            ImageIO.write(bi, "jpg", out);
        } catch (Exception e) {
            response.sendError(HttpServletResponse.SC_NOT_FOUND);
            return;
        }

        captcha = out.toByteArray();
        response.setHeader("Cache-Control", "no-store");
        response.setHeader("Pragma", "no-cache");
        response.setDateHeader("Expires", 0);
        response.setContentType("image/jpeg");
        ServletOutputStream sout = response.getOutputStream();
        sout.write(captcha);
        sout.flush();
        sout.close();
    }
}
```

## 2.4 前端调用kaptcha接口生成验证码

```
<img th:src="@{/kaptcha/kaptcha-image}" class="ver_btn" onclick="this.src=this.src+'?c='+Math.random();"/>
```

## 2.5 服务器进行验证
```
/**
	 * 验证验证码
	 * @param
	 * @return 正确:true/错误:false
	 */
	public static boolean validate(String registerCode) {
		// 获取Session中验证码
		Object captcha = ServletUtils.getAttribute(Constants.KAPTCHA_SESSION_KEY);
		// 判断验证码是否为空
		if (StringUtils.isEmpty(registerCode)) {
			return false;
		}
		// 校验验证码的正确与否
		boolean result = registerCode.equalsIgnoreCase(captcha.toString());
		if (result) {
			// 正确了后，将验证码从session中删掉
			ServletUtils.getRequest().getSession().removeAttribute(Constants.KAPTCHA_SESSION_KEY);
		}
		// 返回验证结果
		return result;
	}
```
