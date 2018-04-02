# Debug 选项

`-?`,`-h`,`--help`

实现所有命令行选项的帮助信息.

`-v`,`--version`

打印 Gradle, Groovy, Ant, JVM 以及操作系统的版本信息.

`-S`,`--full-stacktrace`

打印完整的堆栈信息来查找任意异常. 查看[logging 选项](https://docs.gradle.org/current/userguide/command_line_interface.html#sec:command_line_logging)了解更多.

`-s`,`--stacktrace`

打印完整的堆栈信息来查看用户异常(user exceptions) \(e.g. 编译错误\). 查看[logging 选项](https://docs.gradle.org/current/userguide/command_line_interface.html#sec:command_line_logging)了解更多.

`--scan`

创建 [build scan](https://gradle.com/build-scans) 来查看构建各个方面的详细信息.

`-Dorg.gradle.debug=true`

Debug Gradle 客户端进程 \(非守护进程\). 默认的, 你可以在 `localhost:5005` 给 Gradle 添加一个调试器.

`-Dorg.gradle.daemon.debug=true`

Debug[守护进程](https://docs.gradle.org/current/userguide/gradle_daemon.html).

