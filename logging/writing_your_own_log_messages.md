# 编写自己的日志信息

用于记录在你的构建文件的简单方法是将消息写入标准输出.Gradle重定向任何东西写入到标准输出到它的log系统作为`QUITE`级别的log.

**例 17.1.使用标准输出写入log信息**

**build.gradle**

```gradle
println 'A message which is logged at QUIET level'
```

摇篮还提供了一个`logger`属性来构建脚本，这是[Logger](https://docs.gradle.org/current/javadoc/org/gradle/api/logging/Logger.html)的一个实例.这个接口继承自`SLF4J`接口并且加入了一F些Gradle的具体方法.下面是如何在构建脚本中使用此方法的例子：

**例 17.2.写入自己的log信息**

**build.gradle**

```gradle
logger.quiet('An info log message which is always logged.')
logger.error('An error log message.')
logger.warn('A warning log message.')
logger.lifecycle('A lifecycle info log message.')
logger.info('An info log message.')
logger.debug('A debug log message.')
logger.trace('A trace log message.')
```

你还可以在构建中将其他类直接挂接到Gradle的log系统中(例如`buildSrc`目录下的类).只使用`SLF4J logger`,使用这个`logger`的方式与构建脚本提供的`logger`方式相同.

**例 17.3.使用SLF4J写入log信息**

**build.gradle**

```gradle
import org.slf4j.Logger
import org.slf4j.LoggerFactory

Logger slf4jLogger = LoggerFactory.getLogger('some-logger')
slf4jLogger.info('An info log message logged using SLF4j')
```
