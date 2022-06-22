
# 一、cron 表达式的定义

cron表达式是一个具有时间含义的字符串，字符串以5~6个空格隔开，分为6~7个域，格式为X X X X X X X。其中X是一个域的占位符。最后一个代表年份的域非必须，可省略每一个域代表一个时间含义。 

格式如下：

```
 [秒] [分] [时] [日] [月] [周] [年]
 ```
## 二、cron各个部分定义

| 域 |	是否必填 |	值以及范围 | 通配符 |
| ---- | ---- | ---- | ---- |
| 秒 |	是 | 0-59 |, - * / |
| 分 |	是 | 0-59 |, - * / |
| 时 |	是 | 0-23 |	, - * / |
| 日 |	是 | 1-31 |	, - * ? / L W |
| 月 |	是 | 1-12 或 JAN-DEC |	, - * / |
| 周 |	是 | 1-7 或 SUN-SAT	|, - * ? / L # |
| 年 |	否 | 1970-2099 |, - * / |
