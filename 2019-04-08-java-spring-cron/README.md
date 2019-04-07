Linux 有 [crontab](http://linux.vbird.org/linux_basic/0430cron.php#cron) 命令，Java 的 Spring Framework 有基于 [CronSequenceGenerator
](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/support/CronSequenceGenerator.html) 类相关的定时任务 API，两者使用的 cron 表达式大同小异，都是用来指定定时任务的执行策略。

下面以 Spring Framework 的 [Scheduled](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/annotation/Scheduled.html) 注解为例，记录一下 cron 表达式的基本使用方式。

Spring Framework 中的 cron 表达式由六个空格分隔的字段组成，分别代表了：秒、分、时、月内的天数、月、周内的天数。
```text
*    *    *    *    *    *
|    |    |    |    |    |
|    |    |    |    |    +----- day of week (0 - 7, Sunday=0 or 7)(MON, TUE ...)
|    |    |    |    +---------- month (1 - 12)
|    |    |    +--------------- day of month (1 - 31)
|    |    +-------------------- hour (0 - 23)
|    +------------------------- min (0 - 59)
+------------------------------ second (0 - 59)
```

除此之外，还可以使用以下这些符号控制任务执行的时间逻辑：
* \*: 表示该字段可以是任意值
* \-: 表示该字段值的可选范围，以减号相隔，例如 `6-9`
* \,: 表示该字段值的可选列表，以逗号分隔，例如 `6,7,8,9`
* \\: 表示该字段的可选值，与它的范围值和步长（step）有关，例如 `6-9/2`
* \?: 仅可用于「周内的天数」字段，主要用来解决月内和周内的两个天数的冲突

参考资料：
* [crontab manpage](https://www.manpagez.com/man/5/crontab/)
* [crontab 命令](http://man.linuxde.net/crontab)
* [鸟哥的 Linux 私房菜 - 循环执行的例行性工作安排](http://linux.vbird.org/linux_basic/0430cron.php#cron)