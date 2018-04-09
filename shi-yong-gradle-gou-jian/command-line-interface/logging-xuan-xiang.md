# Logging 选项

### 设置日志级别

以下为自定义 Gradle 日志相关的选项, 从显示最少的信息到最多的信息. 阅读[logging documentation](https://docs.gradle.org/current/userguide/logging.html)了解更多.

`-Dorg.gradle.logging.level=(quiet,warn,lifecycle,info,debug)`

通过 Gradle properties 设置日志级别.

`-q`,`--quiet`

安静模式, 只显示 erros. 其余一概不显示.

`-w`,`--warn`

设置 warn 日志级别.

`-i`,`--info`

设置 info 日志级别.

`-d`,`--debug`

设置日志为 debug 模式 \(包含正常的堆栈跟踪信息\).

Log 级别默认为 Lifecycle.

### 自定义 log 样式

你可以通过下列方法指定 "console" 模式来控制输出的样式 \(颜色和字体\):

`-Dorg.gradle.console=(auto,plain,rich,verbose)`

通过 Gradle properties 指定控制台模式.

`--console=(auto,plain,rich,verbose)`

指定输出的控制台类型.

* `plain` 类型只产生纯文本. 所有的颜色和其他样式都被取消. 这也是 Gradle 没和终端连接在一起的默认类型.

* `auto`\(默认\) 类型是当构建进程显示在控制台的时候会激活控制台输出的颜色和其他样式, 如果未显示在控制台, 那就是纯文本. 这是 Gradle 未连接到终端的默认类型.

* `rich` 类型是指无论构建进程是否连接到控制台, 都会激活控制台输出的颜色和样式. 当没有连接的时候, 构建输出会使用 ANSI control characters 来生成输出的样式.

* `verbose` 类型会像 `rich` 一样激活颜色和样式, 但是是输出任务名和结果在 lifecycle log 级别, 就像在 Gradle 3.5 及以前都是如此.

### 显示或者隐藏警告

默认地, Gradle 不会显示所有的警告 \(e.g. deprecation 警告\). 取而代之, Gradle 会在构建结束后收集并生成一个报告:

```
Deprecated Gradle features were used in this build, making it incompatible with Gradle 5.0.
```

你可以控制控制台中显示的警告的日志级别:

`-Dorg.gradle.warning.mode=(all,none,summary)`

通过 Gradle properties 指定警告模式.

`--warning-mode=(all,none,summary)`

指定如何在日志中显示警告. 默认是`summary`.

* `all` 显示所有警告.

* `summary` 只在构建结束后显示一个报告.

* `none` 忽略所有警告, 也不在结束后生成报告.

### 富文本控制台

Gradle 的富文本控制台可以显示更多额外的信息:

![](https://docs.gradle.org/current/userguide/img/rich-cli.png "Gradle Rich Console")

特点:

* 上面的日志都是通过生成它们的任务来分组的

* 进度条和计时器非常直观的描述了全局的状态

* 进度条下方同步执行的命令行描述当前正在执行的任务



