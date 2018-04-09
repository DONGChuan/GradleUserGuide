# Performance 选项

在你想优化构建性能的时候可以尝试以下选项. 可以阅读[提升 Gradle 构建性能](https://guides.gradle.org/performance/)了解更多.

大部分选项都可以直接在 `gradle.properties` 文件中直接指定, 并不需要在命令行中添加选项. 阅读[配置构建环境指南](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_configuration_properties)了解更多.

`--build-cache`,`--no-build-cache`

这两个选项使用来切换[Gradle 构建缓存](https://docs.gradle.org/current/userguide/build_cache.html). Gradle 将会尝试再使用之前构建的输出._默认是关的_.

`--configure-on-demand`,`--no-configure-on-demand`

切换[Configure-on-demand](https://docs.gradle.org/current/userguide/multi_project_builds.html#sec:configuration_on_demand)(部分文章称之为孵化模式). 只有相关的项目才会在这个构建运行中被配置._默认是关的_.

`--max-workers`

设置 Gradle 可以使用的工作线程的最大数量._默认是处理器的数量_.

`--parallel`,`--no-parallel`

通过构建项目. 可以阅读[同步项目执行](https://docs.gradle.org/current/userguide/multi_project_builds.html#sec:parallel_execution)了解这个选项的局限线._默认是关的_.

`--profile`

在 `$buildDir/reports/profile` 文件夹生成一个高级别的性能报告. 但更推荐使用 `--scan`.

`--scan`

生成一个包含具体性能诊断的 build scan.

![](https://docs.gradle.org/current/userguide/img/gradle-core-test-build-scan-performance.png "Build Scan performance report")

### Gradle 守护进程选项

你可以通过下面的选项来管理[Gradle 守护进程](https://docs.gradle.org/current/userguide/gradle_daemon.html).

`--daemon`,`--no-daemon`

使用[Gradle 守护进程](https://docs.gradle.org/current/userguide/gradle_daemon.html)来构建项目. 开始守护进程如果没有运行或者正在运行的守护进程正忙._默认是开的_.

`--foreground`

在前台进程开始 Gradle 守护进程.

`--status`

\(Standalone command\)

运行 `gradle --status` 将会列出正在运行以及最近刚停止的 Gradle 守护进程. 只显示同一个 Gradle 版本的守护进程.

`--stop`

\(Standalone command\)

运行 `gradle --stop` 来停止同一个版本的所有的 Gradle 守护进程.

`-Dorg.gradle.daemon.idletimeout=(number of milliseconds)`

Gradle 守护进程空闲后, 将会在指定的毫秒数之后自己停止._默认是 10800000_\(3 个小时\).

