
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

