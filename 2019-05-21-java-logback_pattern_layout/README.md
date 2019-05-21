Logback 配置文件的根元素是 *&lt;configuration&gt;*，*&lt;configuration&gt;* 元素由零个或多个 *&lt;appender&gt;* 元素、零个或多个 *&lt;logger&gt;* 元素、最多一个 *&lt;root&gt;* 元素组成。

![image](1.png)

原文连接：https://logback.qos.ch/manual/configuration.html#syntax 。

---

在 Logback 配置文件中，当 *&lt;appender&gt;* 元素的 *class* 属性值是 `ch.qos.logback.core.OutputStreamAppender` 的子类时（例如常见的 `ch.qos.logback.core.ConsoleAppender` 或 `ch.qos.logback.core.RollingFileAppender`），*&lt;appender&gt;* 元素可以使用 *&lt;layout&gt;* -> *&lt;pattern&gt;* 元素或者 *&lt;encoder&gt;* -> *&lt;pattern&gt;* 元素指定「打印日志的事件」转换成「字符串」的转换格式。

在默认情况下，Logback 使用 `ch.qos.logback.classic.PatternLayout` 和 `ch.qos.logback.classic.encoder.PatternLayoutEncoder` 类实现上述功能，后者使用 [装饰器模式](https://blog.fantasticmao.cn/2018/02/14/设计模式概览/#装饰器模式-Decorator-Pattern) 将前者装饰成 `ch.qos.logback.core.encoder.Encoder` 组件，并最终赋值给 `ch.qos.logback.core.OutputStreamAppender` 的 `encoder` 字段。

原文链接：https://logback.qos.ch/manual/layouts.html#ClassicPatternLayout 。

---

顺带说一下，Logback 官方文档说「因为 Logback 的配置语法非常灵活，所以不能使用 DTD 或 XSD 文件来指定标准的语法」，但是我觉得，团队开发项目的时候，最好是有一个基本的 Logback 配置规范。这是我在 GitHub 上找的一个 Logback XML 配置文件对应的 XSD 文件：https://github.com/enricopulatzo/logback-XSD 。