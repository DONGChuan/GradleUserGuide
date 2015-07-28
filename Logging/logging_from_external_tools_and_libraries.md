# 从外部工具和库记录日志

在内部,Gradle使用Ant和lvy,都有自己的log系统,Gradle重定向他们的日志输出到Gradle日志系统.除了Ant/lvy的`TRACE`级别的日志,映射到Gradle的`DEBUG`级别,其余的都会有一个1:1的映射从Ant/lvy的日志等级到Gradlede 的日志等级.这意味着默认的Gradle日志级别将不会显示任何的Ant /lvy的输出，除非它是一个错误或警告。

有许多工具仍然使用标准输出记录,默认的,Gradle将标准输出重定向到`QUIET`的日志级别和标准错误的`ERROR`级别.该行为是可配置的.该项目对象提供了一个[LoggerManager](https://docs.gradle.org/current/javadoc/org/gradle/api/logging/LoggingManager.html),当你构建脚本进行评估的时候,允许你改变标准输出或错误重定向的日志级别。

**例 17.4.配置标准输出捕获**

**build.gradle**

```gradle
logging.captureStandardOutput LogLevel.INFO
println 'A message which is logged at INFO level'
```

任务同样提供了[LoggingManager](https://docs.gradle.org/current/javadoc/org/gradle/api/logging/LoggingManager.html)去更改任务执行过程中的标准输出或错误日志级别。

**例 17.5.为任务配置标准输出捕获**

**build.gradle**

```gradle
task logInfo {
    logging.captureStandardOutput LogLevel.INFO
    doFirst {
        println 'A task message which is logged at INFO level'
    }
}
```

Gradle同样提供了`Java Util Logging`,`Jakarta Commons Logging`和` Log4j logging`的集成工具.

使用这些工具包编写的构建的类的记录的任何日志消息都将被重定向到Gradle的日志记录系统。

