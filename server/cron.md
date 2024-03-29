
# 一、cron 表达式的定义

cron表达式是一个具有时间含义的字符串，字符串以5\~6个空格隔开，分为6\~7个域，格式为X X X X X X X。其中X是一个域的占位符。最后一个代表年份的域非必须，可省略每一个域代表一个时间含义。 

格式如下：

```
 [秒] [分] [时] [日] [月] [周] [年]
 ```
# 二、cron各个部分定义

| 域 |	是否必填 |	值以及范围 | 通配符 |
| ---- | ---- | ---- | ---- |
| 秒 |	是 | 0-59 |, - * / |
| 分 |	是 | 0-59 |, - * / |
| 时 |	是 | 0-23 |	, - * / |
| 日 |	是 | 1-31 |	, - * ? / L W |
| 月 |	是 | 1-12 或 JAN-DEC |	, - * / |
| 周 |	是 | 1-7 或 SUN-SAT	|, - * ? / L # |
| 年 |	否 | 1970-2099 |, - * / |

# 三、通配符定义

| 特殊字符 | 含义 | 示例 |
| ---- | ---- | ---- |
| *	| 所有可能的值。| * 表示所有值，可解读为 “每”。 如果在“日”这个域中设置 *,表示每一天都会触发。在月域中，*表示每个月；在星期域中，*表示星期的每一天。| 
| ,	| 列出枚举值。| 在两个以上的时间点中都执行，如果我们在 “分” 这个域中定义为 8,12,35 ，则表示分别在第8分，第12分 第35分执行该定时任务。5,20表示分别在5分钟和20分钟触发一次。| 
| -	| 范围。	| 指定在某个域的连续范围，如果我们在 “时” 这个域中定义 1-6，则表示在1到6点之间每小时都触发一次，用 , 表示 1,2,3,4,5,6| 
| /	| 指定数值的增量。| 该符号将其所在域中的表达式分为两个部分，其中第一部分是起始值，第二部分为增量，比如 在 “秒” 上定义 5/10 表示从 第 5 秒开始 每 10 秒执行一次，而在 “分” 上则表示从 第 5 秒开始 每 10 分钟执行一次。| 
| ?	| 不指定值，仅日期和星期域支持该字符。	| 使用的场景为不需要关心当前设置这个字段的值。例如:要在每月的8号触发一个操作，但不关心是周几，我们可以这么设置 0 0 0 8 * ?| 
| L	| 单词Last的首字母，表示最后一天，仅日期和星期域支持该字符。说明 指定L字符时，避免指定列表或者范围，否则，会导致逻辑问题。| 在日期域中，L表示某个月的最后一天。在星期域中，L表示一个星期的最后一天，也就是星期日（SUN）。如果在L前有具体的内容，例如，在星期域中的6L表示这个月的最后一个星期六。|
| W	| 除周末以外的有效工作日，在离指定日期的最近的有效工作日触发事件。W字符寻找最近有效工作日时不会跨过当前月份，连用字符LW时表示为指定月份的最后一个工作日。	| 在日期域中5W，如果5日是星期六，则将在最近的工作日星期五，即4日触发。如果5日是星期天，则将在最近的工作日星期一，即6日触发；如果5日在星期一到星期五中的一天，则就在5日触发。| 
| #	| 确定每个月第几个星期几，仅星期域支持该字符。	| 在星期域中，4#2表示某月的第二个星期四。| 

# 四、示例

| 示例| 说明 | 
| ---- | ---- |
| 0 15 10 ? * *	| 每天上午10:15执行任务| 
| 0 15 10 * * ?	| 每天上午10:15执行任务| 
| 0 0 12 * * ?	| 每天中午12:00执行任务| 
| 0 0 10,14,16 * * ?	| 每天上午10:00点、下午14:00以及下午16:00执行任务| 
| 0 0/30 9-17 * * ?	| 每天上午09:00到下午17:00时间段内每隔半小时执行任务| 
| 0 * 14 * * ?	| 每天下午14:00到下午14:59时间段内每隔1分钟执行任务| 
| 0 0-5 14 * * ?	| 每天下午14:00到下午14:05时间段内每隔1分钟执行任务| 
| 0 0/5 14 * * ?	| 每天下午14:00到下午14:55时间段内每隔5分钟执行任务| 
| 0 0/5 14,18 * * ?	| 每天下午14:00到下午14:55、下午18:00到下午18:55时间段内每隔5分钟执行任务| 
| 0 0 12 ? * WED	| 每个星期三中午12:00执行任务| 
| 0 15 10 15 * ?	| 每月15日上午10:15执行任务| 
| 0 15 10 L * ?	| 每月最后一日上午10:15执行任务| 
| 0 15 10 ? * 6L	| 每月最后一个星期六上午10:15执行任务| 
| 0 15 10 ? * 6#3	| 每月第三个星期六上午10:15执行任务| 
| 0 10,44 14 ? 3 WED	| 每年3月的每个星期三下午14:10和14:44执行任务| 
| 0 15 10 ? * * 2022	| 2022年每天上午10:15执行任务| 
| 0 15 10 ? * * *	| 每年每天上午10:15执行任务| 
| 0 0/5 14,18 * * ? 2022	| 2022年每天下午14:00到下午14:55、下午18:00到下午18:55时间段内每隔5分钟执行任务| 
| 0 15 10 ? * 6#3 2022,2023	| 2022年至2023年每月第三个星期六上午10:15执行任务| 
| 0 0/30 9-17 * * ? 2022-2025	| 2022年至2025年每天上午09:00到下午17:30时间段内每隔半小时执行任务| 
| 0 10,44 14 ? 3 WED 2022/2	| 从2022年开始，每隔两年3月的每个星期三下午14:10和14:44执行任务| 
| 0 */1 * * * ?  | 每隔1分钟执行一次| 
| 0 0 22 * * ? | 每天22点执行一次| 
| 0 0 1 1 * ? | 每月1号凌晨1点执行一次| 
| 0 0 23 L * ? | 每月最后一天23点执行一次| 
| 0 0 3 ? * L | 每周周六凌晨3点实行一次| 
| 0 24,30 * * * ? | 在24分、30分执行一次| 
| 0 0 10,14,16 * * ? | 每天上午10点，下午2点，4点| 
| 0 0/30 9-17 * * ? | 朝九晚五工作时间内每半小时| 
| 0 0 12 ? * WED | 表示每个星期三中午12点| 
| 0 0 12 * * ?|  每天中午12点触发| 
| 0 15 10 ? * *|  每天上午10:15触发| 
| 0 15 10 * * ?|  每天上午10:15触发| 
| 0 15 10 * * ? *| 每天上午10:15触发| 
| 0 15 10 * * ? 2005| 2005年的每天上午10:15触发| 
| 0 * 14 * * ?| 在每天下午2点到下午2:59期间的每1分钟触发| 
| 0 0/5 14 * * ?|  在每天下午2点到下午2:55期间的每5分钟触发| 
| 0 0/5 14,18 * * ?|  在每天下午2点到2:55期间和下午6点到6:55期间的每5分钟触发| 
| 0 0-5 14 * * ?| 在每天下午2点到下午2:05期间的每1分钟触发| 
| 0 10,44 14 ? 3 WED|  每年三月的星期三的下午2:10和2:44触发| 
| 0 15 10 ? * MON-FRI|  周一至周五的上午10:15触发| 
| 0 15 10 15 * ?|  每月15日上午10:15触发| 
| 0 15 10 L * ?| 每月最后一日的上午10:15触发| 
| 0 15 10 ? * 6L| 每月的最后一个星期五上午10:15触发| 
| 0 15 10 ? * 6L 2002-2005| 2002年至2005年的每月的最后一个星期五上午10:15触发| 
| 0 15 10 ? * 6#3 |  每月的第三个星期五上午10:15触发| 