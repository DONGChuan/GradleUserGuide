# Logging 选项

### 设置 log level

以下为自定义 Gradle logging 相关的选项, 从显示最少的信息到最多的信息. 阅读[logging documentation](https://docs.gradle.org/current/userguide/logging.html)了解更多.

`-Dorg.gradle.logging.level=(quiet,warn,lifecycle,info,debug)`

Set logging level via Gradle properties.

`-q`,`--quiet`

安静模式, 只显示 erros. 其余一概不显示.

`-w`,`--warn`

Set log level to warn.

`-i`,`--info`

Set log level to info.

`-d`,`--debug`

Log in debug mode \(includes normal stacktrace\).

Lifecycle is the default log level.

### 自定义 log 样式

You can control the use of rich output \(colors and font variants\) by specifying the "console" mode in the following ways:

`-Dorg.gradle.console=(auto,plain,rich,verbose)`

Specify console mode via Gradle properties. Different modes described immediately below.

`--console=(auto,plain,rich,verbose)`

Specifies which type of console output to generate.

Set to`plain`to generate plain text only. This option disables all color and other rich output in the console output. This is the default when Gradle is_not_attached to a terminal.

Set to`auto`\(the default\) to enable color and other rich output in the console output when the build process is attached to a console, or to generate plain text only when not attached to a console._This is the default when Gradle is attached to a terminal._

Set to`rich`to enable color and other rich output in the console output, regardless of whether the build process is not attached to a console. When not attached to a console, the build output will use ANSI control characters to generate the rich output.

Set to`verbose`to enable color and other rich output like the`rich`, but output task names and outcomes at the lifecycle log level, as is done by default in Gradle 3.5 and earlier.

### Showing or hiding warnings

By default, Gradle won’t display all warnings \(e.g. deprecation warnings\). Instead, Gradle will collect them and render a summary at the end of the build like:

```
Deprecated Gradle features were used in this build, making it incompatible with Gradle 5.0.
```

You can control the verbosity of warnings on the console with the following options:

`-Dorg.gradle.warning.mode=(all,none,summary)`

Specify warning mode via[Gradle properties](https://docs.gradle.org/current/userguide/command_line_interface.html). Different modes described immediately below.

`--warning-mode=(all,none,summary)`

Specifies how to log warnings. Default is`summary`.

Set to`all`to log all warnings.

Set to`summary`to suppress all warnings and log a summary at the end of the build.

Set to`none`to suppress all warnings, including the summary at the end of the build.

### Rich Console

Gradle’s rich console displays extra information while builds are running.

![](https://docs.gradle.org/current/userguide/img/rich-cli.png "Gradle Rich Console")

Features:

* Logs above grouped by task that generated them

* Progress bar and timer visually describe overall status

* Parallel work-in-progress lines below describe what is happening now



